## 🎯 Objetivo do Lab

Instalar o controlador do Argo CD e seus componentes no cluster usando o Helm chart oficial, garantindo deploy de uma versao especifica e repetivel.

## 📝 Visao Geral e Conceitos

Vamos usar Helm, o gerenciador de pacotes padrao do Kubernetes, para instalar o Argo CD. Esse e o metodo oficial e recomendado porque lida corretamente com todos os componentes Kubernetes do Argo CD. O processo envolve adicionar o repositorio Helm oficial do Argo Project, criar um namespace dedicado `argocd` para organizacao e usar um comando `helm install` com versao fixada para fazer o deploy do chart.

## 📋 Tarefas do Lab

1.  Adicionar o repositorio Helm oficial do Argo Project ao cliente Helm local.
2.  Atualizar os repositorios Helm para garantir as informacoes mais recentes de charts.
3.  Criar um namespace dedicado `argocd` no cluster.
4.  Instalar o chart `argo-cd` versao `8.6.0` no namespace `argocd`.
5.  Verificar que todos os pods do Argo CD foram criados e estao em estado `Running`.

## 📚 Recursos Uteis

- [Argo CD - Guia de Primeiros Passos](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- [Helm `install` Command Documentation](https://helm.sh/docs/helm/helm_install/)
- [Helm `repo` Command Documentation](https://helm.sh/docs/helm/helm_repo/)
- [Kubectl `create namespace` Command Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create-namespace)

## 💭 Perguntas de Reflexao

1. Por que e importante fixar uma versao especifica ao instalar Argo CD (ou qualquer componente critico de infraestrutura) em vez de usar a versao mais recente?
2. Por que criamos um namespace dedicado para Argo CD em vez de instalar no namespace `default`?
3. Quais sao as vantagens de usar Helm para instalar Argo CD em comparacao com aplicar YAML bruto diretamente com `kubectl`?
