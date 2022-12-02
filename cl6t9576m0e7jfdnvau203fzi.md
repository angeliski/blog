# As mentiras que você conta para não testar

Hoje o post vai ser sobre testes, uma das coisas mais importantes de todas quando estamos falando de software funcionando.

Eu mudei só uma coisinha
------------------------

A primeira mentira que a gente sempre conta é a da alteração pequena.

_"Poxa vida Angeliski,eu só tirei aquele if que estava errado"_

O ponto é exatamente esse. Você realiza uma alteração no código normalmente por dois motivos: ou porque tinha um bug, ou porque a regra de negócio mudou. Qualquer que seja o caso, quando você deixa de fazer o teste cria um brecha para falha não rastreada. O que garante que seu if vai continuar funcionando quando o _**Fulano do lado**_ fizer uma alteração?

**Seu teste!** Não importa o quão pequena seja a alteração que você fez, faça um teste que garanta que o que você mudou está e vai continuar funcionando.

A funcionalidade que eu fiz é pequena, não precisa disso
--------------------------------------------------------

_"Mas o meu botão só faz **X**"_

Você gastou uma hora fazendo uma funcionalidade, apenas um botão que faz **X** (Insira aqui a sua funcionalidade).Você quer garantir que o _**Fulano do lado**(esse cara é terrivel né?)_ não quebre tudo? Se você fez o teste pode ir pra casa tranquilo, mas se não fez... Não importa o quão pequena uma funcionalidade (ou a correção de um bug) é a unica maneira que você garante o funcionamento dela é fazendo o teste. Se não houvesse a necessidade dela estar sempre funcionando, não teria porque desenvolver ela também.

Depois eu faço esse teste
-------------------------

_"Deixa eu acabar isso aqui que eu já escrevo esse teste"_

Muitas explicações podem ser dadas sobre porque o TDD melhora o código, a qualidade e tudo mais. Mas ele sem dúvida faz uma coisa que melhora tudo, ele "obriga" você a escrever o teste antes. Porque quando você deixa ele pra depois fica mais dificil. Hoje em dia os testes ainda não são vistos como uma entrega de valor (não é sempre assim, na [Bluesoft](http://bluesoft.com.br/) por exemplo testes tem valor de negócio. Isso faz parte da cultura da empresa) e por não ter valor, quando aparece uma outra coisa o teste é deixado pra trás. E isso acaba deixando aquele código sem ser testado.

Esse código é muito difícil de testar
-------------------------------------

_"Tá complicado de testar isso aqui"_

Essa é simples: [refatore](http://blog.caelum.com.br/testes-sao-mais-do-que-regressao-os-beneficios-no-design/). Um teste que te dá trabalho pra testar está te passando uma mensagem bem clara, o design dele poderia ser melhor. Quando você faz testes antes você consegue elaborar melhor o design (desde que você conhece design pra poder aplicar isso) mas se você faz seu teste depois precisa escutar o que o teste está dizendo, se ele tem uma complexidade muito grande para testar o feedback é sobre o design ruim do seu código.

Código legado não precisa testar
--------------------------------

_"O negocio tá ai faz dez anos e eu vou testar agora?"_

Não é por estar a dez anos em produção que ele não precisa ser testado. Dois motivos podem ser vistos logo de cara,primeiro porque você continua desenvolvendo junto do código legado, então se ele não tem teste, cedo ou tarde um bug vai ser inserido. Segundo que ele estar lá a dez anos não significa que não haja bugs, só que ninguém pegou um bug ainda. Terceiro (Eu pensei em mais um agora) que quando você desenvolve um teste para o código legado precisa entender o que acontece ali, assim aprende mais sobre a funcionalidade melhorando sua skill sobre o negócio e sobre a capacidade de manutação. Eu sei que testar código legado é uma [verdadeira batalha](https://tasafo.org/2014/07/23/codigo-legado-o-horror/) (em alguns casos), seja pelo alto acoplamento, seja pela complexidade da regra, mas é uma tarefa que resulta em ótimos ganhos.

Não fui eu que desenvolvi isso aqui
-----------------------------------

_"Agora eu vou testar o código do **Fulano do lado**?"_

Time. Equipe. Conjunto. Você pode chamar do que quiser, mas não muda o fato de que vocês todos estão com um mesmo propósito: tomar café construir software de qualidade. Não é porque o cara esqueceu do teste (ou não quis fazer) que você vai deixar a oportunidade de cubrir essa brecha passar. Não importa quem fez o furo no casco do navio, se você não tapar, todo mundo vai se afogar.

* * *

A ideia desse artigo é mostar que existem muitas coisas que pensamos que podem ser evitadas. Testar é bom, mas também é um hábito. É como correr, no começo você tem que se esforçar muito para correr poucos quilometros. Mas depois de um tempo praticando, corre muito em pouco tempo além de ser muito bom.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski