# Angular Directive - Entendendo a magia

Um framework que contém muitas ferramentas é o **[AngularJs](https://angularjs.org/)**. Esse post exige um conhecimento mínimo de angular, se você quiser um post mais básico deixa um comentário que eu tento escrever sobre.

### Pra que serve uma diretiva?

A primeira coisa  que é interessante sabermos é qual é o propósito de uma diretiva. Quando estamos construindo uma aplicação existem certos pedaços dela que se repetem muito. Imagine que você está construindo uma aplicação para uma rede de sapatos,  em várias telas existe um combo que permite você escolher uma loja. Hora você usa esse combo num cadastro, outra hora em uma pesquisa. Dali a pouco você precisa dele num filtro de relatório. Imagine que esse combo é parecido com isso:


```html
<select class="select" name="loja">
    <option value="1">Loja 1</option>
    <option value="2">Loja 2</option>
    <option value="3">Loja 3</option>
 </select>
``` 


Esse código é simples, mas serve de exemplo. Você colocou esse combo em vinte lugares diferentes. Uma semana depois, teve uma loja nova, você precisou mudar em todos os lugares. Depois de um mês, a Loja 1 foi desativada por estar em obras, mais uma modificação... Percebe como é custoso ficar mudando isso?

**Ai que entram as diretivas!**

_"Angeliski, eu ainda não entendi pra que servem essas diretivas"_

Na programação temos um conceito simples chamado **DRY(Don't repeat yourself)**  que preza basicamente para que você evite replicar código sem necessidade. No nosso caso, se todos os combos de loja tem o mesmo propósito (permitir que o usuário selecione uma loja para que nossa aplicação faça algo com a loja selecionada) então ele deveria ser um código único, centralizado. A diretiva nos oferece essa ferramenta, ela permite que criamos algo parecido com um componente que vamos aplicar onde for necessário. Imagine que usando diretivas, você só vai colocar isso no lugar do combo:


```
<diretiva-loja ng-model="lojaKey">
``` 

Só isso. E se precisar mudar qualquer coisa no combo (salvo alguns casos, que veremos adiante) você só altera **UM LUGAR.**  Essa é a maior uma das maiores vantagens das diretivas.

_"Legal! Mas como faz isso?"_

### Construindo nossa diretiva

Eu vou disponibilizar esse [repositório no github](https://github.com/angeliski/diretivas-angular) onde você pode consultar todo o código fonte e verificar como funciona. A cada trecho que nós formos incrementando, eu vou fazer um [commit](https://github.com/angeliski/diretivas-angular/commits/master), pra ficar mais fácil de vocês acompanharem as mudanças. Vamos começar com o mais básico de tudo, a nossa estrutura do index.html.

```
<html>
   <head>
      <title>Diretivas em Angular</title>
   </head>
   <body>
      <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.9/angular.js"></script> 
   </body>
</html>
```

Não tem nada de mais, uma página html com o script do angular adicionado (pelo cdn, assim a gente não precisa baixar nada). Vamos agora criar uma pasta js e adicionar o nosso  bootstrap da aplicação.

`angular.module('diretivaApp',[]);`

Até aqui nada de anormal, criamos um index.html e iniciamos nosso app angular. Vamos criar agora a nossa diretiva, da maneira mais simples possível.

    
```
angular.module('diretivaApp').directive('diretivaLoja', function (){ 
return { 
template: '<select class="select" name="loja">
                    <option value="1">Loja 1</option>
                    <option value="2">Loja 2</option>
                    <option value="3">Loja 3</option>
                  </select>' } });
``` 


Esse é o código mais simples do universo parar criar sua diretiva e você consegue usar ela simplesmente adicionando na sua página.


```
<div diretiva-loja />
``` 


Agora que nós já temos nossa diretiva, vamos tornar as coisas mais interessantes.

### Restringindo a aplicação da diretiva

Quando estamos criando uma diretiva é importante saber como vamos aplicar ela, você pode usar uma diretiva das seguintes maneiras:


```
<div diretiva-loja/>
<div class="diretiva-loja"> 
<diretiva-loja /> 
<!--directive:diretiva-loja -->
``` 


A maneira como você vai fazer isso depende da sua necessidade, mas para que isso funcione você precisa adicionar na sua diretiva uma atributo chamado **[restricted](https://docs.angularjs.org/api/ng/service/$compile#directive-definition-object)**, veja um trecho da documentação:

> #### `restrict`
> 
> String of subset of EACM which restricts the directive to a specific directive declaration > style. If omitted, the defaults (elements and attributes) are used.
> 
>     E - Element name (default): <my-directive></my-directive>
>     A - Attribute (default): <div my-directive="exp"></div>
>     C - Class: <div class="my-directive: exp;"></div>
>     M - Comment: <!-- directive: my-directive exp -->

Isso básicamente pode ser traduzido para o seguinte:

 
```
<div diretiva-loja/> -- Aqui você usa o A, mas ele vem por default
<div class="diretiva-loja"> -- Para esse você usa o C 
<diretiva-loja /> -- Para esse você usa o E, mas ele vem por default 
<!-- directive: diretiva-loja --> Para esse você usa o M
``` 

No nosso cenário, nós queremos que o próprio desenvolvedor escolha como ele quer usar, então vamos adicionar todos os tipos:


```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { template: '<select class="select" name="loja">

<option value="1">Loja 1</option>

<option value="2">Loja 2</option>

<option value="3">Loja 3</option>

</select>', restrict: 'EACM', } });
``` 


Com isso, podemos mudar nosso index.html para algo que tenha uma semantica mais agradável(Pelo menos do meu ponto de vista):

```
<diretiva-loja />
``` 

É importante saber qual restrição você quer usar ou que a diretiva disponibiliza, isso evita que erros bobos ocorram. Vale observar que se você tentar usar o comentário, sua diretiva não vai funcionar. Acontece que a diretiva só pode ser aplicara quando o atributo replace for igual a true, vamos ajustar isso:


```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { template: '<select class="select" name="loja">

<option value="1">Loja 1</option>

<option value="2">Loja 2</option>

<option value="3">Loja 3</option>

</select>', restrict: 'EACM', replace: true } });
``` 

O comentário não é muito utilizado porque ele tem uma semantica fraca, mas vale a pena deixar o replace para que o conteúdo da sua diretiva substitua o código, ao invés de deixar um pequeno "lixo"na página quando a diretiva for aplicada. Outro detalhe importante é que o replace está **dreprecated**, ou seja, na versão 2.0 ele vai ser descontinuado.

### Entendendo o escopo na diretiva

Uma das coisas que chama atenção no angular é o [two way databinding](http://blog.caelum.com.br/angular-2-o-fim-do-two-way-data-binding/). Acontece que para isso funcionar o angular se vale de escopos, seja o do controller, do service, da aplicação e por ai vai. Normalmente quando estamos construindo uma aplicação em angular, temos um controller para gerenciar nossa página, as interações do usuário entre outros detalhes. Eu não vou entrar no mérito de não usar o scope no seu controller, porque o objetivo aqui não é esse. Então vamos adicionar um controller na nossa aplicação.

    angular.module('diretivaApp').controller('BuscaLojaController', function ($scope){
    
    $scope.buscar = function (){ $scope.label = "Buscando..."; }
    
    $scope.label = "Buscar"; });

Vamos usar nosso controller para gerenciar uma " listagem" de vendas, como se a nossa aplicação fosse buscar as vendas da loja. Isso dá um contexto interessante para trabalharmos. Você pode observar que nesse controller injetamos o $scope (essa pode não ser a melhor prática, mas foi feito assim a título didático) e definimos uma função buscar e uma variável chamada **label** nesse escopo, para que possamos fazer uso dela na página.

    
```
<html>
   <head>
      <title>Diretivas em Angular</title>
   </head>
   <body ng-app="diretivaApp" ng-controller="BuscaLojaController as vm">
      <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.9/angular.js"></script> <script type="text/javascript" src="js/app.js"></script> <script type="text/javascript" src="js/controller.js"></script> <script type="text/javascript" src="js/diretiva-loja.js"></script>
      <button ng-bind="label" ng-click="buscar()"></button> 
      <diretiva-loja ></diretiva-loja>
   </body>
</html>
```  
     

Agora vamos incrementar nossa diretiva. Já que ela vai ser um combo de lojas,  faz sentido eu  colocar um label em cima dela toda vez que eu for usar? É mais fácil elegante incluir isso na diretiva. Vejamos como fica.


```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { template: ' <div><label ng-bind="label" /><select class="select" name="loja"> <option value="1">Loja 1</option> <option value="2">Loja 2</option> <option value="3">Loja 3</option> </select></div> ', restrict: 'EACM', replace: true, link: function ($scope){ $scope.label = "Lojas"; } } });
``` 

Pronto!  Nossa diretiva está completa agora,  vamos  ver  o resultado.

![Botão e Diretiva](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445887580/mnQIKvnIq.png)Botão e Diretiva

##### _Ps. Sim, eu sei que o visual está ficando bem feio, mas esse não é nosso objetivo agora._

Espero que você tenha notado o erro. Se não notou, volte na imagem e de uma procurada.

Ok. Você achou, o label que está no botão e na diretiva está idêntico. E pior, se você apertar o nosso botão buscar, os dois vão mudar também!

_"Porque isso acontece angeliski? Eu fiz algo errado?"_

Digamos que não. Acontece que o escopo que a diretiva usa é compartilhado com o controller. Esse é o comportamento padrão da diretiva, e pode ser útil fazer isso, mas uma diretiva que compartilha o escopo do controller vai se tornar mais complexa de usar e pode gerar muitos side-effects se você não tiver muito cuidado. Na maioria dos casos, nós vamos querer um escopo isolado para nossa diretiva, o que é bem simples de ser feito.

    

```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { template: ' <div><label ng-bind="label" /><select class="select" name="loja"> <option value="1">Loja 1</option> <option value="2">Loja 2</option> <option value="3">Loja 3</option> </select></div> ', restrict: 'EACM', replace: true, scope: {

}, link: function ($scope){ $scope.label = "Lojas"; } } });
``` 

Apenas adicionando a propriedade **scope** e criamos um escopo novo e vazio para a nossa diretiva.

### Escopo não tão isolado

Legal, nós já sabemos como não compartilhar nosso escopo com a diretiva, mas como eu  faço pra compartilhar só algumas coisa? Vamos pensar no nosso cenário, temos um label na nossa diretiva, mas e se quisermos mudar para **Filiais**? Não podemos fazer isso dentro da diretiva, porque isso mudaria tudo. Temos que pensar em algo como uma propriedade:

```
<diretiva-loja label="{{labelLoja}}" ></diretiva-loja>
``` 

Onde o **labelLoja** é uma váriavel definida no nosso controller

    angular.module('diretivaApp').controller('BuscaLojaController', function ($scope){
    
    $scope.buscar = function (){ $scope.label = "Buscando..."; }
    
    $scope.label = "Buscar"; $scope.labelLoja = "Filiais"; });

Vejamos o que precisa ser feito na nossa diretiva


```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { template: ' <div><label ng-bind="label" /><select class="select" name="loja"> <option value="1">Loja 1</option> <option value="2">Loja 2</option> <option value="3">Loja 3</option> </select></div> ', restrict: 'EACM', replace: true, scope: { label :'@?' }, link: function ($scope){ $scope.label = $scope.label || "Lojas"; } } });
``` 

Você pode observar que dentro do escopo que criamos na váriavel **scope** adicionamos o **label :'@?'.** O que isso significa? Primeiro o **label** é onde vamos atribuir aquele valor no escopo que estamos criando, no caso, queremos guardar nosso valor dentro de label. Depois vem o **@**  que define como será o compartilhamento das váriaveis. Você pode usar três simbolos aqui:

*   **@** somente para bind de strings.
*   **&** para um para um **one way binding**  que nada mais é que um binding unidirecional, se você alterar o valor no controller muda na diretiva, mas mudar na diretiva não propaga de volta
*   **=** Que é o classico **two way binding**, você cria um vinculo entre os dois escopos somente para aquela váriavel.

[Aqui](https://medium.com/@johnnysitu/angularjs-directive-with-isolated-scope-650451c80e80) você pode encontrar uma explicação bem detalhada sobre isso, caso você não tenha compreendido ou queira saber mais.

E o último elemento é o **?**, esse caracter define a condição de opcional para esse atributo. Se ele não estiver ali, você sempre terá que informar aquela variável, o que não é o nosso caso.

Esse post já está enorme e eu vou falar somente de uma última coisa, o atributo **templateUrl**. Quando você começa a criar diretivas mais complexas, o template deixa de oferecer um organização sustentável, então vamos mover ele para uma página só dele.

Primeiro criamos uma página html (diretiva-loja-template.html) na raiz da nossa aplicação


```
<div> <label ng-bind="label" /> <select class="select" name="loja"> <option value="1">Loja 1</option> <option value="2">Loja 2</option> <option value="3">Loja 3</option> </select></div>
``` 


E na nossa diretiva, ajustamos a **templateUrl** para apontar para essa página.

    
```
angular.module('diretivaApp').directive('diretivaLoja', function (){ return { templateUrl: '../diretiva-loja-template.html', restrict: 'EACM', replace: true, scope: { label :'@?' }, link: function ($scope){ $scope.label = $scope.label || "Lojas"; } } });
``` 

Pronto. Isso já resolve nosso problema, mesmo que o html fique mais complexo (e nós vamos evoluir essa diretiva ainda mais com o tempo) a página garante que ele não vai ficar aninhando no js de maneira estranha.

Aqui vale uma observação, muitos browsers tem algumas limitações de cross origin, o que pode impedir que o **templateUrl** seja carregado. Normalmente eu resolvo isso iniciando um servidor Python simples.

    python -m SimpleHTTPServer 8000

Você pode subir um servidor node também, vai do seu gosto. No próximo post sobre angular vamos nos aprofundar em outra técnicas para construir a nossa aplicação. Até lá eu já devo ter melhorado o visual dessa nossa página. :)

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski