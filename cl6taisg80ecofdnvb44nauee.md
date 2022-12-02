# Provider, Service, Factory - Qual a diferença?

Esse post nasceu de um pedido de um amigo que me ensinou muita coisa sobre Javascript.

### Tudo farinha do mesmo saco

Pra já começar na polemica, eu vou te dizer que é tudo **[Provider](https://docs.angularjs.org/guide/providers)**. Se você dúvida, eu te mostro a documentação:

> The injector creates two types of objects, **services** and **specialized objects**.
> 
> Services are objects whose API is defined by the developer writing the service.
> 
> Specialized objects conform to a specific Angular framework API. These objects are one of controllers, directives, filters or animations.
> 
> The injector needs to know how to create these objects. You tell it by registering a "recipe" for creating your object with the injector. There are five recipe types.
> 
> The most verbose, but also the most comprehensive one **is a Provider recipe**. The remaining **four recipe type**s — Value, Factory, Service and Constant — are just **syntactic sugar on top of a provider recipe**.

O que isso diz é que basicamente, _**Value, Factory, Service e Constant**_ são Providers, mas com um atalho sintático para facilitar a nossa vida, ou seja, eles são providers encapsulados de uma maneira que seja mais fácil criar eles. Claro que na pratica isso cria diferenças entre eles, que é o que importa pra gente vamos ver aqui hoje.

### Value e Constant

Esse dois são sem dúvidas os mais simples de entender. **Value** permite você definir um valor para ser injetado em outras partes da sua aplicação. Você pode querer fazer isso por exemplo para definir o clientId dentro da sua aplicação.

    myApp.value('clientId', 'a12345654321x');
    
    myApp.controller('DemoController', ['clientId', function DemoController(clientId) {
        this.clientId = clientId;
    }]);

Isso permite que você defina valores em uma parte do seu módulo e injete ele em outros.

**Constant**  tem um funcionamento quase que igual, o que muda entre ele e o **Value** é que o **Constant** está disponível na fase de configuração.

    myApp.constant('planetName', 'Greasy Giant');
    
    myApp.config(['planetName', function(planetName) { }]);

Você também consegue injetar o **Constant** no seu controller, mas se tentar injetar o **Value** no seu config ele vai gerar um erro.

### Factory

O **Factory** adiciona algumas vantagens em cima dos outros dois:

*   Injeção de dependências
*   Inicialização de serviços
*   Inicialização Lazy (Lazy Loading) (isso quer dizer que ele só é instanciado quando alguém realmente precisa dele)

A ideia do **Factory** é basicamente criar algo não me diga. Vamos imaginar que temos um **Factory** que gera o nosso token da api, com base no nosso clientId (definido como **Value**):


```
myApp.value('clientId', 'a12345654321x');

myApp.factory('apiToken', ['clientId', function apiTokenFactory(clientId) {
    var encrypt = function(data1, data2) {
        // NSA-proof encryption algorithm:
        return (data1 + ':' + data2).toUpperCase();
    };

    var secret = window.localStorage.getItem('myApp.secret');
    var apiToken = encrypt(clientId, secret);

    return apiToken;
}]);
``` 


Como você pode ver, eu injetei a dependência do **Value**, em seguida gerei um novo token. Se você quer criar algo simples para computar, processar ou criar objetos, o **Factory** pode ser a melhor opção.

### Service

O **Service** adiciona uma vantagem bem simples em  cima do **Factory**, ele cria instâncias únicas. A **Factory** e o **Service** abaixo tem o mesmo valor.


```
this.launchedCount = 0;
this.launch = function() {
    // Make a request to the remote API and include the apiToken ...
    this.launchedCount++;
}

myApp.factory('rocketLauncherFactory', ["apiToken", function(apiToken) {
    return new RocketLauncher(apiToken);
}]);
myApp.service('rocketLauncherService', ["apiToken", RocketLauncher]);
``` 

Com esse código fica simples de ver que o Service é só uma atalho semântico, você consegue usar um Service em locais que precisa algo mais **_Stateless,_**ou seja, que você não precisa de um estado, só de um executor simples.

### Provider

Se você sobreviveu até aqui merece meu respeito ser lembrado que o Provider é o core de todos esses já citados "recipientes". O que acontece que o Provider tem muitas opções de customizações e isso pode ser muito bom, mas para a maioria dos casos, é uma balão de canhão para matar formiga. Desse modo, nas coisas mais simples, você pode usar um Service, ou um Factory. Mas de repente, surge uma situação que você precisa usar um provider. Vamos dar uma olhada em um código.

```
var useTinfoilShielding = false;

this.useTinfoilShielding = function(value) {
    useTinfoilShielding = !!value;
};

this.$get = ["apiToken", function RocketLauncherFactory(apiToken) {
    // let's assume that the RocketLauncher constructor was also changed to
    // accept and use the useTinfoilShielding argument
    return new RocketLauncher(apiToken, useTinfoilShielding);
}];
``` 

Você pode observar que o provider acima tem um método **$get**, esse método é uma espécie de **Factory** que é chamada quando você quer efetivamente gerar o seu provedor para usar. Tanto que se você cria uma **Factory** sem esse $get o Angular cria esse método para você por baixo dos panos.

_"Mas Angeliski qual é a vantagem de usar um Provider?"_

Você tem razão em perguntar, no cenário acima, podemos usar ele dentro do nosso config:

    RocketLauncherProvider.useTinfoilShielding(true); }]);
    

Se você observar, no config, nós usamos o método useTinfoilShielding. Isso só é possível dentro da etapa de configuração, onde nós podemos customizar nosso provider, porque quando estamos dentro de um controller, ou dentro de um Service, esse método não pode ser chamado. Essa é uma das melhores características do **Provider**, a possibilidade de customização dele.

Se você é daqueles céticos, vou deixar a baixo o código do [angular.js (versão 1.6.0).](https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.6.0/angular.js)

Pois é, **TUDO PROVIDER.** Mas isso não é segredo, tanto que a [documentação](https://docs.angularjs.org/guide/providers) de Provider diz isso. A diferença mais útil no dia a dia é onde você pode injetar algo e onde não, vou deixar uma tabela marota pra você ficar ligado.

| Features / Recipe type             | Factory | Service | Value | Constant | Provider |
| ---------------------------------- | ------- | ------- | ----- | -------- | -------- |
| Pode ter dependências              | Sim     | Sim     | Não   | Não      | Sim      |
| Disponível na fase de configuração | Não     | Não     | Não   | Sim      | Sim      |
| Pode criar funções                 | Sim     | Sim     | Sim   | Sim      | Sim      |
| Pode criar primitivos              | Sim     | Não     | Sim   | Sim      | Sim      |

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski