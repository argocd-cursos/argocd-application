## 🎯 Objetivo do Lab

Criar um Argo CD Project customizado com guardrails de seguranca e migrar uma aplicacao existente para ele.

## 📝 Visao Geral e Conceitos

Para escalar o Argo CD com seguranca, precisamos sair do projeto `default` irrestrito. Neste lab, voce vai definir um novo recurso `AppProject`. Esse projeto, chamado `team-finance`, vai aplicar duas politicas criticas: permitir deploy de aplicacoes apenas a partir de um repositorio Git especifico em whitelist e permitir deploy apenas para um namespace especifico em whitelist.

Apos criar o projeto, voce vai fazer deploy da aplicacao `guestbook` e associa-la ao `team-finance`, demonstrando como vincular aplicacoes a uma configuracao mais segura.

## 📋 Tarefas do Lab

1.  Crie um novo arquivo de manifest YAML para o recurso `AppProject` chamado `team-finance-project.yaml`.
2.  No manifest, defina um `AppProject` chamado `team-finance`.
3.  Configure o `spec` do projeto para aplicar guardrails:
    - Coloque em whitelist seu repositorio Helm privado (`argocd-example-apps-labs`) como unico `sourceRepos` permitido.
    - Coloque em whitelist o namespace `guestbook` como unico `destinations` permitido.
4.  Aplique o manifest `AppProject` no cluster para registra-lo no Argo CD.
5.  Modifique o manifest `guestbook-app.yaml`, trocando `spec.project` de `default` para `team-finance`.
6.  Aplique o manifest da aplicacao atualizado.
7.  Verifique na UI do Argo CD que a aplicacao `guestbook` agora pertence ao projeto `team-finance`.
8.  (Bonus) Teste as restricoes do projeto mudando temporariamente o namespace de destino da aplicacao para um nao permitido e observando o erro.

## 📚 Recursos Uteis

- [Argo CD Projects Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/projects/)
- [The `AppProject` CRD Specification](https://argo-cd.readthedocs.io/en/stable/operator-manual/api-docs/#argoproj.io/v1alpha1.AppProject)

## 💭 Perguntas de Reflexao

1. Por que o projeto `default` no Argo CD e considerado inseguro para ambientes multi-tenant?
2. O que acontece se voce tentar fazer deploy de uma aplicacao em um namespace que nao esta na whitelist do `AppProject`?
3. Como `AppProjects` ajudam na segregacao de permissoes para times diferentes (por exemplo, Time Financeiro vs. Time de Marketing)?
