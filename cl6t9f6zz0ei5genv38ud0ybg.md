# Começando com Clojure

Pouco tempo atrás eu fiz uma publicação falando sobre a [programação funcional](https://angeliski.com.br/o-que-a-programacao-funcional-pode-fazer-por-voce/) e em algumas vantagens que ela traz. Hoje nós vamos colocar um pouco da teoria em prática. Pra colocar a nossa programação funcional em prática escolhi a linguagem Clojure porque eu comecei a estudar ela faz pouco tempo (então se você conhece a linguagem e me ver falando alguma bobagem seja bem vindo para me corrigir).  Mas como diria o nosso amigo:

> Falar é fácil, me mostre o código.
> 
> _Linus Torvalds_

### Mas antes...

Se você é um pouco parecido comigo já deve estar doido para ver código, mas temos algumas coisas que me impedem de ir direto ao código. Por exemplo,  como você vai rodar o código que eu colocar aqui? Por conta disso eu preciso falar de coisas que estão por trás do código.

### Por trás do código

Uma informação interessante para  se ter é  que o Clojure é baseado no Lisp. Mas ele tem uma semântica menos assustadora complexa que o Lisp em si.  Mas o que realmente fez o Clojure ganhar força é o fato dele rodar em cima da [JVM](http://migre.me/vCkkP).

**Pausa dramática**

* * *

Ok,  você pode até não gostar do Java. Mas você tem de admitir que a JVM em si  é uma invenção genial! O conceito de uma máquina virtual que permite você rodar seu código em qualquer ambiente sem precisar recompilar é muito bom. Veja quantas linguagens já se infiltraram na JVM, desde  Groovy, Scala e Clojure até Javascript rodando em cima do Rhino...

* * *

**Fim da pausa dramática**

Por ser tratar de uma linguagem que roda em cima da JVM você vai precisar ter a JDK instalada se quiser usar ela. O modo mais simples de rodar Clojure (uma fez que você tem a [JDK](http://migre.me/vCkn0) instalada) é baixar o Jar no [site](https://clojure.org/) e rodar ele... Só  que não é tão eficaz, porque uma vez que você começa a avançar nos seus projetos (e eu pretendo avançar bastante) a coisa fica difícil de gerenciar. Então você pode usar uma ferramenta aceita no mercado chamada [Leiningen](http://leiningen.org/). Eu não vou dizer qual você deve usar, vou apenas deixar um link pra você saber como usar o [Jar com repl](http://clojure.org/guides/getting_started) ou o [lein com repl](http://leiningen.org/#install). Se você não quer instalar nada, pode fazer isso [online](http://www.tryclj.com/) também.  A partir de agora eu vou assumir que você conseguiu instalar tudo (se tiver afim) e que acessou o repl.

_"Que diabos é  **repl** Angeliski "_

Read Eval Print Loop(repl) nada mais é que um console que a linguagem disponibiliza pra você executar seu código. Isso permite que você execute pequenos códigos sem usar um arquivo. Imagine que ele é como o console do Chrome, você executa JS lá, vê o resultado é consegue validar suas ideias de maneira rápida. Vamos voltar para os detalhes da linguagem.

### Notação prefixada

É uma notação na qual os operadores aparecem antes dos operados, também conhecida como notação polonesa. Isso pode  causar alguma estranheza quando você olha pra uma chamada de função em Clojure já que ela é prefixada:

Se você tivesse  que ler isso seria algo como  "chame a função soma com esse dois argumentos". Você pode achar que isso não é estranho, mas e se você se deparar com isso:

Começou a complicar né? O que acontece é que Clojure tem muitas proximidades com as funções matemáticas, então um primeiro contato com essa linguagem gera esse "desconforto". Mas uma vez que você entende, vai ficando mais fácil.

A expressão a cima por exemplo, primeiro você aplica o sinal de menos em 3 e 4, ou seja 3 - 4. O resultado, é aplicado junto do sinal positivo com 2, ou seja 2 + (3 - 4). Ficou mais simples não?

Outra coisa importante a se lembrar na linguagem é que você precisa ter entre parênteses a execução da função, se você digitar + 2 2 no console, ele não vai executar nada, só retornar os valores absolutos pra você.

**Imutabilidade e definição de estruturas**

O Clojure também trabalha com a questão da imutabilidade, ou seja, uma vez que você define um valor ele passar a ser imutável. Observe:

    clojure-noob.core=> (def v1 5) 
    #'clojure-noob.core/v1 clojure-noob.core=> (def v1 15)  

Você deve se perguntar, se eu disse que era imutável, como eu reatribui um valor a variavel v1. O que acontece é que uma nova estrutura é criada quando essa definição acontece. Observe (não se preocupe com a sintaxe que você desconhece):

    clojure-noob.core=> (def emp (defstruct Empregado :nome :rg)) 
    #'clojure-noob.core/emp
    clojure-noob.core=> (def emp1 (struct-map Empregado :nome "Rogerio" :rg 2020)) 
    #'clojure-noob.core/emp1 clojure-noob.core=> (def emp2 (struct-map Empregado :nome "Rogerio" :rg 2020)) 
    #'clojure-noob.core/emp2 clojure-noob.core=> (= emp1 emp2) true 
    clojure-noob.core=> (def emp2 (struct-map Empregado :nome "Rogerio" :rg 2021))
     #'clojure-noob.core/emp2 clojure-noob.core=> (= emp1 emp2) false 

O que acontece é que as estruturas tem valores de [equals e hashCode](http://angeliski.com.br/equals-e-hashcode/) que define elas como iguais, isso permite que elas sejam imutáveis. Não importa em que endereço de memória essa estrutura seja criada, ela será a mesma que a criada em outro momento. A questão da imutabilidade é simples, mas complicada de entender na prática, não se preocupe com ela por enquanto, vamos nos aprofundar nesses detalhes com o tempo.

O que importa nesse momento é você aprender a função **def** que permite você criar algo que neste momento vamos chamar de variáveis (apenas pra não dar mais nó na mente).

Com isso você consegue criar definições básicas e usar em funções, veja abaixo:

    clojure-noob.core=> (def v1 2) 
    #'clojure-noob.core/v1 clojure-noob.core=> (def v2 5) 
    #'clojure-noob.core/v2 clojure-noob.core=> (+ v1 v2) 7

Essse post vai ficando por aqui(porque ele está bem maior do que eu imaginei), ele ensina algumas coisas simples mas importantes. No próximo artigo vamos falar sobre as funções, como definir, como usar, como elas existem e coisa e tal(se você quiser saber algo específico dar funções em Clojure deixa um comentário que eu tento incluir).

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski