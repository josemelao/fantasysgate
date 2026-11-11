# TACTICAL FANTASY RPG

## DIREÇÃO TÉCNICA E DECISÕES — v0.2

**Documento operacional de decisões do protótipo e da v1.0**

Versão 0.2 | 06 de novembro de 2026

Complementa o **GDD Consolidado v1.0** e a **Arquitetura Técnica e Backlog v1.0**.
Substitui esses documentos apenas nos pontos indicados explicitamente na Seção 2.

---

## CONTROLE DO DOCUMENTO

| Campo | Definição |
|---|---|
| Projeto | Tactical Fantasy RPG |
| Documento | Direção Técnica e Decisões |
| Versão | 0.2 — 06/11/2026 |
| Documentos-base | GDD v1.0; Arquitetura Técnica e Backlog v1.0 |
| Engine | Godot 4.7.2 stable |
| Linguagem | GDScript |
| Plataforma prioritária | Android; Redmi Note 13 como dispositivo de referência |
| Modelo de produção | Solo + agente de IA; orçamento zero; sem prazo |
| Status | Documento vivo de decisões; substitui os v1.0 apenas onde indicado |

### Convenção de status

- **DEFINIDO** — decisão tomada; pode ser implementada.
- **PROVISÓRIO** — ponto de partida; será validado em protótipo/playtest.
- **PENDENTE** — ainda não decidido; bloqueia alguma implementação.
- **FUTURO** — previsto na arquitetura, fora da v1.0.

### Regra de precedência

1. Este documento vence onde houver uma linha explícita na Seção 2.
2. O GDD v1.0 continua valendo para visão, pilares, conteúdo e regras não alteradas.
3. A Arquitetura v1.0 continua valendo para responsabilidades, camadas e backlog não alterados.
4. Quando uma decisão provisória for validada, ela deve ser incorporada ao GDD/Arquitetura em uma versão futura (v1.1 ou v2.0) e marcada como incorporada aqui.

---

# 1. SUMÁRIO EXECUTIVO

## 1.1 O projeto

Tactical Fantasy RPG é um RPG tático de fantasia, mobile-first, com estrutura roguelite, combate por turnos e movimento livre em arenas isométricas. A referência de sensação é **“BG3 simplificado para mobile”**, mas o produto não é uma campanha narrativa: é uma sequência de expedições curtas com decisões de construção, risco e recompensa.

O projeto é um **jogo completo de longo prazo**, feito solo, com orçamento zero, usando IA para arte e um agente de IA para engenharia. Não há prazo. A estratégia é construir em fatias verticais, validar diversão cedo e expandir conteúdo depois.

## 1.2 Decisão visual central

O jogo será **2D isométrico com câmera fixa e ângulo mais alto**. Não haverá 2.5D verdadeiro nem tilt de câmera na v1.0. Profundidade será criada com camadas, Y-sort, sombras, oclusão simples e composição de cenário.

O “2.5D” fica como experimento posterior, somente se o teste visual e o playtest mostrarem que a câmera fixa limita a experiência.

## 1.3 Decisão técnica central

- **Godot 4.7.2 stable** (4.8 ainda está em beta; não usar em produção).
- **GDScript** com tipagem estática.
- Conteúdo orientado a dados via **Resources**.
- Sistemas de domínio separados da apresentação e da UI.
- Estado de combate serializável desde o início.
- Testes automatizados das regras críticas.
- Build Android desde a primeira fase.

## 1.4 Estratégia de escopo

O que a v1.0 entrega:

- 3 classes, 4 raças, 1 subclasse por classe.
- 1 região, 12 nodes, 1 chefe.
- 8–10 inimigos, 24–30 itens, 5–6 condições.
- 6 slots de equipamento.
- Tutorial curto, save/retomada, meta progressão horizontal.
- Android funcional no Redmi Note 13.

O que fica para depois:

- 2.5D/tilt, elevação, água/eletricidade, propagação de fogo.
- Feats, sets, multiclass, 10 slots, 12 condições.
- Roubo/prisão, nuvem, múltiplas runs, PC.
- Mais regiões, classes, itens e campanha narrativa.

---

# 2. MAPA DE SUBSTITUIÇÕES SOBRE OS DOCUMENTOS v1.0

| Tema | Onde estava no v1.0 | Decisão v0.2 | Status |
|---|---|---|---|
| Câmera/tilt | GDD §9.4; Arquitetura §8.3 | 2D isométrico fixo, ângulo alto; sem tilt na v1.0 | DEFINIDO |
| Engine | GDD §16; Arquitetura §1 | Godot 4.7.2 stable | DEFINIDO |
| Linguagem | Não especificada | GDScript | DEFINIDO |
| Proficiência de armadura | GDD §3.7 | Sem proficiência: desvantagem em FOR/DES e não conjura | PROVISÓRIO |
| Slots de equipamento | GDD §6.1 (10 slots) | 6 slots na v1.0 | DEFINIDO |
| Condições | GDD §4.8 (12 condições) | 5–6 condições na v1.0 | DEFINIDO |
| Feats/talentos | GDD §5.2 | Fora da v1.0 | DEFINIDO |
| Sets | GDD §6.3 | Fora da v1.0 | DEFINIDO |
| Multiclass | GDD §5.2 | Fora da v1.0 | DEFINIDO |
| Cap de nível por run | GDD §5.1 (12 provisório) | Cap 8 por run; 12 fica para o jogo completo | PROVISÓRIO |
| Subclasse | GDD §5.2 (nível 3) | Nível 3 para todas as classes | DEFINIDO |
| Passivas do protótipo | GDD §15.1 (uma) | Três: permanente, condicional e reativa | DEFINIDO |
| Save | GDD §12.2; Arquitetura §11.2 | Autosave no início de cada turno + fim de node | DEFINIDO |
| Intenção inimiga | GDD §8.2 | Toggle de acessibilidade separado da dificuldade | DEFINIDO |
| Ambiente | GDD §9.3 | Um tipo de hazard na v1.0; água/eletricidade futuras | DEFINIDO |
| “Nível da conta” | GDD §3.4 | Fora da v1.0 | DEFINIDO |
| Evento semanal 4d6 | GDD §3.4 | Fora da v1.0 | DEFINIDO |
| Fuga | Não existia | Retirada custosa | PROVISÓRIO |
| Revive | GDD §4.9 (raro) | Revive nos rests + ação Help em combate | PROVISÓRIO |
| Duração de combate | Não definida | 4–6 min normal; 6–9 elite; 8–12 chefe | PROVISÓRIO |
| UI mobile | GDD §17; Arquitetura §14 (fase 8) | HUD funcional desde a fase 1; polimento depois | DEFINIDO |
| Save na fase 11 | Arquitetura §14 | Modelo de estado desenhado desde a fase 2; implementação cedo | DEFINIDO |

---

# 3. VISÃO, LOOP E PILARES

## 3.1 Fantasia central

O jogador comanda três aventureiros em expedições curtas, perigosas e emergentes. Cada run é uma história tática: escolhas de rota, composição, posicionamento, habilidades, recursos e risco. A morte é real, o loot é temporário, mas o conhecimento e os desbloqueios permanecem.

## 3.2 Loop principal

1. Escolher 3 personagens do elenco permanente.
2. Iniciar uma expedição em uma região.
3. Percorrer um mapa ramificado de 12 nodes.
4. Resolver combates, elites, eventos, tesouro, mercador e descanso.
5. Evoluir temporariamente: XP, níveis, habilidades, equipamentos e ouro.
6. Sobreviver ao chefe ou encerrar a run.
7. Perder o temporário; manter desbloqueios horizontais.
8. Voltar ao elenco e iniciar outra run.

## 3.3 Pilares

1. **Combate tático primeiro.** Posicionamento, alvo, ações, recursos, risco e sinergia.
2. **Identidade D&D adaptada.** Atributos, dados, CA, proficiência, condições, classes e magias reconhecíveis.
3. **Roguelite de progressão temporária.** A run evolui; o fim reinicia o poder e mantém a variedade.
4. **Movimento livre.** Sem grid obrigatório; pathfinding, alcance, linha de visão e cobertura.
5. **Mobile-first.** Toque, legibilidade, sessões interrompíveis e save confiável.
6. **Densidade de decisão.** Combates podem ser longos se cada turno tiver escolhas reais.
7. **Arquitetura expansível.** Dados e sistemas desacoplados, sem implementar conteúdo futuro.

## 3.4 O que torna o jogo único

Não é ter dezenas de classes ou um mundo aberto. É a combinação de:

- Combate tático com liberdade de posicionamento e reações, inspirado em BG3.
- Estrutura de run roguelite com decisões de rota, risco e recursos.
- Construção de personagem e decisões de RPG em sessões mobile.
- Consequência real: morte, loot temporário, revive custoso e retirada difícil.

Todo sistema novo deve responder: **“isso deixa o loop principal mais divertido?”** Se não, fica para depois.

## 3.5 Não objetivos da v1.0

- Mundo aberto.
- Campanha narrativa extensa.
- Multiplayer/PvP.
- Crafting complexo.
- Simulação de economia.
- 3D/2.5D com câmera livre.
- Dezenas de classes, raças, itens ou regiões.
- Progressão permanente de poder.

---

# 4. ESCOPO REVISADO

## 4.1 Conteúdo da v1.0

| Área | Quantidade | Observação |
|---|---|---|
| Classes | 3 | Fighter/Champion, Wizard/Evocation, Rogue/Thief |
| Subclasses | 3 | Uma por classe; escolhida no nível 3 |
| Raças | 4 | Humano, Elfo, Anão, Halfling |
| Antecedentes | 4 | Soldado, Criminoso, Sábio, Herói Popular |
| Regiões | 1 | Uma identidade temática completa |
| Nodes por run | 12 | 6 combates normais, 2 elites, 1 chefe, 3 utilitários |
| Atos | 3 | 4 nodes por ato |
| Chefe | 1 | Com habilidades, passivas e mecânica própria |
| Inimigos | 8–10 | Variedade de funções táticas |
| Variantes de elite | 2 | Reuso de inimigos com ajustes |
| Itens | 24–30 | Inclui armas, armaduras, acessórios e consumíveis |
| Slots de equipamento | 6 | Arma, offhand/escudo, armadura, elmo, amuleto, botas |
| Condições | 5–6 | Caído, agarrado, atordoado, amedrontado, envenenado, cego |
| Habilidades ativas | 18–24 | Inclui magias e habilidades de classe/item |
| Passivas | 12–18 | Raciais, classe, subclasse, equipamento |
| Eventos | 15–20 | Com escolhas e consequências |
| Mercadores | 1–2 por run | Estoque, preços e desconto |
| Arenas | 4–6 | Reutilizáveis com configurações variáveis |
| Consumíveis | 6–8 | Poções, pergaminhos, bombas, veneno, elixir |
| Tutorial | 15–25 min | Com opção de pular para veteranos |

## 4.2 Cortes e adiamentos

| Item | Decisão | Motivo |
|---|---|---|
| 10 slots de equipamento | 6 na v1.0 | UI, conteúdo e balanceamento |
| 12 condições | 5–6 na v1.0 | Complexidade e testes |
| Feats/talentos | Fora da v1.0 | Conteúdo e balanceamento |
| Sets de equipamento | Fora da v1.0 | Conteúdo e UI |
| Multiclass | Fora da v1.0 | Complexidade de progressão |
| 2.5D/tilt | Fora da v1.0 | Custo de arte, câmera e performance |
| Elevação | Fora da v1.0 | LOS, navegação e arte |
| Água/eletricidade | Fora da v1.0 | Subsistema de superfícies |
| Propagação de fogo | Fora da v1.0 | Complexidade de ambiente |
| Roubo/prisão | Fora da v1.0 | Ramificação de conteúdo |
| Nuvem/múltiplos dispositivos | Fora da v1.0 | Persistência local primeiro |
| Múltiplas runs simultâneas | Fora da v1.0 | Uma run ativa por slot |
| PC/Steam | Futuro | Mobile primeiro |
| Campanha narrativa | Futuro | Foco no loop roguelite |

## 4.3 Escopo do protótipo (M1–M2)

O protótipo não é o jogo completo. Ele existe para provar:

- Movimento livre é legível e preciso no toque.
- A fila de iniciativa é compreensível.
- A economia de ações funciona.
- O combate sem grid é divertido.
- A UI permite entender o que está acontecendo.
- O save/retomada é confiável.
- O jogo roda bem no Redmi Note 13.

## 4.4 Demo e versão completa

A possibilidade de **demo + versão completa** fica registrada, mas não será implementada agora.

- Um único projeto, com conteúdo gateado por configuração/export preset.
- A demo provavelmente será o Ato 1 ou uma run curta curada.
- A versão completa terá os 3 atos, todas as classes/raças e o chefe.
- A fronteira exata será decidida depois do M2, com base no que for mais vendável e justo.
- Não construir duas bases de código.

---

# 5. DIREÇÃO TÉCNICA

## 5.1 Stack

| Item | Decisão |
|---|---|
| Engine | Godot 4.7.2 stable |
| Linguagem | GDScript com tipagem estática |
| Dados | Resources (`Resource`) + IDs estáveis |
| Testes | GdUnit4 (preferência inicial; validar compatibilidade na M0) ou GUT |
| Versionamento | Git + Git LFS para arte/áudio |
| Build | Android (APK de teste; AAB para loja) |
| Documentação | Markdown em português; código e identificadores em inglês |
| Localização | en-US no jogo; chaves de tradução desde o dia 1 |

### Por que GDScript

- Mais simples de ler e manter com pouca experiência.
- Melhor integração com o editor e com o agente de IA.
- Builds Android menores que C#/.NET.
- Performance suficiente para um jogo por turnos.
- C# pode ser considerado no futuro, mas não é necessário.

### Por que não Unity/Unreal/GameMaker

- Unity: mais pesado, mais configuração, builds maiores, complexidade desnecessária para 2D.
- Unreal: excesso de engine para o escopo.
- GameMaker: bom em 2D, mas menos flexível para sistemas de RPG orientados a dados.
- Godot: melhor custo/benefício para 2D isométrico mobile solo.

## 5.2 Princípios arquiteturais

1. Separar regras, apresentação, UI e persistência.
2. Conteúdo configurável por dados; evitar regras específicas espalhadas.
3. Sistemas de domínio como classes comuns (`RefCounted`), com dependências explícitas.
4. Evitar autoloads globais para lógica de domínio; usar escopo de combate/run.
5. Estado de combate serializável desde o início.
6. Um EventBus escopado por combate, não um singleton global de tudo.
7. Validadores compartilhados entre jogador e IA.
8. RNG injetável e reproduzível em testes.
9. Nenhuma regra crítica duplicada em scripts de unidade ou UI.
10. Construir apenas o necessário para o marco atual.

## 5.3 Estrutura de diretórios

```text
res://
├── project.godot
├── AGENTS.md
├── assets/
│   ├── characters/
│   ├── enemies/
│   ├── environments/
│   ├── effects/
│   ├── ui/
│   └── audio/
├── scenes/
│   ├── main/
│   ├── combat/
│   ├── characters/
│   ├── enemies/
│   ├── environment/
│   └── ui/
├── scripts/
│   ├── core/
│   ├── combat/
│   ├── turn_system/
│   ├── action_economy/
│   ├── dice/
│   ├── movement/
│   ├── targeting/
│   ├── abilities/
│   ├── passive_abilities/
│   ├── status_effects/
│   ├── environment/
│   ├── characters/
│   ├── enemies/
│   ├── inventory/
│   ├── progression/
│   ├── runs/
│   ├── save/
│   └── ui/
├── data/
│   ├── classes/
│   ├── subclasses/
│   ├── races/
│   ├── backgrounds/
│   ├── abilities/
│   ├── passive_abilities/
│   ├── characters/
│   ├── enemies/
│   ├── items/
│   ├── conditions/
│   ├── encounters/
│   ├── events/
│   ├── regions/
│   └── loot_tables/
├── tests/
│   ├── combat/
│   ├── turn_system/
│   ├── movement/
│   ├── abilities/
│   ├── passive_abilities/
│   ├── progression/
│   └── save/
└── docs/
    ├── architecture.md
    ├── combat_rules.md
    ├── decisions.md
    └── development_log.md
```

O documento atual pode ser movido para `docs/decisions.md` quando o projeto Godot for criado.

## 5.4 Estrutura da cena de combate

```text
CombatScene
├── World
│   ├── NavigationRegion2D
│   ├── Environment
│   └── InteractiveObjects
├── Units
│   ├── PlayerCharacters
│   └── Enemies
├── CombatSystems
│   ├── CombatManager
│   ├── TurnManager
│   ├── ActionEconomy
│   ├── MovementSystem
│   ├── TargetingSystem
│   ├── PassiveAbilitySystem
│   └── CombatEventBus
├── Camera
└── UI
    ├── InitiativeHUD
    ├── ActionPanel
    ├── UnitDetailsPanel
    └── CombatLog
```

## 5.5 Modelo de dados

### Resources de conteúdo

| Resource | Campos conceituais mínimos |
|---|---|
| CharacterData | ID, nome-base, raça, classe, atributos-base, proficiências, aparência, passivas |
| ClassData | ID, progressão, proficiências, habilidades por nível, subclasse |
| SubclassData | ID, classe-base, nível de escolha, características, habilidades |
| RaceData | ID, bônus flexíveis, características, passivas |
| BackgroundData | ID, perícias e proficiências concedidas |
| AbilityData | ID, custo, tipo de ação, alcance, alvos, área, requisitos, rolagem, efeito, tags |
| PassiveAbilityData | ID, gatilhos, condições, prioridade, limites, efeitos, acumulação |
| EnemyData | ID, atributos, CA, PV, deslocamento, função tática, perfil, habilidades, loot table |
| ItemData | ID, slot, raridade, propriedades, atributos, passivas, habilidades, valor |
| ItemInstance | ID da instância, ID do ItemData, estado, modificadores rolados |
| ConditionData | ID, efeitos, duração, acumulação, resistência, remoção |
| EncounterData | ID, arena, composição, posições, dificuldade, configuração ambiental |
| EventData | ID, texto, escolhas, testes, requisitos, resultados, consequências |
| RegionData | ID, identidade, pool de encontros, nodes, eventos, mercadores, recompensas |
| MapData / NodeData | ID, ato, linha, tipo, conexões, dificuldade, seed, estado |
| LootTableData | ID, entradas, pesos, raridade, ouro, garantias |
| ShopData | ID, estoque, preços, desconto, estado |
| SaveData | Versão, metadados, estado permanente, estado da run, estado do combate |

### Dados permanentes e temporários

| Permanente | Temporário da run |
|---|---|
| Slots do elenco e personagens criados | Equipe selecionada e estado atual |
| Nome, raça, classe, aparência, configurações-base | Nível, XP, PV, recursos, condições, habilidades |
| Desbloqueios de classes, subclasses, itens, eventos, cosméticos | Equipamentos, ouro, consumíveis, inventário |
| Conquistas, milestones, configurações | Mapa, node atual, eventos resolvidos, estado de combate |

IDs estáveis são obrigatórios. Nunca salvar nomes de exibição como identificadores.

## 5.6 Pipeline de turno

1. Fim do turno anterior: expirar efeitos com duração “fim do turno”.
2. Início da rodada, se aplicável: eventos de rodada.
3. Início do turno: `on_turn_start`.
4. Restaurar reação e recursos de início de turno.
5. Testes contra morte, se caído.
6. Processar efeitos de início de turno (condições, concentração, dano contínuo).
7. Fase de ações: jogador ou IA.
8. Fim do turno: `on_turn_end`.
9. Expirar efeitos com duração “fim do turno”.
10. Avançar para a próxima unidade viva.
11. Atualizar UI.

A ordem exata de passivas e efeitos deve ser testada. Eventos devem ser enfileirados para evitar recursão descontrolada.

## 5.7 Pipeline de dano e modificadores

1. Rolagem base (dados).
2. Crítico: dobrar os dados de dano, não os modificadores.
3. Somar modificadores flat.
4. Aplicar multiplicadores.
5. Aplicar resistência, vulnerabilidade e imunidade.
6. Arredondar e limitar a zero.
7. Aplicar dano.
8. Publicar eventos (`on_damage_dealt`, `on_damage_received`, `on_kill`).

Modificadores de atributos seguem pipeline semelhante: base → soma → multiplicação → limite. Passivas declaram prioridade e regra de acumulação. Evitar que duas passivas alterem o mesmo valor sem ordem definida.

## 5.8 EventBus e escopo

- `CombatEventBus` por combate, não autoload global.
- Payload estruturado: origem, alvo, habilidade, valores antes/depois, tags, referência ao combate.
- Nenhuma lógica depende de texto do log.
- Eventos iniciais: `on_combat_start`, `on_round_start`, `on_turn_start`, `on_turn_end`, `on_attack_start`, `on_attack_hit`, `on_attack_miss`, `on_damage_dealt`, `on_damage_received`, `on_critical_hit`, `on_kill`, `on_movement`, `on_condition_applied`, `on_condition_removed`, `on_ally_down`, `on_reaction_triggered`, `on_death_save_success`, `on_death_save_failure`, `on_combat_end`.
- Eventos adicionais só quando houver um caso real.

## 5.9 Testes

### Unitários prioritários

- Dados, vantagem/desvantagem, críticos.
- Ataque contra CA, testes de resistência, CD.
- Dano, resistências, vulnerabilidades, imunidades.
- Economia de ações e restauração de reação.
- Iniciativa e transição de turnos.
- Condições: aplicação, duração, remoção.
- Passivas por evento e prioridade.
- Alcance, linha de visão, cobertura e movimento.
- XP, níveis e encerramento de run.
- Serialização e restauração de estado.

### Integração

- Ataque iniciado pela UI percorre validação, resolução e apresentação.
- Passiva reativa dispara no evento correto e não dispara fora das condições.
- Unidade incapacitada sai do fluxo corretamente.
- Save no início do turno restaura estado equivalente.
- Run concluída preserva desbloqueios e descarta temporários.
- Retirada encerra o combate sem conceder recompensas.

### Qualidade

- Nenhum erro crítico no console durante o fluxo validado.
- Regras testáveis sem arena ou animação.
- Comportamento reproduzível com seed.
- Performance aceitável no Redmi Note 13.
- Nenhuma regra crítica duplicada na UI.

## 5.10 Android e performance

- Export Android configurado na M0.
- Dispositivo de referência: Redmi Note 13.
- Meta inicial: 60 fps em combate com 6–8 unidades; aceitável 30 fps em picos.
- Meta de tamanho: abaixo de 100 MB; buscar 50–70 MB.
- Texturas comprimidas, atlas, áudio OGG, sem vídeo, sem 3D.
- Safe area, DPI e aspect ratios de 16:9 a 20:9; testar tablet depois.
- Perfil de performance em combate real, não só no editor.

---

# 6. MATRIZ DE REGRAS v0.1

> Base declarada: D&D 5e 2014 como padrão, com adaptações pontuais de D&D 2024/BG3 e homebrew. Cada regra deve ter uma fonte e um status. Quando houver dúvida, a decisão está aqui.

## 6.1 Personagens e criação

| Regra | Decisão | Status |
|---|---|---|
| Atributos | STR, DEX, CON, INT, WIS, CHA; modificadores tradicionais | DEFINIDO |
| Valores iniciais | Presets por classe + personalização; Point Buy 27 como opção | PROVISÓRIO |
| Bônus racial | Flexível, modelo 2024/BG3: +2/+1 ou +1/+1/+1 | PROVISÓRIO |
| Raças | Humano, Elfo, Anão, Halfling | DEFINIDO |
| Passivas raciais | Identidade própria por raça; exemplos a definir | PENDENTE |
| Antecedentes | 4; concedem perícias/proficiências; sem características narrativas na v1.0 | DEFINIDO |
| Perícias | 18 tradicionais | DEFINIDO |
| Classes | Fighter/Champion, Wizard/Evocation, Rogue/Thief | DEFINIDO |
| Subclasse | Nível 3 para todas as classes | DEFINIDO |
| Cap por run | 8 (proposta); 12 para o jogo completo | PROVISÓRIO |
| ASI | Níveis 4 e 8 com cap 8; 12 no jogo completo | PROVISÓRIO |
| Feats | Fora da v1.0 | DEFINIDO |
| Multiclass | Fora da v1.0 | DEFINIDO |
| Proficiência | +2 níveis 1–4; +3 níveis 5–8; +4 níveis 9–12 | PROVISÓRIO |
| PV | Dado de vida da classe + CON; nível 1 máximo; níveis seguintes média fixa | PROVISÓRIO |
| Proficiência de arma | Sem proficiência: sem bônus de proficiência no ataque | DEFINIDO |
| Proficiência de armadura | Sem proficiência: desvantagem em testes de FOR/DES e não pode conjurar | PROVISÓRIO |
| Equipamento inicial | Definido pela classe + antecedente; alternativas compatíveis | PROVISÓRIO |

## 6.2 Combate

| Regra | Decisão | Status |
|---|---|---|
| Iniciativa | 1d4 + DEX | DEFINIDO |
| Desempate | Maior DEX; depois jogador antes de inimigo; depois ID estável | DEFINIDO |
| Turnos | Individuais; sem agrupar aliados | DEFINIDO |
| Ação principal | Uma por turno | DEFINIDO |
| Ação bônus | Uma por turno, quando permitida | DEFINIDO |
| Movimento | Conforme velocidade; comprimento de caminho | DEFINIDO |
| Reação | Uma; restaurada no início do próprio turno | DEFINIDO |
| Ataque | d20 + mod + proficiência vs CA | DEFINIDO |
| Crítico | 20 natural; dobra todos os dados de dano do ataque, não modificadores | DEFINIDO |
| Falha automática | 1 natural em ataque/teste de resistência | DEFINIDO |
| Vantagem/desvantagem | 2d20, maior/menor; não acumula; opostos se anulam | DEFINIDO |
| Teste de resistência | d20 + mod + proficiência vs CD | DEFINIDO |
| CD de magia | 8 + proficiência + mod de conjuração | DEFINIDO |
| Concentração | Uma por personagem; DC 10 ou metade do dano, o maior | DEFINIDO |
| Ataque de oportunidade | Ao sair do alcance corpo a corpo; usa reação | PROVISÓRIO |
| Empurrão | Ação bônus; teste contestado; empurra 3 m; colisão simples | PROVISÓRIO |
| Cobertura | Meia +2 CA/DEX; três quartos +5; total bloqueia | PROVISÓRIO |
| Dano de queda | Fora da v1.0 (sem elevação) | DEFINIDO |
| Tipos de dano | Físico (corte/perfuração/impacto), fogo, frio, elétrico, veneno, ácido | DEFINIDO |
| Morte | 0 PV = caído; testes contra morte CD 10; 3 sucessos estabilizam; 3 falhas morrem | DEFINIDO |
| 1/20 em morte | 1 natural = 2 falhas; 20 natural = 1 PV | DEFINIDO |
| Revive | Nos rests; custo a definir; volta com 1 PV e condição temporária | PROVISÓRIO |
| Help | Aliado adjacente estabiliza com Medicina CD 10 ou kit | PROVISÓRIO |
| Fuga | Retirada custosa; sem recompensa; perde ouro; node evitado | PROVISÓRIO |

## 6.3 Magia e condições

| Regra | Decisão | Status |
|---|---|---|
| Espaços de magia | Progressão por classe/nível a definir | PENDENTE |
| Cantrips | Ilimitados | DEFINIDO |
| Componentes | Materiais comuns simplificados | DEFINIDO |
| Condições da v1.0 | Caído, agarrado, atordoado, amedrontado, envenenado, cego | DEFINIDO |
| Invisibilidade/furtividade | Furtividade simples para Rogue; invisibilidade plena fora da v1.0 | PROVISÓRIO |
| Surpresa | Fora da v1.0 | FUTURO |
| Petrified/paralisado/surdo | Fora da v1.0 | FUTURO |
| Superfícies | Fogo apenas na v1.0; água/eletricidade futuras | DEFINIDO |

## 6.4 Progressão, descanso e economia

| Regra | Decisão | Status |
|---|---|---|
| XP | Distribuído igualmente entre os 3 ativos | DEFINIDO |
| Curva de XP | Ajustada para nível 3–4 no fim do ato 1, 6 no ato 2, 8 no ato 3 | PROVISÓRIO |
| Progressão de classe | Tabela por nível a definir para as 3 classes | PENDENTE |
| Descanso curto | 2 cargas por run; recuperação parcial | PROVISÓRIO |
| Descanso longo | 1 carga; restaura as curtas; revive | PROVISÓRIO |
| Ouro | Temporário; perdido no fim da run | DEFINIDO |
| Loot | Tabela por inimigo; sem tabela global; piso de loot para evitar run impossível | PROVISÓRIO |
| Inventário | Sem limite de capacidade na v1.0; 6 slots equipados | DEFINIDO |
| Mercadores | Estoque e preços próprios; desconto por CHA | PROVISÓRIO |
| Sets | Fora da v1.0 | DEFINIDO |

## 6.5 Dificuldade e acessibilidade

| Regra | Decisão | Status |
|---|---|---|
| Dificuldades | Aventureiro, Veterano, Lendário | DEFINIDO |
| Intenção inimiga | Toggle separado de acessibilidade; não preso à dificuldade | DEFINIDO |
| Escalonamento | Nodes com dificuldade fixa; sem auto-scale | DEFINIDO |
| Bandas de nível | Nível esperado por ato para orientar encontros | PROVISÓRIO |
| Acessibilidade | Escala de UI, legendas, redução de flashes, velocidade de animação, indicadores visuais | DEFINIDO |
| Colorblind | Paleta e ícones redundantes | PROVISÓRIO |

## 6.6 Rolagens e ordem de operações

| Situação | Decisão | Status |
|---|---|---|
| Teste de perícia em evento | d20 + atributo + proficiência quando aplicável | DEFINIDO |
| 20/1 em perícia | 20 natural = sucesso automático; 1 natural = falha automática | PROVISÓRIO |
| Reliable Talent | Substitui 9 ou menos por 10; interage com 1 natural conforme ordem definida | PENDENTE |
| Halfling Lucky | Rerrola 1; consome o novo resultado | PROVISÓRIO |
| Ordem de rerolls | Rolagem base → rerolls/substituições → total → auto sucesso/falha | PROVISÓRIO |
| Vantagem com reroll | Definir se o reroll usa o mesmo dado ou novo par | PENDENTE |

---

# 7. ESPECIFICAÇÃO ESPACIAL E DE COMBATE

## 7.1 Coordenadas e escala

- O mundo lógico é um plano 2D top-down `(x, y)` em metros.
- **1 unidade = 1 metro.**
- **5 ft = 1,5 m.**
- Deslocamento padrão de 30 ft = **9 m**.
- A projeção visual é isométrica de ângulo alto; a posição lógica é separada da posição visual.
- Não há grid lógico obrigatório.
- Não há elevação na v1.0; todas as unidades estão no mesmo plano.

## 7.2 Arenas

- Tamanho inicial: 20 × 20 m a 30 × 30 m.
- Câmera fixa com zoom por pinça e arraste.
- Limites de câmera por arena.
- Obstáculos estáticos com colisão.
- Unidades ocupam espaço e bloqueiam passagem.
- NavigationRegion2D com agente de raio ~0,4 m.
- Área jogável plana; sem rampas, escadas ou plataformas na v1.0.

## 7.3 Movimento

- O jogador toca o destino; o sistema mostra a rota e o custo.
- O movimento é confirmado antes de executar.
- O custo é o comprimento do caminho, não a distância em linha reta.
- O movimento para quando o orçamento acaba.
- O movimento pode ser interrompido por reação (ataque de oportunidade).
- Unidades não atravessam outras unidades.
- O caminho é recalculado se um obstáculo dinâmico aparecer.
- Não há movimento diagonal especial; o espaço é contínuo.

## 7.4 Alcance, linha de visão e cobertura

- Alcance é medido em metros, de centro a centro, ajustado pelo raio das unidades quando necessário.
- Corpo a corpo: 1,5 m.
- Linha de visão: raycast do ponto de visão da unidade até o alvo; obstáculos bloqueiam.
- Cobertura:
  - Meia: +2 CA e +2 em testes de resistência de DEX.
  - Três quartos: +5 CA e +5 em testes de resistência de DEX.
  - Total: bloqueia ataques e efeitos diretos.
- A prévia de cobertura deve usar a mesma lógica da execução.
- A cobertura é determinada por raycast; casos ambíguos são resolvidos a favor do alvo, por enquanto.
- A altura não influencia cobertura na v1.0.

## 7.5 Áreas de efeito

- Círculo: raio em metros.
- Cone: ângulo e comprimento em metros.
- Linha: largura e comprimento em metros.
- Cubo/esfera 3D: não se aplica na v1.0.
- Friendly fire: sim, salvo indicação contrária da habilidade.
- A prévia visual usa a mesma geometria da execução.
- Objetos e unidades afetados são destacados antes da confirmação.

## 7.6 Ataques de oportunidade

- Disparam quando uma unidade sai voluntariamente do alcance corpo a corpo de outra.
- O gatilho é o primeiro ponto do caminho que cruza a borda do alcance.
- A reação é resolvida antes de o movimento continuar.
- Cada unidade tem uma reação por rodada.
- Movimento forçado (empurrão) não dispara ataque de oportunidade.
- A IA usa a mesma regra.
- A UI deve avisar antes de confirmar um caminho que provoca reação.

## 7.7 Empurrão

- Ação bônus.
- Teste contestado: Atletismo vs Atletismo/Acrobacia do alvo.
- Empurra 3 m na direção escolhida.
- Se o alvo colidir com obstáculo, para no espaço livre mais próximo.
- Sem dano de colisão na v1.0.
- Pode empurrar para dentro de fogo/poça, se houver.
- Não dispara ataque de oportunidade.
- Detalhes de colisão e distância são PROVISÓRIOS.

## 7.8 Turno e reações

- A reação é restaurada no início do turno da unidade.
- Reações fora do turno pausam a resolução e pedem confirmação do jogador, quando aplicável.
- A IA decide reações com a mesma lista de ações legais.
- Reações têm prioridade definida: evitar dano > punir movimento > efeito ofensivo.
- O jogador pode configurar “sempre perguntar” ou “auto-passar” para reações simples.

---

# 8. SAVE E ESTADO

## 8.1 Requisito

O jogador deve poder fechar o jogo e retomar a run no início do turno em que estava, sem perder progresso anterior. Perder as ações do turno atual é aceitável.

## 8.2 Estado de runtime

| Estado | Conteúdo |
|---|---|
| MetaState | Desbloqueios, conquistas, configurações, slots do elenco |
| RunState | Região, mapa, node atual, nodes concluídos, ouro, inventário, mercadores |
| PartyState | Personagens ativos, nível, XP, PV, recursos, condições, equipamentos |
| CombatState | Unidades, iniciativa, turno atual, posições, recursos de ação, condições, objetos, cooldowns |
| RngState | Estado do gerador aleatório |
| SaveData | Versão, metadados, estados acima |

## 8.3 Quando salvar

- Ao iniciar uma run.
- Ao concluir cada node.
- Ao iniciar cada combate.
- **No início de cada turno.**
- Após decisões relevantes de mapa/evento/loja.
- Opcionalmente, ao app ir para segundo plano, se o estado estiver estável.

Não salvar no meio de uma animação ou resolução de ação. O save deve ocorrer em um limite de comando.

## 8.4 Como salvar

- Formato JSON versionado (debuggável) ou binário, se o tamanho justificar; começar com JSON.
- Gravação atômica: escrever em arquivo temporário, validar e renomear.
- Manter um backup da versão anterior.
- Checksum/integridade simples.
- Versão de schema e migração.
- Tamanho alvo: <1 MB por save.
- Uma run ativa por slot; múltiplos slots de elenco podem existir depois.

## 8.5 O que restaurar

- Mapa gerado, rotas, nodes concluídos, node atual.
- Equipe, nível, XP, PV, recursos, condições, habilidades, equipamentos.
- Ouro, consumíveis, inventário, estoque de mercadores.
- Se em combate: fila de iniciativa, unidade ativa, posições, PV, condições, recursos de ação, cooldowns, objetos interativos, estado de IA, RNG.
- Não restaurar animações em andamento; reconstruir a cena a partir do estado.

## 8.6 Testes de save

- Salvar/carregar no início do turno.
- Salvar/carregar após cada tipo de node.
- Corromper o arquivo e validar recuperação pelo backup.
- Migrar um save antigo para o schema novo.
- Verificar que itens temporários não contaminam o elenco permanente.

---

# 9. CONTEÚDO, PROGRESSÃO E BALANCEAMENTO

## 9.1 Progressão por run

Proposta inicial:

- Início no nível 1.
- Fim do ato 1: nível 3–4.
- Fim do ato 2: nível 6.
- Fim do ato 3: nível 8.
- Cap 8 por run.
- Nível 12 fica para o jogo completo/campanha futura.

Motivo: 12 nodes com 9 combates não comportam 12 níveis sem level-up por combate. Um cap 8 mantém progressão perceptível, permite subclasse no 3 e dois ASIs (4 e 8), e evita inflação.

Status: **PROVISÓRIO**. Validar no M2.

## 9.2 Orçamento de encontros

- Usar orçamento de “pontos de ameaça”, não CR de D&D.
- Cada inimigo tem um custo; cada papel tático tem um peso.
- Combate normal: 3–4 inimigos de custo padrão.
- Elite: 4–5 inimigos ou 1 elite + 2–3 apoiadores.
- Chefe: 1 chefe + 2–3 adds, com fases simples.
- A dificuldade do node é fixa; a banda de nível esperada orienta o orçamento.
- Evitar multiplicadores globais de PV/dano; ajustar comportamento e composição.

## 9.3 Loot e economia

- Cada inimigo tem tabela própria.
- Normal: chance de item; Elite: 1–2 itens; Chefe: recompensa garantida.
- Peso de classe no loot: ~50% para itens úteis à equipe; o resto é variado.
- Piso de loot: mercador sempre oferece uma melhoria básica; a run não pode ficar impossível.
- Ouro é temporário; preços e recompensas a definir.
- Consumíveis: poção (ação bônus), pergaminho (ação), bomba (ação), veneno, elixir.
- Inventário sem limite na v1.0; 6 slots equipados.

## 9.4 Descanso e morte

- 2 descansos curtos por run.
- 1 descanso longo por run; restaura as cargas curtas.
- Descanso curto: recuperação parcial; pode reviver 1 morto com custo.
- Descanso longo: recuperação forte; revive todos os mortos, mas consome o benefício do node.
- Revive devolve 1 PV e uma condição temporária.
- Party wipe encerra a run.
- Help em combate estabiliza caídos.
- IA finaliza caídos se não houver ação melhor.

## 9.5 Duração e ritmo

| Tipo | Alvo inicial | Observação |
|---|---|---|
| Combate normal | 4–6 min | 3–5 rodadas |
| Elite | 6–9 min | 4–6 rodadas |
| Chefe | 8–12 min | 5–8 rodadas |
| Run completa | 45–75 min | 12 nodes, 9 combates |
| Sessão mobile | 10–20 min | Save no início do turno |

Se a run passar de 75 min com frequência, reduzir nodes/combates ou encurtar animações. Se os combates forem longos mas densos, manter; se forem repetitivos, cortar.

## 9.6 Métricas de playtest

- Tempo por combate e por turno.
- Número de turnos por combate.
- Tempo de IA por turno.
- Porcentagem de turnos com mais de uma ação relevante.
- Ações repetidas (“atacar e passar”).
- Taxa de acerto e dano médio.
- Mortes, revives e retiradas.
- Frequência de save/load.
- Pontos de confusão de UI.
- Frame time no Redmi Note 13.

---

# 10. ROADMAP E MARCOS

Sem datas. Cada marco tem critério de conclusão e um gate de diversão/qualidade.

## M0 — Fundação

- Instalar Godot 4.7.2, Git e ferramentas Android.
- Criar projeto, estrutura de pastas, `AGENTS.md`, documentação inicial.
- Cena principal vazia.
- Build Android instalado e rodando no Redmi Note 13.
- Teste automatizado trivial rodando headless.

**Critério:** o jogo abre no celular sem erro e o pipeline de build está documentado.

## M1 — Sandbox de combate

- 1 arena isométrica placeholder.
- 1 personagem vs 1 inimigo; depois 3 vs 3.
- Seleção, movimento, pathfinding, obstáculos.
- Iniciativa, turnos, ação/bônus/movimento/re ação.
- Ataque vs CA, dano, PV, crítico, vantagem/desvantagem.
- Uma habilidade, uma condição, uma passiva permanente, uma condicional e uma reativa.
- HUD funcional de debug e toque.
- IA básica por prioridade.

**Critério:** é possível jogar um combate completo, entender o que acontece e identificar se o movimento livre é legível.

## M2 — Fatia vertical

- 3 personagens, 3 classes, 4 raças.
- 1 região, 6–8 nodes, 1 elite, 1 chefe.
- Loot, inventário, mercador, descanso, revive, retirada.
- Mapa ramificado com seed.
- Save no início do turno e retomada.
- UI mobile inicial.
- Primeiro passe de arte placeholder/AI.

**Critério:** uma run curta completa, do início ao chefe, com save/retomada e sensação de progressão.

## M3 — v1.0

- 12 nodes, 3 atos, 4 raças, 3 classes, 3 subclasses.
- 8–10 inimigos, 24–30 itens, 5–6 condições.
- Tutorial curto com skip.
- Meta progressão horizontal.
- Balanceamento, acessibilidade, áudio, polimento.
- Build Android estável.

**Critério:** loop completo jogável, estável e divertido por várias runs.

## M4 — Expansão

- Mais regiões, classes, itens, eventos e chefes.
- Possível demo + versão completa.
- Localização adicional.
- PC/Steam, se fizer sentido.

**Critério:** conteúdo novo entra sem reescrever o núcleo.

### Gate de diversão

Antes de avançar de M1 para M2 e de M2 para M3:

- O combate é legível no celular?
- Cada turno tem escolhas relevantes?
- O movimento livre funciona sem grid?
- A IA é rápida e compreensível?
- O save é confiável?
- O jogador quer jogar outra run?

Se a resposta for “não” em algum ponto, corrigir antes de adicionar conteúdo.

---

# 11. RISCOS E MITIGAÇÕES

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| Escopo crescer demais | Alta | Muito alto | Lista de corte, marcos, gate de diversão |
| Arte com IA inconsistente | Alta | Alto | Style guide, peças modulares, limpeza manual, teste visual antes de conteúdo |
| Save no meio do combate | Média | Alto | Estado serializável desde o início; save no início do turno; testes |
| Navegação em arenas densas | Média | Alto | Protótipo de navegação na M1; NavigationServer2D; agent radius |
| UI mobile confusa | Alta | Alto | HUD cedo; teste no Redmi; safe area; alvos grandes |
| Performance no celular | Média | Médio | 2D, atlas, compressão, perfil na M1 |
| Burnout por projeto longo | Alta | Alto | Marcos curtos, pausas, celebrar entregas, não comparar com jogos AAA |
| Direitos de arte/IA | Média | Alto | Verificar licenças, guardar registros, evitar marcas de terceiros |
| Balanceamento imprevisível | Alta | Médio | Métricas, playtest, bandas de nível, piso de loot |
| Modelo de dados mudar | Média | Médio | IDs estáveis, schema versionado, testes de migração |
| Dependência de uma única pessoa | Alta | Alto | Documentação, decisões registradas, código legível |

---

# 12. PERGUNTAS ABERTAS

1. Cap 8 por run ou 12? (Proposta: 8.)
2. Estilo de arte: pixel art estilizada ou pintado/HD-2D?
3. Custo exato do revive: ouro, carga ou ambos?
4. Fórmula da retirada: teste de grupo, chance fixa ou custo direto?
5. Duração-alvo final da run: 45–75 min é aceitável?
6. Quantas classes/regiões o jogo completo deve ter no lançamento?
7. Haverá furtividade simples para o Rogue na v1.0?
8. Feats e sets entram em uma expansão? Quando?
9. Qual o limite de slots de save?
10. Onde exatamente termina a demo?
11. Qual o plano de áudio (trilha, SFX, licenças)?
12. Quais idiomas além de en-US?
13. Haverá suporte a tablet/gamepad?
14. Qual o modelo de monetização final: premium, demo+full ou outro?
15. Qual o critério pessoal de “divertido” que vamos usar nos playtests?

---

# 13. CHANGELOG

## v0.2 — 06/11/2026

- Registra o projeto como jogo completo de longo prazo, solo, sem orçamento e sem prazo.
- Define Godot 4.7.2 stable e GDScript.
- Define 2D isométrico de câmera fixa, ângulo alto; 2.5D/tilt fora da v1.0.
- Define pipeline inicial de arte com IA.
- Reduz escopo da v1.0: 6 slots, 5–6 condições, sem feats/sets/multiclass.
- Define save no início do turno.
- Define retirada custosa, revive nos rests e ação Help.
- Define intenção inimiga como acessibilidade separada.
- Define bandas de duração de combate e run.
- Propõe cap 8 por run (provisório).
- Registra demo + versão completa como possibilidade futura.
- Define marcos M0–M4 e gates de diversão.

---

# 14. PRÓXIMOS PASSOS

1. Aprovar este documento e resolver as Perguntas Abertas prioritárias (cap, estilo de arte, retirada, revive).
2. Executar M0: instalar Godot 4.7.2, criar projeto, Git e build Android no Redmi Note 13.
3. Criar o style guide visual e um teste de arte pequeno.
4. Refinar a matriz de regras com os casos de borda (Reliable Talent, Lucky, crítico, cobertura).
5. Iniciar M1: arena, movimento e combate 1v1.
6. Só expandir conteúdo depois que o gate de diversão do M1 for aprovado.

---

*Este documento é vivo. Toda decisão nova deve ser registrada aqui com status, data e impacto nos documentos v1.0. Quando uma decisão for validada, incorporá-la ao GDD/Arquitetura e marcar como incorporada.*
