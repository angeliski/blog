# Caching - Serve pra que?

O que é caching?
----------------

O cache é um componente de software/hardware que permite que uma informação que você já acessou seja recuperada de forma mais eficiente. Ele é usado em alguns casos para acelerar a velocidade que um dado é recuperado em outros para aliviar a carga sobre determinados componentes. Já vamos falar mais disso.

Vamos pensar num exemplo que não é de programação: **Supermercado**.

Você precisa fazer o almoço, não tem nada em casa, então você vai até o supermercado, compra algumas coisas pra cozinhar, volta pra casa e faz o almoço. Beleza;

Dai você precisa fazer a janta. Você vai ao supermercado de novo? Provavelmente não, porque você deve ter feito **_cache_** das compras na sua geladeira.

Como assim?

Quando você foi no supermercado, você guardou ingredientes na sua geladeira para **usar depois, de maneira mais rápida**.

Supondo que no almoço você tenha gastado 2h indo até o supermercado, quando você precisou fazer a janta, deve ter gastado no máximo 5min indo até a geladeira para pegar os ingredientes. Percebe como o **_cache_** permitiu que o acesso aos seus igredientes seja muito mais rápido? Imagine como seria complicado se você tivesse que ir até o supermercado toda vez que precisa-se fazer comida?

Esse mesmo conceito se aplica para quando você faz cache de páginas web (HTTP caching), cache de requisições no banco de dados(database caching), cache de memória(memory caching) e muitos outros lugares.

Legal, agora que a gente entendeu o que é caching, vamos entender algumas coisas importantes sobre ele:

Cache não é storage
-------------------

A primeira coisa pra deixar claro aqui é que cache não é o lugar onde você vai guardar as suas informações. O que isso significa?

Que em um cenário que você _“perde”_ a informação que está no cache, isso não deveria ser um problema.

No nosso exemplo, imagina que você comprou uma fruta e ela estragou na geladeira. Isso não é um problema, você só precisa ir até o supermercado e comprar outra. Obvio, isso tem um custo (financeiro, tempo, trabalho), mas não é impossível de fazer.

Agora imagina que a sua mãe fez um bolo de aniversário pra ti. Você colocou na geladeira e estragou. Você pode até fazer outro bolo e tal, **mas aquele bolo de aniversário nunca mais**. Pra gente falar de um exemplo de tecnologia, imagina que você ta trabalhando no facebook e guardou a foto do usuário em um cache. Se deletarem o cache, você não deveria perder a imagem, ela tem que estar salva em algum outro lugar. Pode ser que demore uns 2min para mostrar ela novamente, já que **não está no cache**, mas não é uma informação perdida.

É comum o cache seja em memória, ou em algum outro tipo de localidade volátil, ou seja, a informação que está lá pode ser removida ou perdida muito facilmente.

Qual o tempo que sua informação leva para ficar desatualizada no cache?
-----------------------------------------------------------------------

Uma das coisas importantes pra se considerar quando a gente fala de caching é quanto tempo aquela informação leva para ficar desatualizada. No nosso exemplo da geladeira, você pode pensar na comida estragada. Quanto tempo leva para o leite estragar na geladeira?

A informação que você armazena no cache tem uma validade, também conhecido como **time to live** ou **TTL**. No nosso exemplo do Facebook, a foto do usuário que você guardou no cache, pode ficar lá até que o usuário decida que ele quer uma foto nova. Agora se você estiver falando do número de curtidas de uma postagem, esse cache pode ter que ser menor, vamos supor que de 1 min.

Aqui é importante perceber que cada caso precisa ser analisado, algumas informações mudam menos e podem ter um tempo de vida maior, outras mudam muito e precisam ter um tempo de vida menor. Outras mudam tanto, ou são tão criticas que não podem ou nem devem ser cacheadas.

O tamanho do cache é importante - Ele não pode ser grande nem pequeno demais
----------------------------------------------------------------------------

Antes de começar a falar do tamanho do cache eu quero explicar dois conceitos pra vocês. Quando a gente solicita uma informação pro nosso cache e ela está lá, acontece o que a gente chama de **cache hit.** Se você solicitar essa info e ela não estiver lá, acontece o que a gente chama de **cache miss.**

Isso é importante, porque você quer maximizar o seu cache hit sem aumentar o seu cache miss. O tamanho do seu cache influencia aqui, se ele for muito grande, a chance do dado estar lá é maior, mas **isso custa mais caro**.

Se ele for pequeno é mais barato, mas isso **aumenta a chance de você não encontrar a informação lá**. E ir até o cache e não encontrar a informação é ruim porque **isso é custoso pro seu sistema**. Você perdeu tempo indo lá. Mas não ir até lá, pode ser mais custoso, porque buscar a informação na origem é mais demorado. Delicado esse equilibrio né?

O tamanho do seu cache vai depender também do volume de dados que você está lidando, mas além disso você precisa escolher bem qual estratégias de substituição de cache você vai usar. Ou seja, quando seu cache estiver cheio, _como você vai decidir o que precisa ser removido?_

A estratégia mais comum é a **Least recently used (LRU)**, que diz que o dado mais “antigo” no cache vai ser o removido pra dar espaço pro dado novo. Vamos supor que você ta ai scrolando a sua timeline do facebook, e o cache deles usa LRU e só cabem 10 posts, quando você chegar no 11 post, ele vai remover o primeiro que você viu, porque faz mais tempo que ele foi acessado e é menos provavel que você vai acessar ele.

Outra estratégia é a **Most recently used (MRU)**, que consiste em remover o dado mais “novo” do cache, ao invés do mais antigo, o contrário da LRU.

Essa é uma boa opção quando o dado que foi recem requisitado provavelmente não vai ser usado logo. Imagina o Netflix, quando você termina de assistir um filme, é menos provavel que você vá ver ele novamente, ele é mais recente no cache, então remover ele pode ser uma boa, ao invés de remover aquele monte de imagens dos outros filmes que estão na tela inicial, que ficaram sem uso quase 2h enquanto você assistia o filme.

Show, você entendeu como funciona o cache e suas estratégias de substituição, bora falar de estratégias de uso.

Estratégias de cache
--------------------

As duas estratégias mais básicas são: verificar primeiro no cache e depois na fonte ou verificar na fonte e depois no cache.

Algumas informações podem ter um pequeno atraso na atualização, por exemplo, você alterar o seu emprego no Linkedin. Não é um problema se alguém ficar 5 minutos vendo seu cargo antigo, até porque você não muda ele toda hora, isso não é critico.

Nesse caso faz sentido você sempre verificar primeiro no cache para oferecer uma informação mais rápida, mesmo que possa correr o risco de estar desatualizada.

Outras informações são atualizadas com uma frequência maior, então servir essa informação diretamente do cache pode não ser a melhor ideia.

Imagine um post do facebook, as curtidas que ele vai recebendo deveriam aparecer pro usuário o mais atualizada possível e isso só vai acontecer consultando a fonte original.

Ai você se pergunta, onde o cache entra nisso? Acontece que a fonte original pode estar indisponível por algum motivo, o banco de dados caiu, o usuário ficou sem internet, tem algum bug em produção, sei lá. Nesse caso, servir uma informação desatualizada é melhor que não servir nada.

Pra fechar, vamos falar de prós e contras de usar cache.

Vantagens e desvantagens de usar cache
--------------------------------------

Show, você aprendeu várias paradas sobre cache então é só sair usando em todo lado né?

![Moça negra com pano amarrado na cabeça falando pare em inglês](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445699625/g4eN_SUzj.gif)Moça negra com pano amarrado na cabeça falando pare em inglês

**Calma. Não é assim.**

O primeiro contra do cache que eu quero levantar é **o aumento de complexidade**. Usar cache deixa o seu sistema mais complexo.

Se você não tem um problema no tempo de acesso ao seu recurso, provavelmente não precisa de cache. Existem outros motivos pra usar cache, como uso intensivo de um recurso (uma API, um banco de dados), um calculo complexo no servidor, enfim.

O que eu quero dizer é: **a complexidade de usar o cache tem que ter um bom motivo.**

Outra coisa importante é que o **cache custa dinheiro**.

Normalmente componentes de cache são otimizados pra performance e isso custa naturalmente mais dinheiro, então você precisa entender se colocar cache no seu projeto gera mais dinheiro do que você precisa gastar pra ter ele. **Não adianta você gastar R$100 pra receber de um cliente R$ 90.**

Dado que você realmente entendeu que você precisa usar cache no seu projeto, ele pode te entregar coisa bacanas como **latência reduzida para a suas operações**, uma **carga menor em componentes** como banco de dados e processos computacionais mais pesados e até uma **camada de resiliência**, já que você pode em alguns casos fazer uso do cache para contornar problemas em componentes com falha.

Galera, espero que vocês tenham aprendido bastante coisa sobre cache hoje, vou deixar umas referências bacanas aqui na descrição também se vocês quiserem dar uma olhada.

Qualquer dúvida, manda um comentário ai pra gente trocar ideia. Falou!

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski

Referências
-----------