## 🎯 Objetivo do Lab

Usar anotacoes `sync-wave` para impor uma ordem especifica de deploy, garantindo que recursos dependentes sejam criados e fiquem saudaveis antes do inicio da aplicacao principal.

## 📝 Visao Geral e Conceitos

Neste lab, voce vai orquestrar um deploy em varias etapas usando Sync Waves. Para visualizar claramente o atraso entre waves, vamos criar uma cadeia de dependencias:

1.  **Wave 1**: Um `ConfigMap` representando uma configuracao de banco de dados.
2.  **Wave 2**: Um `Job` que simula uma verificacao de conectividade com banco. Ele vai ler a configuracao e aguardar alguns segundos.
3.  **Wave 3**: O `Deployment` principal da aplicacao, que deve iniciar somente apos a verificacao passar.

Ao sincronizar a aplicacao, voce vai observar o Argo CD pausar entre cada wave, aguardando os recursos da wave atual ficarem "Healthy" antes de seguir para a proxima.

## 📋 Tarefas do Lab

1.  No fork do seu repositorio, dentro da pasta `helm-guestbook`, crie um novo arquivo de manifest chamado `templates/database-config.yaml`.
2.  Defina um `ConfigMap` chamado `db-config` com dados ficticios (por exemplo, `host: "postgres"`).
3.  Adicione a anotacao `argocd.argoproj.io/sync-wave: "1"` ao `ConfigMap`.
4.  Crie outro arquivo chamado `templates/db-check-job.yaml`.
5.  Defina um `Job` Kubernetes que leia o `ConfigMap` via variavel de ambiente e rode um comando para imprimir os dados e aguardar 5 segundos.
6.  Adicione a anotacao `argocd.argoproj.io/sync-wave: "2"` ao `Job`.
7.  Abra o arquivo existente `templates/deployment.yaml`.
8.  Adicione a anotacao `argocd.argoproj.io/sync-wave: "3"` ao `Deployment`.
9.  Faca commit e push de todas as mudancas para seu repositorio Git.
10. Na UI do Argo CD, dispare um `SYNC` da aplicacao `guestbook`.
11. Observe cuidadosamente o processo de sync. Note as etapas na UI: ConfigMap -> Job (Running -> Completed) -> Deployment.

## 📚 Recursos Uteis

- [Argo CD - Sync Waves Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/sync-waves/)
- [Kubernetes Annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)
- [Kubernetes ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)

## 💭 Perguntas de Reflexao

1.  Como Sync Waves diferem de Resource Hooks (como PreSync)?
2.  O que acontece se o Job da Wave 2 falhar? A Wave 3 inicia?
3.  Por que voce usaria Sync Waves em vez de `initContainers` para gerenciar dependencias?
