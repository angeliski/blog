# Trabalhando com Arrays - Parte 1

Uma estrutura que eu tenho visto dar trabalho para o pessoal são os Arrays, então resolvi escrever um pouco sobre eles, vamos antes fazer algumas considerações.

Esses assuntos mais genéricos que podem ser aplicados em distintas linguagens vão ser escritos em C ANSI

“Mas Angeliski, você não falou que tava aprendendo Java? cade o Java man?”

Você empurra a carroça ou os bois que puxam? calma gafanhoto, o caminho do Cara Te (grana) é longo.

Falando sério, o C ANSI é um padrão que deveria ser aprendido por todos, ele da base para muitas outras linguagens, desde o C++ até o Java, que tem sintaxe muito parecida, ele é simples de entender e pode facilmente transpor os conceitos para outra linguagem qualquer que você for implementar, então eu vou usar ele para passar códigos, lógico que se eu for falar de algo especifico do Java, não tem como colocar em C ANSI né? E a Orientação a Objetos (que eu pretendo demorar um pouco para abordar..) provavelmente será abordada em Java, primeiro porque eu ainda não tive vivência valida com o C++, segundo que a gente da uma mudada né, a parte gráfica do Java é uma coisa que da uma motivada a mais em quem esta aprendendo. Esclarecido os Pingos do i vamos começar:

Em vários lugares você não vai encontrar o termo Array e sim Vetor, isso se dá a uma “tradução” feita por alguém, eu aconselho você a chamar de Array.

“Certo, mas o que é esse Array?”

![Eu guardo roupas no vetor?](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445951354/286iigGFA.jpeg)Eu guardo roupas no vetor?

Imagine que você tem um gaveteiro, e que cada gaveta dele tem um numero, se eu falo pra você: ‘Pega o item que esta na gaveta 2’, você vai lá na gaveta certa e me pega o item. Se você entendeu como esse gaveteiro funciona, só troque a palavra por Array. “Quer dizer que o Array serve pra guardar roupas?” ( O.O ) Ok, vou explicar melhor. O Array funciona como um lugar para guardar coisas, ele tem um tamanho, assim como o gaveteiro, ele tem varias posições pra guardar as coisas, varias gavetas, e cada uma dessas posições tem um “endereço”, assim como o gaveteiro tem um numero para cada gaveta. Uma confusão muito comum é confundir o endereço da gaveta com o valor dela veja o exemplo:


| Endereço | 1 | 2 | 3 | 4 | 5 | 6 |
|----------|---|---|---|---|---|---|
| Valor    | 1 | 2 | 4 | 6 | 8 | 6 |

Se eu perguntar qual o valor do endereço 1? Resposta rápida:1

Mas e se eu perguntar qual o valor do endereço 4, a resposta é 4,3 ou 6? Vejamos:

| Endereço | 1 | 2 | 3 | *4* | 5 | 6 |
|----------|---|---|---|---|---|---|
| Valor    | 1 | 2 | 4 | 6 | 8 | 6 |

Agora ficou fácil, o endereço 4 tem o valor 6! Essa é uma confusão muito grande, endereço e valor são diferentes, o endereço 7 pode não conter valor algum, pode contar um valor qualquer, mesmo que ele seja diferente do valor 7, ele continua tendo endereço 7. Não se preocupe se deu uma enrolada, continue lendo que até o fim vai ficar claro.

Se imagine um programador recém contratado, pra você se acostumar com a rotina, seu chefe te passa um programa simples que  seu professor fazia na faculdade.

“É pra escola que abriu, você tem de pegar duas notas, P1 e P2, fazer a media e apresentar tudo, só isso cara.” Você até da risada, já que você tinha feito isso no primeiro semestre. Moleza.

5 minutos depois, quando seu chefe olha o programa, ele questiona:” mas você só fez pra um aluno?” Você usa todo o poder ninja para sair de enrascadas: ”Não, é que o Sr. não me disse quantos alunos queria, ai eu implementei só pra um inicialmente e vim perguntar.”

“Ah entendi, faz pra uns 10”. Você corre la e cria todas as 30 variaveis.(10 de P1, 10 de P2 e 10 da media) e volta felizão. “O pessoal da escola ligou, eles precisam para 100 alunos em vez de 10” De volta a mesa, copia, cola, muda aqui, muda ali, meche aqui.

**300 variaveis** depois. “Ficou legal, mas faz assim, como a escola é nova coloca para 1000 alunos que eles não vão ter problema de crescimento até o ano que vem pelo menos.” A lágrima brota no olho, **3000 Variaveis**! Como eu faria?

    float  P1[1000],P2[1000],MED[1000];

(O.o) “Que diabos isso quer dizer?”

Bom, primeiro você tem de entender uma coisa sobre o Array, _ele é uma estrutura de dados que armazena uma sequência de objetos, todos do mesmo tipo, em posições consecutivas da memória._ Não ajudo? O Array é uma lista de variáveis, em vez de você colocar P1\_1,P1\_2...P1\_N, você cria uma estrutura que faz isso:

onde:

*   P1: é o nome da variável, você pode chamar do que quiser, desde que atenda as regras para criação de nome de variáveis.
*   \[ \]: isso identifica essa variável como uma estrutura sequencial, você afirma para o compilador que aquilo é um Array.
*   N: é um valor qualquer que identifica o TAMANHO do Array,numa primeira declaração, em demais casos se refere ao índice que você quer acessar( anteriormente citado como endereço). Ou seja, quando algum problema tiver um numero grande de variáveis de mesmo tipo, você criar um Array que armazena esses valores, ao invés de ficar escrevendo uma a uma as variáveis.

“E como eu guardo valores nessa sequência ai?”

Igualzinho a uma variável qualquer, mas com o detalhe do índice. imagine que você tem a seguinte situação:

o que podemos analisar dessa situação, temos 4 variaveis ai, X1,X2,X3 E X\[ \]. Se eu pedir pra você colocar 10 em todos eles, como fazer?

    X1=10;
    
    X2=10;
    
    X3=10;
    
    X[ ] =10;

certo? Não.

A ultima atribuição vai gerar um erro no seu compilador, se não acredita pode tentar.

Porque? Se você tivesse feito X=10; daria erro também certo, afinal a variável com nome X não existe, X1 sim. O mesmo é valido para X \[ \], ela é uma sequência, logo X\[1\] e valido, ao contrario de X\[ \] que não aponta para lugar nenhum.Quando você cria uma variável com um Array, você define um tamanho para ela, dizendo que ela vai ter n lugares, o índice final dos Arrays sempre será dado por n-1. “Cara, se você diz que o tamanho do Array é n, como ela vai ter uma posição a menos?”

Quantos números tem de 0 a 9?

Muita gente, **MUITA MESMO** responde 9, e se você respondeu, saiba que esta errado.

“O cara nem sabe contar veio...nem vo continuar lendo...)

Contemos então:

0 - 1º

1 - 2º

2 - 3º

3 - 4º

4 - 5º

5 - 6º

6 - 7º

7 - 8º

8 - 9º

9 - 10º

\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/\\o/

Exatamente, o 0 é um numero, ele é contado! Os Arrays também fazem isso, logo o primeiro numero do índice será sempre 0 ( POSIÇÃO INICIAL!) e termina em n-1 (POSIÇÃO FINAL!) já que a declaração de n não “considera o 0”, temos de considerar no tamanho dele.Entao nosso Array declarado como X\[3\] tem as posições X\[0\],X\[1\] e X\[2\], disponiveis para guarda um valor do tipo INT que é como ele foi declarado. “Por que int?” Leve em consideração uma variável primitiva qualquer([um otimo post se você nao sabe o que é variavel primitiva](https://dicasdeprogramacao.com.br/tipos-de-dados-primitivos/)), se você declara ela como int, nao vai conseguir inserir valores do tipo float, o mesmo é valido para os Arrays, já que eles são apenas estruturas sequenciais do tipo que foi determinado. Já teorizamos demais esse Array ai, que tal a gente por a mão na massa agora?

Nada melhor que começarmos com o simples, colocar os valores no Array. Uma coisa que ninguém percebeu (acho...) até agora é que do jeito que foi explicado o Array ele continua sem utilidade, fora o fato que eu não preciso declarar 100 vezes ele no começo, mas se eu fosse fazer uma soma do X\[3\] como seria:

com X\[3\] (quando me referir a tamanho do Array, eles estão sublinhados para ficar mais fácil de distinguir, do contrario é POSIÇÃO) fica fácil de fazer isso, mas e X\[100\], X\[1000\]? A Vantagem do Array surge no índice, pois ele pode ser uma variável, veja um exemplo de soma com Array:

    for(int i=0;i<100;i++) soma=X[i]+soma;

nesse caso, o for vai rodar de 0 a 100, percorrendo todas as posições do Array. Fácil, simples e pratico. Quem sabe o que esta errado na frase acima? O For não percorre o Array de 0 a 100, percorre ele de 0 a 99 (i<100 e não i<=100), o que é uma diferença importante, pois se ele tentasse acessar a posição 100 poderia haver algum erro grave, já que essa posição não foi reservada pelo nosso programa, então muito cuidado na hora de criar suas condições para laço, seja for, while, if ou qualquer outro. “Você falou, falou e continuo sem saber como inserir valores no Array...” Vamos pensar um segundo. Se você sabe que o índice pode ser uma variável, fica fácil trabalhar com esse Array, para inserir o valor em todas as posições, você vai ter de percorrer todos as posições dele (se for necessário lógico, mas por hora vamos pensar que é!), e em cada posição receber o valor:

Pronto! O for vai de 0 a \_\_(qual o valor?) e cada posição ele vai esperar você inserir o valor, antes de ir para a próxima posição pegar o próximo valor.

Se você entendeu como inserir valor então sabe como imprimir os valores. Senão Volte la pra cima e leia de novo.

“... there is a huge difference between programs that merely work and programs that are well-engineered,just as there is a huge difference between a log thrown over a river and a well-engineered bridge."

\[P. A. Darnell, Ph. E. Margolis, Software Engineering in C\]


Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski


Referencias Usadas:

- [IME](http://www.ime.usp.br/~pf/algoritmos/aulas/array.html)

- [Dicas de Programação](https://dicasdeprogramacao.com.br/tipos-de-dados-primitivos/)