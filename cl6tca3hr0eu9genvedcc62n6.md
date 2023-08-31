---
title: "Tratamento de erro - Além do raise/rescue"
datePublished: Fri Aug 07 2020 00:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cl6tca3hr0eu9genvedcc62n6
slug: tratamento-de-erro-alem-do-raiserescue
canonical: https://angeliski.com.br/tratamento-de-erro/
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/bmJAXAz6ads/upload/v1660482166647/Qc7wWmQe0.jpeg
tags: error-handling, exceptionhandling

---

Uma coisa comum a toda linguagem de programação é o tratamento de exeções (ou tratamento de erros se preferir). Algumas linguagens não tem exeções em tempo de execução ([Elm por exemplo](https://guide.elm-lang.org/error_handling/)), mas aqui vamos focar no cenário mais comum, linguagens que te dão a possibilidade de a qualquer momento gritar: "Hey! Temos um problema!"

Exemplos disso são Ruby, Java, PHP entre outras, onde elas permitem que você indique um problema durante a execução do seu código. A ideia aqui é focar no conceito de como lidar com as exceções, quando e onde lançar elas e não focar em detalhes da linguagem, então ao invés de fazer uso da sintáxe que já existe, vamos combinar a nossa linguagem aqui:

Quando houver um problema:

Grita SaldoInsuficienteParaRealizarOperacao("Você está sem dinheiro")

Quando quisermos tratar algum problema:

Combinado o jogo, bora entender alguns conceitos.

## O que é uma exceção?

Se você for olhar no [dicionário](https://www.dicio.com.br/excecao/), umas das definições é:

> Desvio de um padrão estabelecido; rompimento do que se considera normal: não há regra sem exceção.

Ou seja, quando a gente desenvolve uma aplicação (ou uma feature) nós temos um fluxo que é considerado normal, o famoso [caminho feliz](https://www.ateomomento.com.br/caso-de-uso-fluxo-principal/) (chamado assim dada a felicidade do usuário final quando ele consegue fazer o que ele veio fazer). Vamos pegar um exemplo:

Você entra no app do seu banco pra pagar uma conta de luz. O caminho feliz é você entrar, acessar sua conta, escanear seu código de barras e pagar a conta (simplifiquei muito aqui, mas ta valendo). O que acontece se você entrar no app, digitar a senha e aparecer **"Sistema indisponível"**? Além de você não ficar **feliz**, o caminho **normal** foi rompido: uma exceção ocorreu.

![Homem tirando o óculos escuro em choque depois de entender a minha explicação](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445709067/m7nqSXJNj.gif align="left")

Homem tirando o óculos escuro em choque depois de entender a minha explicação

Nós podemos até ir um pouco alem e delimitar uma separação clara entre erro e exceção. Quando estamos falando de erro, estamos falando de um problema que é "irrecuperável" (por parte do usuário pelo menos), por exemplo: Sua aplicação não consegue conectar no banco de dados. Cara, não tem como seu usuário fazer alguma coisa, ele no maximo pode abrir um ticket e reclamar. Quando falamos de exeção estamos falando de um fluxo alternativo, onde algo pode ser feito. No exemplo do app, se você tentar pagar uma conta de luz sem saldo, uma exeção vai acontecer e você pode transferir dinheiro pra sua conta, ou até esperar seu pagamento cair. Agora que a gente entendeu isso, bora entender onde e quando lançar isso?

## Quando eu devo lançar minha exceção?

É muito comum durante nosso desenvolvimento nos depararmos com casos onde pensamos que aquele trecho de codigo pode dar problema. Tenha em mente que o melhor lugar para lançar um problema (ou gritar no nosso caso) é próximo de onde ele ocorre. Imagine o seguinte código (baseado na nossa linguagem):

Se UsuarioTemSaldoParaPagarAConta(usuario, conta) PagaAConta(usuario,conta) Senao Grita SaldoInsuficienteParaRealizarOperacao("Operação não realizada devido ao saldo não ser suficiente na conta", usuario, conta) FimDoSe

Fica fácil identificar que houve uma execeção no nosso fluxo, porque nossa regra de negocio diz que que o usuário tem que ter saldo pra pagar a conta (aqui já fica a dica, seus fluxos de exceção provavelmente vão vir das regras de negocio). No momento que nós fazemos essa verificação é onde temos mais contexto para informar o problema.

Pensa ai, se você tentar pagar a conta hoje e algo anormal (uma exceção) ocorrer e você decidir anotar só daqui 1 semana, a chance de você esquecer algum detalhe é maior. Isso porque você está mais distante do momento em que o problema ocorreu. O mesmo serve pro código.

Outra coisa importante quando você gerar sua exceção é fornecer o máximo de informaçoes sobre o problema. No nosso exemplo, qual era o saldo? Qual a conta? Qual a data da operação? Qual usuário? Essas informações são uteis para que quando alguém decida lidar com esse problema, ele tenha informação suficiente para agir. Imagine você ligando no suporte do banco, falando que não conseguiu pagar a conta. Eles vão perguntar sobre qual tipo de conta, o número da sua conta corrente, a data da operação e diversas coisas. Tirando os dados sensiveis (não precisa adicionar a senha do usuário aqui né?), as informações são importantes. Aqui vale também deixar uma mensagem clara junto da sua exceção. Veja bem, não estou falando de mensagem para o usuário (vamos falar disso mais pra frente), estou falando de uma mensagem clara do motivo que aquela exceção aconteceu.

## Quando eu devo tratar uma exceção?

Legal, você lançou todos os erros que podia, deixou claro na mensagem, incluiu as informações necessárias. E agora? Onde tratar isso?

É comum nossas aplicações serem separadas em camadas (algumas em [MVC](https://tableless.com.br/mvc-afinal-e-o-que/), outras em [arquitetura hexagonal](https://netflixtechblog.com/ready-for-changes-with-hexagonal-architecture-b315ec967749?source=search_post---------0)) então o que acontece normalmente é lançarmos o erro em uma camada de business e tratar esse erro em uma camada mais próxima da visualização. O problema dessa abordagem é que quando chegamos ao tratamento é comum faltar contexto para tomarmos uma decisão, ou até contexto para informar o nosso usuário. Isso também pode gerar um acoplamento indevido, já que nossas camadas teriam que conhecer "indevidamente" exceções que estão "distantes" dela. Uma abordagem mais interessante é você enriquecer a sua exceção conforme ela for precisando de mais contexto. Você pode lidar com um erro e oferecer mais informações (quando necessário) sem se esquecer de manter a exceção anterior de modo que a [Stacktrace](https://stackoverflow.com/questions/3988788/what-is-a-stack-trace-and-how-can-i-use-it-to-debug-my-application-errors) fique integra (assim você pode saber de onde o erro original veio, mesmo enriquecendo ele durante o caminho).

Vamos ao exemplo: Imagine que você tem um sistema de calculo de frete. Ele é bem simples, você digita um CEP e um produto da Amazon e ele calcula o frete pra você. Imaginemos o seguinte algoritmo (simplificado):

* Recupera os dados do CEP/PRODUTO
    
    * Pergunta pro sistema dos correios o endereço com base no CEP
        
    * Pergunta pra Amazon as dimensões do produto
        
* Calcula o valor do frete (com uma regra que eu não conheço)
    

Show, acontece que tem um cenário de exceção: Pode acontecer de você perguntar o endereço pros correios e ele devolver dois endereços (não sei se isso é real, mas vamo fingir aqui que é). Nesse caso, nosso processo de recuperar o cep vai lançar uma exceção informando essa duplicidade.

Nesse caso, nós poderiamos tratar esse problema em dois pontos. Primeiro onde nós fazemos a chamada para pedir o CEP, onde nós iriamos adicionar o contexto de que não é possível calcular o frete, por conta do problema do cep. Aqui nós poderiamos gerar uma nova exceção mais especifica, que será tratada num local mais próxima da visualização do usuario. Essa segunda exceção nos permite tratar sem ficar acoplado ao erro do CEP ( o usuário não se importa com o CEP, só com o fato de que não é possível calcular o frete dele).

Vou escrever um pseudocodigo aqui pra tentar deixar isso um pouco mais lúdico:

ps: Vamos desconsiderar aqui boas práticas de parametros e outras coisas (paradigmas e afins), vamos focar no nosso fluxo.

```plaintext
// Essa funcao é nosso ponto de entrada
Funcao CalculaFrete(usuario,cep, produto)
	Tenta
		informacoes = RecuperaInformacoesParaCalculearOFrete(cep, produto)
		//Segue o fluxo de negocio
	Deu Ruim ImpossivelDefinirEnderecoParaCalculo erroCapturado
		// Aqui a gente vai enriquecer o erro com o usuário 
		// e mudar o tipo dele para "desacoplar" ele dos erros de mais baixo nivel
		Grita ImpossivelCalcularFrete("Impossível calcular o frete com esses dados", usuario, cep, produto, erroCapturado)
	FimDaTentativa
FimDaFuncao

Funcao RecuperaInformacoesParaCalculearOFrete(cep, produto)
	enderecos = RecuperaDadosDoCEP(cep)
	Se enderecos.quantidade == 1 entao
		// Segue o fluxo de negocio
	Senao
		Grita ImpossivelDefinirEnderecoParaCalculo("Não foi possível definir corretamente para qual endereço calcular o frete", cep, enderecos)
	FimDoSe
FimDaFuncao

Funcao RecuperaDadosDoCEP(cep)
	enderecos = CorreiosWebService.recuperaDados(cep)
	retorna enderecos
FimDaFuncao
```

Em uma das etapas do nosso processo, nós tivemos uma exceção de multiplos endereços e lançamos ela assim que possível. Dando sequência na nossa stack, nós capturamos essa exceção, enriquecemos ela com mais detalhes (usuário nesse exemplo) e geramos outra execeção, primeiro para que o acoplamento seja menor entre partes que não deveriam se conhecer e depois para deixar claro esse enriquecimento de informações. Importante notar, que a exceção capturada foi propagada na nova exceção(muitas linguagens permitem isso de modo a trazer rastreabilidade para a stack do problema).

## Mas ai, eu já retorno a mensagem da exceção pro usuário?

Eu pessoalmente sou contra isso. Por mais que você crie suas exceções com mensagens significativas, nós engenheiros temos uma tendência a ter um foco mais técnico na maneira como nós expressamos. E isso pode atrapalhar o usuário final. Além disso, é comum a evolução do codebase, então a mensagem bonita que você colocou 1 mês atrás, pode sofrer um refactor e passar a ser outra sem você se dar conta (principalmente se você não estiver trabalhando em um projeto pequeno). Mas isso não é uma regra, pode ser combinado e depender do seu projeto, então alinhar com seu time a melhor maneira para fazer isso é valioso. Além disso, algumas linguagens e frameworks tem mecanismos que ajudam e facilitam nesse sentido (como o [exception handler do Spring](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)).

## TL;DR;

Ao lançar exceções:

* Foque em criar ela o mais próximo possível do problema. É lá que você tem mais informações para agregar e facilitar a sua vida depois a vida do colega.
    
* Adicione mensagens claras junto, para que seja possível entender o problema de maneira eficaz sem precisar ficar olhando o código.
    

Para tratar as exceções:

* Lide com as exceções em momentos que você acredita que pode agregar mais contexto. As vezes informações de processo são relevantes, ou até mesmo pode haver informações adicionais que não existiam no contexto que o problema original foi gerado.
    
* Lidar com exceções de baixo nivel permite você desacoplar partes do seu sistema que não deveriam se conhecer
    
* Lide com as exceções antes que elas cheguem ao usuário final, formatando ou modificando as mensagens de modo que o problema tenha um acionável claro. em alguns casos o framework/linguagem pode já ter mecanismos para te ajudar.
    

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski

## Referências

* Error Handling · An Introduction to Elm
    
* Rescuing exceptions in Ruby
    
* How to Throw Exceptions in Java
    
* Caso de Uso - Fluxo Principal - Até o Momento
    
* Exceptions: Why throw early? Why catch late?