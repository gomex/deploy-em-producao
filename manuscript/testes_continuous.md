## Testes Contínuos

Depois de entender quais testes fazem sentido dentro do seu projeto, é hora de encaixar todos esses testes dentro da esteira de desenvolvimento sempre pensando em como viabilizar a produtividade, por isso executamos os testes o mais cedo possível dentro do ciclo para encontrar os problemas e já corrigí-los antes que impactem o cliente final. Esse é o famoso conceito de *shift-left* que ouvimos por aí, trazer o feedback que os testes nos dão para o nosso dia a dia.

### Pré-Submit

Pré-submit é o momento antes de enviar as alterações para o repositório. Nesse momento devemos executar os testes que são mais rápidos e confiáveis, como os testes pequenos que vimos na seção anterior. Eles podem ser executados em segundos ou no máximo minutos e já vão informar se houver algum problema antes mesmo que isso vá para o repositório.

### Pós-Submit

Depois que o código é enviado para o repositório, partimos para o processo de integrar esse novo código no código já existente e garantir que tudo continua funcionando, aqui já executamos os testes médios. Se tudo correr bem, temos uma versão candidata que pode ser lançada em ambiente de teste.

### Versão Candidata

Depois que temos uma versão candidata já publicada em algum ambiente de testes é a hora de executar os testes grandes para validar que os fluxos de negócio estão funcionando de forma integrada com todos os componentes envolvidos. Depois dessa validação finalmente podemos enviar a nossa mudança para ambiente de produção. :rocket:

### Produção

Chegando em produção nós podemos reaproveitar os testes grandes e executar os que sejam mais relevantes para garantir que os fluxos principais continuam funcionando, além de se aproveitar das informações que recebemos dos monitoramentos (teremos uma seção dedicada a falar sobre isso nos próximos capítulos) para continuar observando que a mudança introduzida não causou nenhum problema.
