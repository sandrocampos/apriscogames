# 🎲 Jogos Aprisco

Ranking e histórico do grupo de casais da Aprisco que se reúne para jogar jogos de tabuleiro. O foco é na comunhão e amizade, com encontros rodando nas casas dos irmãos em rodízio.

## O que é

Página estática (`index.html`) que lê duas abas de um Google Sheets publicado como CSV e exibe:

| Aba | Conteúdo |
|-----|----------|
| **🏆 Ranking** | Classificação geral por XP com nível, vitórias e detalhes por jogador |
| **📜 Histórico** | Encontros em ordem cronológica com presenças e partidas |
| **📊 Estatísticas** | Jogos mais populares (com capa), números da temporada e maiores taxas de vitória |
| **⚔️ Regras** | Tabela de níveis e sistema de XP |
| **🎮 Jogos** | Cadastro de URLs das capas/caixas dos jogos (salvo no navegador) |

## Configuração

1. Abra o Google Sheets com as abas `presencas`, `partidas` e `jogos`
2. Publique cada aba em **Arquivo → Compartilhar → Publicar na web → CSV**
3. Cole as três URLs no topo do `index.html`:

```js
window.SHEET_PRESENCAS_URL = "https://docs.google.com/...presencas...";
window.SHEET_PARTIDAS_URL  = "https://docs.google.com/...partidas...";
window.SHEET_JOGOS_URL     = "https://docs.google.com/...jogos...";
```

## Estrutura das planilhas

### Aba `presencas`

| coluna | exemplo | descrição |
|--------|---------|-----------|
| `data` | `2026-03-15` | data do encontro (YYYY-MM-DD) |
| `jogador` | `Maria` | nome do jogador presente |
| `evento` | `Na casa da Ana` | nome/local do encontro (opcional) |

### Aba `partidas`

| coluna | exemplo | descrição |
|--------|---------|-----------|
| `data` | `2026-03-15` | data do encontro |
| `jogo` | `Ticket to Ride` | nome do jogo |
| `minutos` | `75` | duração da partida |
| `vencedor` | `João` | nome do vencedor |
| `participantes` | `João, Maria, Pedro` | todos que jogaram (separados por vírgula) |

## Sistema de XP

**Presença no encontro:** +10 XP

**Vitória (por partida):**
- Até 60 min → +3 XP
- 61–90 min → +6 XP
- Mais de 90 min → +10 XP

**Regra do dia justo:** se o jogador venceu várias partidas no mesmo encontro, conta a vitória mais longa + bônus de +2 XP por vitória extra (máximo +4 XP de bônus).

**Exemplo:** João venceu 3 partidas (45 min, 70 min, 50 min) em um encontro:
- Vitória mais longa (70 min) → +6 XP
- 2 vitórias extras → +4 XP (teto máximo)
- Presença → +10 XP
- **Total no encontro: +20 XP**

## Níveis

| Nível | Nome | XP |
|-------|------|----|
| LV.1 | OVELHA | 0–49 |
| LV.2 | DISCÍPULO | 50–99 |
| LV.3 | PROFETA | 100–159 |
| LV.4 | APÓSTOLO | 160+ |

## Capas dos jogos

Cadastre as capas diretamente na aba `jogos` do Google Sheets com duas colunas:

| coluna | exemplo |
|--------|---------|
| `nome` | `Ticket to Ride` |
| `imagem` | `https://cf.geekdo-images.com/...` |

As miniaturas aparecem automaticamente na lista de jogos mais populares (📊 Estatísticas) ao carregar a página. Nomes devem ser idênticos aos usados na aba `partidas`.

## Deploy

Basta abrir o `index.html` num navegador ou hospedar em qualquer servidor estático (GitHub Pages, Netlify, etc.). Não há backend.
