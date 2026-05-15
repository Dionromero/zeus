# Changelog

## [0.2.0] — Persistência, vitória sticky e painel de variáveis

### Adicionado
- **Vitória sticky** (`src/game/world.ts`): condição de vitória é checada a cada
  tick e marcada como "alcançada" permanentemente. Jogador que passa pelo alvo
  e sai dele ainda vence — não perde por sair da casa-alvo.
  - `evaluateWinCondition()` (puro) + `recordWinAttempt()` (sticky) + `hasWon()` (leitor)
  - `WorldState` ganhou `totalTicks`, `victoryReached`, `victoryAtTick`
  - Score calculado pelo tick em que venceu, não pelo tick final do programa

- **Persistência local** (`src/api/local-repo.ts`): código e progresso salvos
  no `localStorage` com camada de repository tipada.
  - Autosave por debounce (700ms após última digitação) + ao executar + `beforeunload`
  - Mantém só o melhor score por nível (não sobrescreve com pior)
  - Trata graciosamente Safari modo privado, cota cheia e localStorage desabilitado
  - Envelope com `version` e `savedAt` pra migração futura
  - Sidebar mostra ✓ verde nos níveis completos
  - Painel de história mostra badge "🏆 Melhor: X pts (Y ticks)" quando há recorde

- **Painel de variáveis** (`src/ui/app.ts`):
  - Abas Console/Variáveis no painel inferior, troca via clique
  - Tick counter sempre visível no topo
  - Variáveis do usuário (em dourado) + estado do mundo (em itálico):
    `jogador.posicao`, `jogador.olhando`, `jogador.segurando`,
    `pedidos.na_fila`, `pizzas.entregues`
  - Hash-based skip evita re-renderização desnecessária a 60fps
  - Formata valores em sintaxe do Portugol (`"texto"`, `verdadeiro/falso`, `nulo`)

### Testes
- 13 testes (era 12), incluindo `'vitória é sticky'` que valida o fix #5

### Bundle
- 58KB / 17KB gzipped (+5KB JS, +1.6KB CSS vs v0.1.0)

---

## [0.1.0] — MVP jogável

### Adicionado
- **Documentação de design completa** (`docs/`)
  - Especificação da DSL Portugol-Pizzaria
  - 11 níveis desenhados (6 implementados)
  - Catálogo de erros didáticos em PT-BR
  - Arquitetura técnica com ADRs

- **Motor da DSL** (`src/engine/`)
  - Lexer hand-written com indentação significativa
  - Parser recursivo descendente com error recovery
  - Interpretador generator-based com tick control e sandbox
  - Catálogo de erros centralizado

- **Mundo do jogo** (`src/game/`)
  - 14 funções built-in (mover, virar, pegar, entregar, etc)
  - Sistema de pedidos com fila
  - Forno com estados (cozinhando/pronto/queimado)
  - 6 níveis implementados (0-5)

- **Interface** (`src/ui/`)
  - Canvas 2D com pixel art procedural
  - Editor de Portugol com atalhos (Ctrl+Enter, Tab)
  - Sidebar com lista de níveis
  - Console de logs e painel de erros
  - Speed control (1x-30x), pause/resume

- **Cliente KRATOS** (`src/api/`)
  - Stub para integração futura (auth + submitRun + leaderboard)

- **Testes**
  - 7 testes do lexer
  - 5 testes de integração end-to-end
  - Soluções de referência dos níveis viram regressão automática

### Pendente
- Implementar níveis 6-10 (já desenhados em `docs/levels.json`)
- Syntax highlight no editor
- Integração com KRATOS (auth + submissão de score)
- Animações de transição do movimento
- Som
