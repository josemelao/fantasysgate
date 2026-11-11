TACTICAL FANTASY RPG

GAME DESIGN DOCUMENT

Documento consolidado de visão, sistemas e escopo

Versão 1.0 \| 06 de novembro de 2026 Base de design para protótipo
jogável e evolução incremental até o MVP

## CONTROLE DO DOCUMENTO

  -----------------------------------------------------------------------
  Campo                               Descrição
  ----------------------------------- -----------------------------------
  Projeto                             Tactical Fantasy RPG

  Documento                           Game Design Document consolidado

  Versão                              1.0 --- 06/11/2026

  Engine planejada                    Godot 4

  Plataforma prioritária              Android/mobile; PC/Steam como
                                      possibilidade futura

  Status                              Documento-base de design para
                                      protótipo jogável e evolução até o
                                      MVP
  -----------------------------------------------------------------------

Status das decisões: DEFINIDO = aprovado; PROVISÓRIO = ponto de partida
sujeito a testes; PENDENTE = ainda não decidido; FUTURO/FORA DO MVP =
expansão não prevista para a primeira versão.

## 1. VISÃO GERAL DO PRODUTO

### 1.1 Conceito

Tactical Fantasy RPG é um RPG tático de fantasia com estrutura
roguelite, prioritariamente para dispositivos móveis. Combina combate
por turnos individuais, movimentação livre em arenas isométricas, regras
inspiradas em Dungeons & Dragons 5ª edição e progressão temporária
durante expedições.

A experiência pretendida pode ser resumida como um "BG3 simplificado
para mobile": preservar profundidade tática, identidade de regras,
escolhas de construção de personagem e interações de combate, reduzindo
escala, complexidade de apresentação e volume de conteúdo para adequação
ao formato mobile.

### 1.2 Pilares de design

-   Combate tático primeiro: posicionamento, escolha de alvo, ações,
    habilidades, riscos e sinergias formam o núcleo da experiência.

-   Identidade D&D adaptada: atributos, proficiência, dados, CA, testes,
    condições, classes, magias e equipamentos reconhecíveis, adaptados
    para ritmo, equilíbrio e clareza.

-   Roguelite de progressão temporária: a equipe evolui durante a run;
    níveis, equipamentos e recursos temporários são reiniciados ao
    final, enquanto desbloqueios horizontais permanecem.

-   Movimentação livre: sem grid obrigatório, com pathfinding, alcance,
    linha de visão, obstáculos e posicionamento.

-   Mobile-first: interface e controles projetados para toque,
    legibilidade e sessões interrompíveis.

-   Arquitetura expansível: sistemas orientados a dados e desacoplados,
    sem implementar antecipadamente todo o conteúdo futuro.

### 1.3 Referências e limites

-   Baldur's Gate 3: referência de combate, interação ambiental, regras
    adaptadas, leitura tática e profundidade de builds; não é referência
    de escala ou conteúdo a reproduzir.

-   D&D 5e: base conceitual para atributos, dados, classes,
    proficiências, magias, condições e testes; não exige reproduzir
    todas as regras sem adaptação.

-   Dofus/Waven: referência de perspectiva isométrica e apresentação
    visual, não de movimentação em grid.

-   Triangle Strategy/Octopath Traveler II: referência de câmera
    elevada, profundidade e composição visual próxima de HD-2D, sem
    obrigação de copiar o pipeline.

-   Sword of Convallaria: referência complementar de pixel art detalhada
    e fantasia estilizada.

A identidade visual será própria: fantasia estilizada, pixel art
detalhada, arenas isométricas, cenários em camadas e profundidade
visual.

### 1.4 Plataforma e formato

-   Plataforma prioritária: Android/mobile.

-   PC/Steam: possibilidade futura, com adaptação de controles e
    interface.

-   Arenas isométricas, sem mundo aberto.

-   Multiplayer, PvP e PvP assimétrico são possibilidades futuras, fora
    do protótipo e do MVP.

## 2. ESTRUTURA CENTRAL: ROGUELITE

### 2.1 Modelo de progressão

O modelo escolhido é **elenco permanente com progressão temporária por
expedição**.

Permanente: personagens e slots do elenco; raça, classe, aparência e
configurações-base; classes/subclasses desbloqueadas; itens liberados na
pool de drops/lojas; conquistas, milestones e cosméticos.

Temporário: nível e XP da run; equipamentos encontrados; habilidades
temporárias; ouro, consumíveis e recursos de combate; PV, condições e
estado atual da equipe.

Ao terminar a run, os elementos temporários são perdidos. Os
desbloqueios permanentes permanecem.

### 2.2 Loop principal

1.  Selecionar ou criar personagens no elenco permanente.

2.  Escolher três personagens para a equipe.

3.  Iniciar uma expedição em uma região.

4.  Percorrer mapa ramificado, escolhendo nodes e rotas.

5.  Combater, explorar, participar de eventos, descansar, comprar e
    obter recompensas.

6.  Ganhar XP, níveis, habilidades e equipamentos temporários.

7.  Concluir, perder, morrer ou abandonar a expedição.

8.  Aplicar desbloqueios e recompensas permanentes elegíveis.

9.  Retornar ao elenco e iniciar outra run.

### 2.3 Meta progressão

A meta progressão será predominantemente **horizontal**, sem aumentos
permanentes de atributos, dano ou PV por enquanto. O objetivo é
desbloquear variedade e possibilidades, não exigir grind de poder.

-   Personagens, classes e subclasses.

-   Equipamentos na pool de drops, mercadores e eventos.

-   Inimigos, encontros, eventos e regiões.

-   Cosméticos, conquistas, desafios e milestones.

-   Liberar um item na pool não entrega o item automaticamente; apenas o
    torna elegível.

-   Exemplo ilustrativo: derrotas repetidas de um chefe podem liberar
    seu item assinatura, um evento relacionado e um cosmético em
    milestones diferentes.

### 2.4 Modos planejados

-   Quick Run: personagens predefinidos para iniciar rapidamente.

-   Modo completo: criação do zero, com raça, classe, atributos,
    perícias, nome e aparência.

-   Ambos compartilham o elenco permanente e o mesmo núcleo de regras.

-   No MVP, personagens predefinidos podem bastar; a arquitetura deve
    permitir criação completa posteriormente.

O foco é a expedição roguelite. Campanha narrativa, loadout permanente e
progressão de equipamento por pontos não integram o MVP.

## 3. PERSONAGENS, ELENCO E CRIAÇÃO

### 3.1 Equipe e elenco

-   Três personagens controlados simultaneamente.

-   Qualquer combinação de raças e classes; classes repetidas são
    permitidas.

-   Não há composição obrigatória de tanque, suporte ou dano.

-   Personagens permanecem no elenco após vitória, derrota ou morte
    durante a run.

-   É permitido repetir os mesmos personagens em runs consecutivas.

-   MVP: seis slots gratuitos. Slots adicionais podem ser desbloqueados
    por conquistas/milestones. Não impor limite estrutural rígido no
    código.

### 3.2 Classes e subclasses do MVP

  -----------------------------------------------------------------------
  Classe                  Subclasse inicial       Identidade
  ----------------------- ----------------------- -----------------------
  Fighter/Guerreiro       Champion/Campeão        Combate marcial,
                                                  resistência e
                                                  consistência física

  Wizard/Mago             Evocation/Evocação      Conjuração arcana e
                                                  dano mágico em área

  Rogue/Ladino            Thief/Ladrão            Mobilidade,
                                                  furtividade, precisão e
                                                  exploração
  -----------------------------------------------------------------------

Outras subclasses poderão ser liberadas por desafios, conquistas, nodes
especiais ou milestones. Exemplos futuros: Mestre de Batalha e Cavaleiro
Arcano; Abjurador e Ilusionista; Assassino e Trapaceiro Arcano.

### 3.3 Raças

MVP: Humano, Elfo, Anão e Halfling. Cada raça deve ter identidade por
características e passivas próprias. O usuário prefere bônus de
atributos flexíveis, no modelo de D&D 2024/BG3, sem vincular rigidamente
bônus a uma raça. Valor e distribuição exatos permanecem pendentes.

Sub-raças fora do MVP, mas previstas na arquitetura: Alto Elfo, Elfo da
Floresta, Drow; Anão da Montanha/Colina; Halfling Robusto/Pés-Leves.

### 3.4 Atributos

Força (STR), Destreza (DEX), Constituição (CON), Inteligência (INT),
Sabedoria (WIS) e Carisma (CHA), com modificadores tradicionais.

-   Valores iniciais orientados pela classe, com personalização híbrida.

-   Presets por classe disponíveis desde o início.

-   Point Buy de 27 pontos poderá ser desbloqueado após determinado
    nível da conta.

-   Rolagem 4d6 descartando o menor fica reservada a evento semanal;
    regras de balanceamento pendentes.

-   Standard Array não será método independente necessário no MVP.

-   Redistribuição livre durante a criação, antes de salvar; respec
    posterior pendente.

### 3.5 Perícias

Manter as 18 perícias tradicionais:

-   STR: Atletismo.

-   DEX: Acrobacia, Furtividade, Prestidigitação.

-   CON: nenhuma perícia associada no conjunto tradicional.

-   INT: Arcanismo, História, Investigação, Natureza, Religião.

-   WIS: Adestrar Animais, Intuição, Medicina, Percepção, Sobrevivência.

-   CHA: Atuação, Enganação, Intimidação, Persuasão.

### 3.6 Antecedentes

Raça, classe e antecedente são independentes. Quatro antecedentes
iniciais sugeridos: Soldado, Criminoso, Sábio e Herói Popular. No MVP
concedem perícias e proficiências; características narrativas e opções
exclusivas em eventos ficam para o futuro.

### 3.7 Proficiências

Proficiências de armas e armaduras seguem D&D 5e 2014, com penalidades
simplificadas:

-   Fighter: armas simples e marciais; armaduras leves, médias e
    pesadas; escudos.

-   Wizard: adagas, dardos, fundas, bordões e bestas leves; sem
    proficiência em armaduras/escudos.

-   Rogue: armas simples, bestas de mão, espadas longas, rapieiras e
    espadas curtas; armaduras leves.

-   Elfo: espadas longas/curtas e arcos curtos/longos.

-   Anão: machados de batalha, machadinhas, martelos leves e martelos de
    guerra.

-   Humano e Halfling: sem proficiências adicionais no modelo padrão.

-   Arma sem proficiência: não recebe bônus de proficiência nos ataques.

-   Armadura/escudo sem proficiência: pode equipar sem penalidades
    adicionais no MVP.

-   Conjuração não será impedida por armadura/escudo sem proficiência no
    modelo simplificado.

### 3.8 Equipamento inicial e estado

A classe define equipamento básico, com alternativas compatíveis com
proficiências; antecedente pode conceder complemento. Exemplos: Fighter
com armadura/arma e opção de escudo ou arma de duas mãos; Wizard com
cajado/foco arcano/vestimentas; Rogue com armadura leve/armas
ágeis/ferramentas de ladrão.

Separar dados permanentes (nome, raça, classe, aparência e
configurações-base) de estado temporário da run (nível, XP,
equipamentos, habilidades adquiridas, recursos, PV e condições).

## 4. COMBATE

### 4.1 Filosofia

Modelo híbrido: D&D 5e 2014 como base, referências de BG3 e adaptações
próprias para ritmo, equilíbrio, clareza e mobile. Preservar identidade
tática sem reproduzir toda a complexidade de um RPG de mesa.

### 4.2 Dados, atributos e iniciativa

-   Dados poliédricos d4, d6, d8, d10, d12 e d20.

-   Seis atributos tradicionais.

-   Iniciativa: 1d4 + modificador de Destreza.

-   Turnos individuais para cada personagem e inimigo; sem agrupar
    aliados.

-   Desempate de iniciativa pendente.

-   Progressão exata da proficiência pendente.

### 4.3 Economia de ações

  Recurso          Regra-base
  ---------------- -----------------------------------------------------------
  Ação principal   Uma por turno
  Ação bônus       Uma por turno, quando habilidade/ação permitir
  Movimento        Deslocamento disponível conforme velocidade
  Reação           Uma por rodada, restaurada no próximo turno do personagem

Ações, bônus e reações devem ser categorias distintas na interface. Um
sistema central valida disponibilidade e consumo.

### 4.4 Ataques, testes e CD

-   Ataque: d20 + modificador aplicável + proficiência quando aplicável,
    contra CA.

-   Crítico em 20 natural; dobra os dados de dano do ataque.

-   1 natural é falha automática.

-   Vantagem: dois d20, usar o maior. Desvantagem: dois d20, usar o
    menor.

-   Fontes múltiplas não acumulam; vantagem e desvantagem simultâneas se
    anulam.

-   Resistência: d20 + modificador de atributo + proficiência quando
    aplicável, contra CD.

-   CD de magia: 8 + proficiência + modificador do atributo de
    conjuração.

O sistema suporta tipos de dano, resistências, vulnerabilidades e
imunidades. O cálculo de dano deve ser centralizado.

### 4.5 Movimento, alcance e cobertura

-   Movimento livre sem grid obrigatório.

-   Pathfinding para rotas navegáveis.

-   Alcance calculado por distância real.

-   Linha de visão para ataques/efeitos que exigem visão.

-   Meia cobertura: +2 CA; três quartos: +5 CA; cobertura total bloqueia
    ataques e efeitos diretos.

-   Obstáculos podem bloquear movimento, linha de visão ou ambos.

-   Elevação avançada fora do MVP, mas prevista na arquitetura.

-   Escala de distância, unidade de medida e dimensões das arenas
    pendentes.

### 4.6 Reações e empurrão

Ataque de oportunidade quando uma unidade sai voluntariamente do alcance
corpo a corpo sem evitar a reação. Empurrão previsto como ação bônus,
inspirado em BG3, com possíveis colisões, obstáculos e perigos
ambientais. Detalhes de colisão precisam de teste.

### 4.7 Magias e concentração

-   Espaços de magia limitados conforme classe/nível; cantrips
    ilimitados.

-   Uma concentração por personagem; iniciar outra encerra a anterior.

-   Ao receber dano durante concentração, realizar teste de Constituição
    conforme regra definida.

-   Concentração termina quando o personagem fica incapacitado.

-   Componentes materiais comuns simplificados.

-   Progressão de espaços e catálogo de magias pendentes.

### 4.8 Condições

Condições previstas: agarrado, amedrontado, atordoado, caído, cego,
envenenado, enfeitiçado, incapacitado, invisível, paralisado,
petrificado e surdo. Duração, acumulação, resistência, imunidade,
remoção e interações ainda precisam de especificação.

### 4.9 Morte e recuperação

-   Em 0 PV: inconsciente/incapacitado.

-   Testes contra morte no início do turno: 10+ sucesso; 9- falha.

-   1 natural: duas falhas; 20 natural: recupera 1 PV.

-   Três sucessos estabilizam; três falhas causam morte.

-   Estabilizado permanece inconsciente e pode receber cura.

-   Personagem morto fica indisponível até o fim da run, salvo
    ressurreição por habilidade, item ou evento raro.

-   Party wipe encerra a run, salvo recuperação específica prevista.

-   Ao fim da run, personagens retornam ao elenco permanente.

-   Recursos e condições para ressurreição pendentes.

### 4.10 Descanso

Descansos curtos e longos só em nodes apropriados.

-   Curto: recuperação parcial conforme classe e recursos.

-   Longo: recuperação forte; jogador escolhe descansar ou ativar a
    função/recompensa do node, sacrificando-a.

-   Uma carga de descanso longo por run.

-   Base provisória: duas cargas de descanso curto; ao usar longo,
    recupera as duas cargas curtas.

-   Alternativa de balanceamento: uma carga curta e uma longa,
    recuperando uma curta após o longo.

-   Recuperação exata de PV, dados de vida, slots e recursos pendente.

### 4.11 Eventos de combate e passivas

Eventos padronizados previstos:

`on_turn_start`, `on_turn_end`, `on_attack_start`, `on_attack_hit`,
`on_attack_miss`, `on_damage_received`, `on_damage_dealt`,
`on_critical_hit`, `on_kill`, `on_movement`, `on_condition_applied`,
`on_condition_removed`, `on_ally_down`, `on_reaction_triggered`,
`on_death_save_success`, `on_death_save_failure`.

PassiveAbilitySystem administra passivas raciais, de classe, subclasse,
talentos, equipamentos, condicionais e reativas. Deve suportar passivas
permanentes, condicionais, reativas, acumuláveis, exclusivas e
progressivas. Passivas são orientadas a dados e acionadas por eventos;
evitar regras individuais espalhadas no CombatResolver.

## 5. HABILIDADES E PROGRESSÃO DURANTE A RUN

### 5.1 XP e níveis

-   Progressão tradicional por XP, não por marcos.

-   XP igualmente distribuído entre os três personagens ativos.

-   Personagens fora da equipe não recebem XP.

-   Rotas podem conceder mais/menos XP em troca de riscos e recompensas.

-   Encontros não escalam automaticamente ao nível do grupo.

-   Cap por run ainda não definido; nível 12 foi apenas sugestão
    provisória.

-   Se cap 12 for adotado: proficiência sugerida níveis 1--4 +2; 5--8
    +3; 9--12 +4.

### 5.2 Progressão de classe

-   Nos níveis 4, 8 e 12: aumento de atributo ou talento, se esses
    níveis integrarem a progressão final.

-   Subclasse escolhida no nível 3.

-   Características de classe e subclasse desbloqueadas conforme nível.

-   Multiclassing fora do MVP: futuramente limitado; em jogo maduro
    poderá ser completo.

### 5.3 Fontes de habilidades

Habilidades de classe por nível; habilidades de subclasse; talentos;
recompensas/eventos/mercadores; armas e equipamentos que concedem
habilidades próprias. Não haverá escolha aleatória de habilidade após
cada combate: habilidades vêm da progressão ou de nodes/eventos
definidos.

### 5.4 Testes de perícia em eventos

O jogador escolhe qual dos três personagens ativos realiza o teste. Uma
única rolagem: d20 + modificador de atributo + proficiência quando
aplicável. 20 natural é sucesso automático; 1 natural é falha
automática. Não há rolagem separada de proficiência nem categorias
adicionais de sucesso/falha crítica. Cada evento define CD, resultados e
consequências.

## 6. EQUIPAMENTOS, INVENTÁRIO E ECONOMIA

### 6.1 Slots

Arma principal; arma secundária/escudo (arma de duas mãos ocupa ambos);
armadura; elmo; luvas; botas; amuleto; dois anéis; capa. São dez
posições equipáveis considerando duas mãos e dois anéis.

### 6.2 Armas, armaduras e propriedades

-   Armas simples/marciais para proficiência, sem excesso de
    subcategorias.

-   Propriedades: acuidade, leve, versátil, duas mãos, arremesso,
    alcance e munição.

-   Armas têm dano, tipo de dano e podem possuir ações especiais.

-   Armaduras leves: CA + DEX integral; médias: DEX até +2; pesadas: CA
    fixa sem DEX.

-   Escudos concedem bônus de CA.

-   Equipamentos podem oferecer atributos, resistências, passivas,
    habilidades ativas, efeitos elementais e sinergias.

### 6.3 Raridade e sets

Raridades: comum, incomum, raro, muito raro e lendário. Raridade não
significa apenas números maiores; itens raros/lendários devem criar
mecânicas e sinergias. Sets desejados com bônus progressivos por
quantidade de peças (2, 3, 4 etc.). Peças individuais também devem ser
úteis; sets opcionais, não obrigatórios.

### 6.4 Regras de equipamento

-   Armas e escudo: troca consome ação bônus durante combate.

-   Armadura e demais slots: alteração apenas fora de combate.

-   Consumíveis seguem regras próprias.

-   Inventário sem limite de capacidade no MVP; cada equipamento ocupa
    um slot.

-   Equipamentos podem ser transferidos entre personagens fora de
    combate.

-   Futuro: capacidade compartilhada baseada na soma de STR da equipe;
    itens empilháveis e itens grandes podem ocupar mais slots.

### 6.5 Consumíveis

  -----------------------------------------------------------------------
  Item                                Regra-base
  ----------------------------------- -----------------------------------
  Poção de cura                       Ação bônus

  Pergaminho                          Ação principal

  Bomba/arremessável                  Ação principal

  Veneno                              Aplicação fora de combate ou
                                      conforme item

  Elixir                              Um ativo por personagem; dura um
                                      combate; novo substitui anterior
  -----------------------------------------------------------------------

Elixir pode ser usado antes do combate sem custo de ação; durante
combate consome ação bônus. Valores de cura citados como referência:
poção 2d4+2; maior 4d4+4; superior 8d4+8; suprema 10d4+20. Catálogo e
balanceamento pendentes.

### 6.6 Loot

-   Cada inimigo tem tabela própria; não existe tabela global de
    equipamentos.

-   Loot reflete identidade, raça, facção, função e características.

-   Não há garantia de drop ideal para a equipe; ausência de item ideal
    não pode tornar run impossível.

-   Normal: chance de um item.

-   Elite: um a dois itens, com melhores chances de raridade.

-   Chefe: recompensa garantida e possível item exclusivo.

-   Probabilidades e quantidades são provisórias.

### 6.7 Mercadores e moeda

-   Mercadores em nodes específicos, com estoque próprio parcialmente
    aleatório e coerente com região/identidade.

-   Compra e venda de equipamentos encontrados; estoque limitado.

-   Uma moeda temporária por run, obtida em combate, baús e eventos;
    gasta em mercadores, subornos e serviços; perdida ao encerrar run.

-   Teste de Carisma pode conceder desconto; falha pode aumentar preços.

-   Roubo por perícia apropriada pode resultar em descoberta,
    multa/suborno, prisão, combate, fuga ou diálogo.

-   Prisão como node/evento: combater guardas, escapar furtivamente,
    persuadir/subornar, rota alternativa ou recuperar equipamentos
    confiscados.

-   Diplomacia, furtividade e força devem oferecer abordagens
    alternativas e consequências reais.

## 7. EXPEDIÇÕES, MAPA E REGIÕES

### 7.1 Mapa

Mapa ramificado inspirado em Slay the Spire, com escolha entre caminhos
e nodes. Rotas perigosas podem oferecer XP e recompensas melhores.
Geração parcialmente procedural e controlada, não totalmente aleatória.
Dificuldade de cada node é fixa, sem escalonamento automático ao nível
do grupo.

### 7.2 Nodes

Combate normal; elite; chefe; tesouro; evento; mercador; descanso;
desconhecido.

### 7.3 Estrutura inicial da expedição

  Ato     Sequência-base de quatro nodes
  ------- ---------------------------------------------------------
  Ato 1   2 combates normais; 1 evento/tesouro/mercador; 1 elite
  Ato 2   2 combates normais; 1 evento/descanso/mercador; 1 elite
  Ato 3   2 combates normais; 1 tesouro/evento/descanso; 1 chefe

Total inicial: 12 nodes percorridos --- 6 combates normais, 2 elites, 1
chefe e 3 utilitários. Os 12 não representam todos os nodes exibidos; o
mapa pode oferecer caminhos alternativos. Quantidade provisória, a
validar pela duração real.

### 7.4 Regiões

Cada região terá identidade visual/temática, inimigos, facções, eventos
e recompensas próprias. Não precisa conter exclusivamente uma família de
inimigos; encontros mistos coerentes são permitidos.

Exemplos conceituais ainda não aprovados como estrutura final: floresta
com bestas/animais/druidas/espíritos; masmorra com mortos-vivos; terras
vulcânicas com demônios e fogo; cidade em ruínas com mercenários,
bandidos e cultistas.

MVP: uma única região temática. Ainda não definido se runs futuras
poderão mesclar regiões.

### 7.5 Eventos

Eventos devem oferecer escolhas e consequências reais, não apenas
recompensas aleatórias. Exemplos: NPC misterioso, altar, mercenário,
ruínas ou encontro narrativo. Podem conceder recompensas, penalidades,
combates especiais ou alterar nodes futuros.

## 8. INIMIGOS, IA, FACÇÕES E CHEFES

### 8.1 Funções táticas

Tanque, combatente, atirador, assassino, controlador, suporte e
invocador. Um inimigo pode combinar duas funções, mas possui uma
predominante. Encontros combinam funções para criar sinergias.

### 8.2 Dificuldade

Dificuldades de encontro: fácil, normal, difícil e elite; chefe é
categoria própria. Dificuldade geral configurável:

-   Aventureiro: mais acessível; intenção aproximada dos inimigos
    visível.

-   Veterano: padrão; intenção oculta.

-   Lendário: maior exigência tática, inimigos mais eficientes e
    recursos mais escassos; intenção oculta.

A dificuldade geral é independente do nível da equipe. Não usar apenas
multiplicadores de PV/dano.

### 8.3 IA

IA baseada em regras e avaliação contextual, não generativa,
determinística e testável. Considera posição, PV, ameaças, distância,
oportunidades, aliados e ações viáveis. Perfil tático (função), perfil
comportamental (agressivo, defensivo, covarde, tático, protetor) e
avaliação contextual são camadas distintas. Se a ação preferencial não
for viável, avaliar alternativas. Futuro: memória de combate e adaptação
a padrões recorrentes.

### 8.4 Intenção inimiga

Aventureiro exibe categoria aproximada (ataque, movimento, defesa ou
preparação), sem necessariamente revelar alvo/detalhe. Veterano e
Lendário ocultam. Habilidades futuras poderão revelar intenções.

### 8.5 Facções

Famílias conceituais: goblins/criaturas tribais; mortos-vivos;
demônios/cultistas; bestas/monstros; humanoides/mercenários; criaturas
mágicas. Cada facção pode ter identidade visual, papéis, sinergias,
passivas, chefes e recompensas. Encontros mistos são permitidos com
coerência temática.

### 8.6 Chefes

Chefes devem ter identidade mecânica própria: habilidades exclusivas,
passivas/traits/feats, mecânicas especiais, fases, alterações
ambientais, invocações/aliados, resistências/vulnerabilidades. MVP: um
chefe com habilidades, passivas e mecânicas próprias. Fases múltiplas,
ambiente complexo e itens exclusivos podem ser expandidos depois.

## 9. ARENAS, AMBIENTE E INTERAÇÃO

### 9.1 Arenas

Arenas manuais reutilizadas com configurações variáveis de inimigos,
posições, obstáculos, objetos, elementos, iluminação e decoração. Não
criar arena exclusiva por encontro no MVP.

### 9.2 Camadas e objetos

Camadas: terreno/caminhos; vegetação/construções/decoração;
obstáculos/elementos de combate; efeitos ambientais/iluminação.

-   Decorativos: não influenciam combate.

-   Obstáculos: bloqueiam movimento e/ou linha de visão.

-   Interativos: ativados, destruídos, empurrados ou causam efeitos.

### 9.3 Destruição e superfícies

Destruição parcial no MVP, limitada a objetos selecionados (barris
explosivos, estruturas frágeis e obstáculos que desaparecem),
atualizando colisão e pathfinding. Sem destruição generalizada.

Fogo, água e explosões no MVP. Fogo pode queimar/propagar em objetos
específicos; água interage com fogo e pode preparar condução elétrica
futura; explosões causam dano em área, destroem objetos e geram reações.
Dados devem permitir gelo, eletricidade, veneno, ácido etc. no futuro.

### 9.4 Câmera e perspectiva

-   Câmera isométrica 2D, ângulo elevado e leitura clara.

-   Sem rotação horizontal no MVP.

-   Zoom por pinça; deslocamento por arraste.

-   Acompanhamento suave do personagem selecionado e limites por arena.

-   Leve ajuste de inclinação vertical para visão mais elevada em
    agrupamentos; não top-down completo.

-   Compatibilidade do tilt com arte 2D deve ser validada; pode exigir
    projeção/transformação ou preparação 2.5D, sem obrigar sprites de
    múltiplos ângulos.

-   Referências de perspectiva/profundidade: Triangle Strategy e
    Octopath Traveler II.

### 9.5 Animações e feedback

Animações rápidas e dinâmicas, não cinematográficas. Feedback visual
para dano, crítico, esquiva, resistência, habilidades e condições.

## 10. INTERFACE E EXPERIÊNCIA

### 10.1 Princípios

Interface minimalista por padrão, detalhes sob demanda, mobile-first e
desenhada para toque. Evitar painéis permanentes excessivos. Priorizar
iniciativa, estado da equipe, ações disponíveis e consequências.

### 10.2 Combate

-   Barra completa de iniciativa sempre visível, com retratos, ordem,
    turno atual, PV e condições relevantes.

-   Ao selecionar unidade: PV/CA, atributos, modificadores, passivas,
    condições e equipamentos.

-   Painel de ações com abas: Ações, Ações bônus e Reações.

-   Indicadores de alcance, área de efeito, aliados/inimigos afetados,
    linha de visão, obstáculos, cobertura, rota e destinos válidos.

-   Log recolhido por padrão e expansível, com rolagens, acertos, erros,
    dano, condições, reações e eventos.

### 10.3 Controles por toque

Tocar unidade para selecionar; tocar terreno para destino; mostrar rota
antes de confirmar; ajustar área de efeito por toque/arraste; ações na
parte inferior. Sem joystick e sem grid obrigatório.

### 10.4 Menus

  -----------------------------------------------------------------------
  Tela                                Função inicial
  ----------------------------------- -----------------------------------
  Inicial                             Continuar, nova expedição e
                                      configurações

  Elenco                              Personagens desbloqueados e
                                      criação/edição

  Seleção de equipe                   Escolher três personagens

  Expedição                           Mapa de nodes e progresso

  Coleção                             Equipamentos, personagens, classes,
                                      subclasses e desbloqueios

  Conquistas                          Desafios e progresso permanente

  Configurações                       Áudio, gráficos, controles e
                                      acessibilidade
  -----------------------------------------------------------------------

Estrutura inicial, ampliável sem complexidade desnecessária.

### 10.5 Tutorial

Mini dungeon guiada e exclusiva:

1.  Introdução narrativa e equipe.

2.  Movimento, obstáculos e posicionamento.

3.  Iniciativa, turnos e combate básico.

4.  Ações, bônus, reações e habilidades.

5.  Magias, linha de visão e rolagens.

6.  Condições e interações ambientais.

7.  Exploração, baús, recompensas e eventos.

8.  Chefe introdutório e liberação do roguelite.

Deve ser curto, permitir consulta posterior e evitar obrigar jogadores
experientes a repetir tudo.

### 10.6 Acessibilidade

Escala ajustável da interface; legendas para diálogos e avisos sonoros;
volumes independentes; redução de flashes, efeitos e tremores;
indicadores visuais além de sinais sonoros; velocidade de animação
ajustável. Outras opções podem ser adicionadas após testes.

## 11. ÁUDIO

Direção musical épica e orquestral, adaptada à situação:

-   Exploração: atmosférica e aventureira.

-   Combate normal: épica e dinâmica.

-   Elite: intensa e ameaçadora.

-   Chefe: composição grandiosa e exclusiva.

-   Eventos: trilha temática conforme narrativa.

-   Vitória/derrota: temas próprios.

Controles independentes para música, efeitos sonoros e ambiente.
Catálogo de efeitos de habilidades, interface, combate e feedback ainda
pendente.

## 12. SALVAMENTO E CONTINUIDADE

### 12.1 Requisito central

O jogador deve poder interromper uma expedição e retomá-la depois, sem
perder progresso. É requisito central do loop, especialmente em
dificuldades altas e runs longas.

### 12.2 Salvamento automático

-   Salvar após cada node concluído e decisão relevante.

-   Fechar o jogo durante combate não pode apagar progresso.

-   Persistir equipe, níveis, XP, equipamentos, habilidades, recursos,
    ouro, mapa, node atual e progresso.

-   Durante combate, preservar PV, condições, posições, iniciativa,
    estado dos inimigos, objetos interativos e demais dados necessários
    para reconstrução exata.

-   Retomar exatamente do ponto em que o jogador saiu.

-   Uma run ativa por vez por slot de jogo, como diretriz inicial.

-   Separar dados permanentes do elenco e dados temporários da run.

Persistir o estado completo, não apenas o último node. Considerar
gravação segura, versionamento e recuperação contra falhas/corrupção.
Nuvem e múltiplos dispositivos pendentes.

### 12.3 Encerramento

Vitória, derrota total ou abandono encerram a run. Equipamentos, níveis,
XP, recursos e ouro temporários são perdidos; desbloqueios, personagens,
subclasses e conquistas permanecem. Regras de confirmação de abandono e
recompensas específicas ainda pendentes.

## 13. DIFICULDADE E BALANCEAMENTO

A dificuldade do node é fixa e não escala automaticamente ao nível da
equipe. O nível determina recursos e habilidades disponíveis. Rotas mais
perigosas podem oferecer XP e recompensas melhores.

-   Aventureiro: mais acessível e com intenção aproximada visível.

-   Veterano: padrão, intenção oculta.

-   Lendário: maior exigência tática e recursos escassos, intenção
    oculta.

Balancear comportamento, composição e parâmetros dos encontros, não
apenas PV/dano. A ausência de item ideal não pode inviabilizar a run.
Duração de combates, nodes, descansos, XP, loot e economia precisam de
testes.

## 14. MONETIZAÇÃO E DISTRIBUIÇÃO

Modelo comercial em aberto, com preferência por experiência justa e sem
monetização agressiva:

1.  Premium: compra única e acesso completo.

2.  Free limitado, semelhante a demo, com compra da versão completa.

3.  DLCs somente após lançamento completo e com conteúdo
    substancialmente novo, possivelmente em versão 2.0.

Não vender poder ou vantagens competitivas; não fragmentar
artificialmente conteúdo; não implementar monetização no MVP.
Arquitetura não deve depender de modelo comercial definido.

## 15. ESCOPO DO PROJETO

### 15.1 Primeiro marco: protótipo jogável

Objetivo: provar que o núcleo do combate é funcional e divertido, sem
implementar ainda o loop roguelite completo.

-   Uma arena isométrica.

-   Três personagens controláveis e três inimigos.

-   Iniciativa individual.

-   Movimento livre, obstáculos e pathfinding.

-   Ação principal, bônus, movimento e reação.

-   D20, CA, dano, PV, crítico, vantagem/desvantagem.

-   Ataques corpo a corpo e à distância.

-   Uma magia, uma condição e uma passiva representativa.

-   IA básica por prioridades.

-   Vitória, derrota e reinício do encontro.

-   HUD funcional para testar o combate.

### 15.2 Evolução incremental

Após o protótipo, adicionar camadas pequenas e testáveis. Cada camada
deve ser validada antes de ampliar conteúdo. O protótipo não deve ser
confundido com o MVP.

### 15.3 Conteúdo mínimo do MVP

-   Três classes e quatro raças; antecedentes iniciais.

-   Uma subclasse por classe.

-   Combate híbrido, movimento livre, iniciativa e economia de ações.

-   Amostra representativa de habilidades, magias, passivas e condições.

-   Variedade de inimigos e funções táticas.

-   Um chefe com mecânicas próprias.

-   Arenas reutilizáveis com configurações variáveis.

-   Mapa ramificado com 12 nodes percorridos inicialmente.

-   Combates, elites, chefe, evento, tesouro, mercador e descanso.

-   Loot temporário, consumíveis, inventário e mercadores.

-   XP, níveis, habilidades e equipamentos temporários.

-   Meta progressão horizontal.

-   Morte, recuperação e encerramento da run.

-   Salvamento e retomada integral.

-   Mini dungeon tutorial.

-   Menus, mapa, combate e gerenciamento de personagens.

-   Versão Android funcional.

### 15.4 Fora do MVP

Multiplayer/PvP; servidores/matchmaking; mundo aberto; geração
procedural irrestrita; centenas de habilidades; muitas
classes/subclasses; multiclassing; crafting/economia complexa; campanha
extensa; equipamento permanente/loadout por pontos; meta progressão
avançada; elevação/física/destruição complexas; IA adaptativa avançada;
nuvem até decisão específica.

## 16. ARQUITETURA E DIRETRIZES TÉCNICAS

### 16.1 Princípios

Separar regras e apresentação; orientação a dados; sistemas
independentes; testar regras sem arena visual; evitar dependências
circulares; preparar expansão sem implementar conteúdo futuro; separar
estado permanente e temporário.

### 16.2 Sistemas previstos

  Sistema                  Responsabilidade
  ------------------------ --------------------------------------------------
  CombatManager            Orquestrar estado do combate
  TurnManager              Iniciativa, fila e ciclo de turnos
  ActionEconomy            Validar/consumir ação, bônus, movimento e reação
  DiceService              Rolagens e regras de dados
  CombatResolver           Ataques, dano, acertos, falhas e resultados
  CharacterStats           Atributos, PV, CA, proficiência e recursos
  AbilitySystem            Validar e executar habilidades ativas
  PassiveAbilitySystem     Processar passivas por dados e eventos
  StatusEffectSystem       Aplicar, atualizar e remover condições
  MovementSystem           Movimento, pathfinding e validação
  TargetingSystem          Alvos, alcance e seleção
  LineOfSightSystem        Linha de visão e cobertura
  EnvironmentInteraction   Objetos, superfícies e efeitos ambientais
  RunManager               Estado e fluxo da expedição
  SaveSystem               Persistência, versionamento e restauração
  MetaProgression          Desbloqueios permanentes

### 16.3 Resources previstos

Godot Resources: AbilityData, PassiveAbilityData, EnemyData,
CharacterData, ClassData, ItemData e ConditionData. Novos tipos podem
ser criados conforme necessidade.

### 16.4 Fluxo de ataque

1.  Interface solicita ação.

2.  Targeting valida seleção e alvo.

3.  Alcance e linha de visão validam possibilidade.

4.  ActionEconomy verifica disponibilidade e custo.

5.  CombatResolver coordena a resolução.

6.  DiceService executa rolagens.

7.  Eventos são publicados.

8.  Passivas e ouvintes processam efeitos.

9.  Dano/condições são aplicados.

10. Estado visual e interface são atualizados.

### 16.5 Estrutura de diretórios de referência

res:// ├── project.godot ├── AGENTS.md ├──
assets/{characters,enemies,environments,effects,ui,audio} ├──
scenes/{main,combat,characters,enemies,environment,ui} ├──
scripts/{core,combat,turn_system,movement,abilities,passive_abilities,status_effects,targeting,environment,characters,enemies,inventory,progression,runs,save}
├──
data/{classes,abilities,passive_abilities/{racial,class,subclass,feats,equipment},characters,enemies,items,conditions,encounters}
├── tests/{combat,movement,abilities,passive_abilities,progression} └──
docs/{architecture.md,combat_rules.md,development_log.md}

### 16.6 Cena de combate de referência

CombatScene ├── World │ ├── NavigationRegion2D │ ├── Environment │ └──
InteractiveObjects ├── Units │ ├── PlayerCharacters │ └── Enemies ├──
CombatSystems │ ├── CombatManager │ ├── TurnManager │ ├── MovementSystem
│ ├── TargetingSystem │ ├── PassiveAbilitySystem │ └── EventSystem ├──
Camera └── UI

### 16.7 Diretrizes para agentes de código

-   Trabalhar em escopo pequeno e explícito.

-   Analisar arquitetura e dependências antes de alterar arquivos.

-   Planejar mudanças relevantes.

-   Separar lógica de regras e visual.

-   Não duplicar sistemas nem criar exceções sem justificativa.

-   Usar Resources/dados para conteúdo configurável.

-   Implementar passivas por dados e eventos.

-   Adicionar testes para regras críticas.

-   Executar o projeto no Godot e verificar erros.

-   Atualizar documentação e registrar limitações.

-   Informar arquivos alterados, testes e resultados.

-   Fazer commit somente após validação.

## 17. BACKLOG MACRO DO PROTÓTIPO

  -----------------------------------------------------------------------
  Fase                    Objetivo                Validação
  ----------------------- ----------------------- -----------------------
  0 --- Fundação          Godot 4, Git,           Projeto abre, estrutura
                          estrutura, AGENTS.md,   e documentação existem
                          cena principal,         
                          convenções e eventos    

  1 --- Arena/movimento   Arena, câmera iso,      Unidades se movem e
                          personagem, seleção,    respeitam obstáculos
                          NavigationAgent2D,      
                          obstáculos, colisões,   
                          alcance e preview       

  2 --- Turnos            Unidades, iniciativa,   Ordem funciona de forma
                          fila, início/fim e      reproduzível
                          avanço                  

  3 --- Economia          Ação, bônus, movimento, Ações respeitam
                          reação e restauração    recursos e ciclo

  4 --- Combate           Stats, DiceService,     Ataques resolvidos
                          d20, ataque vs CA,      corretamente
                          dano, PV, crítico,      
                          vantagem/desvantagem,   
                          log e eventos           

  5 ---                   Alvos, alcance, visão,  Habilidades funcionam
  Habilidades/passivas    ataques à distância,    por dados
                          magia, resistência, CD  
                          e passivas              

  6 ---                   Condições, empurrão,    Interações funcionam
  Condições/ambiente      colisão, objetos e      
                          feedback                

  7 --- Morte/recuperação 0 PV, testes,           Fluxo de morte funciona
                          estabilização,          
                          recuperação e derrota   

  8 --- UI mobile         HUD, iniciativa, ações, Combate jogável no
                          PV/CA, seleção, preview dispositivo-alvo
                          e log                   

  9 --- Integração        3 personagens, 3        Protótipo estável e
                          inimigos, classes,      validado
                          passivas, arena, testes 
                          e Android               
  -----------------------------------------------------------------------

As fases são marcos, não comandos para execução em lote. Cada uma deve
ser dividida em tarefas pequenas, testadas e aprovadas antes da próxima.

## 18. PENDÊNCIAS CONSOLIDADAS

  -----------------------------------------------------------------------
  Área                                Pendências
  ----------------------------------- -----------------------------------
  Combate                             Desempate de iniciativa; escala de
                                      distância; dimensões de arena;
                                      ataques múltiplos; prioridade de
                                      reações; detalhes de críticos

  Magia                               Progressão de espaços; catálogo;
                                      recuperação de recursos

  Progressão                          Cap por run; tabela de XP;
                                      proficiência definitiva

  Personagens                         Valores de bônus raciais; passivas
                                      raciais; atributos por classe;
                                      respec após salvar

  Classes                             Progressão por nível e
                                      habilidades/características

  Condições                           Duração, acumulação, resistência,
                                      imunidade e remoção

  Descanso                            PV, dados de vida, slots, recursos
                                      e cargas definitivas

  Morte                               Fontes, custos e limites de
                                      ressurreição

  Equipamentos                        Catálogo, atributos, efeitos,
                                      raridades, probabilidades e preços

  Eventos                             Catálogo, CDs, resultados e
                                      consequências

  Expedição                           Geração do mapa, número final de
                                      nodes e mescla de regiões

  Inimigos                            Estatísticas, catálogo, loot e
                                      encontros

  Chefes                              Mecânicas completas do chefe
                                      inicial

  Salvamento                          Backup, corrupção, nuvem e
                                      múltiplos dispositivos

  Interface                           Wireframes, hierarquia, tamanhos e
                                      adaptação de telas

  Áudio                               Catálogo de efeitos, mixagem e
                                      implementação

  Produto                             Modelo comercial e distribuição

  Online                              Multiplayer/PvP e servidor, fora do
                                      MVP
  -----------------------------------------------------------------------

## 19. CRITÉRIOS DE SUCESSO DO PROTÓTIPO

-   Seleção, planejamento e movimento respeitam obstáculos.

-   Fila individual de iniciativa avança corretamente.

-   Economia de ações bloqueia gastos inválidos e restaura recursos no
    momento correto.

-   Rolagens, ataques, CA, dano, críticos e PV produzem resultados
    verificáveis.

-   Uma habilidade com resistência e uma passiva reativa demonstram
    extensibilidade.

-   IA toma decisões válidas e respeita turno e alcance.

-   Vitória, derrota e reinício funcionam.

-   Interface permite entender unidade ativa, ações disponíveis e
    consequências.

-   Protótipo executa no dispositivo-alvo com desempenho aceitável.

-   Sistemas críticos têm testes automatizados ou validação
    reproduzível.

## 20. DIRETRIZES DE PRODUÇÃO

-   Priorizar combate e loop roguelite; não expandir conteúdo antes de
    validar diversão e legibilidade.

-   Arquitetura expansível não significa implementar antecipadamente
    todos os sistemas futuros.

-   Não pedir a agentes a implementação de uma fase inteira em um único
    comando.

-   Cada tarefa deve ter objetivo, escopo, critérios de aceite e testes.

-   Revisar escopo quando uma funcionalidade elevar significativamente a
    complexidade.

-   Registrar decisões novas com status e atualizar arquitetura/backlog
    quando houver impacto.

-   Manter provisórios identificados até validação.

-   Usar placeholders quando acelerarem os testes.

-   Validar uma arena pequena antes de construir expedições completas.

## 21. PROPRIEDADE INTELECTUAL E REFERÊNCIAS LEGAIS

A intenção é utilizar regras e conceitos compatíveis com SRD de D&D sob
Creative Commons Attribution 4.0 (CC BY 4.0), após conferência da versão
e dos termos aplicáveis. SRD aberto não concede direito geral de uso do
universo Forgotten Realms, personagens, cenários, textos, ilustrações,
marcas ou conteúdos proprietários de D&D/Baldur's Gate.

O jogo deverá desenvolver nomes, personagens, facções, mundo, narrativa,
arte, textos e identidade próprios. Antes de lançamento comercial,
revisar atribuições obrigatórias, licença, marcas, assets de terceiros e
restrições de distribuição. Esta seção é uma diretriz de cautela, não um
parecer jurídico.

## 22. PRÓXIMOS PASSOS

1.  Revisar este GDD e corrigir divergências de interpretação.

2.  Atualizar arquitetura técnica e backlog com as decisões
    consolidadas.

3.  Definir critérios de aceite da Fase 0 e preparar repositório Godot.

4.  Implementar a fundação técnica.

5.  Construir arena e movimentação antes de conteúdo extenso.

6.  Validar cada sistema em pequenas entregas até alcançar o protótipo
    jogável.

7.  Após validar combate, adicionar progressivamente loop de expedição e
    sistemas do MVP.

Este GDD é a referência de design do projeto. Decisões futuras devem ser
registradas com status e data, preservando histórico suficiente para
compreender alterações de regras e escopo.
