# JPA e Hibernate - Existe diferença?

Uma das dúvidas mais comuns para quem está iniciando com os frameworks de [ORM](http://www.devmedia.com.br/orm-object-relational-mapper/19056) é:

> _**Eu uso Hibernate ou JPA? Qual é melhor?**_

E normalmente quando você faz essa pergunta a reação costuma ser:

![Mulher fazendo cara de decepção](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445848594/rz4Q2I1EO.gif)Mulher fazendo cara de decepção

Então vamos aprender o que são esses dois caras e porque as pessoas fazem essa cara pra você!

Especificação
-------------

A primeira coisa que temos que entender é o que é uma especificação no mundo Java. Normalmente, existem diversas tecnologias que usamos nas nossas aplicações e cada empresa usa uma ou várias diferentes. Imagine se você tivesse que aprender tudo de novo para cada vez que mudasse de projeto? Ou pior, imagine como seria ter que mudar  todo seu código para usar uma tecnologia diferente que faz a mesma coisa?

Visando isso (e muitas outras facilidades) existe um processo ([JCP](https://www.jcp.org/en/home/index)) que permite que um conjunto de interessados crie um padrão para uma tecnologia. Por exemplo, o [JSR 315](https://www.jcp.org/en/jsr/detail?id=315) é a proposta de padronizar as Servlets (e todo o contexto em volta).

_"Angeliski, mas pra que serve isso?"_

Imagine que você fez a sua aplicação com Servlets, rodando no [Tomcat](http://tomcat.apache.org/), tudo lindo. Dai, um dia, por algum motivo qualquer, você resolve subir ela no JBoss [WildFly](http://wildfly.org/). Imagine como seria tenso se você tivesse que rescrever tudo que você fez em Servlet só pra isso. O motivo de você não ser obrigado a fazer isso é que existe um padrão para as Servelets funcionarem (o tal JSR 315). Ou seja, quando o pessoal do Tomcat ou RedHat ou quem quer que seja for implementar um servidor que interaja com as Servlets, tem uma série de regras que eles tem que seguir.

_"Angeliski, eu já migrei do Tomcat pro WildFly e nem tudo funcionou como era antes!"_

Acontece que a especificação ela pode definir um conjunto de regras, interfaces comuns e comportamentos esperados para serem seguidos, só que ainda assim existem comportamentos que ficam por conta de quem está implementando aquela especificação. A especificação pode dizer que sempre que uma servlet for liberada o método **destroy** será chamado, mas fica por conta de cada implementação saber quando uma servlet será liberada, por exemplo, o Tomcat pode manter uma instancia única de cada Servlet (já que elas não devem ter estado), enquanto o Wildfly pode gerar uma para cada requisição . E isso pode fazer toda diferença na sua aplicação.

Detalhes da implementação que geram custos são conhecidos como [vendor lock-in](https://www.cloudflare.com/pt-br/learning/cloud/what-is-vendor-lock-in/).

Primeiro veio o Hibernate, e os programadores viram que era da hora
-------------------------------------------------------------------

Antes de alguém se interessar por criar um padrão qualquer que seja, é preciso que aquela tecnologia seja aceita. O Hibernate surgiu por volta de 2003 junto do conceito de ORM, trazendo uma nova abordagem para a interação de aplicações com os banco de dados em SQL. Não era mais precisar escrever um **INSERT** ou **UPDATE** na hora de interagir com os dados. Claro que como toda coisa boa, surgiram outros frameworks de ORM como o TopLink e o Eclipselink, mas o que fez sucesso realemente foi o **Hibernate.**

Eu não sei dizer porque ele ficou tão famoso (talvez tenha sido o primeiro?), mas se você souber pode deixar um comentário que eu adiciono aqui!

O ponto é: com o surgimento de diversos frameworks era preciso começar a organizar as coisas antes que tudo ficasse complicado demais. Então alguém teve uma ideia.

Fazendo um puxadinho na JSR 220
-------------------------------

A [JSR 220](https://jcp.org/en/jsr/detail?id=220) é a JCP que fala sobre os **Enterprise JavaBeans 3.0**. Acontece que lá no meio desse amontoado de informações tem também os tópicos sobre a **Java Persistence API** (**JPA**). Isso mesmo, o JPA é definido inicialmente junto com os **EJBs** devido a uma série de detalhes relacionados a transações. Eles começaram a definir quais mecanismos seriam responsabilidade do JPA e quais seriam as interfaces comuns. Em 2009 surge a [JSR 317](https://jcp.org/en/jsr/detail?id=317) tratando da versão do JPA 2.0 trazendo novidades interessantes para a especificação.

_"Legal, mas qual a diferença?"_

Imagine que o JPA é uma interface (muito bem documentada) enquanto o Hibernate é a classe de implementação dessa interface. Ficou mais fácil de entender?

Então só vou usar JPA!
----------------------

![Yoda lhe dizendo que você vai falhar](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445850423/IzYX8TO1G.gif)Yoda lhe dizendo que você vai falhar

O JPA não é a bala de prata. Em algumas situações pode ser vantajoso usar classes, anotações ou funcionalidades que são exclusivas do Hibernate (ou do EclipseLink ou qualquer que seja a sua implementação). O JPA é uma ferramenta para criar um padrão entre as aplicações, tanto para ajudar desenvolvedores quanto para criar aplicações que tem alta flexibilidade.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski