# Site do FoMerco

Site institucional do FoMerco com os **Anais do FoMerco** e a **Carta FoMerco**, estruturado para o pedido de ISSN, e um painel com login para atualizar o conteúdo.

Este guia é para quem nunca publicou um site. Leva de 30 a 60 minutos na primeira vez. Você vai criar três contas gratuitas:

| Serviço | Para quê |
|---|---|
| **GitHub** | Guarda o código do site |
| **Turso** | Guarda o banco de dados: textos, números, trabalhos, PDFs e usuários |
| **Render** | Deixa o site no ar |

> Os planos gratuitos mudam com o tempo. Antes de começar, confira os limites atuais nas páginas de preço de cada serviço.
> - No plano gratuito do Render, o site "dorme" depois de um tempo sem visitas. O primeiro acesso depois disso pode levar cerca de um minuto para carregar.
> - O plano gratuito do Turso tem 5 GB de armazenamento, o que cabe muitos PDFs. Confira também se ele tem alguma regra para bancos sem uso.

---

## Passo 1: colocar o código no GitHub

1. Crie uma conta em **github.com** (botão *Sign up*).
2. Clique no **+** no canto superior direito e escolha **New repository**.
3. Em *Repository name*, escreva `site-fomerco`. Marque **Private** e clique em **Create repository**.
4. Na página do repositório, clique no link **uploading an existing file**.
5. Descompacte o arquivo `fomerco-site.zip` no seu computador, abra a pasta `fomerco-site`, selecione **tudo o que está dentro dela** e arraste para a página do GitHub.
6. Espere o envio terminar e clique em **Commit changes**.

## Passo 2: criar o banco de dados no Turso

1. Crie uma conta em **turso.tech** (dá para entrar com a conta do GitHub).
2. No painel, crie um banco (**Create Database**) com o nome `fomerco`. Se pedir uma região, escolha a mais próxima do Brasil.
3. Abra o banco criado e copie a **URL**. Ela começa com `libsql://`. Guarde num bloco de notas.
4. Na mesma página, gere um **token** (*Generate Token* ou *Create Token*), com permissão de leitura e escrita e sem data de expiração. Copie e guarde junto com a URL.

> O token funciona como a senha do banco. Não compartilhe e não coloque em nenhum arquivo do GitHub.

## Passo 3: colocar o site no ar no Render

1. Crie uma conta em **render.com** usando a conta do GitHub (*Sign in with GitHub*).
2. Clique em **New +** e depois em **Web Service**.
3. Conecte o GitHub quando pedir, dando acesso ao repositório `site-fomerco`, e selecione-o.
4. Preencha:
   - **Name:** `fomerco` (vira parte do endereço, como `fomerco.onrender.com`)
   - **Runtime / Language:** Node
   - **Build Command:** `npm install --omit=dev`
   - **Start Command:** `npm start`
   - **Instance Type:** Free
5. Em **Environment Variables**, adicione estas variáveis, uma por vez:

| Nome (Key) | Valor (Value) |
|---|---|
| `NODE_ENV` | `production` |
| `NODE_VERSION` | `22` |
| `DATABASE_URL` | a URL do Turso (começa com `libsql://`) |
| `DATABASE_AUTH_TOKEN` | o token do Turso |
| `ADMIN_USUARIO` | o nome de usuário do primeiro administrador, por exemplo `danilo` |
| `ADMIN_SENHA` | uma senha temporária com pelo menos 10 caracteres |

6. Clique em **Create Web Service**. A primeira publicação leva alguns minutos. Quando aparecer **Live**, o site está no ar no endereço mostrado no topo.

## Passo 4: primeiro acesso

1. Abra `https://SEU-ENDERECO.onrender.com/admin` (também há o link "Área restrita" no rodapé do site).
2. Entre com o usuário e a senha que você colocou em `ADMIN_USUARIO` e `ADMIN_SENHA`.
3. O site vai pedir uma senha nova. Escolha uma senha forte; uma frase longa funciona bem.
4. Volte ao Render, abra **Environment** e **apague** a variável `ADMIN_SENHA`. Ela só serve para criar o primeiro administrador e não é mais usada.

## Passo 5: preencher o site

No painel, nesta ordem:

1. **Página inicial e contato:** texto de abertura, e-mail e endereço.
2. **Páginas de texto:** Sobre o FoMerco; Apresentação, Expediente (corpo editorial) e Normas de cada publicação.
3. **Anais** e **Carta FoMerco**, em *Dados da publicação*: periodicidade, instituição responsável, endereço completo e o **link para submissão** (formulário, e-mail ou página de normas), que vira o botão "Enviar um texto". Esses dados aparecem no Expediente, como o ISSN exige.
4. **Evento atual:** nome (o número romano fica em vermelho), datas, local, link da Even3 e, a cada edição, os skylines da cidade do encontro.
4b. **Menu:** renomeie, reordene ou esconda itens do menu e adicione links extras.
5. **Novo número:** crie os números com Vol., No. e ano. Na Carta, envie o PDF do número. Nos Anais, adicione os trabalhos um a um, cada um com título, autores, resumo e PDF. A opção "Adicionar e cadastrar outro" agiliza.
6. **Usuários:** crie contas para outras pessoas. O site gera uma senha temporária, que a pessoa troca no primeiro acesso.

Cada publicação tem um quadro **"Pronto para pedir o ISSN?"** que mostra o que ainda falta.

### Para as capturas de tela do pedido de ISSN

| Exigência | Página do site |
|---|---|
| Tela de Apresentação | `/anais/apresentacao` ou `/carta/apresentacao` |
| Normas para Publicação | `/anais/normas` |
| Expediente | `/anais/expediente` ou `/carta/expediente` |
| Edições anteriores e atual | `/anais/numeros` ou `/carta/numeros` |
| Cópia de um artigo | página de um trabalho, por exemplo `/anais/trabalho/1`, e o PDF dele |

A página principal de cada publicação (`/anais` e `/carta`) tem os links para Apresentação, Expediente, Número atual, Números anteriores e Normas.

---

## Atualizar o código no futuro

Se você receber uma versão nova dos arquivos, descompacte e envie **tudo o que está dentro da pasta** para o mesmo repositório do GitHub (*Add file* e depois *Upload files*), como na primeira vez. Arquivos com o mesmo nome são substituídos. O Render publica a nova versão sozinho. O conteúdo fica no Turso e não se perde com as atualizações.

## Mudar de hospedagem (por exemplo, para a UNESP)

O site funciona em qualquer servidor com Node.js 20 ou mais novo. Sem `DATABASE_URL`, ele usa um arquivo de banco local em `data/fomerco.db`. Nesse caso, faça backup periódico desse arquivo, porque ele contém tudo.

```bash
npm install --omit=dev
NODE_ENV=production ADMIN_USUARIO=admin ADMIN_SENHA='senha-temporaria' npm start
```

---

## Segurança: o que já está implementado

- Senhas guardadas só como hash **Argon2id**. Ninguém consegue ver a senha de ninguém, só gerar uma nova.
- Bloqueio de 15 minutos depois de 5 senhas erradas para o mesmo usuário (ou 20 a partir do mesmo endereço de internet).
- Mensagem de erro genérica no login, que não revela se o usuário existe.
- Sessões com cookie `HttpOnly`, `Secure` e `SameSite`, que expiram em 8 horas.
- Proteção CSRF em todos os formulários do painel e checagem de origem dos pedidos.
- Só administradores criam, removem ou alteram contas.
- PDFs e imagens conferidos pelo conteúdo real do arquivo, não só pela extensão.
- Textos do painel limpos antes de ir ao site, para impedir que scripts sejam inseridos.
- Cabeçalhos de segurança (Content-Security-Policy, HSTS e outros).
- Nenhuma página pública lista usuários ou mostra dados de login.
- Números em rascunho não aparecem no site.

Para rodar os testes automáticos (precisa de Node.js instalado): `npm install` e depois `npm test`.

**Limites honestos:** nenhum site é impossível de invadir. Mantenha as senhas fortes, não reaproveite senhas de outros serviços, dê conta só a quem precisa e, se possível, peça a alguém da área de TI uma revisão antes de divulgar amplamente.

## Para testar no seu computador (opcional)

```bash
npm install
npm run demo                                  # cria dados fictícios em data/demo.db
DATABASE_URL=file:data/demo.db npm start      # abre em http://localhost:3000
```
Login de demonstração: `admin` / `demonstracao-2026`. Os dados de demonstração nunca vão para o site real.
