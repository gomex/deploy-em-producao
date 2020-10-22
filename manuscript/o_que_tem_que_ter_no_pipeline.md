# O que deve ter no seu pipeline? (Rafael Gomes)

#### Introdução

Antes de "colocar a mão na massa" e iniciar o processo de construção do seu pipeline, você precisa entender qual problema você está tentando resolver, pois toda intervenção na computação tem (ou deveria ter) como objetivo resolver algum problema, correto? Mesmo que o problema seja otimização, por conta de performance, ou trabalho proativo para que não exista problema no futuro.

Quando você inicia a construção de um pipeline, normalmente, seu objetivo é entregar um produto. Seja ele de software ou infraestrutura.

Se a solução do problema aqui é entregar o produto de forma automatizada, você precisa entender quais são os passos que seu produto precisa seguir para ser colocado em produção.

#### A ordem importa?

Antes de apresentar os passos, precisamos primeiro entender que a ordem das etapas do pipeline importam **e muito**, sendo assim apresentarei as etapas aqui na ordem que elas devem estar no seu pipeline.

##### Por que a ordem importa?

Uma das vantagens de usar pipeline no processo de entregar de software é a ideia dele "economizar" tempo das pessoas que estão produzindo código, então o ideal é que as tarefas que demoram menos, entregam algum valor e não dependem de outros passos posteriores sejam as primeiras no seu pipeline.

Vamos usar um exemplo abstrato. No processo de entrega de um software hipotético, temos os seguintes passos:

- Build do artefato
- Teste unitário
- Provisionamento da infra pré-produção
- Teste de integração
- Teste de aceitação
- Provisionamento de infra produção
- Deploy de pré-produção
- Deploy de produção

Na sua opinião, qual seria o primeiro? Vamos analisar alguns dos candidatos:

**Build do artefato** depende de outro passo? Não. Ele entrega valor? Entrega sim, pois se o build quebrar, a pessoa que está desenvolvendo saberá que tem problemas para fazer build, mas esse processo de build costuma demorar demasiadamente e isso pode fazer com que o feedback seja demorado. Vamos imaginar juntos: A pessoa manda o commit para o repositório, o pipeline automaticamente é executado e depois de alguns longos minutos a pessoa que mandou o commit poderá descobrir que errou, pois a etapa de build vai executar a construção do artefato e assim pegará qualquer problema que apareça nesse processo. Muitas vezes um detalhe bobo pode levar a quebrar o pipeline nessa etapa.

E se pensarmos no **teste unitário**? Depende de outro passo? Não. Ele entrega valor? Entrega sim, e aqui temos um detalhe diferente do **build do artefato**, pois o retorno é mais rápido, uma vez que, normalmente, nada precisa ser realmente construído. Por padrão os testes unitários demoram menos do que o build dos artefatos. Voltando ao processo de imaginação: A pessoa manda o commit para o repositório, o pipeline automaticamente é executado e depois de **segundos** ela já terá um feedback que um determinado teste não está passando. Tudo por culpa daquele "detalhe" bobo que falamos anteriormente.

Seguindo essa lógica, o primeiro passo desse pipeline seria o **teste unitário**, pois não há nada que demore menos e ainda assim não dependa de outro passo. Vejam que são sempre múltiplos fatores para determinar a ordem e em minha opinião são normalmente esses:

- Dependência de outro passo
- Entrega de valor
- Tempo de feedback

Quando falamos de entrega de valor, a preocupação é com o processo de desenvolvimento e não apenas com o produto finalizado. Para o produto finalizando o build talvez seja mais importante do que o teste unitário, pois levando em consideração o processo de desenvolvimento eles tem importâncias bem próximas.
