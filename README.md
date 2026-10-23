# Layout SRM

Ferramenta de gestão de layout de escritório (mesas, andares e colaboradores) da SRM, em **um único arquivo HTML** (`index.html`) com CSS e JavaScript embutidos. Não precisa de servidor nem de build.

## Estrutura do repositório

```
layout-srm/
├── index.html     # aplicação completa + dados criptografados (bloco <script id="app-data">)
├── README.md      # este arquivo
├── .gitignore
└── .nojekyll      # desliga o processamento Jekyll do GitHub Pages
```

## Como funciona o salvamento

Os dados (mesas, colaboradores, áreas, usuários extras) ficam **dentro do próprio `index.html`**, no bloco `<script id="app-data" type="application/json">`.

- **Abrindo o arquivo localmente** (duplo clique, `file://`) no **Chrome ou Edge**, logado como administrador: na **primeira alteração** o navegador pede para selecionar o arquivo `index.html`. A partir daí, cada alteração é gravada direto nesse arquivo.
- **Abrindo pelo GitHub Pages**: o navegador só *lê* o arquivo. Não é possível gravar de volta no GitHub a partir da página. Alterações feitas ali ficam apenas no `localStorage` daquele navegador.
- Firefox e Safari não suportam gravação direta no arquivo (File System Access API).

## Fluxo de atualização (administrador)

1. Clone ou baixe o repositório e abra o `index.html` localmente no Chrome/Edge.
2. Faça login como administrador e faça as alterações (a primeira pede para escolher o arquivo; selecione esse mesmo `index.html`).
3. Confira o resultado e publique:

```bash
git add index.html
git commit -m "Atualiza layout: <descreva a mudança>"
git push
```

O histórico de commits passa a ser o histórico de alterações do layout (`git log -p index.html`).

## Publicar no GitHub Pages

1. Crie o repositório no GitHub e envie os arquivos:

```bash
git init
git add .
git commit -m "Versão inicial do Layout SRM"
git branch -M main
git remote add origin https://github.com/<usuario>/<repositorio>.git
git push -u origin main
```

2. No GitHub: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save**.
3. Em alguns minutos o site fica em `https://<usuario>.github.io/<repositorio>/`.

## 🔒 Segurança e privacidade

Os dados (mesas, colaboradores, usuários) ficam **criptografados dentro do `index.html`** (AES-256-GCM; chaves derivadas das senhas com PBKDF2-SHA256, 310 mil iterações). Quem abrir o arquivo ou o site sem um login válido vê apenas texto embaralhado, inclusive pelo "Exibir código-fonte". Por isso o repositório e o GitHub Pages podem ser públicos.

Como funciona:

- Não existe mais acesso de "visitante": a página abre direto na tela de login.
- Cada usuário tem a própria senha. O login só é aceito se a senha abrir os dados.
- Ao salvar como administrador, o app **regrava o arquivo já criptografado** (nunca em texto aberto).
- Ao clicar em **Sair**, o app apaga os dados em cache do navegador e volta para a tela de login.
- Senhas novas precisam ter **no mínimo 10 caracteres**.

Limites que você precisa conhecer:

- **A segurança depende da força das senhas.** Qualquer pessoa pode baixar o arquivo e testar senhas offline, sem limite de tentativas. Use senhas longas e únicas (de preferência geradas por gerenciador de senhas) e não reaproveite senhas de outros sistemas.
- **Trocar ou remover uma senha não protege cópias antigas do arquivo.** Quem já tem uma versão antiga e a senha antiga continua abrindo aquela versão. Se alguém com acesso sair da empresa, considere recriar os acessos.
- **Quem entra consegue ler tudo.** Todo usuário logado vê todos os dados e, tecnicamente, consegue ler a lista de usuários e senhas pelo console do navegador. Não compartilhe a senha de administrador.
- O histórico do Git guarda cada versão do arquivo cifrado. Não faça commit de nenhuma versão com dados em texto aberto.

## Requisitos

- Chrome ou Edge atualizados para gravação direta no arquivo.
- Conexão com a internet apenas para carregar bibliotecas externas (ex.: planilhas Excel), conforme o `<head>` do arquivo.
