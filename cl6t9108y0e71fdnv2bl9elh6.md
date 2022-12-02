# O que a Programação Funcional pode fazer por você?

Um dos tópicos da programação que se tem falando muito é a Programação Funcional... Algum de vocês devem estar se perguntando, que diabos é isso? Vamos começar esclarecendo algumas coisas então!

No meio da programação existem três paradigmas mais conhecidos, o primeiro comumente chamado de [Programação estruturada](http://www.fisica.ufjf.br/~sjfsato/fiscomp1/node23.html). Isso se deve ao fluxo do programa, seguindo um fluxo continuo e ESTRUTURADO. O nosso amigo Cobol é um grande exemplo desse paradigma. Temos também a [Programação Orientada a Objetos (POO)](http://www.ufjf.br/peteletrica/files/2012/10/Curso-Orienta%C3%A7%C3%A3o-a-objetos.pdf) que tem como sua premissa a criação de objetos, que contém propriedades (variáveis) e funções (métodos). O funcionamento do programa se dá com base nas iterações entre objetos, que vão chamando e interagindo entre si. O grande representante desse paradigma com certeza é o Java. E não menos importante temos a [Programação Funcional](http://www2.ic.uff.br/~bazilio/cursos/lp/material/ProgFuncional.pdf). A ideia desse paradigma é trabalhar principalmente com funções. Tudo se resume a funções que podem ser agrupadas ou combinadas para produzir um resultado. Ela se vale de muitos principios matematicos, o que as vezes pode ser complicado de entender. Como exemplo podemos citar o Haskell.

Essa é uma explicação muito simplificada dos paradigmas, visando mostrar a ideia principal por trás deles, cada um tem um conjunto maior de características e regras que definem ele como um todo. Dito isso podemos voltar ao nosso tópico principal, que é a Programação Funcional!

### **Porque ela tem sido tão falada ultimamente?**

Acontece que cada vez mais os hardwares que temos hoje estão se limitando e a evolução está tomando um caminho de multi processamento, ou seja, os computadores cada vez mais estão sendo feitos com múltiplos núcleos de processamento para realizar muitas operações em paralelo. Só que gerenciar esses multi processamentos, também conhecidos como processamento multithread, exige um esforços cada vez maior e é ai que as linguagens funcionais se destacam no cenário atual. **Você ganha o processamento multithread de graça!**

_"Nossa Angeliski, que mágica é essa?"_

Calma, não é mágica.

Acontece que muitos dos conceitos que estão definidos dentro do Paradigma Funcional permitem que as linguagens funcionais tenham um comportamento multithread sem muito esforço. Vamos a um exemplo, a [Imutabilidade](http://www.ibm.com/developerworks/library/j-ft4/). Um dos conceitos que existe dentro do paradigma funcional é a imutabilidade, ou seja, um dado é criado e nunca mais será mudado até o fim da sua vida, ele é imutável.

_"Mas como pode isso? Eu não posso mudar nada?"_

É um pouco mais complexo que isso, além de existir algumas exceções....

**Pausa Dramática no Texto**

* * *

Sim, eu sei que existem muitas lacunas nas coisas que eu estou te dizendo, mas se você é alguém que não entende do que eu estou falando é está lendo isso, explicar essas lacunas vai te deixar completamente perdido. Se você entende, as lacunas já estão preenchidas. Então vamos por partes, eu vou explicar algumas coisas nesse texto e outras mais pra frente, em outros textos.

* * *

**Fim da Pausa Dramática**

Voltando a imutabilidade, suponha que você criou uma variável **idade** com o valor _**10**_. Para todo o sempre a variável idade terá o valor _**10**_. Mas e se eu quiser incrementar esse valor? Então uma nova variável, que pode ter o mesmo nome (para simplificar as coisas), será criada, com o valor _**11**_ por exemplo. A imutabilidade garante que você pode usar essa variável em milhões de processamentos paralelos e que o valor será imutável. Se fosse no Java por exemplo, você teria que usar mecanismos de sincronização para que essa variável pudesse ser compartilhada em muitas threads sem nenhum problemas. Consegue enxergar a vantagem? Como a imutabilidade já faz parte do paradigma funcional você não precisa se preocupar com nada disso. Maravilha né?

Além disso, uma das coisas boas sobre aprender o paradigma funcional é a chance de pensar fora da caixa. Quando você aprende um novo paradigma, seja ele estruturado, orientado a objetos ou qualquer outra coisa, sua mente precisa pensar de uma maneira completamente diferente para resolver os mesmos problemas. Isso te permite crescer e aprender novas estratégias para resolução de problemas, novas ferramentas. Como programador você deve deixar de lado a _**bala de prata**_, cada caso tem uma solução mais adequada, hoje pode ser estruturada (Cobol vive até hoje nos bancos), amanhã pode ser programação funcional (Clojure é a linguagem principal da Nubank).

Você deve estar se perguntando se eu domino a Programação Funcional e a resposta é... não.

"O.o Mas como pode? Se faz maior propagando do negocio e não entende?"

Eu disse que não domino. Atualmente estou estudando sobre o assunto e por isso vim compartilhar a minha visão do porque aprender isso, mais pra frente vou tentar trazer alguns exemplos práticos.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski