# Slotbound Evolution

> Roguelite 2D de fantasia arcana sobre uma máquina 3×3 que transforma probabilidade em criaturas, ataques e combinações.

**Status:** pré-produção / preparação técnica  
**Engine planejada:** Godot 4.x + GDScript  
**Plataforma inicial:** PC  
**Distribuição pretendida:** Steam

## Comece por aqui

Se você está chegando ao projeto agora, leia primeiro:

1. **[Handoff completo](docs/SLOTBOUND_EVOLUTION_HANDOFF.md)**: decisões, regras, arquitetura, simulações, pendências e próximo passo.
2. **[Índice da documentação](docs/README.md)**: mapa dos documentos e orientação de continuidade.

## Conceito em uma frase

Você não controla a sorte. Você constrói o mecanismo que ensina a sorte a trabalhar a seu favor.

O jogador manipula uma máquina de nove células, preserva símbolos entre giros, invoca uma equipe limitada de criaturas e enfrenta ondas de inimigos.

## Pilares

- **Reprogramar a sorte:** RNG com intervenção do jogador por meio de travas e decisões.
- **Construir um exército:** símbolos invocam criaturas e fortalecem a equipe.
- **Desencadear reações:** padrões e combinações geram efeitos sinérgicos.

## Estado atual

Este repositório está na fase de documentação e preparação.

- [x] Conceito central e pilares documentados.
- [x] Decisões iniciais de gameplay consolidadas.
- [x] Arquitetura técnica preliminar documentada.
- [x] Especificação inicial do Machine Core documentada.
- [ ] Godot instalado no ambiente do usuário.
- [ ] Projeto Godot criado.
- [ ] Machine Core implementado.
- [ ] Combination Engine implementado.
- [ ] Combat Core implementado.
- [ ] Vertical Slice 01 jogável.

**Importante:** não há ainda código-fonte do jogo nem projeto Godot neste repositório. As especificações descrevem o que será construído, não funcionalidades já implementadas.

## Próximo passo

Instalar Godot 4.x Standard para Windows (sem .NET/C#), criar o projeto `SlotboundEvolution` com o renderizador Compatibility e então implementar o Machine Core em etapas pequenas.

- [Download oficial do Godot para Windows](https://godotengine.org/download/windows/)
- [Documentação oficial](https://docs.godotengine.org/en/stable/)

## Organização do repositório

```text
bound_evolution/
├── README.md
├── .gitignore
└── docs/
    ├── README.md
    └── SLOTBOUND_EVOLUTION_HANDOFF.md
```

Quando o projeto Godot for criado, os diretórios de código, cenas, recursos e testes serão adicionados gradualmente. Não criar pastas vazias só para preencher a árvore.

## Regras de colaboração

- Preservar decisões marcadas como confirmadas no handoff.
- Identificar claramente propostas, hipóteses e questões em aberto.
- Não tratar simulações artificiais como prova de balanceamento.
- Manter gameplay independente da UI e das animações.
- Inspecionar arquivos antes de modificá-los.
- Fazer mudanças incrementais e testáveis.
- Atualizar a documentação quando decisões ou implementação mudarem.

## Continuidade entre sessões

Para continuar em uma nova sessão do ChatGPT, envie o link do repositório e use:

> Continue o desenvolvimento de Slotbound Evolution a partir deste repositório. Leia o README e o documento `docs/SLOTBOUND_EVOLUTION_HANDOFF.md` integralmente antes de agir. Verifique o estado atual do GitHub e continue do próximo passo registrado, sem presumir que tarefas ainda não concluídas foram realizadas.

---

[Documentação completa](docs/SLOTBOUND_EVOLUTION_HANDOFF.md) · [Índice de docs](docs/README.md)
