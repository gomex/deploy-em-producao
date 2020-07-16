## Definindo Testes

Existem muitas definições na literatura sobre quais testes criar e onde, uma das mais famosas é a  pirâmide de testes citada pelo [Martin Fowler](https://martinfowler.com/articles/practical-test-pyramid.html). Nessa visão a ideia é que você tenha mais testes na base da pirâmide (testes unitários) que são mais rápidos de executar, um pouco menos no meio (testes integrados) e menos ainda no topo (testes de interface e manuais).

No nosso contexto eu vou usar a categorização feita pelo Google, que pode ser conferida no livro [Software Engineering at Google](https://www.amazon.com.br/Software-Engineering-Google-Titus-Winters/dp/1492082791). O Google utiliza dois critérios para categorizar os testes: tamanho e escopo.

Tamanho tem relação com a quantidade de recursos consumidos pelo teste e escopo tem relação com quanto código aquele teste está validando.

Eu acredito que essa é uma abordagem melhor para falar sobre teste já que pode ser aplicada a qualquer que seja o tipo do projeto: monolitos, microserviços, micro frontends, etc, e não fica preso a quantidade de testes que você precisa ter em cada uma das camadas. Utilizando esse conceito você se desprende um pouco daquela pressão de seguir a pirâmide e consegue ter uma outra visão para analisar que testes fazem sentido para você.

São 3 as categorizações:

- Pequeno: são os testes mais contidos, geralmente são utilizados dublês de teste para evitar chamadas externas as funções testadas. São testes rápidos e determinísticos;
- Médio: aqui temos testes que executam múltiplos processos mas ainda assim sem acessar componentes externos, nessa categoria entram os testes que acessam banco de dados por exemplo;
- Grande: esses testes são os que necessitam de uma maior complexidade para execução, nesse momento os sistemas já estão integrados. São mais lentos e menos determinísticos.

### Pequenos

#### Testes Unitários
Testes Unitários são aqueles que tem um escopo mais limitado, normalmente uma simples classe ou método. Esses testes são os que vão te ajudar no dia a dia do processo de desenvolvimento já que eles são mais rápidos de executar, devido ao escopo mais contido. Isso ajuda a otimizar a produtividade dado que esses testes podem ser executados antes de fazer o push para o repositório (vamos falar mais sobre isso no tópico sobre Testes Contínuos).

Exemplo de um teste unitário que testa um controller de autenticação:

```javascript
import User from '@models/User'
import mockingoose from 'mockingoose'
import Response from '@tests/utils/response'
import authController from '@controllers/auth.controller'

const user = {
  name: 'Test User',
  email: 'test@user.com',
  password: 'password'
}

describe('The Auth Controller', () => {

  it('Should register a new user with the required fields', async () => {
    const request = {
      body: user
    }
    const response = new Response()
    const jsonSpy = jest.spyOn(response, 'json')

    mockingoose(User).toReturn(user, 'create')

    await authController.register(request, response)
    expect(jsonSpy).toHaveBeenCalledWith(expect.objectContaining(accountRegistered))
  })
```

Esse é um teste que tem o escopo limitado, nesse caso testar o registro de um novo usuário e não tem dependências externas a essa função, tanto que ao invés de ser utilizada a integração com o banco de dados, utilizamos uma biblioteca para mockar o model.

Esses testes ajudam muito na manutenibilidade posto que como exercitam um escopo mais contido, se um deles quebra você consegue rapidamente identificar o ponto de falha, diferente de um caso onde por exemplo você tivesse mais elementos envolvidos como um banco de dados, um container da aplicação, etc.

No decorrer desse tópico vamos falar mais sobre isso, mas vale já começar a reforçar que os testes precisam trazer segurança para o time fazer o deploy em produção sem peso na consciência. Se os testes não revelam os bugs ou se quebram demais desnecessariamente o time acaba perdendo a confiança e a crença de que os testes são um elemento importante no processo de desenvolvimento. Por isso você deve tratar testes como trata código de produção: utilizando boas práticas, fazendo refactor para implementar melhorias e sempre buscando otimização.

Não tem como falar de testes unitários sem tocar no assunto de dublês de teste, que é o que vamos abordar no próximo tópico.

#### Dublês de Testes
TODO

### Médios

#### Testes de Integração
TODO

#### Testes de Contrato
TODO

### Grandes

#### Testes ponta-a-ponta
TODO

#### Testes de Desempenho
TODO

#### Testes de Compatibilidade
TODO

#### Testes Exploratórios
TODO

#### Outras Verificações

Aqui temos algumas verficações bônus que vão te ajudar a elevar a barra de qualidade do seu projeto. 

##### Análise Estática
TODO

##### Testes de Mutação
TODO

