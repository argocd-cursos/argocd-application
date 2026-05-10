## 🎯 Objetivo do Lab

Definir e fazer o deploy da sua primeira aplicacao gerenciada por GitOps usando o recurso central do Argo CD: o `Application` CRD.

## 📝 Visao Geral e Conceitos

O coracao do Argo CD e uma Custom Resource Definition (CRD) chamada `Application`. Esse recurso e um manifest declarativo que funciona como um contrato, dizendo ao Argo CD tres coisas principais: **onde** o estado desejado esta definido (um repositorio Git), **o que** fazer deploy (um caminho e versao especificos nesse repo) e **onde** fazer deploy (cluster e namespace de destino).

Neste lab, voce vai escrever um manifest de `Application` do zero. Voce vai fazer commit desse arquivo no seu repositorio Git. Depois, vai executar um `kubectl apply` pontual nesse manifest para fazer o "bootstrap" da aplicacao e registra-la no Argo CD. A partir desse momento, o Argo CD assume o controle, buscando manifests no repositorio de origem e fazendo deploy no namespace de destino.

## 📋 Tarefas do Lab

1.  Crie um novo arquivo YAML para o recurso `Application`.
2.  Defina o `metadata` do `Application`, com nome `guestbook` e no namespace `argocd`.
3.  Defina `spec.project` como `default` e `spec.source` apontando para um repositorio Git publico com manifests Kubernetes simples. Para aplicacao demo, voce pode usar:
    - **Repo URL:** `https://github.com/westibcar/argocd-example-apps.git`
    - **Revision:** `HEAD`
    - **Path:** `guestbook`
4.  Defina `spec.destination` para fazer deploy no cluster local (`https://kubernetes.default.svc`) no namespace `default`.
5.  Aplique o manifest `Application` no cluster usando `kubectl apply`.
6.  Verifique na UI do Argo CD se a aplicacao `guestbook` aparece e sincroniza com sucesso.
7.  Use `kubectl` para verificar que os recursos da aplicacao `guestbook` (Deployments, Services) estao em execucao no namespace `default`.

## 📚 Recursos Uteis

- [Argo CD Application CRD Specification](https://argo-cd.readthedocs.io/en/stable/operator-manual/api-docs/#argoproj.io/v1alpha1.Application)
- [Declarative Setup in Argo CD](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/)
- [The `argocd-example-apps` Repository](https://github.com/argoproj/argocd-example-apps)
- [Meu Fork do repositorio `argocd-example-apps`](https://github.com/westibcar/argocd-example-apps)

## 💭 Perguntas de Reflexao

1. Por que voce acha que o recurso `Application` fica no namespace `argocd`, mas a aplicacao que ele faz deploy vai para o namespace `default`?
2. Qual e o significado de usar `HEAD` como `targetRevision`, e quais alternativas voce usaria em producao?
3. Como armazenar o manifest de `Application` no Git se alinha aos principios de GitOps, e quais beneficios isso traz?
