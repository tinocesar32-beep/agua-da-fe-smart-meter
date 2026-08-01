# Água da Fé · Smart Meter

App instalável (PWA) para leitura do contador de água por fotografia, cálculo de
consumo, produção, faturação e lucro.

## Como funciona os dados

- **Leituras, vendas e configuração** ficam guardadas só no dispositivo (IndexedDB do
  browser). Não há base de dados nem servidor para esses dados — se limpar os dados do
  browser ou trocar de telemóvel, o histórico não vai consigo.
- **A leitura por foto** é o único ponto que passa pela internet: a foto é enviada para
  uma função serverless (`/api/analyze`) que chama a IA da Anthropic e devolve só o
  valor lido. Isto é necessário porque a chave da API nunca pode estar no telemóvel —
  senão qualquer pessoa podia vê-la e usá-la à sua conta.

## Publicar na Vercel (passo a passo)

1. **Criar uma conta na Anthropic Console** (https://console.anthropic.com), se ainda
   não tiver, e gerar uma chave de API em *Settings → API Keys*.

2. **Enviar este projeto para o GitHub**
   ```bash
   cd agua-da-fe-pwa
   git init
   git add .
   git commit -m "Água da Fé Smart Meter"
   ```
   Crie um repositório novo no GitHub e siga as instruções que o GitHub mostra para
   ligar e enviar (`git remote add origin ...` e `git push`).

3. **Importar o repositório na Vercel**
   - Entre em https://vercel.com, clique em **Add New → Project**.
   - Escolha o repositório que acabou de criar.
   - O Vercel deteta automaticamente que é um projeto Vite — não precisa de mudar
     nenhuma definição de build.

4. **Configurar a chave da API**
   - Antes de clicar em *Deploy*, abra **Environment Variables**.
   - Adicione: `ANTHROPIC_API_KEY` = a sua chave da Anthropic.
   - Isto garante que a chave fica só no servidor da Vercel, nunca no telemóvel.

5. **Publicar** — clique em *Deploy*. Em cerca de 1 minuto terá um link do tipo
   `https://agua-da-fe-smart-meter.vercel.app`.

6. **(Opcional) Domínio próprio** — em *Project Settings → Domains* pode ligar um
   domínio como `smartmeter.aguadafe.ao`, se tiver um.

## Instalar no telemóvel

- **Android (Chrome)**: abra o link → menu (⋮) → "Adicionar ao ecrã principal" ou
  "Instalar aplicação".
- **iPhone (Safari)**: abra o link → ícone de partilha → "Adicionar ao Ecrã Principal".

A partir daí abre como uma app normal, com ícone próprio, sem barra de endereço.

## Testar localmente antes de publicar

```bash
npm install
npm run dev
```
Abre em http://localhost:5173 — mas a leitura por foto só funciona depois de publicar
na Vercel (ou correndo `vercel dev` com a variável `ANTHROPIC_API_KEY` definida).

## Estrutura

```
agua-da-fe-pwa/
├── api/analyze.js       ← função serverless (chama a IA em segurança)
├── src/App.jsx          ← aplicação principal
├── src/db.js            ← guarda de dados local (IndexedDB)
├── public/icons/        ← ícones da app
├── vite.config.js       ← configuração PWA (manifesto, ícone, instalável)
└── index.html
```

## Personalizar

Os preços, custos e stock mínimo podem ser ajustados dentro da app, no separador
**Configuração** — não é preciso mexer no código.
