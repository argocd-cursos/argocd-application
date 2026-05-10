## 🎯 Objetivo do Lab

Configurar e fazer deploy de um hook `PreSync` usando um `Job` do Kubernetes para executar uma tarefa _antes_ do deploy principal da aplicacao.

## 📝 Visao Geral e Conceitos

Muitas aplicacoes exigem tarefas de preparo, como migracao de banco, antes do deploy da propria aplicacao. Neste lab, voce vai implementar esse padrao com um hook `PreSync` do Argo CD. Voce criara um manifest padrao de `Job` do Kubernetes dentro do diretorio `templates` do Helm chart. Esse `Job` vai simular uma migracao de banco imprimindo uma mensagem e aguardando alguns segundos. Ao adicionar anotacoes especificas do Argo CD nesse `Job`, voce vai instruir o Argo CD a executa-lo ate a conclusao durante a fase `PreSync`, antes de tentar sincronizar o `Deployment` e o `Service` principais.

## 📋 Tarefas do Lab

1.  No fork do seu repositorio, crie um novo arquivo de manifest na pasta `helm-guestbook` com o nome `templates/presync-job.yaml`.
2.  Defina um recurso `Job` padrao do Kubernetes nesse novo arquivo.
3.  O container do `Job` deve usar uma imagem simples `busybox`. O comando deve imprimir uma mensagem como "Executando migracoes de banco...", dormir por 10 segundos e sair com sucesso.
4.  Adicione a anotacao `argocd.argoproj.io/hook: PreSync` no metadata do `Job` para atribuir a fase PreSync.
5.  Adicione a anotacao `argocd.argoproj.io/hook-delete-policy: BeforeHookCreation,HookSucceeded` para garantir limpeza do job apos sucesso e permitir nova execucao em syncs seguintes.
6.  Faca commit e push do novo manifest `presync-job.yaml` para seu repositorio Git.
7.  Na UI do Argo CD, dispare manualmente um `Sync` da aplicacao `guestbook`.
8.  Observe o status de sync da aplicacao e a arvore de recursos. Repare que o recurso `Job` aparece e roda primeiro; somente depois do sucesso dele o `Deployment` e os demais recursos sincronizam.

## 📚 Recursos Uteis

- [Argo CD - Resource Hooks](https://argo-cd.readthedocs.io/en/stable/user-guide/resource_hooks/)
- [Kubernetes `Job` Documentation](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [Kubernetes Annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)

## 💭 Perguntas de Reflexao

1. Por que e importante usar `BeforeHookCreation` na hook delete policy para jobs que rodam em todo sync?
2. Como um hook `PreSync` difere de um `initContainer` padrao em um Pod?
3. O que acontece com o deploy principal da aplicacao se o hook `PreSync` falhar?
