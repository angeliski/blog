# Slice e Splice - Tem diferença?

Uma situação muito comum quando estamos trabalhando com Arrays em Javascript é a necessidade de ter que remover um item. No cenário onde temos exatamente o index que queremos remover nós podemos usar o **Splice**. Ou será o **Slice?**

Bom, vem comigo descobrir qual deles e qual é a diferença entre eles.

O Splice é um método que está disponível no Prototype do Array. A informação importante que você precisa ter sobre ele é que _**ele altera o array original**_. Se você estiver indo pro mundo funcional, talvez seja melhor evitar usar ele. Vamos dar uma olhada na assinatura dele:

```js
array.splice(index , deleteCount[, elemento1[, ...[, elementoN]]])
```

O primeiro parâmetro (index) serve para dizer qual é a posição que o método vai começar atuando. O segundo parâmetro (deleteCount) serve para dizer **quantos itens** serão deletados. Do terceiro parâmetro em diante, serão itens a serem adicionados ao array (sim, você pode remover adicionando). Importante ressaltar que os itens serão adicionados a partir da posição que você especificou no index. Vamos ver uns exemplos?

```js
const numerosDaSena = [20, 34, 56, 12, 45, 72, 12]
const indiceDoNumeroQueNaoVamosApostar = 3
numerosDaSena.splice(indiceDoNumeroQueNaoVamosApostar,1)
console.log(numerosDaSena)
```
O código acima é o exemplo mais comum do dia a dia: Você tem um array com dados e precisa remover um item (ou mais). Nesse caso, como queremos remover o número 12 do array (index 3), nós usamos o **splice** , informando o indice e a quantidade em seguida.

Vamos a um exemplo diferente.

Imagine que você é um desenvolvedor de um sistema de concessionárias. Nesse sistema, você está desenvolvendo uma feature, que permite que o vendedor selecione os carros que ele deseja remover da entrega e adicione os carros que ele quer adicionar a entrega (vamos imaginar que ele quer corrigir alguma coisa). Vamos dar uma olhada em como isso poderia ser implementado:

  ```js
const carrosParaEntrega = ['Focus', 'Ecosport', 'Fiesta', 'Fusion']
const carrosParaEntregaDeUltimaHora = ['KA', 'Mustang']
const posicaoParaRemover = 1
carrosParaEntrega.splice(posicaoParaRemover, 2, ...carrosParaEntregaDeUltimaHora)
console.log(carrosParaEntrega)
```

Se você executar esse código, vai notar que a **'Ecosport'** e o **'Fiesta'** serão removidos e a partir da posição 1, serão adicionados o **'KA'** e o **'Mustang'**. Aqui vale observar que eu usei o [Spread Operator](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Spread_operator) no último argumento. Isso é necessário pois se você passar um array no último argumento, a função vai considerar apenas um objeto, não múltiplos.

_"Hey Angeliski, eu vi que eu posso usar o delete pra apagar valores de um array também"_

![Perigo, diz o Yoda](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445755919/3US_0CVvI.gif)Perigo, diz o Yoda

Você não deveria (é minha recomendação só) usar delete para remover itens de um array. Porque? Quando você usar o delete, o array apenas remove a referência. Isso gera vários inconvenientes: O tamanho do array não é atualizado, a posição se mantém com o valor undefined e iterações sobre índice tem um potencial de problemas ilimitados. Eu te recomendo, a não fazer isso.

O que acontece muito quando você ainda não tem domínio do **Splice** é de chamar a função **Slice.** Vamos dar uma olhada no nosso primeiro exemplo, só que desta vez usando o slice.

```js
const numerosDaSena = [20, 34, 56, 12, 45, 72, 12]
const indiceDoNumeroQueNaoVamosApostar = 3
numerosDaSena.slice(indiceDoNumeroQueNaoVamosApostar,1)
console.log(numerosDaSena)
```

Se você executar isso, sabe o que vai ser impresso? Exatamente o mesmo array. A primeira coisa que você precisa saber sobre o slice é que ele não altera o array original, ou seja, ele mantém a referência original intacta. Isso é bom para os casos funcionais. Acontece que a assinatura do **slice** é um pouco diferente:

```js
arr.slice([begin[, end]])
``` 

O que o método **slice** faz é retornar uma cópia do array com base nos parâmetros de inicio e fim. O último parâmetro é opcional, se você só informar o começo, o array vai ser copiado até o final.

Realmente diferentes
--------------------

Agora que nós conhecemos os dois, conseguimos perceber que as duas funções realmente são diferentes. **Splice** altera o objeto original, permitindo alterar e remover itens de um array. **Slice** não modifica o objeto original, apenas retorna uma cópia do array. Isso pode fazer a diferença no seu dia-a-dia.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski