## 🎯 Objetivo do Lab

Acessar a web UI do Argo CD e instalar a interface de linha de comando (CLI) para ter controle total da instalacao.

## 📝 Visao Geral e Conceitos

O Argo CD ja esta em execucao no cluster, mas por padrao ele nao fica exposto externamente. Para interagir com ele, precisamos conectar de forma segura. Neste lab, primeiro vamos recuperar a senha inicial de admin gerada automaticamente a partir de um secret do Kubernetes. Em seguida, vamos usar `kubectl port-forward` para criar um tunel seguro da maquina local ate o pod do servidor Argo CD. Isso vai permitir login pelo navegador. Por fim, vamos instalar a CLI `argocd` para operacoes por linha de comando e fazer login por ela tambem.

## 📋 Tarefas do Lab

1.  Recuperar a senha inicial do usuario `admin` a partir do secret `argocd-initial-admin-secret` do Kubernetes e decodifica-la.
2.  Usar `kubectl port-forward` para deixar a UI do Argo CD acessivel em `localhost:8080`.
3.  Fazer login na web UI do Argo CD no navegador com usuario `admin` e a senha recuperada.
4.  Instalar a ferramenta CLI `argocd` na maquina local.
5.  Fazer login no servidor de API do Argo CD usando a CLI `argocd`.

## 📚 Recursos Uteis

- [Argo CD Docs - Log In To The UI / CLI](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)
- [Argo CD CLI Installation Guide](https://argo-cd.readthedocs.io/en/stable/cli_installation/)
- [Kubectl `port-forward` Command Documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward)

## 💭 Perguntas de Reflexao

1. Por que `kubectl port-forward` e uma opcao para acessar o Argo CD em desenvolvimento, e por que pode nao ser adequada para producao?
2. Quais implicacoes de seguranca voce deve considerar apos recuperar a senha inicial de admin, e qual e a melhor pratica para gerencia-la?
3. Em quais cenarios voce prefere usar a CLI do Argo CD em vez da web UI, e vice-versa?
