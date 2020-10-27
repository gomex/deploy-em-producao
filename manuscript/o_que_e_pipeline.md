# O que é pipeline

## Introdução

O pipeline usado para entregar software segue o mesmo conceito usado normalmente nas indústrias, que é uma esteira metálica que faz o produto "se mover" por dentro da fábrica, e os robôs, que estão parados, montam o produto à medida que ele passa.

Usando o exemplo da fabricação de carros, primeiro a estrutura metálica do carro (chassi) é colocado no pipeline e ela é movimentada pela fábrica, o primeiro robô, que está parafusado no chão, é responsável por pintar o chassi completamente. Sendo assim a cada chassi colocado no pipeline o primeiro robô pintará ele automaticamente.

Quando o chassi chega no primeiro robô, o pipeline para, pois o robô precisa de um tempo no processo de pintura, e somente após terminar o pipeline se movimenta novamente, para que o chassi seguinte seja movido para passar pela etapa de pintura e assim sucessivamente.

O segundo robô nesse pipeline é responsável por colocar as rodas e nesse processo o pipeline também espera ele terminar, e somente quando ele acaba o pipeline se move novamente.

No pipeline de software é bem parecido, sendo que ao invés do chassi, nesse modelo temos o código como "objeto" a ser movido pela esteira.

A grande diferença entre os modelos está na natureza do "objeto" usado para construir o produto. No caso do pipeline de carros, o chassi tenderá a sempre ser o mesmo, mas no caso do código é o exato oposto. O código tenderá a ser sempre diferente. Cada execução do pipeline, o "objeto" normalmente será diferente. 

## Como inicia um pipeline?

Na entrega de software o código vem do controle de versão, que é o local onde ficará armazenado todo código produzido pelo time de desenvolvimento.

O repositório de código armazenado no controle de versão normalmente tem uma ligação com a ferramenta responsável pelo pipeline de software, sendo assim é correto dizer que,  em casos como esses, quando o código é modificado essa ação automaticamente ativa a execução do pipeline, ou seja, uma pessoa adicionou uma linha nova dentro do código? O pipeline será iniciado automaticamente com base nesse fonte atualizado.

## Etapas do pipeline de software

O código é colocado na "esteira" e ela caminha de forma parecida com o que foi usado no exemplo da montagem do carro, ou seja, o código chega no primeiro "robô" e ele executa uma função específica e repetitiva. Esse momento é normalmente chamado de **"step"**. A tradução para português seria **"etapa"**, mas usaremos o termo em inglês, pois além de introduzir e fixar um termo tão importante nesse idioma, é esse nome que é usado em boa parte das ferramentas de mercado.

É correto dizer que a execução completa de um pipeline é a "movimentação" do código por múltiplos **steps**, ou seja, o código é depositado na esteira e transferido para o primeiro **step** que fará a primeira intervenção no código, que pode ser uma validação estática do código. Esse mesmo código, que foi validado na primeira **step** passa para uma nova, que pode ser a responsável por transformar esse código em um executável, e na posterior esse artefato é armazenado em repositório.

![Pipeline com três steps](resources/images/o_que_e_pipeline1.png)

Perceba que no exemplo demonstrado na imagem o mesmo código passou por três **steps** diferentes na mesma execução do pipeline.

Cada **step** do seu pipeline é um comando, que é executado em uma console. Essa execução normalmente é executada dentro da pasta que tem os arquivos atualizados que foram recém baixados do controle de versão.

No exemplo anterior, a primeira **step** seria um comando para avaliar estaticamente o código fonte que está na sua pasta local, a segunda seria o comando para fazer **build* e o terceiro um comando para fazer upload do binário construído na **step** anterior para um repositório de artefatos. 

## Separação por "job"

Como já sabemos que no pipeline existem várias **steps**, que são responsáveis por executar ações no código a medida que elas avançam na esteira de entrega de software, é importante salientar que existe um outro nível de abstração chamada de **job**. A tradução para português seria "trabalho", mas usaremos o termo em inglês para facilitar seu uso no futuro, pois esse é o nome que muitas vezes é usado pelas ferramentas de mercado.

O **job** é um conjunto de **steps**, onde essas **steps** normalmente são executadas sequencialmente por padrão, ou seja, a segunda **step** só será executada após terminar  a primeira. 

![Pipeline com dois jobs](resources/images/o_que_e_pipeline2.png)

Se seu pipeline tiver vários **jobs** geralmente eles serão executados em paralelo, ou seja, se você tiver um **job** que executa seu código em uma plataforma específica, e um outro job que executa em outra. Os dois **jobs** serão iniciados ao mesmo tempo e não haverá nenhuma hierarquia entre eles, a não ser que seja explicitamente descrita.

Obs: Algumas ferramentas de pipeline não tem esse conceito de **jobs** ou utilizam outro nome. Lembre-se que essa explicação tem como objetivo oferecer uma ideia dos fundamentos. 

## Onde é executada essa "step"?

Como cada **step** tem seu comando, é importante saber onde esse comando é executado, pois quando o pipeline é iniciado, o código é copiado para uma pasta local e os comandos são executados no mesmo lugar.

Normalmente é usado um outro servidor para clonar o código e executar todos os comandos de cada **step**. Essa máquina é comumente chamada de **agente**.

Esse agente é o sistema que será usado para executar os comandos. Ele conecta no servidor de pipeline para buscar as informações necessárias para executar os **steps** e **jobs** da forma que foi configurada.

Esse agente pode ser de fato uma máquina, mas também pode ser um container. Esse modelo é inclusive a melhor forma de utilização de agentes, pois não há necessidade de manutenção de servidores, que consome um tempo de gerência absurdo do time responsável pela infraestrutura.

Imagina ter de instalar uma máquina, especificar todos os pacotes, bibliotecas e afins que é necessário para diferente pipeline que tem dentro de uma organização? E se o time de produto quiser usar uma linguagem nova? Abre um ticket pro time de infra e espera ele criar um agente novo? Não precisa. Usando o docker você mesmo pode criar sua imagem ou utilizar as milhares que existem prontas nos repositório públicos de imagens que temos hoje na internet.

Vale salientar que mesmo utilizando containers como agente, ainda é aconselhável ter uma outra máquina, pois é nela que será instalado o software responsável pela gerência dos containers e onde será iniciado os containers, ou seja, basicamente os **jobs** serão executadas nesse agente, mas não no sistema operacional padrão e sim naquela que foi iniciado dentro do container.

Se você ainda não sabe como funciona containers, aconselhamos a leitura do livro [Docker Para Desenvolvedores](https://leanpub.com/dockerparadesenvolvedores), mas acredito que o conhecimento de containers não vai afetar a sua capacidade de assimilar esse conceito sobre pipelines. Basta saber que o container é um processo rodando em um sistema operacional isolado, mas que todos os containers do mesmo **host** rodam na mesma máquina.
 
## Quebrando o pipeline

Usualmente as ferramentas de pipeline só permitem que a segunda **step** seja executada se a primeira for finalizada com sucesso. Isso é um comportamento extremamente esperado, porque a ideia do pipeline é justamente garantir que as ações sejam executadas em sequência, pois elas em geral são configuradas de forma gradual, ou seja, as primeiras **steps** fazem as primeiras validações, e as construções e outras intervenções mais críticas e demoradas acontecem depois. Isso quer dizer que se uma validação inicial falhar, e essa validação por via de regra é mais rápida, poupa o tempo de esperar a falha da etapa de construção de artefato, que costumeiramente é mais demorada.

É comum encontrar pessoas afirmando que o objetivo de um pipeline é "quebrar", pois é nesse processo que se percebe se o código enviado para esteira de fato está preparado para ser entregue ou não.

A automatização das validações e construções são parte central de um processo de entrega de software moderno.

### Conclusão

O pipeline é uma abstração, onde temos **jobs** e **steps** compondo as etapas para construção de um produto a ser entregue no final da esteira. Tendo isso em mente, podemos pensar que muito mais do que apenas a ferramenta, o pipeline serve também para criar um fluxo rápido de feedback, onde em caso de quebra podemos entender que aquele código precisa de cuidados até que o problema seja resolvido.
