# Slotbound Evolution — Handoff Completo para Continuidade

> Documento de transferência de contexto para uma nova sessão do ChatGPT.  
> Objetivo: continuar o desenvolvimento de Slotbound Evolution sem perder decisões, regras, hipóteses, resultados de simulação ou o próximo passo operacional.  
> Idioma de trabalho: português brasileiro.  
> Repositório: https://github.com/HexerVoodoom/bound_evolution  
> Última consolidação: 26/11/2026.

---

## 0. Instruções para a próxima sessão

Leia este documento integralmente antes de propor mudanças ou pedir informações ao usuário.

### Como conduzir o trabalho

1. Continue do estado descrito neste documento. Não reinicie o processo de concepção.
2. Preserve as decisões marcadas como **CONFIRMADAS**.
3. Trate itens marcados como **PROPOSTA**, **HIPÓTESE**, **PROVISÓRIO** ou **EM ABERTO** como não definitivos.
4. Não invente que o usuário instalou Godot, criou um projeto ou implementou scripts. Na data deste documento, nada disso foi feito.
5. O usuário quer avançar em etapas pequenas, confirmando cada etapa com mensagens como “ok, pode avançar”.
6. Explique procedimentos técnicos de forma prática, com passos curtos e verificáveis. Não despeje todo o código de uma vez.
7. Quando houver código, informe o caminho exato do arquivo, seu conteúdo completo e como testar no Godot.
8. Antes de modificar arquivos do repositório, inspecione o estado atual. Não sobrescreva conteúdo existente sem verificar.
9. Separe regras do jogo, apresentação visual e infraestrutura. A lógica de gameplay não deve depender da UI ou das animações.
10. Não trate resultados de simulações artificiais como prova de balanceamento real.

### Primeira ação da nova sessão

O usuário ainda não instalou o Godot. Portanto, retome pela instalação no Windows, usando a página oficial:

- https://godotengine.org/download/windows/
- Selecionar Godot 4.x Stable, Standard, Windows x86_64.
- Não selecionar .NET/C#.
- Extrair o ZIP para um local permanente, por exemplo `C:\Godot\`, e executar o .exe.
- Não instalar Visual Studio, .NET SDK, Android Studio ou export templates neste momento.
- Depois que o Project Manager abrir, orientar a criação do projeto `SlotboundEvolution`.

**Atenção à versão:** em conversa anterior foi sugerido Godot 4.7.2 Stable, mas a versão disponível no momento da instalação deve ser conferida na página oficial. Não assumir que esse número continua sendo o release estável mais recente.

---

## 1. Estado atual do repositório e do projeto

### Repositório

O usuário forneceu:

https://github.com/HexerVoodoom/bound_evolution

A consulta ao GitHub em 26/11/2026 confirmou:

- Repositório público: `HexerVoodoom/bound_evolution`.
- Branch padrão declarada: `main`.
- Repositório recém-criado.
- Tamanho reportado: 0.
- A API retornou que o repositório está vazio, sem conteúdo/commit inicial.
- Nenhum código-fonte do jogo foi inspecionado porque ainda não há arquivos.

Este documento é o primeiro artefato de contexto adicionado ao repositório. Não inferir que existe um projeto Godot nele.

### Estado local do usuário

- Sistema operacional: Windows.
- Godot: **ainda não instalado**.
- Projeto Godot: **ainda não criado**.
- Scripts: **nenhum implementado**.
- Machine Core: especificado conceitualmente, mas ainda não implementado.
- O usuário está pronto para começar pela instalação do Godot.

### Ferramenta e stack escolhidas

**CONFIRMADAS:**
- Engine: Godot 4.x.
- Linguagem: GDScript.
- Renderizador: Compatibility.
- Plataforma inicial: PC.
- Distribuição pretendida: Steam.
- Versão Standard do Godot, sem C#/.NET.

**PROPOSTAS DE CONFIGURAÇÃO:**
- Resolução-base: 1280×720.
- Orientação: paisagem.
- Stretch Mode: `canvas_items`.
- Aspect: `keep`.
- VSync ligado inicialmente.
- Alvo inicial: 60 FPS.

Essas configurações podem ser ajustadas se testes de UI ou desempenho justificarem.

---

## 2. Conceito do jogo

### Visão geral

Slotbound Evolution é um roguelite 2D de fantasia arcana no qual o jogador constrói e reprograma uma máquina 3×3 que invoca criaturas, executa efeitos de combate e manipula probabilidades.

A máquina é simultaneamente:
- Invocador de criaturas.
- Arsenal de efeitos.
- Catalisador de combinações.
- Sistema de decisão probabilística.

O combate ocorre em ondas. O jogador interfere nos resultados da máquina preservando símbolos e construindo combinações.

### Fantasia central

> “Você não controla a sorte. Você constrói o mecanismo que ensina a sorte a trabalhar a seu favor.”

### Pilares de design

1. **Reprogramar a sorte:** o jogador manipula probabilidades e preserva símbolos.
2. **Construir um exército:** os símbolos invocam e fortalecem criaturas.
3. **Desencadear reações:** combinações de símbolos geram efeitos maiores e sinergias.

### Identidade

- Universo arcano.
- Combate híbrido.
- Equilíbrio entre máquina, criaturas e estratégia.
- A sorte importa, mas o jogador tem ferramentas para influenciá-la.
- A equipe de criaturas é limitada e ocupa posições fixas.

---

## 3. Decisões de design confirmadas

Os itens abaixo foram confirmados pelo usuário ao longo da concepção:

| Tema | Decisão confirmada |
|---|---|
| Tipo de combate | Híbrido |
| Universo | Arcano |
| Foco | Equilíbrio entre máquina, criaturas e estratégia |
| Estrutura de combate | Ondas |
| Giros | RNG + intervenção do jogador |
| Exército | Limitado |
| Equipe na arena | Slots fixos |
| Recuperação | Parcial |
| Símbolos | Família funcional + afinidade elemental |
| Máquina | Grade 3×3 |
| Capacidade padrão de travas | 2, provisoriamente recomendada por simulação |
| Equipe máxima | 3 criaturas |
| Elementos do primeiro protótipo | Fogo, Água e Terra |

### Interpretação operacional

- A máquina possui nove células.
- Um giro sorteia simultaneamente todas as células desbloqueadas.
- Células travadas preservam símbolo e posição.
- A equipe comporta até três criaturas.
- A arena possui três posições fixas: esquerda, centro e direita.
- Criaturas persistem entre rodadas e ondas, respeitando as regras de recuperação.
- Os símbolos têm duas dimensões: família funcional e elemento.

---

## 4. Símbolos e distribuição probabilística

### Famílias funcionais

1. **Creature:** invocação e evolução de criaturas.
2. **Attack:** dano e efeitos ofensivos.
3. **Support:** cura, proteção e buffs.

### Elementos iniciais

- **Fire:** ofensiva, queimadura e buffs de dano.
- **Water:** cura, recuperação e controle.
- **Earth:** defesa, resistência e proteção.

A afinidade elemental modifica o efeito, mas não substitui a função principal da família.

### Os nove símbolos

| Família | Fogo | Água | Terra | Peso por símbolo |
|---|---|---|---|---:|
| Creature | Creature Fire | Creature Water | Creature Earth | 25 |
| Attack | Attack Fire | Attack Water | Attack Earth | 40 |
| Support | Support Fire | Support Water | Support Earth | 35 |

Total de pesos: 300.

Distribuição pretendida:
- Creature: 25%.
- Attack: 40%.
- Support: 35%.

Cada elemento tem probabilidade igual dentro de sua família:
- Creature: 25/3? Não. Cada símbolo tem peso 25, portanto cada um representa 8,33% do total.
- Attack: cada símbolo tem peso 40, portanto cada um representa 13,33% do total.
- Support: cada símbolo tem peso 35, portanto cada um representa 11,67% do total.

**Correção importante:** a implementação deve usar pesos por símbolo de 25, 40 e 35 respectivamente, totalizando 300. Não usar pesos 3/5/4, pois isso gera 25% / 41,67% / 33,33%, divergindo da intenção.

Cada célula desbloqueada deve ser sorteada de forma independente, com os mesmos pesos, sem correção artificial para produzir combinações.

---

## 5. Máquina e sistema de travas

### Máquina

- Grade 3×3.
- Nove células indexadas de 0 a 8.
- Organização:

```text
0 1 2
3 4 5
6 7 8
```

Conversão:
- Linha = índice / 3.
- Coluna = índice % 3.

Cada célula contém:
- `index`
- `symbol`
- `is_locked`
- `is_active`

### Travas

**CONFIRMADO / BASELINE:**
- Capacidade padrão recomendada: 2.
- Capacidade configurável para testes: 1, 2 ou 3.
- Trava preserva símbolo e posição.
- Células travadas não participam do novo sorteio.
- Células desbloqueadas recebem novos símbolos.

**REGRAS OPERACIONAIS PROPOSTAS:**
- Não travar célula vazia.
- Não ultrapassar capacidade.
- Após cada giro, liberar automaticamente as travas para que o jogador reavalie a grade.
- Uma célula não pode ser travada duas vezes.
- O limite deve ser controlado por configuração, não hardcoded.

A liberação automática após cada giro foi proposta como comportamento do primeiro protótipo, mas não deve ser tratada como decisão final sem validação do usuário.

### Ordem dos sorteios

Para reprodutibilidade, processar as células sempre na ordem de índice 0 a 8. Somente células ativas e desbloqueadas recebem novo símbolo.

---

## 6. Combinações: configuração intermediária C

Houve versões anteriores de regras. A configuração mais recente da especificação de protótipo é a **Configuração Intermediária C**. Não misturar com as regras antigas.

### Regras atuais do protótipo

1. **Trincas de família:** somente horizontais; três símbolos da mesma família, elementos podem ser diferentes.
2. **Combinação mista:** Creature + Support do mesmo elemento, adjacentes ortogonalmente.
3. **Vapor:** Attack Fire + Support Water, adjacentes ortogonalmente.
4. Prioridade de resolução:
   - Trincas.
   - Combinações mistas.
   - Vapor.
   - Efeitos individuais.
5. Cada símbolo pode ser consumido no máximo uma vez por giro.
6. Não há cascatas nesta versão.

### Histórico de versões anteriores

- O GDD inicial considerava trincas horizontais, verticais e diagonais.
- Uma versão mais restritiva exigia três símbolos idênticos e adjacência horizontal para combinações mistas/Vapor.
- Simulações mostraram que regras muito permissivas faziam quase todo giro gerar combos; regras muito restritivas reduziam demais as trincas.
- A configuração C foi escolhida para testes, mas precisa ser validada com jogadores e protótipo funcional.

---

## 7. Efeitos de gameplay propostos

Os valores abaixo são parâmetros experimentais da Prototype Specification, não balanceamento final.

### Efeitos individuais

**Creature**
- Invoca uma criatura.

**Attack**
- Dano-base: 6 no inimigo prioritário.
- Fire: aplica 2 de queimadura no fim da próxima rodada inimiga.
- Water: reduz em 25% o próximo ataque do inimigo.
- Earth: reduz defesa do alvo em 2 por uma rodada.

**Support**
- Fire: +2 de dano ao próximo ataque.
- Water: cura 5 HP.
- Earth: concede escudo 4.

### Trincas

**Creature Trine**
- Invoca criatura do elemento dominante.
- +5 HP máximo para a criatura durante a onda.

**Attack Trine**
- 24 de dano ao alvo prioritário.
- Aplica efeito do elemento dominante.

**Support Trine**
- Cura 12 e concede escudo 6 ao aliado com menor porcentagem de HP.

### Combinação mista Creature + Support

- Invoca uma criatura.
- Aplica o efeito Support à criatura recém-invocada.

### Vapor

- 4 de dano a todos os inimigos.
- Reduz em 25% o próximo ataque dos inimigos atingidos.

### Criaturas

Valores-base propostos:
- 20 HP.
- 4 de ataque.
- Um ataque por rodada.

Regras:
- Invocação repetida da mesma criatura cura a criatura existente em 5 HP.
- Se equipe estiver cheia e não houver criatura correspondente, regra de overflow ainda está em aberto.
- Equipe máxima de três slots fixos.
- Unidades incapacitated permanecem no slot durante a onda, não agem e não são revividas automaticamente.

### Alvos

- Inimigo prioritário: inimigo vivo com menor HP atual.
- Aliado prioritário: aliado vivo com menor porcentagem de HP.
- Escudo absorve dano antes do HP.
- Escudos expiram no fim da onda.
- Escudos não acumulam acima do maior valor vigente.

### Ondas e recuperação

- Primeira versão de teste: três ondas em uma especificação antiga; o Vertical Slice atual foi reduzido para duas ondas.
- Cada onda inicial contém dois inimigos.
- Entre ondas: criaturas vivas recuperam 25% do HP máximo.
- Status e escudos são removidos entre ondas.
- Criaturas incapacitated não são revividas na recuperação.

### Inimigos ilustrativos

| Inimigo | HP | Dano |
|---|---:|---:|
| Vigia | 18 | 3 |
| Brutamonte | 28 | 5 |
| Rastejante | 14 | 2 |

Uma versão posterior do protótipo de batalha usou Vigia com 24 HP e Brutamonte com 34 HP. Esses números eram apenas experimentais e não substituem os valores da especificação original.

---

## 8. Simulações realizadas e interpretação

Os resultados abaixo são históricos e devem ser tratados como evidência exploratória, não como garantia de balanceamento.

### Simulation Report v0.1

Foram executados 1 milhão de giros por configuração.

Com regras permissivas:
- 65,82% dos giros tiveram pelo menos uma trinca de família.
- 72,73% tiveram combinação mista resolvida.
- 50,58% tiveram Vapor.
- 99,74% tiveram pelo menos uma combinação.
- Média de 5,55 símbolos consumidos por giro.

Conclusão: configuração permissiva demais; efeitos individuais perdem relevância.

Com regras mais restritivas:
- Trinca exigia símbolos idênticos.
- Combinação mista exigia mesmo elemento e adjacência horizontal.
- Vapor exigia Attack Fire + Support Water horizontalmente adjacentes.
- 48,79% dos giros tinham algum combo.
- Média de 1,28 símbolos consumidos.
- Trincas caíram para 10,37%.

Conclusão: configuração possivelmente restritiva demais. Isso motivou a Configuração C.

### Strategic Lock Simulation v0.1

Políticas comparadas:
- Sem travas.
- Travas aleatórias.
- Busca por combos.
- Trava tática sensível ao estado da batalha.

Na dificuldade-base, todas venceram 100%, indicando que o modelo estava fácil demais.

No stress test artificial (HP inimigo ×2,5 e dano inimigo ×1,5):
- Sem trava: 62,30%.
- Aleatória: 65,87%.
- Busca por combo: 70,93%.
- Tática: 87,36%.

Interpretação: retenção contextual parece importar, mas isso não prova que jogadores humanos terão resultados equivalentes. O agente tático tinha acesso direto ao estado.

### Lock Capacity Simulation v0.1

Comparou capacidade 0, 1, 2 e 3, usando a mesma heurística tática e 10 mil simulações por configuração.

Stress test artificial: HP inimigo ×4 e dano inimigo ×2.

- 0 travas: 88,34%.
- 1 trava: 93,66%.
- 2 travas: 94,03%.
- 3 travas: 92,99%.

Diferença entre 1 e 2 travas: apenas 0,37 ponto percentual. Três travas não trouxeram ganho. Duas travas são recomendação provisória, não decisão definitiva.

### Relatórios previamente gerados em conversas anteriores

- `slotbound_evolution_gdd_v0_2.md`
- `slotbound_evolution_prototype_spec_v0_1.md`
- `slotbound_evolution_simulation_report_v0_1.md`
- `slotbound_evolution_strategic_lock_simulation_v0_1.md`
- `slotbound_evolution_lock_capacity_simulation_v0_1.md`

Esses arquivos foram gerados em conversas anteriores e não estão confirmados como presentes no repositório atual. Não afirmar que estão no GitHub sem verificar.

---

## 9. Protótipos interativos anteriores

### Machine Core prototype

Foi criado um protótipo interativo na conversa com:
- Grade 3×3.
- Nove símbolos.
- Capacidade de trava 1/2/3.
- Giro com seed.
- Resolução de combinações e log de eventos.
- Sem batalha.
- Pesos 25/40/35.
- Trincas horizontais, combinação mista com mesmo elemento adjacente e Vapor adjacente.

Estado observado numa interação anterior:
- Giro 11.
- Capacidade 3.
- Travas nos índices 8, 5 e 2.
- Duas combinações Creature + Support foram ativadas.

Não assumir que o usuário aprovou formalmente o comportamento ou que o protótipo está no repositório.

### Machine + Battle prototype

Outro protótipo interativo incluía:
- Guardian Earth inicial (20 HP, 4 ATK).
- Vigia (24 HP) e Brutamonte (34 HP).
- Grade 3×3.
- Capacidade 1/2/3.
- Giro, combos e combate automático.
- Uma batalha, sem loop completo de ondas.

Limitações conhecidas:
- Support Earth curava +2 em vez de conceder escudo.
- Vapor causava somente 4 de dano, sem redução de ataque.
- Fire buff aumentava ataque permanentemente, em vez de afetar apenas o próximo ataque.
- Efeitos passivos de criaturas não estavam implementados.
- Trinca Support curava sem conceder escudo.
- Combinação mista buffava a primeira criatura viva, não necessariamente a recém-invocada.
- Não havia sistema de status completo.
- Não havia loop de múltiplas ondas nem recuperação.
- Não havia harness completo de testes.
- A reprodutibilidade da seed era simplificada.

Esses protótipos eram exploratórios. Não os tratar como implementação de referência.

---

## 10. Arquitetura técnica escolhida

### Princípio central

A lógica do jogo deve funcionar independentemente da interface, animações e efeitos visuais.

A UI observa eventos e apresenta resultados. Ela não decide regras de combate.

### Camadas

1. **Presentation Layer:** telas, HUD, animações, feedback e controles.
2. **Game Flow Layer:** Run Manager, Wave Manager e Battle Controller.
3. **Gameplay Systems:** Machine, Pattern Detection, Combination Resolver, Effects e Creatures.
4. **Domain Data:** símbolos, criaturas, inimigos, ondas e regras.
5. **Infrastructure:** save, RNG, configurações, logs e testes.

### Sistemas centrais

| Sistema | Responsabilidade |
|---|---|
| MachineController | Orquestrar grade, travas e giros |
| SymbolGenerator | Sorteio ponderado |
| PatternDetector | Identificar padrões espaciais |
| CombinationResolver | Resolver combinações e prioridades |
| EffectDispatcher | Executar efeitos |
| CreatureManager | Equipe e estados das criaturas |
| BattleController | Turnos e combate |
| EnemyAI | Ações dos inimigos |
| WaveManager | Ondas e recuperação |
| RunManager | Partida completa |
| SeededRNG | Aleatoriedade reproduzível |
| CombatLog | Registro dos acontecimentos |

### Estrutura de pastas de destino

```text
SlotboundEvolution/
├── project.godot
├── assets/
│   ├── fonts/
│   ├── icons/
│   ├── textures/
│   ├── audio/
│   └── shaders/
├── scenes/
│   ├── main/
│   ├── machine/
│   ├── battle/
│   ├── creatures/
│   └── enemies/
├── scripts/
│   ├── core/
│   ├── machine/
│   ├── combat/
│   ├── creatures/
│   ├── run/
│   └── infrastructure/
├── resources/
│   ├── symbols/
│   ├── creatures/
│   ├── enemies/
│   └── waves/
├── ui/
│   ├── components/
│   ├── screens/
│   └── themes/
└── tests/
    ├── machine/
    ├── combinations/
    ├── combat/
    └── run/
```

Não criar todas as pastas antecipadamente sem necessidade. A estrutura é um mapa de destino.

### Cena principal proposta

```text
Main
└── MainController
    ├── ScreenContainer
    │   ├── MainMenu
    │   └── BattleScreen
    │       ├── BattleHUD
    │       ├── EnemyArea
    │       ├── CreatureArea
    │       ├── MachineArea
    │       └── CombatLog
    └── TransitionLayer
```

Para o Vertical Slice, a navegação será:
Main → BattleScreen → Wave 1 → Wave Result → Recovery → Wave 2 → Victory/Defeat.

### Autoloads propostos

Usar poucos autoloads:
- EventBus: sinais globais de acontecimentos.
- GameManager: estado geral e transições.
- RunManager: dados da partida atual.

O RNG deve ser um serviço independente/injetado, não necessariamente autoload.

### Eventos propostos

- `spin_started`
- `spin_completed(results)`
- `combination_detected(combination)`
- `effect_applied(effect)`
- `creature_summoned(creature_id)`
- `creature_damaged(creature_id, amount)`
- `enemy_defeated(enemy_id)`
- `wave_completed(wave_number)`
- `battle_won`
- `battle_lost`

Usar sinais para notificar acontecimentos; não substituir chamadas diretas quando um sistema precisa de retorno imediato.

---

## 11. Dados estáticos e estados dinâmicos

Separar configurações de execução.

### Recursos estáticos (Godot Resource)

- `SymbolData`
- `CreatureData`
- `EnemyData`
- `WaveData`

### Estados dinâmicos

- `MachineState`
- `CreatureState`
- `EnemyState`
- `BattleState`

Exemplo inicial de SymbolData:

```gdscript
class_name SymbolData
extends Resource

enum Family {
    CREATURE,
    ATTACK,
    SUPPORT
}

enum Element {
    FIRE,
    WATER,
    EARTH
}

@export var id: StringName
@export var family: Family
@export var element: Element
@export_range(0, 1000) var weight: int = 25
@export var icon: Texture2D
```

Exemplo inicial de CreatureState:

```gdscript
class_name CreatureState
extends RefCounted

var creature_id: StringName
var current_hp: int
var max_hp: int
var attack: int
var shield: int = 0
var is_incapacitated: bool = false

func is_alive() -> bool:
    return current_hp > 0
```

Os Resources são configurações originais e não devem ser mutados durante o combate. O estado dinâmico armazena HP atual, escudo, status e outros valores temporários.

---

## 12. Machine Core Technical Specification

O próximo módulo a implementar é o Machine Core.

### Componentes

1. `MachineCell`: célula individual.
2. `MachineState`: grade, capacidade de trava e contador de giros.
3. `SymbolGenerator`: sorteio ponderado.
4. `LockManager`: valida, ativa e libera travas.
5. `MachineController`: orquestra o giro.
6. `SeededRNG`: controla sequência aleatória.

### Exemplo de MachineCell

```gdscript
class_name MachineCell
extends RefCounted

var index: int = -1
var symbol: SymbolData
var is_locked: bool = false
var is_active: bool = true
```

### Exemplo de MachineState

```gdscript
class_name MachineState
extends RefCounted

var cells: Array[MachineCell] = []
var lock_capacity: int = 2
var spin_count: int = 0

func get_locked_count() -> int:
    var count := 0

    for cell in cells:
        if cell.is_locked:
            count += 1

    return count
```

### RNG reproduzível

Exemplo de esqueleto:

```gdscript
class_name SeededRNG
extends RefCounted

var rng := RandomNumberGenerator.new()

func set_seed(value: int) -> void:
    rng.seed = value

func random_range_int(min_value: int, max_value: int) -> int:
    return rng.randi_range(min_value, max_value)

func get_seed() -> int:
    return rng.seed
```

A sequência é reproduzível se seed, configuração e ordem de chamadas forem iguais. Processar células em ordem de índice para manter determinismo.

### Sorteio ponderado

```gdscript
func get_weighted_symbol(symbols: Array[SymbolData]) -> SymbolData:
    var total_weight := 0

    for symbol in symbols:
        total_weight += symbol.weight

    if symbols.is_empty() or total_weight <= 0:
        return null

    var roll := rng.randi_range(1, total_weight)
    var accumulated := 0

    for symbol in symbols:
        accumulated += symbol.weight

        if roll <= accumulated:
            return symbol

    return null
```

A implementação real deverá validar:
- Lista não vazia.
- Nenhum peso negativo.
- Peso total maior que zero.
- Recursos válidos.
- Erros tratados explicitamente.

### Regras de giro

1. Validar que a máquina pode girar.
2. Registrar o estado anterior.
3. Processar índices de 0 a 8.
4. Ignorar células inativas.
5. Preservar células travadas.
6. Gerar novo símbolo nas células ativas desbloqueadas.
7. Incrementar contador.
8. Capturar resultado imutável do giro.
9. Liberar travas, se o comportamento provisório for mantido.
10. Emitir resultado.

O Controller não deve detectar combos nem aplicar efeitos de batalha.

### Histórico

Cada giro deverá registrar:
- Número do giro.
- Seed.
- Grade anterior.
- Grade resultante.
- Células travadas.
- Células geradas.

### Testes do Machine Core

| ID | Teste | Critério |
|---|---|---|
| MC-001 | Criar grade | 9 células |
| MC-002 | Índices | 0 a 8 sem duplicação |
| MC-003 | Gerar símbolos | Nenhuma célula ativa vazia após giro |
| MC-004 | Distribuição | Frequências próximas aos pesos |
| MC-005 | Travar célula | Símbolo e posição preservados |
| MC-006 | Limite | Capacidade não excedida |
| MC-007 | Destravar | Célula pode receber novo símbolo |
| MC-008 | Seed igual | Mesma sequência |
| MC-009 | Seed diferente | Sequências reproduzíveis e distintas |
| MC-010 | Histórico | Registro correto |
| MC-011 | Célula inativa | Não participa do sorteio |
| MC-012 | Dados inválidos | Erro tratado |

Executar pelo menos 100.000 sorteios por configuração para avaliação estatística. A distribuição não deve ser artificialmente corrigida durante o jogo.

---

## 13. Roadmap do Vertical Slice 01

### Objetivo

Construir uma fatia jogável que valide o núcleo de máquina, criaturas e combate, sem implementar sistemas de metaprogressão.

### Escopo

- Máquina 3×3.
- Símbolos e pesos.
- Travas.
- Combinações.
- Até três criaturas.
- Dois inimigos por onda.
- Combate automático.
- Vitória/derrota.
- Duas ondas.
- Recuperação parcial entre ondas.
- Testes automatizados.

### Fora do escopo

- Relíquias.
- Evolução de criaturas.
- Loja.
- Chefes.
- Metaprogressão.
- Save completo.
- Conteúdo de campanha.
- Arte final e animações complexas.

### Etapas

1. **Fundação do projeto:** Godot, configuração, pastas, autoloads, EventBus, dados e testes iniciais.
2. **Machine Core:** grade, símbolos, probabilidades, travas e RNG.
3. **Combination Engine:** detector, prioridades, consumo de símbolos e efeitos.
4. **Combat Core:** criaturas, inimigos, HP, dano, escudos, status e turnos.
5. **Run Controller:** duas ondas, recuperação, vitória e derrota.
6. **Vertical Slice UI:** interface integrada, feedback visual e validação completa.

### Fluxo de combate

```text
PLAYER INPUT
    ↓
VALIDATE LOCKS
    ↓
GENERATE SYMBOLS
    ↓
DETECT PATTERNS
    ↓
RESOLVE COMBINATIONS
    ↓
DISPATCH EFFECTS
    ↓
CREATURE ACTIONS
    ↓
REMOVE DEFEATED ENEMIES
    ↓
ENEMY ACTIONS
    ↓
UPDATE STATUS EFFECTS
    ↓
CHECK BATTLE STATE
    ↓
NEXT ROUND / VICTORY / DEFEAT
```

### Critérios de conclusão do Vertical Slice

- [ ] O jogador consegue girar e travar até duas células.
- [ ] Probabilidades respeitam a distribuição configurada.
- [ ] Combinações são detectadas na prioridade correta.
- [ ] Efeitos são aplicados corretamente.
- [ ] Criaturas podem ser invocadas, receber dano e ficar incapacitadas.
- [ ] Inimigos executam ações na ordem correta.
- [ ] A primeira onda pode ser vencida.
- [ ] A equipe recebe recuperação parcial.
- [ ] A segunda onda pode ser vencida ou causar derrota.
- [ ] Uma seed reproduz os mesmos resultados.
- [ ] Testes automatizados passam.

---

## 14. Riscos e questões em aberto

Não decidir silenciosamente os seguintes pontos:

1. A trava deve ser liberada automaticamente após cada giro ou permanecer até o jogador destravá-la?
2. Como funciona o overflow de invocação quando a equipe está cheia e a criatura invocada não corresponde a nenhuma existente?
3. Uma criatura incapacitada pode ser substituída durante a mesma onda?
4. As trincas horizontais devem continuar sendo a única orientação no jogo final?
5. A resolução das combinações deve permitir que um símbolo participe de mais de um padrão? A regra atual diz não, cada símbolo é consumido uma vez.
6. A prioridade de padrões precisa de desempate formal quando há múltiplos combos concorrentes.
7. Como calcular elemento dominante em uma trinca com três afinidades diferentes?
8. Como acumular, substituir ou renovar queimadura, redução de ataque, buffs e debuffs?
9. O escudo deve expirar no fim de cada onda ou ter exceções por criatura/reliquia?
10. A recuperação parcial deve ser 25% do HP máximo em todas as situações? Valor provisório.
11. Como será o targeting quando todos os aliados estiverem incapacitados?
12. Como lidar com uma combinação que invoca criatura quando não há slot disponível?
13. Como o jogo apresenta a informação de probabilidade ao jogador?
14. A capacidade de travas poderá evoluir durante uma run ou será fixa por configuração?

### Correções de implementação importantes

O protótipo de batalha anterior possuía diferenças em relação à especificação. Não copiar essas simplificações como regras oficiais:
- Support Earth precisa ser escudo, não cura.
- Vapor precisa incluir redução de ataque.
- Fire buff deve afetar o próximo ataque, não permanentemente.
- Trinca Support deve curar e conceder escudo.
- Combinação mista deve aplicar suporte à criatura recém-invocada.
- Status precisam de duração e resolução explícitas.

---

## 15. Próxima sessão: roteiro operacional detalhado

### Passo 1 — Instalar Godot

1. Abrir https://godotengine.org/download/windows/.
2. Selecionar versão estável Godot 4.x, Standard, Windows x86_64.
3. Não selecionar .NET.
4. Extrair o ZIP em uma pasta permanente, como `C:\Godot\`.
5. Executar o Godot.
6. Confirmar que o Project Manager abriu.

Não instalar ferramentas adicionais nesta etapa.

### Passo 2 — Criar projeto

No Project Manager:
1. New Project.
2. Nome: `SlotboundEvolution`.
3. Escolher diretório de projeto.
4. Renderer: Compatibility.
5. Criar e abrir.

Depois configurar a resolução-base e stretch conforme as recomendações deste documento.

### Passo 3 — Criar a estrutura mínima

Criar apenas:
- `scenes/main/`
- `scenes/machine/`
- `scripts/core/`
- `scripts/machine/`
- `scripts/infrastructure/`
- `resources/symbols/`
- `tests/machine/`

Expandir a estrutura quando cada sistema for implementado.

### Passo 4 — Implementar dados e RNG

Criar:
- `SymbolData.gd`
- Nove arquivos Resource de símbolos.
- `SeededRNG.gd`
- Testes para pesos e seed.

Validar a distribuição antes de avançar.

### Passo 5 — Implementar estado e travas

Criar:
- `MachineCell.gd`
- `MachineState.gd`
- `LockManager.gd`
- Testes de preservação de símbolos e limite de travas.

### Passo 6 — Implementar Controller e UI mínima

Criar:
- `SymbolGenerator.gd`
- `MachineController.gd`
- Cena de máquina.
- Grade 3×3 clicável.
- Botão Spin.
- Indicador de travas.
- Exibição do número do giro e seed.

A interface deve ser simples e provisória. Não investir em arte final.

### Primeiro marco jogável

Considerar o primeiro marco atingido quando:
- A grade mostra nove símbolos.
- O jogador consegue selecionar até duas células.
- O botão Spin substitui as outras células.
- As células travadas permanecem idênticas.
- A mesma seed e as mesmas ações reproduzem os mesmos resultados.
- Os testes essenciais passam.

Só então avançar para o Combination Engine.

---

## 16. Como a próxima sessão deve responder ao usuário

O usuário está no começo da instalação. Não responder com uma longa aula de arquitetura antes de resolver isso.

A resposta inicial recomendada é:

1. Confirmar que vamos começar pela instalação.
2. Dar o link oficial do Godot para Windows.
3. Explicar que é a versão Standard, sem .NET.
4. Pedir que baixe, extraia e abra.
5. Aguardar confirmação antes de criar o projeto.

Quando o usuário disser que abriu o Project Manager, continuar com a criação do projeto. Não perguntar novamente informações que já estão registradas aqui.

---

## 17. Referências

- Repositório: https://github.com/HexerVoodoom/bound_evolution
- Download Godot Windows: https://godotengine.org/download/windows/
- Documentação oficial Godot: https://docs.godotengine.org/en/stable/
- GUT (testes Godot): https://github.com/bitwes/Gut

---

## 18. Resumo executivo para continuidade

Slotbound Evolution é um roguelite 2D arcano centrado em uma máquina 3×3 de símbolos, equipe de até três criaturas e combate em ondas. O usuário escolheu Godot 4.x + GDScript + Compatibility, PC como plataforma inicial e Steam como distribuição pretendida. A distribuição de símbolos é Creature 25%, Attack 40%, Support 35%, implementada com pesos individuais 25/40/35 para os três símbolos de cada família. A capacidade de trava padrão recomendada é 2, mas permanece configurável. A configuração de combos mais recente é a intermediária C: trincas horizontais de família, combinação Creature + Support do mesmo elemento adjacentes ortogonalmente e Vapor Attack Fire + Support Water adjacentes ortogonalmente; prioridade trincas → mistas → Vapor → efeitos individuais; cada símbolo consumido uma vez, sem cascatas.

Foram realizadas simulações exploratórias de combos, estratégias de trava e capacidade de trava. Elas apontam riscos de regras excessivamente permissivas e dificuldade artificial, mas não validam o balanceamento humano. Existem protótipos interativos anteriores, porém com limitações e divergências; não são implementação de referência.

A arquitetura deve separar UI, fluxo de jogo, sistemas de gameplay, dados e infraestrutura. O primeiro módulo será Machine Core: dados dos símbolos, RNG com seed, grade, travas, sorteio ponderado, histórico, UI mínima e testes. Depois virão Combination Engine, Combat Core, Run Controller e UI do Vertical Slice.

**Estado real neste momento: o Godot ainda não foi instalado, o projeto ainda não foi criado e não há código implementado no repositório. A próxima sessão deve começar pela instalação do Godot no Windows e seguir passo a passo.**
