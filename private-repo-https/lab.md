## 🎯 Objetivo do Lab

Conectar o Argo CD a um repositorio Git privado usando autenticacao HTTPS com usuario e Personal Access Token (PAT).

## 📝 Visao Geral e Conceitos

Em ambientes de producao, seus repositorios GitOps quase sempre serao privados. O Argo CD precisa de credenciais para acessar esses repositorios. O metodo mais simples de autenticacao e HTTPS com usuario e Personal Access Token (PAT).

Um Personal Access Token e como uma senha, mas desenhado para aplicacoes e nao para pessoas. Ele pode ter escopos com permissoes especificas (como acesso somente leitura aos repositorios) e pode ser revogado facilmente em caso de comprometimento. Isso torna PATs muito mais seguros do que usar sua senha real.

Neste lab, voce vai criar um repositorio privado, gerar um PAT no seu provedor Git e configurar o Argo CD para autenticar com essas credenciais.

## 📋 Tarefas do Lab

1.  Crie um novo repositorio **privado** no seu provedor Git (GitHub, GitLab etc.)
2.  Clone o repositorio privado na maquina local
3.  Copie o chart `helm-guestbook` do seu fork de `argocd-example-apps` para o novo repositorio privado
4.  Faca commit e push do chart para o repositorio privado
5.  Gere um Personal Access Token (PAT) no seu provedor Git com permissoes adequadas:
    - O token precisa de acesso de leitura aos repositorios
    - Defina uma data de expiracao apropriada
    - **Salve o token imediatamente** - voce nao conseguira visualiza-lo novamente
6.  Na UI do Argo CD, navegue ate Settings → Repositories
7.  Conecte um novo repositorio com:
    - Metodo de conexao: HTTPS
    - Repository URL: URL HTTPS do seu repositorio privado
    - Username: Seu usuario no provedor Git
    - Password: Seu Personal Access Token
8.  Teste a conexao para garantir que o Argo CD consegue acessar o repositorio
9.  Crie um novo manifest de Application (ex.: `guestbook-private.yaml`) apontando para seu repositorio privado
10. Aplique o manifest e verifique que a aplicacao sincroniza com sucesso a partir do repositorio privado

## 🔍 Conceitos-Chave

**Por que usar PAT em vez de senha?**

- PATs podem ter escopo minimo de permissao (principio do menor privilegio)
- PATs podem ser revogados individualmente sem trocar sua senha
- PATs podem ter data de expiracao para rotacao automatica
- Muitos provedores Git hoje exigem PAT para acesso de API

**Boas praticas de seguranca:**

- Use tokens somente leitura quando possivel
- Defina datas de expiracao razoaveis
- Armazene tokens com seguranca (nunca commit em Git!)
- Faca rotacao periodica dos tokens
- Revogue tokens quando nao forem mais necessarios

## 📚 Recursos Uteis

- [Argo CD - Private Repositories Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/)
- [GitHub - Creating a Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

## 💭 Perguntas de Reflexao

1. Por que Personal Access Tokens sao considerados mais seguros do que senhas para autenticacao entre servicos, mesmo funcionando de forma parecida?
2. Quais permissoes voce deve conceder a um PAT usado pelo Argo CD seguindo o principio do menor privilegio, e por que acesso somente leitura geralmente e suficiente?
3. Como a expiracao de token melhora a seguranca, e quais consideracoes operacionais ela introduz no seu fluxo GitOps?
