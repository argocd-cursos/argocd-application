## 🎯 Objetivo do Lab

Ver o loop central do GitOps em acao ao fazer uma mudanca declarativa em um repositorio Git e observar o Argo CD detectar e aplicar automaticamente no cluster.

## 📝 Visao Geral e Conceitos

Este lab e o momento "Aha!" do GitOps. Ate aqui, mandamos o Argo CD acompanhar um repositorio publico que nao controlamos. Para ver a magia acontecer, precisamos alterar o estado desejado. Para isso, voce vai primeiro fazer um fork do repositorio de aplicacoes de exemplo para sua conta no GitHub. Em seguida, vai atualizar o manifest `Application` do Argo CD para apontar para seu fork. Por fim, vai fazer uma mudanca simples em um manifest no repo forkado, alterando o numero de replicas de um Deployment, e observar na UI o Argo CD detectar automaticamente o drift e sincronizar o cluster para refletir a mudanca.

## 📋 Tarefas do Lab

1.  Navegue ate o repositorio GitHub `westibcar/argocd-example-apps` e crie um fork pessoal.
2.  Edite o arquivo `guestbook-app.yaml` e atualize `spec.source.repoURL` para a URL do seu repositorio forkado.
3.  Aplique esse manifest `Application` atualizado no cluster para informar o Argo CD da nova origem.
4.  No seu **repositorio forkado**, encontre e edite o arquivo `guestbook/guestbook-ui-deployment.yaml`.
    1.  Altere o valor de `spec.replicas` de `1` para `3`.
    2.  Faca commit e push dessa mudanca.
5.  Volte para a UI do Argo CD e observe. O status da aplicacao vai para `OutOfSync`. Isso pode levar ate 3 minutos, pois esse e o intervalo padrao de verificacao de mudancas do Argo CD.
6.  Use a UI para disparar uma operacao de Sync.
7.  Use `kubectl` para verificar que agora existem tres pods `guestbook-ui` em execucao no namespace `default`.

## 📚 Recursos Uteis

- [GitHub Docs - Fork a repo](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
- [Argo CD - Sync Policies](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/) (Explica como o auto-sync funciona)

## 💭 Perguntas de Reflexao

1. Por que fazer fork do repositorio e necessario para vivenciar de fato o fluxo GitOps, em vez de apenas observar mudancas em um repositorio que voce nao controla?
2. O que significa quando o Argo CD mostra uma aplicacao como `OutOfSync`, e por que ele nao sincroniza automaticamente por padrao?
3. Em um ambiente real de producao, que passos ou protecoes adicionais voce gostaria entre push no Git e sync no cluster?
