# 🎲 Aprisco Game · Ranking

Site estático que lê os dados de pontuação direto de um Google Sheets e renderiza um ranking com a vibe geek do grupo. Sem backend, sem build — só HTML/CSS/JS.

## 🚀 Deploy em 5 passos

### 1. Criar repositório no GitHub
Crie um repositório público com nome `aprisco-game` (ou outro, mas o nome aparecerá na URL).

### 2. Subir o `index.html`
```bash
git init
git add index.html
git commit -m "primeiro commit"
git branch -M main
git remote add origin git@github.com:SEU_USUARIO/aprisco-game.git
git push -u origin main
```

### 3. Ativar GitHub Pages
- Vá em **Settings** → **Pages**
- Em "Source", selecione **Deploy from a branch**
- Escolha branch `main` e pasta `/ (root)`
- Salve. Em ~1 minuto seu site estará no ar em `https://seu-usuario.github.io/aprisco-game/`

### 4. Criar o Google Sheets
Crie uma nova planilha com **estas colunas exatas** na primeira linha:

| data | jogador | tipo | minutos | evento |
|------|---------|------|---------|--------|
| 2026-06-15 | João | presenca |  | Encontro de Junho |
| 2026-06-15 | João | vitoria | 75 | Encontro de Junho |
| 2026-06-15 | Maria | presenca |  | Encontro de Junho |
| 2026-06-15 | Maria | vitoria | 45 | Encontro de Junho |
| 2026-06-15 | Maria | vitoria | 50 | Encontro de Junho |

**Regras de preenchimento:**
- **data**: formato `AAAA-MM-DD` (ex: `2026-06-15`) — mesma data agrupa o encontro
- **jogador**: nome da pessoa (cuidado com escrita — "João" e "joão" viram pessoas diferentes!)
- **tipo**: apenas `presenca` ou `vitoria` (sem acento, em minúsculas)
- **minutos**: só preencher para vitórias — duração da partida
- **evento**: nome amigável do encontro (opcional, mas bonito de ver)

### 5. Publicar o CSV e plugar no site
- No Google Sheets: **Arquivo** → **Compartilhar** → **Publicar na web**
- Em "Vincular", escolha:
  - Aba específica (ex: "Página1") OU "Todo o documento"
  - Formato: **Valores separados por vírgula (.csv)**
- Clique em **Publicar** e copie a URL gerada
- Abra o `index.html` e cole a URL na linha:
  ```js
  window.SHEET_CSV_URL = "COLE_AQUI_A_URL_CSV_DO_SEU_SHEETS";
  ```
- Faça commit e push. Pronto!

## 📊 Como funciona o cálculo

Para cada linha de **presença**: +10 XP
Para cada **vitória** (com `minutos` preenchidos):
- Até 60 min → +3 XP
- 61–90 min → +6 XP
- 90+ min → +10 XP

**Regra do dia justo:** se a pessoa venceu várias partidas no mesmo encontro:
- A pontuação principal vem da vitória **mais longa** do dia
- Cada vitória extra rende +2 XP de bônus (máximo +4 XP)

Exemplo: João venceu 3 partidas (45 min, 70 min, 50 min) em um encontro:
- Vitória mais longa (70 min) → +6 XP
- 2 vitórias extras → +4 XP (2 × 2, dentro do teto)
- Mais a presença → +10 XP
- **Total no encontro: +20 XP**

## ✏️ Como anotar durante o encontro

Mais prático no celular:
1. Abra a planilha pelo app do Google Sheets
2. No início do encontro, registre a presença de todos (uma linha por pessoa, tipo=`presenca`)
3. Após cada partida, registre uma linha por vencedor (tipo=`vitoria`, com os minutos)
4. Pronto — o site atualiza sozinho quando alguém recarrega

**Dica:** use a feature de "duplicar linha" do app pra agilizar a entrada de presenças.

## 🎨 Customização

O arquivo `index.html` está autocontido (HTML + CSS + JS no mesmo arquivo). Pode editar:
- **Nomes dos níveis e faixas de XP**: procure o array `LEVELS` no `<script>`
- **Pontuação**: procure o objeto `POINTS`
- **Cores e visual**: procure o `<style>` no topo, variáveis em `:root`

## 🛡️ Sobre privacidade

A planilha publicada como CSV é **pública** — qualquer pessoa com o link pode ler os dados. Se isso for um problema, pode-se trocar por uma solução com Google Apps Script, mas para um ranking de jogos do grupo o modo público resolve perfeitamente.
