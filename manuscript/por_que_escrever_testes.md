## Por que escrever Testes

Quando falamos de teste, estamos basicamente falando de uma lista de passos que são executados seja manual ou automaticamente:

```
1. Em primeiro lugar você tem um comportamento que precisa ser verificado
2. Uma entrada que vai ser passada para o seu sistema
3. Uma saída que pode ser observada
```

Tudo isso dentro de um ambiente controlado. Quando você executa esse teste você descobre se o sistema se comporta ou não como o esperado. Há algum tempo atrás esse processo era feito majoritariamente de forma manual, despendendo muito tempo, às vezes meses, para conseguir dizer se uma versão podia ser implantada em produção ou não.

Com o passar do tempo esse trabalho manual foi substituído por testes automatizados que podem ser reaproveitados e executados de forma mais rápida e assertiva em relação aos manuais.

Mas nem tudo são flores nessa história, criar e manter uma suíte de testes automatizados saudável requer esforço. Existem muitos desafios envolvidos que vamos ver no decorrer dos próximos tópicos.

Mesmo com esses desafios é importante entender que escrever testes é parte do processo de entrega de software e ajuda a manter a entrega das qualidades. Isso não pode ser negociado. É um esforço que compensa quando você consegue pegar os bugs antes de chegar em produção ao invés de esperar que algum cliente ligue reclamando.

Além de aumentar a confiança na publicação de novas versões, os testes servem como uma documentação de como o software deveria se comportar e te obriga a pensar também no design do que está sendo desenvolvido, já que isso implica na dificuldade ou facilidade de criar testes.
