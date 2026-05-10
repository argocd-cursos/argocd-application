## 🎯 Objetivo do Lab

Conectar o Argo CD a um repositorio Git privado usando autenticacao SSH com deploy keys, implementando um padrao de seguranca nivel producao.

## 📝 Visao Geral e Conceitos

Embora autenticacao com usuario/PAT funcione bem, SSH deploy keys representam o padrao ouro para fluxos GitOps em producao. Uma deploy key e um par de chaves SSH dedicado que concede acesso a um unico repositorio, normalmente com permissao somente leitura.

As vantagens de deploy keys sobre PATs incluem:

- **Especifica por repositorio**: Cada chave concede acesso a apenas um repositorio
- **Sem dependencia de conta de usuario**: As chaves nao ficam atreladas a conta de uma pessoa
- **Somente leitura por padrao**: A maioria dos provedores Git reforca acesso read-only para deploy keys
- **Auditavel**: O uso de deploy keys e registrado separadamente da atividade de usuarios
- **Longa duracao**: Sem datas de expiracao para gerenciar

Neste lab, voce vai gerar um par de chaves SSH especificamente para o Argo CD, configurar o repositorio para aceitar essa chave e atualizar sua aplicacao para usar autenticacao SSH.

## 📋 Tarefas do Lab

1.  Crie um novo repositorio **privado** no seu provedor Git (ou use o do lab anterior)
2.  Se ainda nao fez isso, copie o chart `helm-guestbook` para esse repositorio e faca push
3.  Gere um novo par de chaves SSH na maquina local:
    - Use um nome descritivo (ex.: `argocd-deploy-key`)
    - **Nao use passphrase** (o Argo CD nao lida com chaves protegidas por passphrase)
4.  Adicione a **chave publica** ao seu repositorio como Deploy Key:
    - Va para as configuracoes do repositorio
    - Encontre a secao Deploy Keys
    - Adicione a chave publica com um titulo adequado
    - Garanta acesso **somente leitura**
5.  Na UI do Argo CD, adicione uma nova conexao de repositorio:
    - Metodo de conexao: SSH
    - Repository URL: URL SSH do seu repositorio privado (comeca com `git@`)
    - SSH private key: Cole o conteudo do arquivo da **chave privada**
6.  Teste a conexao para verificar que o Argo CD consegue autenticar
7.  Atualize o manifest da Application para usar a URL SSH do repositorio
8.  Aplique o manifest e confirme que a aplicacao sincroniza a partir do repositorio privado

## 📚 Recursos Uteis

- [Argo CD - Private Repositories Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/)
- [GitHub - Managing Deploy Keys](https://docs.github.com/en/developers/overview/managing-deploy-keys)
- [SSH Key Generation Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

## 💭 Perguntas de Reflexao

1. Por que SSH deploy keys sao consideradas o "padrao ouro" para fluxos GitOps em producao em comparacao com Personal Access Tokens, apesar de serem mais complexas de configurar?
2. Quais vantagens de seguranca surgem do fato de deploy keys serem especificas por repositorio em vez de amplas para conta de usuario, e como isso se alinha ao principio do menor privilegio?
3. Por que e importante que deploy keys usadas pelo Argo CD nao tenham passphrase, e como compensar essa consideracao de seguranca?
