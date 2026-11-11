TACTICAL FANTASY RPG

ARQUITETURA TÉCNICA E BACKLOG DO MVP

Documento técnico alinhado ao GDD Consolidado v1.0

Godot 4 \| Versão 1.0 \| 06 de novembro de 2026 Plano de implementação
incremental: protótipo jogável → MVP

## CONTROLE DO DOCUMENTO

  -----------------------------------------------------------------------
  Campo                               Definição
  ----------------------------------- -----------------------------------
  Projeto                             Tactical Fantasy RPG

  Documento                           Arquitetura Técnica e Backlog do
                                      MVP

  Versão                              1.0 --- 06/11/2026

  Documento de referência             GDD Consolidado v1.0

  Engine                              Godot 4

  Plataforma prioritária              Android/mobile; PC/Steam como
                                      possibilidade futura

  Objetivo                            Orientar a construção do protótipo
                                      jogável e a evolução incremental
                                      até o MVP
  -----------------------------------------------------------------------

Convenção de status: OBRIGATÓRIO = necessário para o marco descrito;
PROVISÓRIO = decisão técnica a validar; FUTURO = previsto na
arquitetura, mas fora do escopo atual.

## 1. OBJETIVO E DIRETRIZES

Este documento transforma as decisões de design do GDD em uma proposta
de arquitetura implementável e um backlog incremental. O primeiro
objetivo não é construir o jogo completo: é validar o núcleo de combate
em um protótipo pequeno, jogável e testável.

### 1.1 Princípios arquiteturais

-   Separar regras de jogo, apresentação visual, interface e
    persistência.

-   Orientar conteúdo configurável a dados, evitando regras específicas
    espalhadas pelo código.

-   Manter sistemas coesos, com contratos explícitos e dependências
    controladas.

-   Permitir testes de regras sem depender da arena ou da interface.

-   Separar dados permanentes do elenco e estado temporário de cada run.

-   Construir apenas o necessário para o marco atual; preparar extensões
    sem implementar antecipadamente sistemas futuros.

-   Priorizar desempenho, legibilidade e interação por toque desde o
    início.

### 1.2 Regra de escopo

A arquitetura deve permitir adicionar classes, raças, habilidades,
equipamentos, inimigos, eventos e regiões por dados e módulos. Isso não
significa criar desde já centenas de recursos, sistemas online, geração
procedural completa ou funcionalidades fora do MVP.

## 2. VISÃO GERAL DA ARQUITETURA

A aplicação deve ser organizada em camadas. A interface solicita ações;
os sistemas de domínio validam e resolvem regras; os dados descrevem
conteúdo; a camada de apresentação representa o estado; e a persistência
salva/restaura o estado necessário.

  ------------------------------------------------------------------------
  Camada                   Responsabilidade        Exemplos
  ------------------------ ----------------------- -----------------------
  Apresentação             Cenas, sprites,         CombatScene, UnitView,
                           animações, câmera, HUD  InitiativeHUD
                           e feedback visual       

  Aplicação/Orquestração   Fluxos e coordenação    CombatManager,
                           entre sistemas          RunManager

  Domínio                  Regras independentes da DiceService,
                           interface               CombatResolver,
                                                   ActionEconomy

  Conteúdo                 Definições              CharacterData,
                           configuráveis de        AbilityData, EnemyData
                           entidades e efeitos     

  Persistência             Salvar, carregar,       SaveSystem, RunSaveData
                           versionar e restaurar   
                           estado                  

  Testes                   Validar regras,         Testes de ataque,
                           integrações e           turno, movimento e save
                           regressões              
  ------------------------------------------------------------------------

### 2.1 Fluxo geral de uma ação

1.  A interface recebe a intenção do jogador (ex.: atacar um alvo).

2.  TargetingSystem valida alvo, alcance, linha de visão e seleção.

3.  ActionEconomy verifica se a ação pode ser executada e qual recurso
    será consumido.

4.  CombatResolver executa a resolução usando DiceService e dados das
    unidades.

5.  O sistema publica eventos de combate relevantes.

6.  PassiveAbilitySystem e StatusEffectSystem processam efeitos
    disparados.

7.  O estado de domínio é atualizado.

8.  A interface recebe o novo estado e apresenta animações, números e
    log.

A interface não deve calcular dano, decidir acerto ou alterar
diretamente recursos de combate. Ela solicita comandos e apresenta
resultados.

## 3. ESTRUTURA DE DIRETÓRIOS

Estrutura inicial sugerida. Pastas podem ser subdivididas quando o
volume justificar; não criar arquivos vazios ou abstrações sem uso
concreto.

res:// ├── project.godot ├── AGENTS.md ├── assets/ │ ├── characters/ │
├── enemies/ │ ├── environments/ │ ├── effects/ │ ├── ui/ │ └── audio/
├── scenes/ │ ├── main/ │ ├── combat/ │ ├── characters/ │ ├── enemies/ │
├── environment/ │ └── ui/ ├── scripts/ │ ├── core/ │ ├── combat/ │ ├──
turn_system/ │ ├── action_economy/ │ ├── dice/ │ ├── movement/ │ ├──
targeting/ │ ├── abilities/ │ ├── passive_abilities/ │ ├──
status_effects/ │ ├── environment/ │ ├── characters/ │ ├── enemies/ │
├── inventory/ │ ├── progression/ │ ├── runs/ │ ├── save/ │ └── ui/ ├──
data/ │ ├── classes/ │ ├── races/ │ ├── backgrounds/ │ ├── abilities/ │
├── passive_abilities/ │ │ ├── racial/ │ │ ├── class/ │ │ ├── subclass/
│ │ ├── feats/ │ │ └── equipment/ │ ├── characters/ │ ├── enemies/ │ ├──
items/ │ ├── conditions/ │ ├── encounters/ │ └── regions/ ├── tests/ │
├── combat/ │ ├── turn_system/ │ ├── movement/ │ ├── abilities/ │ ├──
passive_abilities/ │ ├── progression/ │ └── save/ └── docs/ ├──
architecture.md ├── combat_rules.md ├── development_log.md └──
decisions.md

## 4. SISTEMAS E RESPONSABILIDADES

  ------------------------------------------------------------------------
  Sistema                  Responsabilidade        Não deve fazer
                           principal               
  ------------------------ ----------------------- -----------------------
  CombatManager            Coordenar estado e      Calcular regras
                           ciclo do combate        isoladamente ou
                                                   desenhar HUD

  TurnManager              Iniciativa, fila        Resolver dano ou
                           individual, início/fim  escolher ações da IA
                           de turno                

  ActionEconomy            Validar, consumir e     Definir efeitos de
                           restaurar ação, bônus,  habilidades
                           movimento e reação      

  DiceService              Rolagens e operações de Decidir regras de alvo
                           dados                   ou efeitos visuais

  CombatResolver           Resolver ataques,       Conter lógica exclusiva
                           testes, dano e          de cada classe/inimigo
                           resultados              

  CharacterStats           Atributos, PV, CA,      Controlar cenas ou
                           proficiência e recursos interface

  AbilitySystem            Validar e executar      Armazenar lógica visual
                           habilidades ativas      de animações

  PassiveAbilitySystem     Processar passivas por  Conter casos especiais
                           eventos e condições     espalhados pelo combate

  StatusEffectSystem       Aplicar, atualizar,     Controlar a
                           resistir e remover      apresentação de ícones
                           condições               

  MovementSystem           Movimento, navegação e  Controlar seleção de UI
                           validação de            
                           deslocamento            

  TargetingSystem          Alvos, alcance, área e  Aplicar dano
                           regras de seleção       

  LineOfSightSystem        Linha de visão e        Escolher comportamento
                           cobertura               de IA

  EnvironmentInteraction   Objetos, superfícies,   Implementar regras
                           destruição e reações    dentro de scripts de UI

  EnemyAIController        Escolher ações com base Usar IA generativa ou
                           em perfil e contexto    depender de texto livre

  RunManager               Mapa, nodes, estado e   Resolver regras de
                           encerramento da         combate
                           expedição               

  InventorySystem          Itens, slots,           Definir recompensas
                           transferência e         permanentes
                           consumíveis             

  ProgressionSystem        XP e níveis temporários Persistir diretamente
                           da run                  arquivos

  MetaProgression          Desbloqueios            Conceder bônus
                           permanentes e           numéricos de poder no
                           milestones              MVP

  SaveSystem               Serializar, versionar e Ser fonte de regras de
                           restaurar estados       gameplay
  ------------------------------------------------------------------------

## 5. MODELO DE DADOS

### 5.1 Resources de conteúdo

  -----------------------------------------------------------------------
  Resource                            Campos conceituais mínimos
  ----------------------------------- -----------------------------------
  CharacterData                       ID estável, nome-base, raça,
                                      classe, atributos-base,
                                      proficiências, referências a
                                      passivas

  ClassData                           ID, progressão, proficiências,
                                      habilidades por nível, subclasse
                                      disponível

  RaceData                            ID, atributos
                                      flexíveis/configuração,
                                      características e passivas raciais

  BackgroundData                      ID, perícias e proficiências
                                      concedidas

  AbilityData                         ID, custo, tipo de ação, alcance,
                                      alvos, área, requisitos, rolagem,
                                      efeito e tags

  PassiveAbilityData                  ID, gatilhos, condições,
                                      prioridade, limites, efeitos e
                                      regras de acumulação

  EnemyData                           ID, atributos, CA, PV,
                                      deslocamento, função tática, perfil
                                      comportamental, habilidades e loot
                                      table

  ItemData                            ID, slot, raridade, propriedades,
                                      atributos, passivas, habilidades e
                                      valor

  ConditionData                       ID, efeitos, duração, regras de
                                      acumulação, resistência e remoção

  EncounterData                       ID, arena, composição, posições
                                      iniciais, dificuldade e
                                      configuração ambiental

  RegionData                          ID, identidade, pool de encontros,
                                      nodes, eventos, mercadores e
                                      recompensas
  -----------------------------------------------------------------------

### 5.2 Dados permanentes e temporários

  -----------------------------------------------------------------------
  Persistência permanente             Estado temporário da run
  ----------------------------------- -----------------------------------
  Slots do elenco e personagens       Equipe selecionada e estado atual
  criados                             dos três personagens

  Nome, raça, classe, aparência e     Nível, XP, PV, recursos, condições
  configurações-base                  e habilidades adquiridas

  Desbloqueios de classes,            Equipamentos encontrados e ouro
  subclasses, itens, eventos e        
  cosméticos                          

  Conquistas e milestones             Mapa, node atual, eventos
                                      resolvidos e estado de combate

  Configurações do jogador            Inimigos, posições, iniciativa,
                                      objetos e estado do encontro
  -----------------------------------------------------------------------

IDs estáveis devem ser usados para referenciar conteúdo. Evitar salvar
nomes de exibição como identificadores. O save deve registrar versão de
esquema para permitir migração futura.

## 6. COMBATE: CONTRATOS TÉCNICOS

### 6.1 Ordem de execução do turno

1.  TurnManager identifica a unidade ativa.

2.  Disparar evento on_turn_start.

3.  Atualizar recursos que recuperam no início do turno, conforme regra
    definida.

4.  Permitir ações válidas da unidade ativa ou execução do controlador
    de IA.

5.  Ao encerrar o turno, disparar on_turn_end.

6.  Avançar para a próxima unidade viva/participante conforme regras.

7.  Atualizar indicadores de interface.

### 6.2 Economia de ações

-   Ação principal, ação bônus, movimento e reação devem ser recursos
    separados.

-   A validação deve ocorrer antes de executar efeitos.

-   A ação não deve ser consumida se a solicitação for inválida, salvo
    regra explicitamente definida.

-   Reação: uma por rodada, restaurada no próximo turno do respectivo
    personagem.

-   Ações e habilidades devem declarar seu custo em AbilityData ou
    estrutura equivalente.

### 6.3 Dados e resolução

-   DiceService deve permitir rolagens reproduzíveis em testes, com
    possibilidade de injetar sequência/seed controlada no ambiente de
    teste.

-   CombatResolver calcula ataque contra CA, testes de resistência,
    crítico, dano, resistências, vulnerabilidades e imunidades.

-   Vantagem/desvantagem deve ser resolvida em uma única etapa;
    múltiplas fontes não acumulam e estados opostos se anulam.

-   Separar resultado matemático da apresentação textual/visual do log.

-   Evitar que scripts de unidade implementem versões próprias de
    cálculo de dano.

### 6.4 Eventos padronizados

Eventos iniciais para integração entre sistemas:

-   on_turn_start / on_turn_end

-   on_attack_start / on_attack_hit / on_attack_miss

-   on_damage_received / on_damage_dealt / on_critical_hit

-   on_kill / on_movement

-   on_condition_applied / on_condition_removed

-   on_ally_down / on_reaction_triggered

-   on_death_save_success / on_death_save_failure

Eventos devem transportar contexto estruturado: unidade de origem, alvo,
habilidade, valores antes/depois, tags e referência ao combate. Não
depender de mensagens de texto para acionar lógica.

## 7. PASSIVEABILITYSYSTEM

O sistema de passivas é transversal e deve existir como mecanismo
reutilizável. Ele processa efeitos raciais, de classe, subclasse,
talentos, equipamentos, condições e efeitos temporários.

### 7.1 Tipos suportados

-   Permanentes: sempre ativas enquanto a fonte estiver
    equipada/desbloqueada.

-   Condicionais: ativadas quando condições verificáveis forem
    atendidas.

-   Reativas: acionadas por eventos do combate.

-   Acumuláveis: acumulam cargas ou intensidade conforme regra
    declarada.

-   Exclusivas: não coexistem com passivas incompatíveis.

-   Progressivas: alteram comportamento por nível, carga ou marco.

### 7.2 Estrutura conceitual

  Campo                      Finalidade
  -------------------------- --------------------------------------------
  id                         Identificador estável da passiva
  trigger_event              Evento que pode disparar a passiva
  conditions                 Condições para ativação
  priority                   Ordem de processamento em conflitos
  once_per_turn / cooldown   Limites de ativação
  stacking_rule              Acumulação, substituição ou exclusividade
  effects                    Modificadores ou comandos aplicados
  source                     Origem: raça, classe, subclasse, item etc.

O formato exato deve ser implementado somente quando os primeiros
exemplos reais forem conhecidos. Não construir um motor genérico
excessivamente abstrato antes de testar três casos representativos:
passiva permanente, condicional e reativa.

## 8. MOVIMENTO, ALVOS E ARENA

### 8.1 Movimento livre

-   Usar navegação 2D do Godot (NavigationRegion2D/NavigationAgent2D ou
    solução equivalente validada).

-   Separar posição lógica da apresentação isométrica.

-   Calcular deslocamento permitido pelo valor de movimento disponível.

-   Exibir rota e destino antes da confirmação pelo jogador.

-   Validar colisões e obstáculos durante a execução; impedir atravessar
    unidades/objetos conforme regras adotadas.

-   Movimento deve ser cancelável ou interrompível conforme regras de
    combate a definir.

### 8.2 Targeting e linha de visão

-   TargetingSystem valida alcance, tipo de alvo, área, aliados/inimigos
    e requisitos.

-   LineOfSightSystem resolve bloqueios e cobertura.

-   Prévia de área deve usar a mesma lógica de validação da execução
    real.

-   A escala de distância e o tratamento de diagonais precisam ser
    definidos antes de balancear habilidades.

### 8.3 Câmera e arte

-   Câmera isométrica 2D elevada, sem rotação horizontal no MVP.

-   Zoom por pinça e deslocamento por arraste.

-   Acompanhamento suave do personagem selecionado, com limites da
    arena.

-   Tilt vertical é requisito visual a validar em protótipo; não
    comprometer a legibilidade da arte 2D.

-   Separar camadas de cenário, obstáculos, objetos interativos,
    unidades e efeitos.

-   Não exigir sprites de múltiplos ângulos no primeiro protótipo.

### 8.4 Interação ambiental

-   Classificar objetos como decorativos, obstáculos ou interativos.

-   No protótipo, usar obstáculos estáticos; interação e destruição
    entram em fase posterior.

-   No MVP, limitar destruição a objetos selecionados e atualizar
    colisão/pathfinding após alteração.

-   Fogo, água e explosões são os primeiros elementos; criar contratos
    de interação para expansão futura.

## 9. IA DE INIMIGOS

A IA do MVP será baseada em regras, prioridades e avaliação contextual.
Deve ser determinística e testável, sem modelo generativo.

### 9.1 Camadas de decisão

1.  Função tática: tanque, combatente, atirador, assassino, controlador,
    suporte ou invocador.

2.  Perfil comportamental: agressivo, defensivo, covarde, tático ou
    protetor.

3.  Avaliação contextual: posição, PV, ameaças, distância, aliados,
    cobertura, recursos e ações disponíveis.

### 9.2 Contrato mínimo do controlador

-   Receber snapshot do estado relevante do combate.

-   Gerar lista de ações legais.

-   Pontuar ações por prioridade e contexto.

-   Selecionar ação válida de maior prioridade.

-   Se a ação preferida não for viável, escolher alternativa válida.

-   Registrar decisão suficiente para depuração e testes.

Memória de combate, reconhecimento de padrões e adaptação avançada ficam
fora do MVP.

## 10. EXPEDIÇÃO, PROGRESSÃO E LOOT

### 10.1 RunManager

-   Controlar região, mapa ramificado, nodes disponíveis, node atual e
    transições.

-   Representar nodes por tipo e configuração, sem acoplar a lógica do
    mapa às cenas visuais.

-   Permitir estrutura parcialmente procedural e controlada.

-   MVP inicial: 12 nodes percorridos em três atos: 6 combates normais,
    2 elites, 1 chefe e 3 nodes utilitários.

-   Dificuldade do node é fixa; não escalar automaticamente ao nível da
    equipe.

### 10.2 ProgressionSystem

-   Distribuir XP igualmente entre os três personagens ativos.

-   Personagens fora da equipe não recebem XP.

-   Controlar nível, XP, proficiência e desbloqueios temporários da run.

-   Cap por run e tabela de XP permanecem pendentes; não codificar nível
    12 como valor imutável.

### 10.3 Loot e economia

-   Cada inimigo referencia sua própria tabela de loot.

-   Loot de combate normal, elite e chefe deve ser configurável por
    dados.

-   Mercadores possuem estoque e preços próprios, parcialmente
    aleatórios e coerentes com a região.

-   Ouro é temporário e desaparece ao encerrar a run.

-   Inventário sem limite de capacidade no MVP; transferência entre
    personagens fora de combate.

-   Itens temporários não devem contaminar o save permanente do elenco.

### 10.4 MetaProgression

-   Persistir desbloqueios horizontais: personagens, subclasses, itens
    na pool, inimigos, eventos, regiões, cosméticos e conquistas.

-   Desbloquear conteúdo significa torná-lo elegível, não conceder
    automaticamente o item.

-   Não implementar upgrades permanentes de atributos, dano ou PV no
    MVP.

-   Manter a camada de meta progressão independente do estado temporário
    da run.

## 11. SAVE SYSTEM E RETOMADA DE RUN

Salvar e retomar a run é requisito central de produto, não melhoria
opcional. Uma expedição pode durar bastante, especialmente em
dificuldades altas.

### 11.1 Dados que precisam ser persistidos

-   Equipe e IDs dos personagens selecionados.

-   Nível, XP, PV, recursos, condições, habilidades e equipamentos
    temporários.

-   Ouro, consumíveis, inventário e estado dos mercadores.

-   Mapa gerado, rotas disponíveis, nodes concluídos, node atual e
    escolhas feitas.

-   Se estiver em combate: fila/iniciativa, unidade ativa, posições, PV
    e condições de todas as unidades, recursos de ação, estados de IA e
    objetos interativos.

-   Estado permanente do elenco, desbloqueios, conquistas e
    configurações.

### 11.2 Estratégia

-   Autosave após concluir cada node e após decisões relevantes.

-   Autosave em pontos seguros do combate, ou snapshot completo
    suficiente para retomar o combate exatamente.

-   Usar identificadores estáveis e versão do formato de save.

-   Escrever de forma segura para reduzir risco de arquivo parcialmente
    gravado.

-   Validar integridade e manter estratégia de recuperação/backup local.

-   Não depender de conexão com internet para salvar localmente.

A implementação exata do snapshot de combate deve ser testada cedo.
Nuvem, sincronização entre dispositivos e múltiplos slots de run ficam
para decisão posterior.

## 12. INTERFACE E INTEGRAÇÃO COM O DOMÍNIO

-   HUD de iniciativa sempre visível, com retratos, ordem, turno atual,
    PV e condições relevantes.

-   Painel de ações separado em Ações, Ações bônus e Reações.

-   Painéis de detalhes sob demanda: atributos, CA, passivas,
    equipamentos e condições.

-   Indicadores de alcance, rota, linha de visão, cobertura, área de
    efeito e unidades afetadas.

-   Log recolhido por padrão, expansível para rolagens e eventos.

-   Controles por toque; sem joystick e sem grid obrigatório.

-   A UI envia comandos e recebe estado/resultados; não é proprietária
    das regras.

## 13. TESTES E QUALIDADE

### 13.1 Testes unitários prioritários

-   Rolagens de dados, vantagem/desvantagem e críticos.

-   Ataque contra CA e testes de resistência contra CD.

-   Cálculo de dano, resistências, vulnerabilidades e imunidades.

-   Economia de ações e restauração de reações.

-   Fila individual de iniciativa e transição de turnos.

-   Aplicação, duração e remoção de condições.

-   Disparo de passivas por eventos.

-   Validação de alcance, linha de visão e movimento.

-   Distribuição de XP e encerramento de run.

-   Serialização e restauração do estado da run.

### 13.2 Testes de integração

-   Ataque iniciado pela UI percorre todo o fluxo de validação e
    resolução.

-   Passiva reativa dispara após evento correto e não dispara fora das
    condições.

-   Unidade incapacitada sai corretamente do fluxo de combate.

-   Save durante combate restaura estado equivalente.

-   Run concluída preserva desbloqueios e descarta estado temporário.

### 13.3 Critérios de qualidade

-   Nenhum erro crítico no console durante o fluxo validado.

-   Sistemas centrais testáveis sem depender de animações.

-   Comportamento reproduzível para depuração.

-   Desempenho aceitável no dispositivo Android-alvo.

-   Nenhuma regra crítica duplicada em scripts de interface ou unidades.

## 14. BACKLOG DE IMPLEMENTAÇÃO

O backlog abaixo é sequencial em termos de dependências, mas cada fase
deve ser dividida em tarefas pequenas. Não solicitar a implementação de
uma fase inteira em um único comando.

  -----------------------------------------------------------------------------
  Fase                    Escopo                        Critério de aceite
  ----------------------- ----------------------------- -----------------------
  0 --- Fundação          Criar projeto Godot 4, Git,   Projeto abre sem erros;
                          pastas essenciais, AGENTS.md, estrutura e instruções
                          cena principal, convenções,   estão versionadas.
                          logger e documentação         
                          inicial.                      

  1 --- Arena e movimento Arena simples, câmera         Unidade seleciona e se
                          isométrica, unidade           move até destinos
                          placeholder, seleção,         válidos sem atravessar
                          NavigationRegion2D/Agent2D,   obstáculos.
                          obstáculos, colisões e        
                          preview de rota.              

  2 --- Turnos            Combatant, iniciativa 1d4 +   Fila correta e
                          DEX, fila individual, unidade reproduzível; cada
                          ativa, início/fim de turno e  unidade recebe seu
                          avanço.                       turno individual.

  3 --- Economia de ações Ação principal, bônus,        Recursos são consumidos
                          movimento e reação; consumo,  e restaurados no
                          bloqueio de ações inválidas e momento esperado.
                          restauração.                  

  4 --- Combate básico    CharacterStats, DiceService,  Casos de teste cobrem
                          ataque vs CA, dano, PV,       acerto, erro, crítico e
                          crítico, vantagem/desvantagem dano.
                          e log.                        

  5 --- Habilidades e     AbilityData, alvos, alcance,  Uma habilidade ativa e
  passivas                linha de visão, ataque à      passivas permanente,
                          distância, magia com save, CD condicional e reativa
                          e três passivas               funcionam por dados.
                          demonstrativas.               

  6 --- Condições e       StatusEffectSystem, uma       Condição aplica/remove
  ambiente                condição inicial, empurrão,   corretamente; interação
                          colisão e objeto interativo   altera estado do
                          simples.                      ambiente.

  7 --- Morte e           0 PV, death saves,            Fluxo de incapacitação
  recuperação             estabilização, recuperação e  e recuperação validado
                          condição de derrota do grupo. por testes.

  8 --- UI mobile         HUD, iniciativa, painéis,     Combate pode ser
                          seleção por toque, preview,   concluído em
                          log e escalas de tela.        dispositivo touch sem
                                                        depender de
                                                        mouse/teclado.

  9 --- Integração do     Três personagens, três        Protótipo jogável
  protótipo               inimigos, classes iniciais    completo do encontro,
                          representativas, arena, IA    com vitória, derrota e
                          básica, testes e build        reinício.
                          Android.                      

  10 --- Base roguelite   Elenco, seleção de equipe,    Uma expedição mínima
                          RunManager, nodes, XP, loot,  pode iniciar, avançar
                          inventário e encerramento.    nodes e encerrar
                                                        preservando meta
                                                        progressão.

  11 --- Persistência da  Autosave, snapshot de         Fechar e reabrir o jogo
  run                     combate, retomada,            restaura a run,
                          versionamento e recuperação   inclusive no meio de
                          local.                        combate.

  12 --- Conteúdo MVP     Uma região, mapa de 12 nodes, Loop completo de
                          eventos, mercador, descanso,  expedição é jogável do
                          elites, chefe e tutorial mini início ao fim.
                          dungeon.                      

  13 --- Polimento e      Balanceamento, UX, áudio,     MVP atende critérios
  validação               acessibilidade, desempenho,   funcionais e de
                          estabilidade e distribuição   qualidade definidos.
                          de teste.                     
  -----------------------------------------------------------------------------

## 15. PROTÓTIPO JOGÁVEL: DEFINIÇÃO DE PRONTO

O primeiro protótipo deve demonstrar a diversão e a viabilidade do
núcleo tático. Não precisa conter mapa roguelite, meta progressão,
inventário completo ou conteúdo final.

-   Uma arena isométrica pequena.

-   Três personagens do jogador e três inimigos.

-   Iniciativa individual baseada em 1d4 + Destreza.

-   Movimentação livre com obstáculos e pathfinding.

-   Ação principal, bônus, movimento e reação.

-   Ataques corpo a corpo e à distância.

-   D20, CA, dano, PV, crítico, vantagem e desvantagem.

-   Uma magia com teste de resistência.

-   Uma condição de estado.

-   Uma passiva permanente, uma condicional e uma reativa como prova do
    sistema.

-   IA baseada em prioridades.

-   Vitória, derrota e reinício.

-   HUD de iniciativa e comandos essenciais para toque.

A primeira validação deve responder: o movimento é legível? A fila de
iniciativa é compreensível? As decisões de ação são interessantes? O
combate funciona sem grid? O controle por toque é preciso? O ritmo é
adequado?

## 16. ESTRUTURA DA CENA DE COMBATE

CombatScene ├── World │ ├── NavigationRegion2D │ ├── Environment │ └──
InteractiveObjects ├── Units │ ├── PlayerCharacters │ └── Enemies ├──
CombatSystems │ ├── CombatManager │ ├── TurnManager │ ├── ActionEconomy
│ ├── MovementSystem │ ├── TargetingSystem │ ├── PassiveAbilitySystem │
└── EventBus ├── Camera └── UI ├── InitiativeHUD ├── ActionPanel ├──
UnitDetailsPanel └── CombatLog

## 17. AGENTS.MD --- DIRETRIZES PARA AGENTES DE CÓDIGO

O arquivo AGENTS.md do repositório deve conter instruções permanentes
semelhantes às seguintes:

-   Antes de editar, leia a documentação de arquitetura e as regras de
    combate relevantes.

-   Descreva o objetivo, os arquivos que pretende alterar, dependências
    e critérios de aceite.

-   Trabalhe somente no escopo solicitado; não implemente fases futuras
    por iniciativa própria.

-   Mantenha regras de gameplay fora da UI e da apresentação visual.

-   Use Resources/dados para conteúdo configurável quando isso reduzir
    acoplamento.

-   Não introduza frameworks, plugins ou dependências sem justificar
    necessidade e custo.

-   Evite abstrações genéricas antes de existir pelo menos um caso real
    de uso.

-   Adicione ou atualize testes para regras alteradas.

-   Execute os testes e abra o projeto no Godot para verificar erros
    relevantes.

-   Atualize docs/development_log.md e decisões arquiteturais quando
    necessário.

-   Ao concluir, informe arquivos alterados, comportamento implementado,
    testes executados, resultado e limitações.

## 18. FORA DO ESCOPO TÉCNICO DO MVP

-   Multiplayer, PvP, matchmaking, servidor e sincronização online.

-   Mundo aberto e geração procedural irrestrita.

-   IA generativa ou adaptação avançada baseada em memória.

-   Multiclassing e grande catálogo de classes/subclasses.

-   Crafting e economia complexa.

-   Campanha narrativa extensa.

-   Loadout permanente ou progressão numérica permanente de poder.

-   Elevação, física e destruição ambiental complexas.

-   Sincronização em nuvem e múltiplos dispositivos, até decisão
    específica.

## 19. DECISÕES TÉCNICAS PENDENTES

  -----------------------------------------------------------------------
  Tema                    Decisão necessária      Quando resolver
  ----------------------- ----------------------- -----------------------
  Escala espacial         Unidade de distância,   Antes de fechar alcance
                          velocidade e conversão  e balanceamento de
                          entre pixels/unidades   habilidades.
                          lógicas.                

  Navegação               Validar                 Fase 1.
                          NavigationAgent2D,      
                          colisões entre unidades 
                          e movimentação em       
                          arenas densas.          

  Câmera/tilt             Determinar se tilt é    Protótipo visual.
                          viável com arte 2D ou   
                          se exige solução 2.5D.  

  Arquitetura de eventos  Escolher                Fase 0--2.
                          EventBus/sinais e       
                          contrato de payload.    

  Dados de conteúdo       Definir formato final   Quando primeiros dados
                          de Resources e          reais forem criados.
                          referências por ID.     

  Persistência em combate Definir snapshot        Antes de concluir fase
                          completo e pontos de    11; protótipo técnico
                          autosave seguros.       antecipado recomendado.

  Testes automatizados    Escolher framework ou   Fase 0.
                          harness compatível com  
                          Godot 4.                

  Build Android           Configurar SDK, export  Fase 0--1.
                          templates, resolução e  
                          dispositivos mínimos.   

  Áudio                   Pipeline de música      Polimento do MVP.
                          adaptativa e efeitos.   
  -----------------------------------------------------------------------

## 20. REGRAS DE TRABALHO E CONTROLE DE MUDANÇAS

1.  Consultar o GDD e este documento antes de propor alterações
    estruturais.

2.  Classificar novas decisões como definidas, provisórias, pendentes ou
    futuras.

3.  Se uma mudança afetar regras, dados, save ou contratos entre
    sistemas, registrar a decisão em docs/decisions.md.

4.  Atualizar o backlog somente quando o escopo ou a dependência mudar.

5.  Implementar a menor fatia vertical que possa ser executada e
    testada.

6.  Não avançar de fase sem critérios de aceite atendidos ou limitações
    explicitamente registradas.

7.  Fazer commits pequenos após validação, com mensagens que descrevam a
    mudança.

## 21. PRÓXIMA SEQUÊNCIA RECOMENDADA

1.  Criar/validar o repositório Godot 4 e a estrutura inicial.

2.  Criar AGENTS.md, docs/architecture.md, docs/combat_rules.md e
    docs/development_log.md.

3.  Abrir uma cena vazia e validar build/export Android desde cedo.

4.  Construir uma arena placeholder com câmera e navegação.

5.  Implementar seleção e movimento antes de adicionar regras complexas.

6.  Adicionar turnos e economia de ações.

7.  Adicionar resolução de ataque e dados.

8.  Integrar habilidades, passivas, condições e IA.

9.  Validar o protótipo com sessões reais de jogo e ajustar o design
    antes de expandir.

## 22. CRITÉRIO GERAL DE SUCESSO

A arquitetura será considerada adequada se permitir adicionar conteúdo
sem reescrever sistemas centrais, manter regras testáveis e
reproduzíveis, restaurar uma run com fidelidade, executar de forma
estável em dispositivos mobile e sustentar a evolução incremental do
protótipo ao MVP.

Este documento deve evoluir junto com o projeto. Ele não substitui o
GDD: traduz as decisões de design em responsabilidades técnicas,
contratos e entregas verificáveis.
