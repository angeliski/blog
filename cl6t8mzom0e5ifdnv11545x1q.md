# Trabalhando com Arrays - Finishin...

Faz Muito tempo que não posto alguma coisa.. vou tentar manter uma constancia de posts.. vamos terminar aquele post de arrays? falta só uma certa busca...

Para ficar mais interessante vamos ver como fazer uma busca de determinado valor em um vetor, sem levar em consideração algorimos otimizados, de menor custo e blablabla (que AGORA não deve ser seu foco, primeiro entenda o que fazer, depois aprenda como fazer do melhor modo possivel.), logico que tambem não vamos fazer um serviço qualquer, vamos tentar fazer um programa razoavel. Vamos ao codigo:( [Se você nao sabe o que é função..](http://www.inf.pucrs.br/~pinho/LaproI/Funcoes/AulaDeFuncoes.htm))


```js
// A função recebe x, n >= 0 e v e devolve um índice k em 0..n-1 tal que x == v[k]. 
// Se tal k não existe, devolve -1. 
int busca(int x, int n, int v[]) {
    int k;
    k = n - 1;
    while (k >= 0 && v[k] != x) {
        k -= 1;
    }
    return k;

}
``` 


"Cara, eu nem entendi o que esse negocio ai faz."

Tudo bem, eu confesso que num primeiro vislumbre é uma função sem sentido, mas vamos analisar ela pra você ver como é simples. O Comentario ajuda num entendimento geral do que ela faz, mas vamos começar pelo como: Ela recebe o x, que é o valor que queremos saber se esta ou não lá dentro, o n que é o tamanho desse Array e o Array em si.

Logo de cara ela declara um k qualquer e atribui para ele o ultimo indice do Array (n-1), e partimos para o while. Ele testa primeiro se o k esta numa posição valida, ou seja se ele é maior ou igual a zero( estamos indo do fim do Array para o começo), em seguida ele testa se o valor que esta em v\[k\] é diferente de x, se ele for diferente ele subtrai 1 de k(indo para outra posição, da maior posição para a menor), caso o valor de x e v\[k\] seja igual ele retorna k, ou seja ele devolve o indice onde se encontra o valor que é igual ao procurado. “E se ele não achar?” Ele retorna -1, como você pode observar quando k for igual a 0 ele entra no laço, dixa k=-1, em seguida o teste da falso e vem o retorno. Vale observar uma coisa importante nesse caso, a função vai retornar o indice do PRIMEIRO valor igual a x que ela achar, se houver mais de um valor igual em indices diferentes, ela simplesmente retorna o primeiro que achar.

"Ah Angeliski, mas eu quero todos os indices que tem o valor igual do meu x, como eu faço?"

Logica amigão, pense assim, você passa para a função um valor n que é o tamanho do Array, mas isso nada quer dizer pra ela, esse valor só representa apartir da onde ela vai buscar. Se você tem um Array de 100 posições e tem certeza que não esta nas ultimas 20, você passa 80 para a função, ela começa a procurar na posição 79 (n-1). De repente ela retorna o indice 46, mas como ter certeza de que não tem mais nenhum valor igual a x no restante do Array? É só passar para a função o valor de n como 46, assim ela vai iniciar a busca em 45 (n-1), testando o restante. Se o retorno for -1, você sabe que o indice 45 é o unico que contem um valor igual a x, se não você manda esse novo indice para ela testar os outros, isso até que o retorno seja -1,assim você tera todos os indices onde o valor de x esta.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski