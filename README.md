# Layout SRM

Ferramenta de gestão de layout de escritório (mesas, andares e colaboradores) da SRM, em **um único arquivo HTML** (`index.html`) com CSS e JavaScript embutidos. Não precisa de servidor nem de build.

## Estrutura do repositório

```
layout-srm/
├── index.html     # aplicação (+ cópia inicial dos dados criptografados)
├── data.json      # dados criptografados; é este arquivo que a publicação automática atualiza
├── README.md      # este arquivo
├── .gitignore
└── .nojekyll      # desliga o processamento Jekyll do GitHub Pages
```

## Como funciona o salvamento

Os dados (mesas, colaboradores, usuários) ficam criptografados no arquivo **`data.json`**. O `index.html` é só o programa; ele traz uma cópia inicial dos dados, usada como reserva (por exemplo, ao abrir o arquivo localmente).

- **Ao abrir o site**, o app baixa o `data.json` mais recente e pede login. Sem login válido, nada é legível.
- **Ao alterar algo como administrador**, o app grava o novo `data.json` (criptografado) no repositório pela API do GitHub. Várias alterações seguidas viram uma única publicação (espera de ~1,5 s).
- **Quem abre o site depois vê a alteração** quando o GitHub Pages terminar de publicar, em geral **1 a 2 minutos**.
- Cada publicação é um commit, então o histórico do Git guarda todas as versões (`git log data.json`).
- **Se outro acesso publicou uma versão mais nova**, o app avisa antes de sobrescrever.

### Ativar a publicação automática (uma vez por navegador do administrador)

1. No GitHub: **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
   - *Repository access*: **Only select repositories** → escolha só este repositório.
   - *Permissions → Repository permissions*: **Contents → Read and write**.
   - Defina uma validade (por exemplo, 90 dias).
2. Abra o site, entre como administrador e faça qualquer alteração. Aparece a janela **Publicar alterações automaticamente**.
3. Informe `usuario/repositorio`, a branch (`main`) e cole o token. Clique em **Testar e salvar**.

A partir daí as alterações são publicadas sozinhas, sem escolher arquivo. O token fica guardado só nesse navegador (clique em **✓ Sync** para ver ou remover). **Nunca coloque o token no `index.html`, no `data.json` nem no Git.** Se suspeitar de vazamento, revogue o token no GitHub.

Alternativa: em navegadores Chrome/Edge, a janela também oferece **Usar arquivo local**, que grava direto no `index.html` aberto localmente (a primeira vez pede para escolher o arquivo).

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
- **O token do GitHub dá poder de escrita no repositório.** Use token restrito a este repositório, com validade, e remova-o de navegadores que você não usa mais.

## Requisitos

- Chrome ou Edge atualizados para gravação direta no arquivo.
- Conexão com a internet apenas para carregar bibliotecas externas (ex.: planilhas Excel), conforme o `<head>` do arquivo.
