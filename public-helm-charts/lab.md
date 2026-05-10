## 🎯 Objetivo do Lab

Fazer deploy de uma aplicacao no cluster usando Argo CD a partir de um repositorio publico de Helm chart, especificamente o Kubernetes Dashboard.

## 📝 Visao Geral e Conceitos

Embora repositorios Git com Helm charts sejam comuns, muitas aplicacoes sao distribuidas por repositorios publicos de charts Helm. O Argo CD consegue fazer deploy de forma nativa a partir desses repositorios publicos, como charts do Bitnami, oficiais do Kubernetes e muitos outros.

Neste lab, voce vai aprender a configurar um `Application` do Argo CD para fazer deploy a partir de um repositorio Helm publico. Em vez de apontar para um repositorio Git com um `path` para o diretorio do chart, voce vai usar o campo `chart` para referenciar o chart pelo nome e definir a URL do repositorio de charts.

Vamos fazer deploy do Kubernetes Dashboard, uma UI web para gerenciamento de cluster Kubernetes, hospedado no repositorio oficial de Helm charts do Kubernetes.

## 📋 Tarefas do Lab

1.  Crie um namespace chamado `k8s-dashboard`.
2.  Crie um novo arquivo de manifest `Application` do Argo CD chamado `k8s-dashboard-app.yaml`.
3.  Configure `spec.source` para deploy a partir de repositorio Helm publico:
    - Defina `repoURL` como `https://kubernetes.github.io/dashboard/` (repositorio Helm do Kubernetes Dashboard).
    - Em vez de `path`, use o campo `chart` e defina como `kubernetes-dashboard`.
    - Defina `targetRevision` como `7.13.0` para fixar uma versao especifica do chart.
4.  Configure `spec.destination`:
    - Defina `server` como `https://kubernetes.default.svc` (API server interno do cluster).
    - Defina `namespace` como `k8s-dashboard` (ou namespace de sua preferencia).
5.  Aplique o manifest para criar a Application no Argo CD.
6.  Abra a UI do Argo CD e observe a aplicacao sincronizando.
7.  Apos sincronizar, verifique com `kubectl` se os pods do Kubernetes Dashboard estao em execucao.
8.  (Opcional) Acesse o Kubernetes Dashboard com port-forward no service e explore a UI.

## 🔍 Diferencas-chave em relacao a charts baseados em Git

Ao fazer deploy de repositorio Helm publico em vez de repositorio Git:

- **`repoURL`**: Aponta para a URL do repositorio de charts Helm (nao para repositorio Git)
- **`chart`**: Especifica o nome do chart (substitui o campo `path` usado em repos Git)
- **`targetRevision`**: Refere-se a versao do chart (nao a commit/branch/tag Git)

## 📚 Recursos Uteis

- [Argo CD - Helm Chart Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/)
- [Kubernetes Dashboard Helm Chart](https://artifacthub.io/packages/helm/k8s-dashboard/kubernetes-dashboard)
- [Argo CD Application Specification](https://argo-cd.readthedocs.io/en/stable/operator-manual/application.yaml)

### Acessando o Dashboard

Use port-forward para acessar localmente:

```bash
kubectl port-forward svc/k8s-dashboard-kong-proxy 8443:443 -n k8s-dashboard
```

Depois acesse em: `https://localhost:8443`

**Observacao:** voce precisara criar service account e token para login:

```bash
# Criar service account
kubectl create serviceaccount k8s-dashboard-admin -n k8s-dashboard

# Criar cluster role binding
kubectl create clusterrolebinding k8s-dashboard-admin \
  --clusterrole=cluster-admin \
  --serviceaccount=k8s-dashboard:k8s-dashboard-admin

# Obter o token
kubectl create token k8s-dashboard-admin -n k8s-dashboard
```

Use o token gerado para fazer login no dashboard.

## 💭 Perguntas de Reflexao

1. Quais sao as diferencas principais entre usar o campo `chart` e o campo `path` em uma Application do Argo CD, e quando usar cada um?
2. Por que e importante fixar um `targetRevision` (versao do chart) especifico ao fazer deploy de repositorio Helm publico em vez de usar "latest"?
3. Como fazer deploy de repositorio Helm publico afeta os principios de GitOps, ja que o chart em si nao fica armazenado no seu repositorio Git?
