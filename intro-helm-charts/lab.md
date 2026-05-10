## 🎯 Objetivo do Lab

Refatorar um `Application` existente do Argo CD para fazer deploy da mesma aplicacao, mas desta vez a partir de um Helm chart localizado no repositorio Git.

## 📝 Visao Geral e Conceitos

Fazer deploy de aplicacoes com YAML simples e otimo, mas muitas aplicacoes reais sao empacotadas como Helm charts para gerenciar complexidade e templating. Neste lab, voce vai aprender a adaptar um manifest de `Application` do Argo CD para fazer deploy a partir de um Helm chart em vez de um diretorio de manifests puros.

Vamos usar um Helm chart pronto para a aplicacao `guestbook`, que ja esta no repositorio de exemplos. Voce vai modificar o manifest `guestbook-app.yaml`, alterando `spec.source` para apontar para esse Helm chart, e observar o Argo CD fazer a transicao da aplicacao em execucao para gerenciamento via chart.

## 📋 Tarefas do Lab

1.  Explore o diretorio `helm-guestbook` no repositorio `argocd-example-apps` para se familiarizar com a estrutura simples do Helm chart.
2.  Abra o manifest `guestbook-app.yaml` para edicao.
3.  Modifique a secao `spec.source` do manifest:
    - Altere o `repoURL` para apontar para seu fork do repositorio `westibcar/argocd-example-apps`.
    - Defina `path` como `helm-guestbook`.
    - Adicione um novo bloco `helm`.
    - Dentro do bloco `helm`, adicione uma entrada em `valueFiles` apontando para o `values.yaml` padrao do chart.
4.  Aplique o manifest atualizado no cluster.
5.  Abra a UI do Argo CD e aguarde a aplicacao ficar `OutOfSync`.
6.  Dispare uma operacao de `Sync` e observe na UI do Argo CD a sincronizacao. Repare que, embora a origem tenha mudado de forma significativa, os recursos implantados permanecem os mesmos, mostrando as capacidades de diff do Argo CD.
7.  Verifique com `kubectl` que os pods do `guestbook` continuam em execucao.

## 📚 Recursos Uteis

- [Argo CD - Helm Chart Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/)
- [Helm `valueFiles` Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/#values-files)

## 💭 Perguntas de Reflexao

1. Por que a maioria das aplicacoes reais usa Helm charts em vez de YAML puro do Kubernetes, e que complexidade o Helm ajuda a gerenciar?
2. Como a capacidade do Argo CD de detectar que os mesmos recursos estao sendo implantados (apesar da troca de manifests puros para Helm chart) demonstra seu diff sofisticado?
3. Quais as vantagens de armazenar Helm charts em repositorios Git em vez de distribui-los por repositorios de charts?
