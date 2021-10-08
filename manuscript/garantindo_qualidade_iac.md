# Garantindo a qualidade para sua Infra como código (Rafael Gomes)

## Introdução

A infra como código ainda é um assunto em desenvolvimento, e ao contrário da engenharia de software no desenvolvimento convencional de código, que está bastante estabelecida, na automação de infra as melhores práticas ainda estão em amplo debate e pouco adotadas pela maioria das equipes que fazem esse tipo de implementação.

É muito comum em infra as code (IaC) não existir muita preocupação com a qualidade do código escrito, seja porque, normalmente, eles são feitos por pessoas com pouca experiência em desenvolvimento em geral, ou talvez por existir poucos materiais falando sobre isso.

Enviar o código sem uma validação para que o pipeline "tente" aplicar seu código de infra pode ser uma atitude catastrófica. O entendimento da infraestrutura é quase sempre limitado e raramente temos uma compreensão completa do que está sendo executado em nosso ambiente de produção. Tudo bem assumir isso, ok?

O que leva a pessoa que trabalha com infra a cometer esse tipo de atitude? Será que existe outra possibilidade? 

Nesse artigo mostraremos alguns caminhos do que pode ser feito para evitar esse tipo de situação, afinal não é uma boa prática essa atitude de "tentativa e erro". Como estamos falando de infraestrutura, uma tentativa muito equivocada pode causar danos com grandes chances de lhe oferecer novos desafios por algumas horas ou até mesmo dias.

## O que seria QA para IaC?

*Quality Assurance* (QA) é a disciplina que trata sobre prevenir erros ou falhas na entrega de software. É o estudo e prática do que deve ser feito para oferecer alguma garantia antes do código ser compartilhado com todo time, pois todo mundo erra, mas o mais importante é que esse erro seja visualizado o mais rápido possível, evitando assim que ele cause problemas para outros membros da sua equipe ou até mesmo para a pessoa que usará seu software após entregue em produção.

QA pode e deve ser usado para códigos de infra, mas temos pouca literatura sugerindo esse tipo de prática, infelizmente.

Esse texto terá um enfoque especial na ferramenta de gerência de configuração chamado [Ansible](https://docs.ansible.com/ansible/latest/index.html), pois é um dos softwares de IaC mais usado na comunidade.

Será apresentado formas de como validar o seu código antes dele ser compartilhado com outras pessoas do seu time e como garantir que o pipeline identifique rapidamente o erro no código que você ou outra pessoa do seu time compartilhou sem perceber que tinha problemas.

## Testando seu código antes do commit

Pensando no Ansible, o teste não se reduz ao comando **ansible-playbook** executar com sucesso. A execução correta do **ansible-playbook** é um dos passos necessários, mas não o único.

A primeira checagem que seu código precisa passar é o que chamamos de **lint**.

**Lint** é uma checagem simples e estática do seu código, que é um binário que acessa todos seus arquivos IaC e procura por falhas na escrita do código, como uma indentação incorreta, um espaço em branco desnecessário, uma linha que não precisa ou um ponto e vírgula necessário. Ele busca erros simples e executa muito rápido. Esse é talvez o teste mais rápido que você poderá executar no seu código IaC.

Na checagem estática o código não precisa ser implementado em nenhum servidor, pois ele só olha o conteúdo do código e não seu resultado.

Para o Ansible, temos o [yamllint](https://yamllint.readthedocs.io/en/stable) que pode ser instalado com o comando abaixo:

```
pip3 install yamllint
```

Para testar usando essas ferramentas é muito simples:

```
yamllint .
```

Eu aconselho muito a criação do arquivo [.yamllint](https://yamllint.readthedocs.io/en/stable/configuration.html) na raiz do seu repositório para configurar o uso correto do lint.

## Testes automatizados para Ansible

Agora que você já sabe validar estaticamente o conteúdo do seu código Ansible, podemos passar para validação automatizada do comportamento do seu código, que é a escrita de um código que tem como objetivo validar se o produto final entregue pelo seu IaC de fato faz o que você espera que ele faça.

Usando um exemplo mais prático, imagine que você tem uma role Ansible que tem como objetivo instalar e configurar um servidor web. Considere que, alguém precisou alterar o código para adicionar a feature de configurar certificados para oferecer uma conexão HTTPS. Porém, a alteração de código quebrou o arquivo de configuração e, seu servidor web, uma vez configurado com essa role, não se sustentará executando no servidor. Pois, o mesmo está com erro no arquivo de configuração.

A execução do Ansible, nesse caso, não falhará, muito menos o lint apontará qualquer problema. Você só perceberá qualquer problema após aplicar o código Ansible em um servidor e então acessar o servidor manualmente tanto na porta HTTP como HTTPS.

Para evitar situações como essas, deve ser usado testes automatizados Uma boa ferramenta para testar códigos de IaC é o [Testinfra](https://testinfra.readthedocs.io/en/latest/).

Para o utilizar o Testinfra, primeiro você deve executar o seguinte comando para instalar:

```
pip install pytest-testinfra
```

Após instalado, você deve criar um arquivo de teste.

Esse arquivo deve ser python, e o nome desse arquivo deve começar com "test_", ou seja, seu arquivo pode ser "test_http.py" por exemplo.

Para o exemplo citado acima, podemos fazer o seguinte teste:

```
def test_http_is_listening(host):
    http = host.socket("tcp://0.0.0.0:80")
    assert http.is_listening
```

No arquivo acima, estou fazendo a seguinte pergunta: "Servidor, você tem o endereço socket "tcp://0.0.0.0:80" escutando? Se a resposta for sim, isso indica que meu código não afetou esse comportamento, pois esse teste só será executado após a aplicação do Ansible.

A valor de **host** é inserido pelo **Testinfra** e por padrão ele usará a máquina onde está rodando o código.

### Complicações ao utilizar o Testinfra

Na forma como foi apresentado até aqui, o **Testinfra** tem uma série de obstáculos para seu uso, que tornam o processo de teste um pouco complexo.

No código apresentado anteriormente, o pacote **pytest-testinfra** e o arquivo **test_http.py** devem estar no servidor destino, o servidor que será configurado com o **Ansible**.  Isso quer dizer que você deve, de alguma forma, fazer isso na máquina destino e ela, possivelmente, ficará com esse "lixo" depois de testada, a não ser que você retire após executar o teste.

É possível utilizar o Testinfra de forma remota, usando o conector [ssh](https://testinfra.readthedocs.io/en/latest/backends.html#ssh) ou [Ansible](https://testinfra.readthedocs.io/en/latest/backends.html#ansible), mas isso tornará o processo ainda mais complicado.

Isso, sem falar, na própria necessidade de ter um servidor destino para testar seu código Ansible, pois imagina toda complicação de ter que destruir o servidor e reinstalar toda vez que seu código Ansible inviabilize o uso daquela máquina? 

Qualquer atividade de QA deve ser automatizada ao máximo, ou terá grandes chances de não ser executada pelo time.

## Molecule

Para resolver esses problemas, eu te apresento o [Molecule](https://molecule.readthedocs.io/en/latest/), que é uma ferramenta de apoio para testes automatizados no Ansible.

O Molecule visa facilitar a execução de uma rotina completa de QA, pois ela também pode aplicar o lint, realizar os testes, fazer verificação de idempotência e se o código consegue lidar com efeitos colaterais.

Para instalar é muito simples:

```
pip install "molecule[ansible]"
```

Para criar uma role já seguindo os padrões do Molecule é muito fácil também:

```
molecule init role sua-role-nova --driver-name docker
```

Com o comando acima, será criada uma role com toda estrutura esperada pelo Ansible, mas também com uma pasta chamada **molecule** e dentro dela todo padrão que você pode utilizar para seus testes.

Caso sua role já exista, poderá utilizar o seguinte comando:

```
cd sua-role
molecule init scenario default --role-name sua-role-nova --driver-name docker
```

Como eu utilizo Ubuntu e **Testinfra** como verificador, meu arquivo **molecule/default/molecule.yml** ficaria da seguinte maneira:

```
---
dependency:
  name: galaxy
  options:
    ignore-certs: True
    ignore-errors: True
    role-file: requirements.yml
driver:
  name: docker
platforms:
  - name: ubuntu-18.04
    image: "geerlingguy/docker-${MOLECULE_DISTRO:-ubuntu1804}-ansible:latest"
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    published_ports:
      - 80:80/tcp
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
    privileged: true
    pre_build_image: true
provisioner:
  name: ansible
  playbooks:
    prepare: prepare.yml
    converge: converge.yml
verifier:
  name: testinfra
  env:
    PYTHONWARNINGS: "ignore:.*U.*mode is deprecated:DeprecationWarning"
```

Vamos explicar parte a parte.

```
---
dependency:
  name: galaxy
  options:
    role-file: requirements.yml
```

Essa parte é responsável por informar qual gerenciador de dependência será usado e, até o momento, não conheço algo melhor do que o [galaxy](https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html). Nas opções, informamos que o arquivo **requirements.yml** conterá a lista das roles que precisa ser baixada para que esteja disponível para ser usada. Perceba que aqui não diz que ela será usada, apenas que fará o download das roles mencionadas.

```
driver:
  name: docker
platforms:
  - name: ubuntu-18.04
    image: "geerlingguy/docker-${MOLECULE_DISTRO:-ubuntu1804}-ansible:latest"
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    published_ports:
      - 80:80/tcp
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
    privileged: true
    pre_build_image: true
```

Nas seções informadas acima, definimos que usaremos docker para emular a máquina, e a plataforma usada nessa "máquina" será o Ubuntu 18.04. Usaremos uma imagem do [geerlingguy](https://twitter.com/geerlingguy) nesse exemplo, pois ela tem tudo que precisamos. O **command** é padrão e raramente precisará ser modificado. O **published_ports** define qual porta esse container vai expor e você só precisa colocar caso você queria testar da sua máquina, pois todo teste que falaremos aqui será executado de dentro da "máquina" e não de onde é executado o molecule. Quanto aos valores de **volumes**, **privileged** e **pre_build_image** você também, possivelmente, não precisará mudar.

```
provisioner:
  name: ansible
  playbooks:
    converge: converge.yml
```

Na seção acima, é informado que usaremos o Ansible como ferramenta de IaC, definimos também o playbook que será usado na etapa do **converge** do Molecule. Para explicação de cada etapa do Molecule, acesse [essa parte da documentação](https://molecule.readthedocs.io/en/latest/usage.html). É importante salientar que o Molecule aceita apenas o Ansible como provisioner.


```
verifier:
  name: testinfra
```

Nessa última seção, configuramos que será usado o Testinfra como verificador automatizado de código.

O seu teste deve ser colocado dentro da pasta **molecule/default/tests**, com seu nome seguindo o mesmo padrão apresentado anteriormente (começando com "test_"), porém o conteúdo do arquivo deve seguir a estrutura:

```
import os
 
import testinfra.utils.ansible_runner
 
testinfra_hosts = testinfra.utils.ansible_runner.AnsibleRunner(
    os.environ['MOLECULE_INVENTORY_FILE']).get_hosts('all')
 
 
def test_http_is_listening(host):
    http = host.socket("tcp://0.0.0.0:80")
    assert http.is_listening
```

A seção nova que aparece nesse arquivo é responsável por configurar o Testinfra para utilizar a "máquina" entregue pelo Molecule.

Após tudo configurado, só nos resta executar o teste:

```
molecule test
```

Esse comando será responsável por: 

  - dependency
  - lint
  - cleanup
  - destroy
  - syntax
  - create
  - prepare
  - converge
  - idempotence
  - side_effect
  - verify
  - cleanup
  - destroy

Os nomes de cada etapa são auto-explicativos, mas se tiver dúvidas, basta ler a [documentação oficial](https://molecule.readthedocs.io/en/latest/usage.html).

Caso você queria testar seu código à medida que o cria, você pode apenas executar os seguintes comandos:

```
molecule create
molecule converge
```

O **create** será responsável por criar o container que atuará como "máquina" e o **converge** aplicará o código Ansible nesse "servidor" temporário, que na verdade é um container docker.

Após aplicar com sucesso seu código Ansible no **converge**, você deve executar o comando abaixo:

```
molecule verify
```

Ele será responsável por executar o **Testinfra** nessa "máquina" temporária e assim validar o comportamento após execução do **converge**.


Meu conselho, antes de fazer commit e push do seu código IaC é que você execute o **molecule test**. Ele executa todos os passos em um servidor recém-criado. Além disso, ele também executa o **idempotence** que é responsável por avaliar se alguma tarefa que você definiu no Ansible está idempotente. Em outras palavras, significa que a segunda aplicação do Ansible na mesma máquina, sem modificar nenhum código Ansible, deve ter um resultado sem modificação alguma do servidor.

A idempotência é uma melhor prática que deve ser buscada sempre, pois não faz sentido algum, uma mesma task sendo aplicada no mesmo servidor duas vezes fazer modificações no destino. Isso indica que seu código tem problemas e pode estar atuando na máquina de forma equivocada, o que pode ocasionar em duplicação de arquivos ou até mesmo configuração com problemas em arquivos.

## Conclusão

Automação de QA para IaC não é fácil e não temos muito materiais, mas o pouco que temos já consegue oferecer alguma segurança e devemos utilizá-las em nosso processo de entrega automatizada de IaC.