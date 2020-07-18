# O que é Pull Request?

## Introdução

Antes de entender o que é um Pull Request é necessário entender os conceito de branchs , fork e o básico de repositórios git. Esse [site](http://rogerdudler.github.io/git-guide/index.pt_BR.html) pode lhe ajudar nisso.

Uma vez que você já sabe que a branch é uma ramificação do código, eu acrescento que o fork é uma cópia inteira do seu repositório. Essa cópia mantém uma ligação simbólica entre o fork e o repositório origem. De forma prática, essa ligação não tem grande efeito no uso do dia a dia, ou seja, se alguém fizer fork do seu repositório e fizer mudanças nesse fork o seu repositório não será afetado automaticamente.

O seu repositório original só poderá ser alterado com commits diretos ou através de um pull request.

Para evitar qualquer confusão, vamos dar nomes aos repositórios. Temos dois repositórios aqui:

**repositório original**, que é o primeiro repositório, aquele que foi a origem do fork.
**repositório fork**, que é a cópia exata do **repositório original** no momento do fork.

O pull request, é o pedido para que o **repositório original**, ou uma branch do **repositório original**, faça a ação de pull (puxar) as atualizações do **repositório fork** ou de um branch do próprio repositório. Confuso, né? Vamos para um exemplo.

Imagine que você tem um repositório que tem o código para um site, nesse site você recebe como entrada num campo o tempo de vida de um cachorro e você faz a conta para saber qual a "idade de cachorro" dele, depois de um tempo com esse projeto no ar uma pessoa muito interessada no seu projeto propõe colocar uma opção também para gatos.

Só pra alinhar e facilitar o entendimento, o nome do repositório exemplo é gomex/idade-de-animal.

Essa pessoa que pretende colaborar, ela faz um fork do seu repositório e agora ela tem um repositório chamado colaboradora/idade-de-animal (imaginando que o usuário dessa pessoa seja "colaboradora", ok?). Esse repositório tem uma ligação simbólica com o gomex/idade-de-animal.

![Repositório Fork](/images/pullrequest1.png)

Todas as mudanças feitas no "colaboradora/idade-de-animal" serão visíveis apenas nesse repositório e todas as mudanças feitas no "gomex/idade-de-animal" depois desse fork não serão automaticamente atualizadas no "colaboradora/idade-de-animal", mas por definição não seria um problema, afinal a funcionalidade que a pessoa está trabalhando deve ser específica, ou seja, o que ela está trabalhando não deveria conflitar com as alterações que acontecem no repositório original, ou seja, não tem mais ninguém além dela trabalhando em "idade de gato", né? Falaremos sobre resolução de conflitos depois.

Depois que a colaboradora adiciona a funcionalidade de calcular a idade de gato o que ela faz? Ela faz um pedido para que o repositório "gomex/idade-de-animal" puxe (pull em português) tudo que "colaboradora/idade-de-animal" tem diferente do seu repositório e agora essa diferença faça parte do repositório oficial. Isso é o pull request. Um pedido para que o repositório original se atualize a partir de mudanças feitas no repositório novo criado a partir de um fork.

Você pode estar se perguntando "E se alguém nesse meio tempo adicionou uma funcionalidade nova tipo 'idade de papagaio', isso pode afetar o pull request do idade de gato?" A resposta é: depende.

Se a funcionalidade for feita no mesmo local de código, mesmas linhas e afins, não terá problema, mas caso contrário teremos um conflito e trataremos disso em outro artigo.

A minha sugestão é funcionalidades diferentes sejam tratadas de forma isolada, a fim de não causar conflito algum no processo.

Todo esse processo que descrevi aqui, ele pode ser feito também baseado em branch, mas a pessoa que colabora precisa ser membro do repositório e não uma pessoa aleatória na internet, pois ela precisa ter permissão para criar branch no repositório. No fim é o mesmo propósito, mas ao invés de repositório inteiro, tudo que expliquei aqui acontece no nível de ramificações.

## Como usar Pull Request para o processo de revisão?

A maioria das organizações utiliza o pull request  como mecanismo padrão para revisão de código, pois ele é basicamente a "porta de entrada" para a base "oficial" de código, seja em relação ao repositório ou branch.

Normalmente as branchs que serão usadas para construir o artefato final do repositório oficial são protegidas e não podem receber commits diretos, ou seja, tudo que entra nessas branchs devem entrar por um PR (pull request). Existe a possibilidade do administrador do repositório mandar o código direto, mas isso deve ser apenas uma exceção. Dito isso, eu reforço, **mesmo os administradores do repositório**, **pessoas desenvolvedoras experientes**, ou até mesmo a **liderança técnica** do time devem mandar suas mudanças por PR e elas devem ser avaliadas por outras pessoas.

Quando começar a trabalhar em uma funcionalidade nova do repositório. Eu faço parte da organização? Tenho acesso a criar uma branch? Caso positivo, eu crio uma branch.

Existe um Padrão para criação de branch? Eu gosto do modelo "feature/nome-da-funcionalidade" assim fica muito claro para todo mundo no que você está trabalhando. Se você usa algum sistema de ticket para gerenciar as tarefas você pode colocar o identificador do ticket também: ""feature/nome-da-funcionalidade#435".

![Proposta de fluxo para pull request](/images/pullrequest2.png)

Lembre-se que sua branch precisa ser bem específica, ou seja, se "aparecer" outra demanda, o aconselhável é abrir outra branch a partir de branch "oficial" (que normalmente é a "master").

Quando você tiver muita confiança que seu código entrega tudo que a funcionalidade precisa para existir, você deve abrir um PR e na descrição desse PR você deve detalhar qual comportamento esperar dessa mudança que você está propondo.

Segue abaixo um ótimo exemplo:

![Exemplo de pull request](/images/pullrequest3.png)

O ideal é que o PR tenha o seguinte conteúdo:

 - Descrição clara e objetiva do comportamento que será adicionado caso o PR seja aceito;
 - Mínimo de detalhe sobre como a nova funcionalidade é usada, talvez um link para uma documentação externa seja uma boa, caso o detalhe seja muito grande;
 - Passo a passo sobre como testar, da forma **mais direta e clara** possível e quais comportamentos esperados para os testes executados, ou seja, se for para clicar, diga onde clicar e o que deve acontecer se clicar no lugar informado, se possível diga também o que **não** deve acontecer.

Uma descrição seguindo esse modelo ajudará a pessoa que vai avaliar seu PR e ela talvez não precisará lhe perguntar nada, pois tudo que precisa saber sobre o trabalho e como avaliar ele está descrito lá.

Acredite, cinco ou dez minutos investidos na criação de uma boa descrição de PR pode lhe "salvar" várias interrupções para explicação da sua mudança.

A dificuldade em escrever na descrição do seu PR é um possível indicativo que você não está confiante e não tem uma real noção sobre o que foi entregue. Imagine que talvez esse seja o momento de você organizar mentalmente o que foi entregue.

Algumas pessoas criam uma PR draft (rascunho) para ir atualizando a medida que vão mexendo no código. Eu gosto desse modelo, pois assim nada se perde e você não precisa relembrar de tudo que foi feito em horas de trabalho naqueles últimos minutos de trabalho antes de entregar sua tarefa.

## Como revisar o Pull Request?

A pessoa que vai olhar um PR ela precisa ter em mente alguns pontos:

 - Qual o objetivo daquele PR? Ele está claro na descrição? 
 - As mudanças que estão sendo propostas no PR seguem o padrão que é usado nessa organização?
 - A forma que a pessoa entregou à funcionalidade é a melhor? Existe maneira mais eficiente de fazer a mesma coisa? 
 - Os testes descritos no PR são o suficiente?

### Qual o objetivo daquele PR? Ele está claro na descrição? 

É importante estar **muito** claro sobre o que se trata o PR em questão, pois a sua avaliação será com base nisso, sendo assim, a primeira coisa a analisar é a clareza no que está sendo entregue, se houver qualquer inconsistência nesse momento você deve pontuar e deixar claro baseado em que está fazendo a avaliação.

Um exemplo:

Você abre o PR sobre idade de gatos, lê a descrição e não está claro pra ti se ideia é criar uma forma separada para calcular idade de outros animais ou apenas colocar uma opção na lógica atual feita para cachorro, sendo assim seu comentário poderia ser: 

"Não está claro pra mim se você colocou a lógica de calcular idade pra gato separado porque seja de fato a forma que você acha que seja ideal ou se fez isso apenas para não conflitar com o código original por agora e refatorar no futuro. Eu vou analisar seu código atual separado mesmo, mas adianto que mudar para que evite repetição de código seja uma boa no futuro"

Pronto, com isso você está dizendo que sua análise não levará em conta a quantidade de repetição de código que isso possa gerar e que você não concorda, mas que por agora não vê problemas nisso. 

### As mudanças que estão sendo propostas no PR seguem o padrão que é usado nessa organização?

A maioria das organizações seguem alguns padrões para como escrever código, seja em sua  formatação (ex. quatro espaço, ponto e vírgula em cima ou embaixo)  ou em como organizar funções, métodos e afins.

Esse padrão deve estar claro em algum lugar, e a pessoa que vai colaborar deve ler isso antes, mas nem sempre isso é possível e dessa forma a colaboração pode não seguir esse padrão. Você que está avaliando deve deixar bem claro para pessoa qual regra ela está infligindo e qual parte do código isso acontece. O github oferece a funcionalidade de comentar nas linhas do código do PR.

![Comentário no review](/images/pullrequest4.png)

Depois que comentar todo o PR, não se esqueça de finalizar sua revisão, pois caso não faça isso a pessoa que fez o PR não verá seu comentário.

![Revisão de PR](/images/pullrequest5.png)

Se precisar que a pessoa atualize algo para que o PR seja aceito escolha a opção de "Request changes" (Solicita mudanças), caso contrário aprove ou comente sem aprovar, caso precise de mais tempo para decidir sobre aceitar ou não.

### A forma que a pessoa entregou à funcionalidade é a melhor? Existe maneira mais eficiente de fazer a mesma coisa? 

Esse ponto é um pouco abstrato, pois depende muito da experiência de quem está revisando, mas é talvez a parte **mais importante** desse processo de revisão. Está aqui a grande oportunidade de uma pessoa proporcionar para a outra que mandou o PR maneiras de deixar o código ainda melhor. 

**ATENÇÃO!!!** Não faça uso desse espaço para diminuir ou ridicularizar a pessoa que mandou o PR, pois caso faça isso, além de perder uma grande oportunidade de melhorar a habilidade de outra que você "julga" inferior, você também perderá a oportunidade de ser uma pessoa melhor. Ajudar as pessoas que trabalham no mesmo projeto que você é a coisa mais básica de trabalhar em equipe. Caso tenha problemas em trabalhar dessa forma, aconselho criar um projeto onde você seja a única pessoa a enviar código.

Mais importante de que enviar as sugestões de mudança e apontar os erros do PR é validar se de fato isso é um erro ou uma abordagem diferente, da qual você discorda. 

Se sua sugestão melhorar a performance do que será entregue, tente mostrar algum elemento que embase sua sugestão.

Se sua sugestão tem como objetivo seguir uma boa prática, aponte o link para onde a pessoa possa ler mais sobre ela, e se possível, aponte caminhos para que a pessoa possa aplicar aquela melhor prática de uma forma mais fácil. Essa é uma boa oportunidade para exercitar seu uso dessa boa prática também.

### Os testes descritos no PR são o suficiente?

É importante avaliar se há testes o suficientes e não importa se os testes podem ser de exploração ou automatizados, você precisa praticar a avaliação disso. Não precisa ser uma pessoa especializada em QA (Quality Assurance) para fazer isso.

Ter uma pessoa QA no seu time é aconselhável, mas não ache que ela será a única a fazer essa análise. Nos primeiros PR você pode pedir a ajuda dela e fazer essa parte da avaliação juntas, mas aconselho que pratique o suficiente para internalizar esse tipo de revisão, pois a necessidade de entender qualidade de código, assim como segurança, "devops" ou afins deve ser de interesse de todos. Esses assuntos devem ser uma preocupação do **time** e não apenas de um cargo específico. A pessoa que está nesse cargo deve ser responsável por ajudar o time a evoluir nesse assunto, ajudando como uma espécie de consultor interno. Repito, essa pessoal **não** deve ser a única responsável sobre o assunto que é experiente.

## Recebi uma lista imensa de coisas a corrigir no meu PR, fico triste?

Caso a pessoa que comentou no seu PR foi respeitosa e teve cuidado ao criar o review, não há motivos para tristeza. Encare essa longa lista de correções como uma boa experiência para melhorar sua habilidade de escrever código.

A pessoa que mandou o PR não é necessariamente melhor do que você, ela apenas dedicou parte do seu tempo para escrever melhorias no seu trabalho. Ela teve a atenção e cuidado necessário para ajudar o time como um todo para entregar um código melhor para a organização. Ela provavelmente não tem nada contra você, e quanto mais detalhista ela foi, isso não é necessariamente uma coisa ruim, pois muitas vezes é a oportunidade que você terá para olhar nesse nível de detalhe o seu trabalho.

Normalmente os prazos em empresas são apertados e uma longa lista de correções desanima, mas veja que o problema está no prazo curto que normalmente as empresas trabalham. Nesse caso, o que pode ser feito se divide em duas possibilidades:

Você pode calcular no futuro o prazo levando em consideração esse nível de exigência na revisão
Negociar com a pessoa que revisou partes das críticas, tentando explicar sobre os prazos e afins. 

Uma dica aqui é fazer com que seu PR seja o menor possível, pois quanto menor a possibilidade de retrabalho no retorno da revisão é menor.

## Quantas pessoas devem revisar meu código?

Algumas empresas colocam no mínimo duas, outra colocam três, mas existem muitas que com apenas uma revisão já torna o PR disponível para ser de fato aceito. Isso depende da empresa.

O ideal seriam duas pessoas, mas se isso atrasar demais o andamento do seu projeto, uma deve ser o suficiente, mas lembre que essa pessoa a revisar terá **muito** mais responsabilidade e normalmente não poderá ser uma pessoa com pouca experiência. Isso quer dizer que você estará usando ainda mais o tempo de pessoas mais experientes para avaliação de código do que de fato produzindo códigos.

## Conclusão

O PR é um método ideal, simples, com possibilidade de interação assíncrona através de comentários no código, possibilidade de debate, múltiplas opiniões e uma forma centralizada de entender como seu código avançou ao passar do tempo, quais os motivos que deixaram determinado comportamento entrar no código e quais foram as argumentações que embasaram as decisões. 

Uma ferramenta ideal, que se usada sabiamente, pode ser muito poderosa!