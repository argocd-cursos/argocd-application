## 🎯 Objetivo do Lab

Configurar uma aplicacao do Argo CD para sincronizar automaticamente quando uma mudanca for detectada no Git e remover recursos que nao estao mais definidos na source of truth.

## 📝 Visao Geral e Conceitos

Um fluxo GitOps realmente automatizado nao exige intervencao manual. Neste lab, voce vai habilitar duas politicas cruciais de sync na aplicacao `guestbook`: `automated` e `prune`. A politica `automated` diz ao Argo CD para aplicar mudancas do Git automaticamente, sem precisar clicar em "Sync". A politica `prune` garante que, se voce remover um manifest do repositorio Git, o Argo CD remova automaticamente o recurso correspondente do cluster.

Voce vai testar isso adicionando primeiro um novo `ConfigMap` ao Helm chart e observando o deploy automatico. Depois, vai remover o manifest de `ConfigMap` do chart e observar o Argo CD fazer o prune no cluster, mantendo o ambiente em sincronia com o Git.

## 📋 Tarefas do Lab

1.  Adicione um bloco `syncPolicy` ao manifest `guestbook-app.yaml`.
2.  Dentro de `syncPolicy`, defina `automated` como um objeto vazio `{}`.
3.  Aplique o manifest atualizado no cluster para atualizar as configuracoes da aplicacao.
4.  No Helm chart, adicione um novo manifest para um `ConfigMap` simples.
5.  Commit e push dessa mudanca no seu repositorio Git.
6.  Observe na UI do Argo CD a mudanca sendo detectada e sincronizada automaticamente, sem cliques manuais.
7.  Verifique com `kubectl` que o novo `ConfigMap` existe.
8.  Agora, remova o arquivo de manifest do `ConfigMap` do Helm chart.
9.  Commit e push dessa remocao.
10. Observe na UI do Argo CD o recurso sendo marcado como inexistente, mas nao removido imediatamente.
11. Habilite o pruning mudando a opcao `automated` de objeto vazio para `prune: true`.
12. Aplique o manifest atualizado no cluster para atualizar as configuracoes da aplicacao.
13. Observe na UI do Argo CD a politica `prune` entrando em acao e removendo automaticamente o `ConfigMap` do cluster.
14. Verifique com `kubectl` que o `ConfigMap` foi removido.

## 📚 Recursos Uteis

- [Argo CD - Automated Sync Policy](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)
- [Kubernetes ConfigMaps Documentation](https://kubernetes.io/docs/concepts/configuration/configmap/)

## 💭 Perguntas de Reflexao

1. Por que o automated sync nao remove recursos automaticamente quando os manifests sao removidos do Git, e qual politica especifica e necessaria para habilitar esse comportamento?
2. O que pode acontecer se voce habilitar pruning em uma aplicacao que compartilha namespace com recursos gerenciados por outras ferramentas ou times, e como evitar delecoes acidentais?
3. Em que cenario voce escolheria habilitar automated sync mas deixar pruning desabilitado de forma intencional, e quais sao os trade-offs dessa configuracao?
