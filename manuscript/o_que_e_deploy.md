# O que é deploy? (Rafael Gomes)

Muito se fala sobre deploy e a maioria das pessoas que já estão, há algum tempo, na área de Tecnologia da Informação (TI) , provavelmente já tenham algum entendimento sobre o que é isso, e, quem iniciou na área há pouco, possivelmente já  captou "alguma coisa" pelo contexto.

Essa é a oportunidade para se  estabelecer aqui um entendimento sobre o que é deploy, como ele funciona e porque ele é tão importante para área de TI.

Pra começar, deploy é um verbo do idioma inglês, cuja tradução mais próxima talvez seria posicionar. 

Quando se fala em fazer deploy, imagine que isso significa uma forma de posicionar algo, ou seja, é basicamente pegar algo que está em uma posição/localização e colocar em outra.

Isso quer dizer que quando alguém falar que vai "deployar" algo, é basicamente o jeitinho brasileiro de usar uma palavra em inglês e verbalizar ela em português, o que não é de todo ruim, uma vez que quem escuta entende perfeitamente, ou seja, a comunicação funciona e não há problema algum com isso. Chato mesmo é alguém que complica a comunicação para forçar palavras não usuais e assim tentar demonstrar uma certa superioridade linguística. Aconselha-se a leitura [desse livro](https://www.amazon.com.br/Preconceito-Lingu%C3%ADstico-Marcos-Bagno/dp/8579340985/ref=asc_df_8579340985/?tag=googleshopp00-20&linkCode=df0&hvadid=379708411098&hvpos=&hvnetw=g&hvrand=890780466304631420&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1001533&hvtargid=pla-387685959250&psc=1) sobre preconceito linguístico.

## Como funciona o deploy de um produto de software

O que aqui se chama de "produto de software" é qualquer conjunto de arquivos que tenha como objetivo entregar uma funcionalidade como produto final. Um exemplo seria um site, que pelo conjunto de arquivos html, css e javascript, entrega uma exibição de informações que é traduzida pelo seu navegador e assim você pode ter acesso a informações navegando na internet.

O deploy é o ato de pegar esse conjunto de arquivos e levar até um determinado lugar. Esse lugar é normalmente um servidor, que hospedará esse software e exibirá para o usuário sempre que solicitado.

## Destinos possíveis de um deploy

Normalmente um software passa por alguns destinos antes de chegar no seu habitat final, que é o ambiente de produção. O que se chama de produção, dessa forma, é basicamente o lugar oficial de onde os usuários finais deste produto poderão acessá-lo.

As boas práticas apontam que antes de chegar no ambiente de produção esse software passe por outros ambientes, que normalmente são os seguintes, e, muitas vezes seguem também nessa ordem:

 - Desenvolvimento
 - Teste/Staging
 - Produção

### Desenvolvimento

É o local no qual a pessoa que desenvolve tem acesso direto, é onde se executa rapidamente o código, a fim de verificar se o que está sendo escrito atenderá as expectativas da funcionalidade que está sendo desenvolvida

Normalmente essa é a máquina da pessoa que desenvolve o software, e o verbo "deployar" faz pouco sentido aqui, porque não há uma movimentação de código, uma vez que este será usado na mesma máquina onde foi produzido.

Em alguns casos, a infraestrutura necessária para simular o ambiente de produção é tão complexa que é preciso um ambiente de desenvolvimento fora da máquina local, neste caso,  o verbo "deployar" volta a ter seu sentido completo, pois o código será transferido para um outro ambiente, no caso, um de desenvolvimento remoto.

### Teste/Staging

Não existe um nome para esse ambiente que possa ser considerado unânime, mas esse é o ambiente no qual se espera que o software esteja mais maduro, isso quer dizer que aqui o código já passou por alguma análise e está pronto para ser validado por outras pessoas.

O que aqui é chamado de **análise** será mais detalhado nos capítulos posteriores, mas, por hora, basta saber que esse é o processo usado para avaliar se há algum problema no código, normalmente de forma manual, e feito por uma outra pessoa, que analisa seu código a fim de encontrar possíveis erros.

Esse é, via de regra,  o último local que o código "visitará" antes de ser conduzido para o ambiente que será usado pelos usuários reais do serviço.Ou seja,  é aqui o local no qual costumeiramente acontecem os testes mais "pesados".

Esses testes muitas vezes usam dados mais próximos do que os que seriam usados no ambiente real, de forma que validações bem mais elaboradas podem acontecer. Habitualmente, times de software simulam o uso do sistema, de forma automatizada ou não, a fim de encontrar possíveis erros. Esses tipos de testes serão tratados posteriormente. Por agora é suficiente saber que é nesse ambiente que isso habitualmente acontece.

É importante salientar que se você copiar os dados de produção para ajudar na validação dos ambientes de "teste" e/ou desenvolvimento, deverá lembrar de apagar dados pessoais das pessoas que utilizam seu sistema. Imagine se fosse você a pessoa que utiliza um sistema, sabendo que seus dados pessoais estão acessíveis para qualquer membro da equipe de desenvolvimento?

### Produção

Aqui é oficial, todo produto agora pode ser utilizado pelos clientes. Em geral, esse ambiente demanda um cuidado maior, tanto de quem pode fazer o deploy, como na disposição da quantidade de recursos. Muitas vezes o ambiente de produção tem, ao menos, duas máquinas, que carregam o mesmo conteúdo. Isso permite criar uma situação chamada de alta-disponibilidade, que ocorre quando o serviço permite que uma máquina seja perdida por falhas não esperadas, sem que isso afete a sua disponibilidade.

Usando o exemplo anterior do site, basicamente, seria a hipótese de se ter duas máquinas hospedando os mesmos arquivos do site, e, caso aconteça uma falha elétrica, ou qualquer outro problema em uma das máquinas, a segunda pode assumir o serviço sozinha sem muitos prejuízos à disponibilidade do serviço ofertado, que, neste cenário, significa exibir o site para as pessoas que o acessam.

## Na prática, como funciona o deploy?

Frequentemente, o ato de fazer deploy se resume às ações de copiar os arquivos de um lugar - que pode ser o repositório de código ou de artefato - e depositar ele no ambiente de destino. 

Seguindo o exemplo anterior do site, o ato de fazer o deploy corresponderia a copiar os arquivos html, css e javascript, que estão no repositório de código, e depositá-los no servidor que hospedará aquele ambiente.

Deploy para testes do site usado como exemplo acima? O ato de fazer deploy seria resumido a copiar os arquivos do repositório de código e depositá-los no servidor que foi designado para ser ambiente de teste.