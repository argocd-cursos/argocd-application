## 🎯 Objetivo do Lab

Habilitar e observar a capacidade de self-healing do Argo CD para reverter automaticamente configuration drift e garantir Git como source of truth absoluta.

## 📝 Visao Geral e Conceitos

Self-healing e o mecanismo final de garantia em um fluxo GitOps. Enquanto o automated sync reage a mudancas no Git, o self-heal reage a mudancas feitas diretamente no cluster, corrigindo qualquer desvio em relacao ao estado desejado.

Neste lab, voce vai habilitar a politica `selfHeal` na aplicacao `guestbook`. Em seguida, vai alterar manualmente um recurso no cluster usando `kubectl`. Depois, vai observar a politica de self-heal do Argo CD detectar imediatamente esse desvio e reverter sua alteracao automaticamente, garantindo que o estado real do cluster sempre corresponda ao que esta declarado no Git.

## 📋 Tarefas do Lab

1.  Adicione a opcao `selfHeal: true` ao bloco `syncPolicy` do manifest `guestbook-app.yaml`.
2.  Aplique o manifest atualizado no cluster.
3.  Use o comando `kubectl scale` para alterar manualmente o numero de replicas do deployment `guestbook` para outro valor (ex.: `1`).
4.  Observe na UI do Argo CD a aplicacao ficando `OutOfSync` e sendo quase instantaneamente sincronizada de volta para o estado definido no Git.
5.  Verifique com `kubectl` que o numero de replicas foi restaurado para o valor especificado no `valuesObject` do manifest `Application`.

## 📚 Recursos Uteis

- [Argo CD - Automated Sync Policy (includes Self-Heal)](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)
- [Kubectl `scale` Command Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale)

## 💭 Perguntas de Reflexao

1. Por que automated sync sozinho nao corrige configuration drift (mudancas manuais no cluster), e como self-healing completa o ciclo de enforcement do GitOps?
2. Quais sao os riscos potenciais de habilitar pruning em um namespace compartilhado onde varios times fazem deploy de aplicacoes, e como mitigar esses riscos?
3. Em quais circunstancias voce poderia querer desabilitar temporariamente self-healing em producao, e quais os trade-offs disso?
