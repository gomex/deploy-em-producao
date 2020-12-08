## Testes Contínuos

Depois de entender quais testes fazem sentido dentro do seu projeto, é hora de encaixar todos esses testes dentro da esteira de desenvolvimento sempre pensando em como viabilizar a produtividade, por isso executamos os testes o mais cedo possível dentro do ciclo para encontrar os problemas e já corrigí-los antes que impactem o cliente final. Esse é o famoso conceito de *shift-left* que ouvimos por aí, trazer o feedback que os testes nos dão para o nosso dia a dia.

### Pré-Submit

Pré-submit é o momento antes de enviar as alterações para o repositório. Nesse momento devemos executar os testes que são mais rápidos e confiáveis, como os testes pequenos que vimos na seção anterior. Eles podem ser executados em segundos ou no máximo minutos e já vão informar se houver algum problema antes mesmo que isso vá para o repositório. Aqui podemos usar ferramentas específicas de cada linguagem para criar esse tipo de regra, se você está trabalhando com JavaScript, por exemplo, existe o [Husky](https://www.npmjs.com/package/husky) que você pode configurar no seu package.json com os comandos que serão executados automaticamente quando você fizer o `git push`. No caso do JavaScript por exemplo, você configuraria para executar `npm run test`.

```javascript
"husky": {
    "hooks": {
      "pre-push": "npm run test",
      "...": "..."
    }
  },
```

Você pode conferir o exemplo completo neste [repositório](https://github.com/samycici/auth-app/blob/master/package.json#L59-L64).

### Pós-Submit

Depois que o código é enviado para o repositório, partimos para o processo de integrar esse novo código no código já existente e garantir que tudo continua funcionando, aqui já executamos os testes médios. Se tudo correr bem, temos uma versão candidata que pode ser lançada em ambiente de teste. Esse é o primeiro passo do seu pipeline.

### Versão Candidata

Como resultado do passo anterior você terá sua versão candidata, ou seja, um pacote com o código já alterado e que passou por todos os testes. Depois que temos uma versão candidata já publicada em algum ambiente de testes é a hora de executar os testes grandes para validar que os fluxos de negócio estão funcionando de forma integrada com todos os componentes envolvidos. Aqui você pode ter de 1 a N ambientes de teste, algumas empresas tem o ambiente de staging, pré-prod, homolog, os nomes são variados, mas o importante é ter ambientes de teste onde a sua versão será publicada e testada antes de prosseguir para produção.

Depois dessa validação finalmente podemos enviar a nossa mudança para ambiente de produção. 🚀

### Produção

Chegando em produção nós podemos reaproveitar os testes grandes e executar os que sejam mais relevantes para garantir que os fluxos principais continuam funcionando, além de se aproveitar das informações que recebemos dos monitoramentos (teremos uma seção dedicada a falar sobre isso nos próximos capítulos) para continuar observando que a mudança introduzida não causou nenhum problema.
