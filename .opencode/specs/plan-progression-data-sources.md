# Plano — Dados de Progressão de Personagem e Arma

> **Status:** documento de planejamento — não é um contrato de implementação.

## Objetivo

Construir uma fonte versionada para calcular os atributos-base de personagens e armas por nível e ascensão, destinada às engines `stats` e `damage`.

**Patch-alvo atual:** 7.1.

## Estado atual

- `src/control/progression/calculateCharacterBaseStats.ts` resolve HP, ATK e DEF de personagens com a fórmula documentada neste arquivo.
- A entidade de personagem fornece `calculation.initialStats`, `growthCurves` e `ascensions`.
- O endpoint local `GET /calculation/curves` fornece as quatro curvas de personagem e declara `patch: "7.1"`; o mesmo caminho na API pública retorna HTTP 404.
- Progressão de arma, adaptadores de arma/set/buff e validação no jogo permanecem pendentes. A interface já exibe os status-base, bônus estruturados de ascensão e artefatos para nível/ascensão; fontes textuais não são interpretadas.

## Separação de responsabilidades

A curva de crescimento não define a escala de uma habilidade.

| Camada               | Responsabilidade                                          | Exemplo                         |
| -------------------- | --------------------------------------------------------- | ------------------------------- |
| Progressão           | Resolve HP, ATK, DEF e substats-base por nível e ascensão | DEF-base do Albedo no Lv. 90 A6 |
| Composição de status | Aplica HP%, ATK%, DEF%, valores planos e buffs            | DEF total do Albedo             |
| Escala do hit        | Escolhe o status total usado pelo talento                 | Flor Transitória: `134% DEF`    |
| Dano direto          | Aplica bônus de dano, CRIT, DEF/RES do inimigo            | dano final da Flor Transitória  |

## Fórmula de progressão

Para cada atributo que possui curva de crescimento:

```text
status-base no nível =
    status inicial × multiplicador da curva no nível
    + bônus cumulativo da ascensão selecionada
```

A engine não deve supor crescimento linear nem arredondar o valor interno. O arredondamento pertence à apresentação.

## Inputs públicos da interface

```ts
{
    entityId: number,
    level: number,
    ascension: number,
    patch: "7.1",
}
```

`ascension` é obrigatório: níveis de limite possuem estados distintos antes e depois da ascensão, como Lv. 20 A0 e Lv. 20 A1.

## Dados internos necessários

```ts
{
    initialStats: Record<Property, number>,
    growthCurves: Record<Property, CurveId>,
    ascensions: Array<{
        level: number,
        maxLevel: number,
        bonuses: Partial<Record<Property, number>>,
    }>,
}
```

`Property` inclui, no mínimo, HP-base, ATK-base, DEF-base e o substatus da arma ou atributo de ascensão quando aplicável.

## Fontes validadas por consulta HTTPS

### 1. Yatta API — dados estruturados de entidade

- Personagem: `https://gi.yatta.moe/api/v2/en/avatar/{avatarId}`
- Arma: `https://gi.yatta.moe/api/v2/en/weapon/{weaponId}`
- Índice de personagens: `https://gi.yatta.moe/api/v2/en/avatar`

Consultas validadas:

- `https://gi.yatta.moe/api/v2/en/avatar/10000002` — Kamisato Ayaka, HTTP 200;
- `https://gi.yatta.moe/api/v2/en/weapon/11501` — Aquila Favonia, HTTP 200.

Campos confirmados:

- `upgrade.prop[].initValue`: atributo inicial;
- `upgrade.prop[].type`: identificador de curva, por exemplo `GROW_CURVE_HP_S5`;
- `upgrade.promote[]`: ascensões, níveis máximos e `addProps` cumulativos;
- `talent.*.promote.*.params`: parâmetros numéricos de talentos por nível;
- armas: ATK-base, substat, curva e bônus de ascensão.

Limitação confirmada: o endpoint de curvas tentado, `https://gi.yatta.moe/api/v2/en/curve`, retorna HTTP 404. Não assumir que exista outro endpoint sem validação.

### 2. KQM TCL — tabela pública de curvas

- Curvas de personagem: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/character_curves.json`
- Curvas de arma: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/data/weapon_curves.json`
- Implementação de referência para personagens: `https://raw.githubusercontent.com/KQM-git/TCL/master/src/utils/stats/charstats.ts`

A implementação publicada da KQM confirma esta sequência:

```ts
stat = initialStat * curve[level - 1];
stat += ascensionBonus;
```

A KQM é uma fonte de tabelas versionáveis via Git, não uma API REST. O hash do commit usado na sincronização deve ser persistido junto com o patch e a data de acesso.

### 3. Verificação documental e numérica

Antes de marcar o dataset como validado para um patch:

1. salvar respostas brutas e data de acesso;
2. associar o snapshot ao patch declarado;
3. comparar ao menos personagens e armas de raridades/curvas distintas com a tabela KQM;
4. confrontar resultados de nível/ascensão com Genshin Optimizer e, quando possível, com o jogo;
5. registrar divergências sem corrigi-las por aproximação.

## Arquitetura recomendada

Não consultar Yatta e KQM a cada cálculo no frontend.

1. Um job no backend baixa os dados da Yatta e as tabelas de curva da KQM;
2. o job normaliza `FIGHT_PROP_*` para o contrato interno;
3. o resultado é persistido como snapshot imutável por patch;
4. o backend expõe endpoints próprios ao frontend;
5. o frontend usa apenas o snapshot selecionado para construir inputs puros de progressão, status e dano.

Isso reduz dependência externa em tempo real, permite reprodução de cálculos antigos e impede mistura acidental de fontes de patches diferentes.

## Próxima implementação proposta

1. Definir os contratos de snapshot e de propriedades normalizadas;
2. criar uma engine pura de progressão compartilhada por personagem e arma;
3. criar adaptadores do snapshot para a engine;
4. cobrir Lv. 1, limites de ascensão, A6, substats de arma e máximo de nível específico por entidade;
5. integrar o resultado a `calculateCharacterStats` somente após validar os snapshots do patch 7.1.
