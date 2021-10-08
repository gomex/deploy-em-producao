# Deploy de Infraestrutura como código

## Introdução

Explicamos [aqui](o_que_e_deploy.md) o que é um deploy, mas nesse artigo falaremos sobre um tipo especial de deploy. Esse que lida com a infraestrutura.

Infraestrutura como código (IaC) é um grande domínio de conhecimento. Nesse artigo falaremos especificamente de infraestrutura de servidores, onde é criado e/ou mantido uma plataforma que receberá código posteriormente.

Existem algumas formas de manter uma infraestrutura mantida de forma automatizada. Quando se fala sobre "deploy de infraestrutura como código (Deploy de IaC)" estamos falando de criar e/ou fazer modificação em uma infraestrutura com base em um código *infra as code* (IaC) de um repositório.

Para facilitar o entendimento, ignore a existência de container (docker, podman ou afins), pois isso seria uma outra camada, que explicaremos em uma outra oportunidade.

## Exemplo

Imagine a necessidade de criar e manter um servidor web para servir um código PHP. 

Nesse servidor precisa ser instalado alguns softwares principais:

 - [FPM](https://www.php.net/manual/pt_BR/install.fpm.php): que é responsável pela tradução do código PHP.
 - [NGINX](https://www.nginx.com/): que é responsável por fazer o proxy da requisição do usuário para o FPM.

Esses softwares precisam ser devidamente instalados e configurados de acordo com definições do seu uso.

Quando se fala em fazer deploy de um servidor web para PHP, estamos falando da existência de um código IaC que será responsável por todo processo de instalar e configurar tudo que precisa para que exista a infraestrutura pronta para receber o código PHP e servir o site que você deseja.

## Deploy de infra é sobre infra

Muitas pessoas têm dificuldade para entender deploy de infraestrutura, pois há uma confusão sobre deploy do produto em si e de sua infra.

Em todo momento que falarmos sobre deploy neste capítulo será apenas sobre infraestrutura. Que a depender da situação, pode ser chamado também de plataforma.

Em alguns casos o código pode ser "deployado" ao mesmo tempo, mas isso não é uma regra. 

No exemplo citado anteriormente de um serviço PHP, não estamos falando do código PHP, apenas os softwares que deixaremos pronto para receber o deploy do código posteriormente.

## Tipos de deploy de IaC

Quando se fala de deploy de plataformas, especificamente aquelas que envolvem servidores, temos dois caminhos possíveis:


  - Gerência de configuração
  - Infraestrutura imutável

### Gerência de configuração

Deploy de IaC por gerência de configuração é quando você usa uma ferramenta que conecta em um servidor existente e faz intervenções nele. Nesse modelo o mesmo servidor sofre alterações com o passar do tempo.

Sempre que for preciso modificar o comportamento da plataforma, você precisará rodar a ferramenta de gerência de configuração escolhida no mesmo servidor, e ela será responsável por modificar o servidor com base na mudança que você informou no código. Seja por mudança do código de IaC ou suas variáveis.

Essa comunicação entre a ferramenta de gerência de configuração e o servidor normalmente acontece via SSH (Para servidores GNU/Linux) e winrm (Para servidores Microsoft Windows). Esse software de IaC conecta na máquina e faz intervenções baseado no que está no código e suas variáveis.

É importante salientar que as ferramentas de gerência de configuração farão de tudo para garantir que o que foi definido no código, pois estes softwares garantem mudanças de estado atual para o estado desejado definido como IaC, desde que este processo não falhe por algum motivo especifico.

Exemplo:

Se o IaC define que um determinado serviço deveria estar em execução, como podemos ver no código abaixo:

```
- name: Iniciar o nginx
  ansible.builtin.service:
    name: nginx
    state: started
```


No código acima temos as seguintes instruções detalhadas:

```
- name: Iniciar o nginx
```

> Descreve o nome da task em questão.

```
ansible.builtin.service:
```

> Descreve o nome do módulo ansible, que nesse caso é o [service](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html) que é responsável por gerenciar serviços.

```
name: nginx
state: started
```

> Descreve o nome do serviço que deseja gerenciar e qual estado esperado dele, ou seja, caso o serviço esteja desligado, ele vai iniciar. Caso esteja iniciado, não faz nada.

> Nesse exemplo, o código garante que o serviço iniciará caso esteja parado, mas se o serviço cair logo após o ansible inicia-lo, o ansible não saberá disso, a menos que exista outra task que faça essa checagem posteriormente.


### Infraestrutura imutável

Antes de falar o que é Infraestrutura **imutável**, vamos explicar o que é Infraestrutura **mutável**.

A infraestrutura mutável é exatamente o que é feito na gerência de configuração. Uma máquina é criada com uma determinada configuração e a **mesma máquina** sofre alterações para que um novo estado do serviço seja alcançado.
No exemplo demonstrado anteriormente usando o ansible, ele se conecta a um servidor existente. Tudo é feito na mesma instância, a não ser que alguém solicite a recriação dessa máquina.

**Exemplo**

Ao instalar o serviço nginx pela primeira vez, é escolhida a máquina que tem o nome de  **bakunin** e o ansible conecta em bakunin por ssh e faz a instalação do nginx.

Se for necessário modificar a versão do nginx ou instalar um outro software, o ansible conectará em **bakunin** via ssh novamente e aplicará o que de novo foi adicionado no código ansible.

Infraestrutura mutável é aquela que pode mudar dentro da mesma instância. Ela "nasce" em um estado de configuração e um processo externo pode fazer esse estado mudar.

### E a Infraestrutura imutável?

Infraestrutura imutável é aquela que não muda, certo? Sim, mas pra explicar isso melhor vamos ampliar a visão sobre essa questão.

Quando falamos sobre infra que muda ou não, falamos do servidor em específico, no exemplo apresentado anteriormente, uma vez que o servidor **bakunin** fez boot pela primeira vez ele não seria modificado até o dia que fosse desligado.

O serviço que **bakunin** hospeda poderá continuar em outra máquina, mas a instância **bakunin** não mudará no modelo de Infraestrutura imutável.

Não há nenhuma conexão ssh, ou outro método, para instalar ou configurar nada. A instância "nasce" com tudo que precisa para hospedar o serviço a que foi designada.

Na infraestrutura imutável é necessário criar uma imagem, uma espécie de fotografia da instância em um determinado momento, para que quando necessário criar a instância, tudo seja iniciado sem precisar de gerência de configuração após ligar a máquina.

> Fotografia ou Snapshot, é o termo utilizado para referenciar um recurso que não pode ser modificado, igualmente a fotografia, podemos escrever(desenhar) por cima mas seu conteúdo base é impossível de ser modificado por este processo.

**Exemplo**

Usando o mesmo exemplo de uma máquina com nginx, ao invés de iniciar uma instância genérica, sem nginx, e após seu boot conectar via ssh e completar sua configuração, é criada uma máquina temporária, onde a instalação e configuração do nginx é feita através de uma ferramenta de configuração, e o processo final resulta em uma imagem (snapshot da instância)

A imagem será usada quando for necessário fazer o deploy da infraestrutura imutável, pois nesse caso iniciaremos a máquina com essa imagem. Dessa forma a instância inicia já com nginx e nenhuma configuração posterior será necessária. E toda vez que for necessário trocar algo na instância, ao invés de conectar na mesma, é criada uma máquina nova a partir de uma imagem nova. Você pode inclusive manter as duas instâncias imutáveis executando ao mesmo tempo e desligar a antiga apenas quando tiver certeza que a nova está funcionando como esperado.

**Importante** notar que nem todos os serviços suportam uma infraestrutura imutável, mas falaremos disso melhor em outra oportunidade.


## Conclusão

Existem dois tipos de deploy de IaC e o que diferencia eles é como eles podem ser modificados, na gerência de configuração a modificação é feita na mesma máquina e na infra imutável ela é feita com a criação de outra máquina e modificando o apontamento do serviço para essa máquina nova.

Em ambos casos o serviço é modificado, mas apenas na gerência de configuração é utilizada a mesma máquina.

O módelo de infra imutável é o ideal, mas nem sempre é possível. Aprender a conviver com a gerência de configuração, da melhor forma possível, é uma habilidade necessária para se desenvolver, caso seu objetivo seja trabalhar com IaC em ambientes reais.

