# Como configurar a Landing Page

## Passo 1 — Criar a planilha no Google Sheets

1. Acesse [Google Sheets](https://sheets.google.com) e crie uma nova planilha.
2. Na primeira linha (cabeçalho), coloque:
   - **A1:** `Nome`
   - **B1:** `Telefone`
   - **C1:** `Email`
   - **D1:** `Data`

## Passo 2 — Criar o Google Apps Script

1. Na planilha, vá em **Extensões > Apps Script**.
2. Apague todo o código existente e cole o seguinte:

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

  sheet.appendRow([
    e.parameter.nome,
    e.parameter.telefone,
    e.parameter.email,
    new Date().toLocaleString('pt-BR', { timeZone: 'America/Sao_Paulo' })
  ]);

  // Retorna JSONP para evitar problemas de CORS
  var callback = e.parameter.callback || 'callback';
  return ContentService
    .createTextOutput(callback + '({"status":"ok"})')
    .setMimeType(ContentService.MimeType.JAVASCRIPT);
}
```

> **IMPORTANTE:** Se voce ja tinha implantado antes, voce precisa criar uma **nova implantacao** (nao basta editar a existente). Va em **Implantar > Nova implantacao** e refaca o processo.

3. Clique em **Salvar** (ícone de disquete).
4. Vá em **Implantar > Nova implantação**.
5. Em "Tipo", selecione **App da Web**.
6. Configure:
   - **Executar como:** Eu mesmo
   - **Quem tem acesso:** Qualquer pessoa
7. Clique em **Implantar** e autorize quando solicitado.
8. **Copie a URL** gerada (algo como `https://script.google.com/macros/s/ABC.../exec`).

## Passo 3 — Configurar o index.html

Abra o arquivo `index.html` e altere as duas variáveis no início do `<script>`:

```javascript
const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/SUA_URL_AQUI/exec';
const PDF_URL = 'https://drive.google.com/file/d/SEU_ID_AQUI/view';
```

## Passo 4 — Hospedar a página

Opções gratuitas:
- **GitHub Pages** — suba o `index.html` em um repositório e ative o Pages.
- **Netlify** — arraste a pasta no painel do Netlify.
- **Vercel** — importe o projeto.

Depois de hospedada, gere o QR Code apontando para a URL da landing page.
