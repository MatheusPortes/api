# Lacunas de Dados para Cálculos

> **Status:** documento de planejamento — não é um contrato de implementação.

Este documento inventaria os dados necessários para alimentar `src/control/stats/`, `src/control/progression/` e `src/control/damage/` no patch-alvo 7.1. Ele compara os contratos atualmente consumidos de `genshin.jmp.blue` com os inputs mecânicos exigidos pelas funções de cálculo.

## Regra de versão

- **Patch-alvo atual:** 7.1.
- Dados mecânicos, curvas, talentos e efeitos devem ser versionados pelo patch de origem.
- Valores textuais descritivos não são uma fonte adequada para cálculo até serem normalizados, associados ao patch e validados.

## Personagem

| Dado                                          | Fonte atual                                 | Situação                                               | Necessário para                                 |
| --------------------------------------------- | ------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- |
| ID, visão e tipo de arma                      | `GenshinCharacterData`                      | Disponível                                             | Identidade, seleção de arma e tipo de dano base |
| Nível e ascensão selecionados                 | `metadata.character`                        | Disponível                                             | Resolver progressão do personagem               |
| HP, ATK e DEF base por nível/ascensão         | `calculation` da entidade + progressão pura | Disponível apenas localmente                           | `CharacterBaseStats`                            |
| Curvas de crescimento                         | `GET /calculation/curves` local, patch 7.1  | Disponível apenas localmente; produção retorna 404     | Resolver atributos por nível                    |
| Atributo de ascensão e valor por estágio      | `calculation.ascensions[].bonuses`          | Adaptador estruturado para `StatModifier` implementado | Bônus de ascensão                               |
| Multiplicadores do talento por nível          | `skillTalents.upgrades[].value` textual     | Parcial, não estruturado                               | `DamageScaling`                                 |
| Atributo de escala do hit: ATK, HP, DEF ou EM | Nenhuma                                     | Ausente                                                | `DamageScaling.stat`                            |
| Elemento, infusão e conversão de cada hit     | Nenhuma                                     | Ausente                                                | Seleção de bônus de dano e RES                  |
| Regras calculáveis de passivas e constelações | `description` textual                       | Parcial, não estruturado                               | Modificadores e condições                       |

## Arma

| Dado                                    | Fonte atual                                           | Situação                 | Necessário para                    |
| --------------------------------------- | ----------------------------------------------------- | ------------------------ | ---------------------------------- |
| Tipo, raridade e ATK base de referência | `GenshinDatasWeapon`                                  | Disponível               | Seleção e identificação            |
| ATK base por nível/ascensão             | Nenhuma                                               | Ausente                  | `CharacterStatsInput.weaponAttack` |
| Substat e valor por nível               | `subStat` textual; `baseDamage` sem contrato de curva | Parcial, não estruturado | `StatModifier` da arma             |
| Passivo por refinamento                 | `passiveDesc` textual                                 | Parcial, não estruturado | Buffs e procs                      |
| Condições, duração, limites e procs     | Nenhuma                                               | Ausente                  | Regras declarativas de efeitos     |

## Artefatos

| Dado                                           | Fonte atual                                                | Situação                 | Necessário para             |
| ---------------------------------------------- | ---------------------------------------------------------- | ------------------------ | --------------------------- |
| Raridade, set e texto de bônus                 | `GenshinDatasArtifact`                                     | Disponível               | Seleção e apresentação      |
| Main stat e substats configurados              | `metadata.artifact`                                        | Disponível               | Construção de status        |
| Tabelas de roll/main stat por raridade e nível | `src/constants/artifact/scaling.ts`                        | Disponível               | Resolver status do artefato |
| Elemento do bônus de dano do cálice            | `elemental_dmg_percent` genérico                           | Ausente                  | Bônus específico do hit     |
| Regras dos bônus de set                        | Texto de `1-piece_bonus`, `2-piece_bonus`, `4-piece_bonus` | Parcial, não estruturado | Modificadores condicionais  |

## Inimigo

| Dado                                          | Fonte atual                        | Situação                             | Necessário para                          |
| --------------------------------------------- | ---------------------------------- | ------------------------------------ | ---------------------------------------- |
| ID, grupos, fases e tipos de dano             | `EnemyV2`                          | Disponível                           | Seleção da variante                      |
| Resistências por fase                         | `resistance[].stages[].resistance` | Parcial: valores são `string`        | `EnemyResistanceInput.base`              |
| Nível do inimigo                              | `metadata.enemy.data.level`        | Configurável, não fornecido pela API | `EnemyDefenseInput.level`                |
| Redução de DEF e ignore DEF                   | Nenhuma                            | Ausente                              | `EnemyDefenseInput.reduction` e `ignore` |
| Redução de RES por elemento                   | Nenhuma                            | Ausente                              | `EnemyResistanceInput.reduction`         |
| Mapeamento do tipo de dano para a RES correta | Nenhuma                            | Ausente                              | Seleção da resistência do hit            |

## Inputs sem fonte estruturada

### `calculateCharacterStats`

- `character.hp`, `character.attack`, `character.defense` são resolvidos por `calculateCharacterBaseStats` e exibidos em `SectionContent.vue` para nível/ascensão salvos; adaptadores de ascensão e artefatos estão implementados, enquanto arma, sets e buffs continuam pendentes;
- `weaponAttack` já resolvido para nível e ascensão;
- modificadores da ascensão, arma, artefato, set, passiva, constelação e buffs;
- normalização de percentuais de apresentação para razão (`46,6%` → `0.466`).

### `calculateDirectDamage`

- `ownerId` e `ownerLevel` do dono real do hit;
- `scalings`: atributo, multiplicador do talento no nível configurado e multiplicador-base do efeito;
- `additiveBaseDamage`;
- `damageBonus`, `targetDamageReduction` e `elevationMultiplier` aplicáveis ao hit;
- elemento/tipo físico do hit para selecionar bônus de dano e RES;
- redução/ignore de DEF e redução de RES da configuração ativa.

## Ordem recomendada de preenchimento

1. Disponibilizar em produção um dataset mecânico versionado para o patch 7.1 com curvas de personagem e arma; atualmente apenas as curvas de personagem existem no ambiente local.
2. Modelar hits de talentos com elemento, escalas, multiplicadores por nível e regras de conversão/infusão.
3. Implementar adaptadores puros que convertam personagem, arma e artefatos selecionados em `CharacterStatsInput`.
4. Normalizar resistências do inimigo por fase e criar o adaptador para `DirectDamageInput`.
5. Adicionar efeitos declarativos e versionados para sets, armas, passivas, constelações, buffs e debuffs.
6. Validar cada fonte mecânica no patch atual antes de habilitá-la na interface.
