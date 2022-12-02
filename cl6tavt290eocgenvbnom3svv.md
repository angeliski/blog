# Especificidade CSS -  Magic!

A minha principal formação vem do backend e por consequência, sempre existiu uma certa dificuldade em lidar com o CSS. Até o dia que eu descobri que ele não funciona aleatoriamente! Hoje eu vou mostrar a magia que existe por baixo do funcionamento do css.

A primeira coisa que nós precisamos entender quando estamos falando de css é que seu funcionamento se baseia no conceito de seletores. Que são maneiras que o css tem para encontrar um conteúdo X (desde uma div, até um simples span) e aplicar nesse conteúdo o valor das regras definidas. Vamos ver um exemplo.

    
    
    .conteudo-site{
      background-color: red;
    }

Esse é um dos usos básicos, definimos uma classe na nossa div e atribuímos uma regra para essa classe. Existe um conjunto de seletores, que podem ser usados no css e esse não é o enfoque deste artigo. Mas vamos a um segundo exemplo.

    
    .conteudo-site{
       background-color: red;
    }
    #principal{
       background-color:green;
    }

Nesse caso, dois seletores se aplicam a div, tanto a classe quanto o id. Você sabe qual vai ser a cor aplicada na div? Se você conhece um pouco de css sabe que é a cor aplicada vai ser a verde. Mas a pergunta verdadeira é, você sabe porque é a cor verde?

![Tipo magia](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445862793/M8NHwNuM2.gif)Tipo magia

Por muito tempo eu não entendi muito bem como as regras se aplicavam. Eu sabia que o id tinha prioridade sobre a classe, mas eu não sabia porque isso. Até que eu aprendi sobre **Especificidade.** 

Imagine que todo seletor, tem um peso, composto por 4 números: prioridade, id, classe, elemento. Vamos olhar nossos dois seletores do exemplo.

*   conteudo-site = 0010
*   principal = 0100

Se você observar, o valor obtido pelo seletor do ID é maior que o da classe. Isso define que o seletor aplicado vai ser o ID e não a classe. Acontece que esse peso é registrado pelo número de itens de um certo elemento. Imagine o seguinte seletor:

    .conteudo div p span {
        font-size: 13px;
    }

O valor desse seletor segue a mesma regra. Temos o valor 0013, pois existem 3 elementos (quarto número) e apenas ma classe(terceiro número). Fica um pouco confuso não? Talvez uma imagem ajude:

![Especificidade](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445865221/rpOjSY5Gn.png)Especificidade

Além da imagem, fica a recomendação [dessa página](http://www.laurensperber.com/2013/06/25/css-specify-me/) que mostra o score de um seletor. Se você prestou atenção no texto até aqui percebeu que a imagem acima só tem três casas e eu venho falando até agora de 4 casas. Essa quarta casa é "ativada" quando você usa aquele !important (Não faça isso. Mesmo) no seu código. Uma última coisa que vale a pena mencionar, é que caso dois seletores tenham o mesmo valor, o último a aparecer vence. Espero que agora o css não seja mais um mistério para você apesar de ainda continuar complicado.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski