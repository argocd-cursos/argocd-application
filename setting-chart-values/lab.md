## 🎯 Objetivo do Lab

Aprender a customizar deploys de Helm chart no Argo CD definindo valores personalizados por diferentes metodos de override, e entender a ordem de precedencia quando houver conflito.

## 📝 Visao Geral e Conceitos

Uma das funcionalidades mais poderosas dos Helm charts e a capacidade de configuracao por valores. Ao fazer deploy de charts com Argo CD, voce precisa saber como sobrescrever valores padrao para customizar seus deploys em diferentes ambientes e casos de uso.

O Argo CD oferece tres metodos principais para override de valores Helm dentro do bloco `spec.source.helm`:

1. **`valueFiles`**: Referencia um ou mais arquivos de valores do seu repositorio Git
2. **`valuesObject`**: Define overrides diretamente no manifest da Application, como YAML estruturado
3. **`parameters`**: Sobrescreve valores individuais com pares chave-valor

### Entendendo a precedencia de valores

Quando o mesmo valor e definido em varios lugares, o Argo CD segue uma ordem estrita de precedencia (da menor para a maior prioridade):

1. `values.yaml` do proprio chart (padrao)
2. `valueFiles`
3. `valuesObject` / `values`
4. `parameters` (maior prioridade)

Isso significa que `parameters` sempre prevalece sobre `valuesObject`, que prevalece sobre `valueFiles`, que prevalece sobre o `values.yaml` padrao do chart.

## 📋 Tarefas do Lab

### Parte 1: Explorar os valores padrao

1. Navegue ate seu fork do repositorio `argocd-example-apps`
2. Abra o arquivo `helm-guestbook/values.yaml` para ver os valores padrao do chart
3. Anote o valor padrao de `replicaCount`

### Parte 2: Override com `valueFiles`

1. No repositorio `argocd-example-apps`, crie um novo arquivo chamado `values-custom.yaml` no diretorio `helm-guestbook`
2. Nesse arquivo, defina:
   - `replicaCount: 3`
3. Faca commit e push desse arquivo para o repositorio
4. Atualize o manifest `guestbook-app.yaml` da Application para usar esse arquivo de valores:
   ```yaml
   helm:
     valueFiles:
       - values-custom.yaml
   ```
5. Aplique o manifest atualizado e sincronize a aplicacao na UI do Argo CD
6. Verifique com `kubectl` que agora existem 3 replicas em execucao

### Parte 3: Override com `valuesObject`

1. Atualize `guestbook-app.yaml` para substituir `valueFiles` por `valuesObject`:
   ```yaml
   helm:
     valuesObject:
       replicaCount: 2
       service:
         type: NodePort
   ```
2. Aplique o manifest e sincronize no Argo CD
3. Verifique que agora apenas 2 replicas estao em execucao e o tipo de service mudou para NodePort
4. Observe como `valuesObject` substituiu completamente a configuracao anterior

### Parte 4: Override com `parameters`

1. Atualize `guestbook-app.yaml` para adicionar `parameters`, mantendo `valuesObject`:
   ```yaml
   helm:
     valuesObject:
       replicaCount: 2
       service:
         type: NodePort
     parameters:
       - name: replicaCount
         value: '1'
   ```
2. Aplique o manifest e sincronize
3. Verifique que agora apenas 1 replica esta em execucao
4. Observe que o valor em `parameters` (1) sobrescreveu o valor em `valuesObject` (2)

## 📚 Recursos Uteis

- [Argo CD - Helm Values Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/helm/#values-files)
- [Helm Values Documentation](https://helm.sh/docs/chart_template_guide/values_files/)
- [Argo CD Application Specification](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications)

## 💡 Boas praticas

- **Use `valueFiles`** para configuracoes por ambiente (dev, staging, prod)
- **Use `valuesObject`** para overrides estruturados que precisam ficar visiveis no manifest da Application
- **Use `parameters`** com parcimonia, normalmente para valores pontuais que devem sobrescrever todos os outros
- Lembre da ordem de precedencia ao investigar valores inesperados

## 💭 Perguntas de Reflexao

1. Por que entender a ordem de precedencia (padrao do chart → valueFiles → valuesObject → parameters) e critico ao investigar por que um valor especifico nao esta sendo aplicado como esperado?
2. Em quais cenarios voce escolheria `valueFiles` em vez de `valuesObject`, e quais os trade-offs entre guardar configuracao em arquivos no Git versus inline no manifest da Application?
3. Considerando que `parameters` tem a maior precedencia, por que esse metodo deve ser usado com parcimonia em vez de para todos os overrides?
