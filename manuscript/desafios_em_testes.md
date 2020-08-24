## Desafios

Agora que já entendemos a importância de ter testes para garantir nossas entregas em produção é importante falarmos um pouco sobre os desafios que ter esses testes automatizados podem trazer e algumas sugestões sobre como lidar com eles.

### Testes Intermitentes

Como vimos anteriormente, temos diferentes tipos de teste e cada um deles abraça diferentes níveis dentro da nossa aplicação. Alguns estão apenas no escopo reduzido de uma função, enquanto outros exercitam diferentes componentes incluindo serviços externos e infraestrtura. Quanto mais integrados os testes são maior a chance de serem intermitentes. Testes intermitentes são aqueles que uma hora passam e na outra falham sem nenhum motivo aparente. É inevitável ter algum nível de intermitência posto que trabalhamos cada vez mais com aplicações complexas e cheias de integração, a diferença está na estratégia que adotamos para mitigar esse risco de intermitência. A primeira dica aqui é não testar serviços de terceiros dentro do seu fluxo de desenvolvimento, prefira mockar esses componentes porque caso esse serviço externo esteja indisponível em ambiente de teste, seu fluxo não será prejudicado. Testes intermitentes podem ser causados por componentes internos também, por exemplo se o seu teste utiliza dados de um teste anterior para seguir e esse teste anterior falhar ou demorar mais do que o normal, seu teste irá falhar por um motivo externo a ele mesmo. 

Ao se deparar com um teste intermitente, a primeira coisa que você deve fazer é movê-lo para uma outra suíte, que normalmente chamamos de quarentena e ali analisar esse teste de forma isolada e sem atrapalhar o dia a dia do time.

### Consumo de Recursos

Outro ponto importante quando falamos de testes médios e grandes é o consumo de recursos. Nem sempre você terá disponível um ambiente cópia de produção para executar os seus testes, seja pela dificuldade de reprodução desse ambiente ou até mesmo pelo custo de mantê-lo. Para isso você pode adotar algumas estratégias que te auxiliem a ter um ambiente que atenda a todas as necessidades. Para componentes externos você pode utilizar um serviço como [WireMock](http://wiremock.org/) para ser o servidor dublê desse componente externo. Você pode também utilizar um ambiente compartilhado com outros times (aqui tome cuidado porque essa decisão pode aumentar sua intermitência posto que existem várias pessoas manipulando o mesmo ambiente).

### Gestão das falhas

Por último, é muito importante fazer a gestão dos testes que falharem e para isso é necessário ter visibilidade do que está acontecendo através de relatórios de execução de testes e fluxos automáticos que parem a "linha de produção" quando algum teste falha. Isso vai te ajudar a entender melhor qual foi o motivo da falha do teste, se é um caso de intermitência ou um bug mesmo. 
