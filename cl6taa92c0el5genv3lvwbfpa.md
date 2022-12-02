# Clojure - Criando e Usando funções

Se você acompanha o blog tem reparado que eu não publico sobre um assunto em especifico, mas sobre o que me der na tela um assunto bacana e diversificado a cada semana. Isso acontece porque o tema do post surge conforme as coisas vão aparecendo na minha timeline, seja pelo [Twitter](https://twitter.com/), pelo [InfoQ](https://www.infoq.com/), o [Mattermost](https://about.mattermost.com/) no trabalho ou no [Linkedin](https://www.linkedin.com/).

Essa semana vamos retornar ao Clojure e aprender um pouco sobre as funcões. Tomara que esse post não seja longo.

### First-class Citizen

A primeira coisa para se entender quando estamos falando em funções Clojure, e nas linguagens funcionais, é o conceito de first-class citizen, isso quer dizer basicamente que a linguagem trata as funções como um valor.

_"Ham? Que valor Angeliski? Do que se ta falando?"_

É mais simples do que parece. Se uma função é tratada como valor, ela pode ser usada em qualquer lugar que um valor poderia. Você pode atribuir uma função a uma variável, passar a função como argumento de outra função e até mesmo retornar funções dentro de outras funções. Sabe aquele callback que você costuma usar em Javascript? Pois é, ele entra exatamente nesse conceito. Sim, o Javascript também pode ser considerado uma linguagem funcional.

### Hello World - Definindo uma função

Podemos começar aprendendo como definir uma função. Como todo bom primeiro exemplo de linguagem de programação, vamos criar uma função que nos diz Hello World.

    (defn hello
    "Essa é a nossa função dizendo oi!"
    []
    (println "Hello World "))

Vamos entender cada linha disso. A linha 1 contém a nossa palavra chave **defn** que é maneira como dizemos que queremos criar uma nova função(esse é um atalho sintático para **def fn**, mas isso não vem ao caso agora), seria como o function do Java. Em seguida nomeamos a nossa função, no nosso caso de hello, para mais tarde usarmos ela. A linha 2 é como uma documentação da nossa função, ela é opcional, então não se preocupe muito com isso agora. Na linha 3 temos a definição dos nosso parâmetros, no nosso caso não queremos passar nenhum, então temos o colchete vazio. Na linha 4 temos o corpo da função, aqui é onde vamos executar o que quisermos efetivamente fazer. Vale lembrar(ou aprender se você não sabe) que o Clojure sempre retorna por padrão a avaliação da ultima linha, tal como o Ruby ou o Groovy. No Groovy a palavra return é opcional, não é o caso aqui, se você tentar colocar return ali no fim o compilador vai acusar erro. Vamos incrementar a nossa função passando um parâmetro?

    defn hello
    "Essa é a nossa função dizendo oi!"
    [name]
    (println (str "Hello World " name)))

A principal mudança é a adição do parâmetro dentro do colchetes. Além é claro, da modificação no corpo da função para usarmos esse parâmetro. Só que tem um problema, modificar nossa função desse jeito, obriga ela sempre ter um parâmetro e isso é ruim. E se eu quiser simplesmente dizer Hello World as vezes e em outras vezes dizer um Hello mais pessoal? (Para aquela moça bonita da infra, por exemplo?) Existe uma coisa em Clojure que é chamada de _arity,_ isso é, o número de parâmetros que uma função precisa para ser executada. Você pode definir uma função com dois parâmetros, ou não, mas ela sempre vai precisar do numero certo para funcionar. Só que tem uma coisa interessante na definição de funções que é a possibilidade de criar funções com múltiplos _arities._

    (defn hello
    "Essa é a nossa função dizendo oi!"
    ([]
    (hello ""))
    ([name]
    (println (str "Hello World " name))))

Você pode ver pela linha 3 e 5 que eu criei uma função com um \_overloading \_de parâmetros, a primeira versão não recebe nada e chama a si mesmo (linha 4) passando como argumento uma string vazia, ou seja, vem o Hello World sem nome algum. A segunda opção é a chamada direta para a função que recebe o nome, onde o Hello World vem customizado.

_"Achei muito legal angeliski, mas como eu executo isso?"_

### Chamando a função

Executar a função, uma vez que ela esteja definida, é bem simples.

    (hello)
    (hello "Angeliski!")

Só isso. A primeira chamada dispara a nossa função hello sem parâmetros, enquanto a segunda dispara a nossa função customizada com o nome. Vamos incrementar as coisas...

Imagine que nós temos duas funções agora, uma função vai receber dois nomes e criar um novo nome, a outra é a nossa velha função hello, que vai continuar a mesma coisa. Vejamos nossa função **join-name.**

    (defn join-name
    [first-name last-name ]
    (str first-name " " last-name))

Ela simplesmente recebe dois nomes (first e last) e retorna uma junção dos dois. Agora que temos as duas funções, vamos executar as duas usando nosso conceito de first-class citizen.

    def t (join-name))
    (t "Rogerio" "Teste")

Aqui você pode ver a atribuição de uma função a uma variável e logo em seguida a execução dela. Existem outras interações que podemos fazer com as funções, mas acho que com isso você já consegue começar a entender a linguagem. Até a próxima!

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski