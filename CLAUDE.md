# CLAUDE.md — Livro-Caixa Pessoal

## O que é
App pessoal de receitas e despesas do titular (Wanderson Corrêa Barrada), em um arquivo só (`index.html`, HTML + CSS + JS, sem build). Roda offline no navegador; no celular, instalado a partir do GitHub Pages. Uso: `LEIA-ME.txt`.

Não confundir com o livro-caixa do escritório, que fica no sistema do escritório (`wcbarrada/escritorio-wbarrada`, `livroCaixa(ano)`). São sistemas separados: aproveitar ideias, nunca dados.

## Regras que não mudam
- **Dados nunca entram no repositório.** Ficam só no `localStorage` de cada aparelho; backup JSON é baixado pelo titular. Sem servidor, sem login, sem envio de dados.
- **Chave de armazenamento `livrocaixa:dados:v1`.** Trocar a chave ou o formato apaga, na prática, os dados de quem já usa. Mudança de formato exige migração em `normalize()` lendo o formato antigo.
- **Criptografia opcional** (senha → PBKDF2 SHA-256 → AES-GCM 256, em `deriveKey`/`persist`/`decryptRaw`). Senha perdida = dados irrecuperáveis: isso é intencional, não defeito. Não enfraquecer nem guardar a senha.
- **Valores em centavos inteiros** (`centavos`). Nada de ponto flutuante em soma de dinheiro.
- **Uma conta só para totais:** `totals(rows)` (receita, despesa, saldo, pendentes), usada por painel, relatório, orçamento e acumulado (`acumAno`). Tela nova reutiliza `totals`/`monthRows`; se não servir, consertar nela e dizer o que mudou.
- **Atrasado** = não pago com data anterior a hoje (`isLate`). Aqui a data de referência é o relógio do aparelho, porque o app é de uso diário (diferente de uma base fechada).
- **Offline:** ao mudar `index.html` ou qualquer arquivo do `SHELL`, subir a `VERSION` em `sw.js` (ex.: `livro-caixa-v2`); senão o celular continua com a versão antiga em cache.
- PC e celular não sincronizam: cada aparelho tem a sua base (ver `LEIA-ME.txt`, seção 3).

## Formatos
- Valores: R$ 1.234,56. Datas na tela: dd/mm/aaaa; gravadas como `AAAA-MM-DD`; mês como `AAAA-MM`.
- Textos curtos, em português, do ponto de vista de quem usa.

## Como trabalhar aqui
- Ler o trecho do código antes de mudar; uma alteração por rodada.
- Perguntar antes de regra nova (categoria padrão, forma de contar atraso, formato do backup).
- Conferir no navegador em 1366 × 768 e 390 × 800, com dados de exemplo (`exampleData`), e refazer pelo menos um total à mão contra a tela.
- Testar restauração de backup antigo depois de qualquer mudança em `normalize`.
- Commit em branch e pull request; **push, merge e publicação só com confirmação do titular** (se o GitHub Pages estiver ligado na `main`, o merge publica no celular — [VERIFICAR] em Settings › Pages).
- Identidade visual: o app é pessoal e não usa a marca do escritório.
