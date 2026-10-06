# Chave Mestra

Gestão de aluguéis (imóveis, pessoas, contratos, vistorias, recebimentos, despesas, manutenção, estoque e planilhas em Excel) e Caixa Mensal (receitas e despesas, projeção do ano e arquivo dos meses fechados).

Cada pessoa entra com e-mail e senha e vê só os próprios dados. A **conta mestra** libera quem pode usar o app e consegue abrir a conta de cada pessoa para acompanhar os aluguéis dela.

O site é estático (GitHub Pages). Login e dados ficam no **Firebase** (plano gratuito Spark): Authentication para as contas e Firestore para os dados, inclusive fotos e vídeos das vistorias.

## Arquivos

| Arquivo | Para quê |
|---|---|
| `index.html` | Página, visual e tela de login |
| `shell.js` | Login, contas, conta mestra, backup e ligação com o Firebase |
| `app.js` | O app em si (Aluguéis + Caixa Mensal) |
| `config.js` | Configuração do seu projeto Firebase |
| `firestore.rules` | Regras de segurança do banco (cole no Console do Firebase) |

## Como colocar no ar

### 1. Criar o projeto no Firebase
1. Acesse <https://console.firebase.google.com> e clique em **Criar projeto** (pode desativar o Google Analytics).
2. **Authentication → Começar → Método de login → E-mail/senha → Ativar → Salvar.**
3. **Firestore Database → Criar banco de dados** → local `southamerica-east1 (São Paulo)` → **modo de produção**.
4. Em **Firestore → Regras**, apague o que estiver lá, cole o conteúdo de `firestore.rules`, troque `EMAIL_DO_ADMIN` pelo e-mail da conta mestra e clique em **Publicar**.
5. Em **Configurações do projeto (engrenagem) → Seus apps → `</>` (Web)**, registre um app com o nome *Chave Mestra* (sem Hosting) e copie o bloco `firebaseConfig`.

### 2. Ligar o site ao Firebase
Abra `config.js` aqui no GitHub (ícone de lápis), cole os valores do `firebaseConfig` nos campos `COLE_AQUI` e salve (Commit changes).

### 3. Publicar no GitHub Pages
**Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / `(root)` → Save.**
Em um ou dois minutos o site fica em `https://SEU-USUARIO.github.io/chave-mestra/`.

Depois, no Firebase, vá em **Authentication → Configurações → Domínios autorizados → Adicionar domínio** e adicione `SEU-USUARIO.github.io`.

### 4. Primeiro acesso
1. Abra o site, clique em **Criar conta** e use o e-mail da conta mestra.
2. Confirme o e-mail pelo link que chega (veja o spam) e clique em **Já confirmei**.
3. Em **Minha conta → Importar backup** dá para carregar um backup (.json) com dados antigos.

### Amigos
Mande o link do site. A pessoa cria a conta e aparece em **Minha conta → Contas** para você clicar em **Liberar acesso**. Pelo mesmo painel você abre a conta dela para acompanhar.

## Backup
Em **Minha conta → Baixar backup** sai um arquivo `.json` com tudo da conta aberta (inclusive fotos). Guarde de vez em quando. O mesmo arquivo pode ser importado de volta.

## Limites do plano gratuito
O Firestore gratuito tem 1 GB de armazenamento e cotas diárias de leitura e escrita que sobram para uso pessoal e de alguns amigos. Fotos são reduzidas antes de salvar; vídeos podem ter até 20 MB cada.

## Para quem vai mexer no código (guia do projeto)

O site está **no ar e em uso**: os dados reais ficam no Firebase, não neste repositório. Trocar `app.js` ou `index.html` **não apaga dados**, mas um erro de código pode deixar o site fora do ar até ser corrigido.

**Regras**
- **Não altere** `config.js` nem `firestore.rules` sem combinar antes (ligação com o Firebase e segurança).
- Em `shell.js` (login, contas, backup) mude só o necessário.
- **Nunca** coloque neste repositório (que é público) backups `.json`, senhas ou chaves secretas do Firebase.
- Antes de mudar algo grande, baixe um backup em **Minha conta → Baixar backup**.

**Como o código está organizado (`app.js`)**
- A parte de cima é o **Caixa Mensal** (receitas, despesas, transferências, meses).
- A parte de baixo é **Aluguéis**. Cada aba é um módulo em `MODS` (pessoas, imóveis, contratos, vistorias, financeiro, despesas, manutenção, estoque, recibos), com campos (`fields`), colunas da lista (`cols`) e ações.
- Os dados de Aluguéis ficam nas coleções `cm_<módulo>` do Firestore; os do Caixa em `itens`, `meses`, `transf` e `config`.
- Contratos modelo: `MODELO`, `MODELO_COM`, `MODELO_TEMP` (o texto pode ser personalizado em Configurações; o salvo lá tem prioridade).

**Como publicar uma mudança**
1. Edite o arquivo aqui no GitHub (lápis) ou envie a versão nova em **Add file → Upload files**.
2. Escreva uma descrição curta e clique em **Commit changes**.
3. Em 1 a 2 minutos o site atualiza. Se aparecer a versão antiga, recarregue a página.
4. Para desfazer, abra o histórico do arquivo (**History**), copie a versão anterior e envie de novo.
