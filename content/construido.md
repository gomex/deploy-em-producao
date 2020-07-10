---
title: Como está sendo construído
include_footer: true
---

{{% title3 "Como funciona isso no repositório git?" %}}
O livro será feito de forma colaborativa, a partir [desse repositório](https://github.com/gomex/deploy-em-producao), escrito em markdown e lançado como ebook no [Leanpub](https://leanpub.com).

Dentro desse repositório apenas a pasta **manuscript** importa para a criação do livro. Todo restante tem a ver com esse site que você está lendo nesse momento.

Dentro da pasta **manuscript** você verá um arquivo chamado **Book.txt**, que é onde você encontrará a lista de arquivos markdown, que juntos são a base de código para criação do livro pelo [Leanpub](https://leanpub.com).


{{% title3 "Como funciona o processo para aceitação de novo conteúdo?" %}}

A linha editorial é feita por Rafael Gomes, também conhecido como Gomex, ou seja, ele recebe a colaboração via PR (Pull Request) e vai interagir com a pessoa para alinhar o material para uma didática específica. A ideia é que o livro faça sentido como um todo e não seja uma "colcha de retalhos".

{{% subtitle5 "Sim, mas como se espera que o contéudo seja escrito?" %}}

A proposta é seguir um fluxo bem explícito, no qual a ordem que segue abaixo deve ser seguida:

 1. Se o capítulo em questão resolve algum problema, por favor comece apresentando o problema;
 2. Apresente os conceitos mais **básicos**, se possível com links para documentação oficial (se houver);
 3. Se possível apresente exemplos práticos sobre os conceitos **básicos**;
 4. Apresente os conceitos mais **avançados**, com links para documentação oficial (se houver);
 5. Se possível apresente exemplos práticos sobre os conceitos **avançados**;
 6. Na conclusão evite deixar "ponta solta" para um outro capítulo.
