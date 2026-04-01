# Requisitos — Copa do Mundo FIFA 2026
**Produto:** Aplicação web responsiva para acompanhamento e simulação da Copa do Mundo FIFA 2026  
**Versão:** 1.0  
**Data:** Abril de 2026  
**Design System:** Arena Negra

---

## 1. Visão Geral

Aplicação single-page (SPA) responsiva para web e mobile que permite ao usuário acompanhar a tabela completa da Copa do Mundo 2026, navegar por grupos, visualizar o calendário de jogos, acompanhar o chaveamento do mata-mata e simular resultados de partidas com atualização de classificação em tempo real.

### 1.1 Contexto

| Atributo | Valor |
|---|---|
| Edição | FIFA World Cup 2026 |
| Sedes | Canadá, México, Estados Unidos |
| Período | 11 de junho — 19 de julho de 2026 |
| Seleções | 48 (12 grupos de 4) |
| Total de jogos | 104 |
| Fase de grupos | 72 jogos · 11–27 jun |
| Mata-mata | 32 jogos · 28 jun — 19 jul |

---

## 2. Requisitos Funcionais

### RF01 — Visão por Grupos

**Prioridade:** Alta

- Exibir os 12 grupos (A a L) em grid responsivo
- Cada card de grupo deve conter:
  - Letra e cor identificadora do grupo
  - Cidade(s) sede dos jogos
  - Tabela de classificação com colunas: posição, bandeira, nome da seleção, jogos (J), pontos (PTS), saldo de gols (SG), gols a favor (GF)
  - Indicador visual das 2 seleções classificadas (linha divisória após o 2º colocado)
  - Badge especial para o Grupo C (Brasil)
- Classificação deve ser calculada em tempo real a partir dos resultados simulados
- Critérios de desempate: pontos → saldo de gols → gols marcados
- Cada card deve ter um painel expansível com os 6 jogos do grupo e seus resultados
- Botão global "Auto-Simular" gera resultados aleatórios ponderados pelo ranking FIFA para todos os grupos
- Botão "Limpar" remove todos os resultados simulados
- Indicador de quantos jogos foram simulados

### RF02 — Calendário Cronológico

**Prioridade:** Alta

- Listar todos os 72 jogos da fase de grupos ordenados por data (11/06 a 27/06)
- Agrupamento visual por data com cabeçalho destacado
- Cada card de jogo deve exibir:
  - Grupo e rodada
  - Bandeiras e nomes das seleções
  - Placar (se simulado) ou "vs" (se pendente)
  - Horário (quando disponível)
- Destaque visual para jogos do Brasil
- Cards coloridos por grupo (cor identificadora na borda esquerda)
- Responsividade: grid multi-coluna em desktop, coluna única em mobile

### RF03 — Chaveamento (Mata-Mata)

**Prioridade:** Alta

- Exibir o bracket completo do mata-mata com as seguintes fases:
  - 16-avos de final (32 seleções · 28 jun — 1 jul)
  - Oitavas de final (16 seleções · 4–7 jul)
  - Quartas de final (8 seleções · 10–12 jul)
  - Semifinais (4 seleções · 15–16 jul)
  - Disputa pelo 3º lugar (18 jul)
  - Final (19 jul · MetLife Stadium, Nova York/NJ)
- Quando não há simulação ativa, exibir estado vazio com instrução ao usuário
- Quando há simulação, popular os 16-avos com os classificados de cada grupo (1º e 2º de cada grupo + 8 melhores 3ºs colocados)
- Fases subsequentes devem ser calculadas automaticamente a partir dos resultados simulados no mata-mata
- Exibir campeão destacado ao término da simulação completa
- Regra: times do mesmo grupo não se enfrentam nos 16-avos

### RF04 — Simulação de Resultados

**Prioridade:** Alta

- Listar todos os 72 jogos da fase de grupos com interface de entrada de placar
- Funcionalidades:
  - **Inserir placar:** input numérico para cada time (0–20)
  - **Editar:** atualizar placar já inserido
  - **Remover:** excluir resultado individual
  - **Auto-Simular (todos):** gera placar aleatório ponderado pelo ranking FIFA para todos os jogos
  - **Auto-Simular (grupo):** simula apenas os jogos de um grupo selecionado
  - **Limpar tudo:** remove todos os resultados
- Algoritmo de simulação automática:
  - Diferença de força baseada no ranking FIFA (posição 1–91+)
  - Média de gols realista (~2,6 por jogo)
  - Probabilidade de vitória proporcional à diferença de ranking
- Filtros:
  - Por status: Todos / Jogados / Pendentes
  - Por grupo: Todos os Grupos / Grupo A…L
- Barra de progresso: `X / 72 jogos simulados (Y%)`
- Indicadores visuais de resultado: vencedor em dourado, empate em ciano, perdedor em cinza
- Confirmação por Enter ou botão OK
- Atualização em tempo real das classificações nos demais views

### RF05 — Navegação entre Views

**Prioridade:** Alta

- 4 abas principais na navegação fixa:
  - ⚽ Grupos
  - 📅 Calendário
  - 🏆 Chaveamento
  - 🎮 Simulação
- Header fixo com logo e badges informativos (48 seleções / 104 jogos)
- Navegação sticky (permanece visível ao rolar)
- Estado ativo da aba com indicador visual (linha dourada inferior)
- Scroll horizontal na navbar em telas pequenas

---

## 3. Requisitos Não Funcionais

### RNF01 — Performance

- Tempo de carregamento inicial < 2s em conexão 4G
- Todas as atualizações de classificação devem ser síncronas (< 16ms / 60fps)
- Sem dependências de backend — aplicação 100% client-side
- Bundle minificado < 150KB (sem fontes externas)

### RNF02 — Responsividade

| Breakpoint | Layout |
|---|---|
| < 480px | Mobile · 1 coluna · navbar scrollável |
| 480–768px | Mobile largo · 1–2 colunas |
| 768–1024px | Tablet · 2–3 colunas |
| > 1024px | Desktop · até 4 colunas |

- Touch targets mínimos de 44×44px (WCAG / Apple HIG)
- Sem scroll horizontal em nenhum breakpoint
- Fontes mínimas: 12px body, 10px labels

### RNF03 — Acessibilidade

- Contraste mínimo 4.5:1 para texto normal (WCAG AA)
- Navegação por teclado em todos os controles interativos
- Labels descritivos em inputs de placar
- Não depender exclusivamente de cor para transmitir informação (ícones + texto)

### RNF04 — Compatibilidade

- Browsers: Chrome 100+, Firefox 100+, Safari 15+, Edge 100+
- Mobile: iOS Safari 15+, Chrome Android 100+
- Sem dependência de cookies ou localStorage para funcionamento básico

---

## 4. Dados

### 4.1 Grupos e Seleções (sorteio de dezembro/2025)

| Grupo | Seleções |
|---|---|
| A | México · África do Sul · Coreia do Sul · Rep. Tcheca |
| B | Canadá · Bósnia · Catar · Suíça |
| **C** | **Brasil · Marrocos · Haiti · Escócia** |
| D | EUA · Paraguai · Austrália · Turquia |
| E | Alemanha · Curaçao · Costa do Marfim · Equador |
| F | Holanda · Japão · Suécia · Tunísia |
| G | Bélgica · Egito · Irã · Nova Zelândia |
| H | Espanha · Cabo Verde · Arábia Saudita · Uruguai |
| I | França · Senegal · Iraque · Noruega |
| J | Argentina · Argélia · Áustria · Jordânia |
| K | Portugal · RD Congo · Uzbequistão · Colômbia |
| L | Inglaterra · Croácia · Gana · Panamá |

### 4.2 Calendário do Brasil (Grupo C)

| Data | Adversário | Local |
|---|---|---|
| 13 jun (sáb) | Marrocos | MetLife Stadium, Nova Jersey |
| 19 jun (sex) | Haiti | Lincoln Financial Field, Filadélfia |
| 24 jun (qua) | Escócia | A confirmar |

### 4.3 Formato de Classificação

- Avançam: 1º e 2º de cada grupo (24 seleções) + 8 melhores 3ºs colocados = **32 seleções no mata-mata**
- Critérios de desempate: pontos → saldo de gols → gols marcados → confronto direto → fair play → sorteio

### 4.4 Cronograma do Mata-Mata

| Fase | Datas | Jogos |
|---|---|---|
| 16-avos de final | 28 jun — 1 jul | 16 |
| Oitavas de final | 4 — 7 jul | 8 |
| Quartas de final | 10 — 12 jul | 4 |
| Semifinais | 15 — 16 jul | 2 |
| 3º lugar | 18 jul | 1 |
| **Final** | **19 jul** | **1** |

---

## 5. Design System — Arena Negra

### 5.1 Paleta de Cores

| Token | Hex | Uso |
|---|---|---|
| `--bg` | `#05050F` | Background principal |
| `--surface` | `#0C0C1F` | Superfícies elevadas |
| `--card` | `#111128` | Cards e painéis |
| `--gold` | `#FFD700` | Accent primário · títulos · destaques |
| `--gold2` | `#FFE566` | Gradientes dourados |
| `--cyan` | `#00E5FF` | Accent secundário · empates |
| `--green` | `#00C851` | Vitórias · classificados · Brasil |
| `--red` | `#FF4444` | Derrotas · alertas |
| `--muted` | `#7A7A9D` | Texto secundário · labels |

### 5.2 Tipografia

| Fonte | Uso |
|---|---|
| Bebas Neue | Títulos, placares, labels de navegação, identificadores de grupo |
| Rajdhani | Corpo de texto, nomes de seleções, dados de tabela |

### 5.3 Componentes Principais

- **GroupCard** — card de grupo com tabela, badge Brasil, toggle de jogos
- **CalendarMatchCard** — card de jogo no calendário com cores por grupo
- **SimRow** — linha de simulação com inputs e ações
- **BracketMatch** — card de confronto no chaveamento
- **Header** — logo + badges + sticky
- **NavBar** — abas com estado ativo
- **ProgressBar** — progresso da simulação
- **EmptyState** — estado vazio do chaveamento

### 5.4 Efeitos Visuais

- Fundo com gradientes radiais atmosféricos (dourado no topo, azul navy na diagonal)
- Estrelas/partículas via `radial-gradient` no pseudo-elemento `::after`
- Header com `backdrop-filter: blur(24px)` para glassmorphism
- Hover em cards: `translateY(-2px)` + `box-shadow` suavizado
- Animação de entrada `fadeUp` com `animation-delay` escalonado por card
- Borda superior colorida por grupo nos cards (accent de 3px)
- Glow verde nas bordas do card do Brasil

---

## 6. Regras de Negócio

### RN01 — Cálculo de Classificação
```
PTS = vitórias × 3 + empates × 1
SG  = gols marcados − gols sofridos
Ordem: PTS DESC → SG DESC → GF DESC
```

### RN02 — Algoritmo de Simulação Automática
```
força_home = max(1, 101 - ranking_FIFA_home)
força_away = max(1, 101 - ranking_FIFA_away)
prob_home  = força_home / (força_home + força_away)
média_gols = 2.6 por partida

gols_home = round(rand() × 1.4 × média_gols/2 + (rand() < prob_home ? rand() × 1.5 : 0))
gols_away = round(rand() × 1.4 × média_gols/2 + (rand() < (1-prob_home) ? rand() × 1.5 : 0))
```

### RN03 — Classificação para o Mata-Mata
- Os 2 primeiros de cada grupo classificam diretamente (24 seleções)
- Os 8 melhores 3ºs colocados entre os 12 grupos completam o quadro (8 seleções)
- Critério dos 8 melhores 3ºs: mesmos critérios de desempate da fase de grupos
- Total: 32 seleções nos 16-avos de final

### RN04 — Chaveamento dos 16-avos
- Times do mesmo grupo **não** se enfrentam
- 1ºs colocados enfrentam 2ºs de grupos cruzados (conforme tabela FIFA)
- 3ºs colocados são alocados conforme chave pré-definida pela FIFA

### RN05 — Mata-Mata Geral
- Em caso de empate no tempo normal: prorrogação (2 × 15min)
- Persistindo empate: disputa por pênaltis
- Para fins de simulação automática: empate no tempo normal → penaltis aleatórios (50/50)

---

## 7. Escopo e Limitações — v1.0

### Incluso
- Fase de grupos completa (12 grupos, 72 jogos)
- Classificação em tempo real
- Chaveamento dos 16-avos populado automaticamente
- Simulação automática e manual de resultados
- Responsividade web + mobile
- Dados do sorteio (dezembro/2025) hardcoded

### Fora do Escopo (versões futuras)
- Simulação interativa do mata-mata (inserir resultados das fases eliminatórias)
- Integração com API da FIFA para dados ao vivo
- Autenticação e persistência de simulações por usuário
- Modo multiplayer / bolão
- Estatísticas de jogadores
- Notificações push
- PWA / app nativo
- Internacionalização (i18n)

---

## 8. Próximas Evoluções — Backlog

| ID | Feature | Prioridade | Esforço |
|---|---|---|---|
| V2-01 | Simulação interativa do mata-mata fase a fase | Alta | M |
| V2-02 | Integração com API FIFA (dados reais ao vivo) | Alta | L |
| V2-03 | Modo Bolão — salvar palpites por usuário | Alta | L |
| V2-04 | PWA — instalável + offline | Média | M |
| V2-05 | Compartilhamento de simulação via URL | Média | S |
| V2-06 | Estatísticas de seleções (histórico, artilheiros) | Baixa | M |
| V2-07 | Dark/Light mode toggle | Baixa | S |
| V2-08 | Animação de gol ao inserir placar | Baixa | S |
| V2-09 | Exportar tabela como imagem (PNG) | Baixa | S |
| V2-10 | Widget embutível para outros sites | Baixa | M |

---

## 9. Stack Técnica Recomendada

### v1.0 (atual — protótipo HTML)
- HTML5 + CSS3 + JavaScript Vanilla
- React 18 via CDN (sem build)
- Google Fonts (Bebas Neue + Rajdhani)
- Deploy: Netlify / Vercel (arrastar e soltar)

### v2.0 (produção recomendada)
- **Framework:** Next.js 14+ (App Router)
- **UI:** React + Tailwind CSS
- **Estado:** Zustand (simulação) + React Query (dados FIFA)
- **Banco:** Supabase (PostgreSQL) para bolão
- **Auth:** Supabase Auth / Clerk
- **API:** FIFA Official Data API ou scraping FIFA.com
- **Deploy:** Vercel
- **CI/CD:** GitHub Actions

---

## 10. Referências

- Dados dos grupos: [NSC Total — Grupos Copa 2026](https://www.nsctotal.com.br/noticias/veja-os-grupos-da-copa-do-mundo-de-2026)
- Tabela oficial: [FIFA.com — Copa 2026](https://www.fifa.com/pt/tournaments/mens/worldcup/canadamexicousa2026)
- Formato do torneio: [Wikipedia — Copa do Mundo 2026](https://pt.wikipedia.org/wiki/Copa_do_Mundo_FIFA_de_2026)
- Regulamento: [ESPN Brasil — Tabela Completa](https://www.espn.com.br/futebol/copa-do-mundo)
- Ranking FIFA: novembro de 2025 (usado no algoritmo de simulação)

---

*Documento gerado em 01/04/2026 · Arena Negra Design System · Copa do Mundo FIFA 2026*
