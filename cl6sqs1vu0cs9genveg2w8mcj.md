# Teorema CAP - O que é isso?

![Imagem com os três pontos do teorema: consistência, disponibilidade e partição tolerante a falhas](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445692507/VgcYllN1J.png)Imagem com os três pontos do teorema: consistência, disponibilidade e partição tolerante a falhas

Bora entender como usar isso no desenvolvimento das nossas aplicações?

Entendendo o teorema
--------------------

Vamos começar entendendo o que é o teorema e o que ele significa. Serei breve aqui porque já existem ótimos materiais falando disso (deixarei o link nas referências, caso você tenha dúvidas):

O Teorema CAP vem do inglês: Consistency (Consistência), Availability (Disponibilidade) e Partition Tolerance (Partição tolerante a falhas). Ele afirma que, em um sistema com distribuição de dados, é impossível obter simultaneamente mais de duas das três garantias. Ou seja, quando estamos falando de sistemas distribuídos, você não conseguirá consistência, disponibilidade e partição tolerante a falhas, terá sempre que escolher entre duas das três. Só que tem uma pegadinha aí: no cenário atual, não existe a possibilidade de criar um sistema distribuído sem partição tolerante a falhas, ou seja, no final do dia você vai ter que escolher entre consistência e disponibilidade.

O Cenário hipotético
--------------------

Para conseguirmos navegar nas nossas teorias, vamos criar um exemplo que seja viável para todo mundo entender. Imagina que nós estamos construindo o Twitter, e como tal, vamos tomar decisões de arquitetura que vão guiar nossas escolhas.

Disclaimer: Estou usando o Twitter como um exemplo didático, isso não significa que o Twitter funcione da maneira como vou expor, nem que o que estou falando é a maneira que deveria funcionar. É apenas um exemplo lúdico.

Consistência
------------

A ideia da consistência é que todos as partições do sistema tenham a informação mais atualizada possível. Imagine que uma conta com 3 milhões de seguidores faça uma alteração no seu nome.

Para simplificar, vamos imaginar que cada continente tenha uma partição rodando, se a nossa estrela do twitter mudar seu nome no continente americano, em um sistema consistente, significa que essa alteração estará refletida “imediatamente” no continente africano.

Aqui vale entender que a velocidade da consistência é relativa à necessidade. Em um sistema como o Twitter, a velocidade que essa informação precisa ficar consistente é alta, ou seja, segundos devem se passar entre uma alteração e sua replicação. Em outros cenários, como relatórios consolidados, essa informação pode levar (até) 1 dia (o famoso D-1) para ficar consistente. Isso vai depender dos seus requisitos de negócio.

Disponibilidade
---------------

O segredo por trás da disponibilidade é garantir que as partições do sistema estejam sempre o mais acessíveis possível. É comum você ver por aí, pessoas falarem em disponibilidade com base na quantidade de 9 que elas conseguem prover:

*   Quatro noves - 99,99% disponível, ou 52,60 minutos de indisponibilidade por ano;
*   Cinco noves - 99,999% disponível, ou 5,26 minutos de indisponibilidade por ano;
*   Seis noves - 99,9999% disponível, ou 31,56 segundos de indisponibilidade por ano.

Isso quer dizer que quanto maior sua disponibilidade, menor é o tempo que seu usuário fica sem acessar essa informação quando necessário.

Vamos voltar ao nosso exemplo do Twitter, quando nós provemos uma alta disponibilidade, garantimos que quando um usuário acessar a uma da tarde, ou uma da manhã, ele conseguirá acessar o site, ver postagens, alterar informações da conta e fazer tudo que o Twitter tem a oferecer. Aqui também vale entender que a disponibilidade necessária varia de acordo com a sua necessidade de negócio: O Twitter ficar fora por 1h é completamente diferente do sistema de tráfego aéreo fora por 1h.

Legal, agora que começamos a entender melhor esses conceitos, bora ver algumas dicas para usar no nosso dia-a-dia?

Entenda a necessidade do negócio
--------------------------------

Eu vou dedicar um tempo para escrever só sobre isso, mas é importante entender que quando você está pensando em soluções de software, você está resolvendo problemas de negócio. Entender isso vai te ajudar (não só aqui) em todas as suas decisões. Quão necessária é a consistência? Qual o impacto de uma indisponibilidade? Quanto seu negócio perde em cada um dos casos? Quanto seu cliente perde nesses cenários? Com essas informações em mãos, a sua decisão vai sem dúvidas ser mais certeira.

Você não precisa necessáriamente escolher entre consistência e disponibilidade
------------------------------------------------------------------------------

Um erro comum de quem está começando nesse assunto (que eu cometi também) é acreditar que, uma vez que você escolhe entre um deles, você precisa eliminar completamente o outro. Não é verdade, é mais como um cabo de guerra. Quanto mais você puxar para um lado, mais difícil será para o outro.

Se você aumentar sua disponibilidade, a consistência começará a ficar prejudicada, pois a falha na rede é inevitável. Então é inevitável que, em algum momento, seu sistema fique inconsistente. Se você aumentar a consistência, sua disponibilidade ficará prejudicada, pois quando a falha de rede acontecer você será obrigado a negar a informação devido à uma inconsistência.

Isso não quer dizer que você não possa obter um resultado interessante combinando estratégias. Um exemplo bem comum é o uso da consistência eventual: o sistema favorece a disponibilidade e é convencionado que eventualmente o sistema chegará ao estado consistente (pode ser uma questão de tempo, ou até de recuperação de uma falha de rede).

Favoreça consistência ao invés de disponibilidade
-------------------------------------------------

Vamos elaborar um pouco sobre isso: é fato, você não conseguirá uma disponibilidade de 100%. Seja devido aos ataques de tubarão ou porque houve algum incidente, a única garantia que você tem é que a rede irá falhar. Quanto mais próximo de 100% você quiser estar, mais caro isso vai ser. Além disso, você como usuário se incomoda mais com um app fora do ar, ou com um app mostrando informações erradas (inconsistentes)? Sim, indisponibilidade é ruim e tem seu impacto, não dá para você ficar metade do dia fora, mas, dado que você lida com os problemas de maneira saudável, vamos dizer que a sua disponibilidade vai estar bem. Mas e a sua consistência? Garantir que seu usuário receberá a informação certa no momento certo, pode ser a diferença entre um negócio de sucesso e um que está sempre disponível, mas nem sempre com informações relevantes para gerar valor aos usuários. Vou reforçar aqui: entenda seu caso de uso e a necessidade de negócio. Isso vai te ajudar a entender no que focar.

Pra fechar vamos fazer uma revisão então: Teorema CAP fala de consistência, disponibilidade e partição tolerante a falhas (que é seu sistema rodando em várias máquinas para simplificar). Falando de sistemas distribuídos e de aplicações escaláveis, a partição a falha é basicamente obrigatória, então você precisará escolher entre consistência (todo mundo vendo a mesma coisa) e disponibilidade (todo mundo acessando). É importante entender o seu neǵocio e a sua necessidade para saber qual opção é mais necessária e como você pode minimizar o impacto dessa decisão.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski

Referências
-----------

* [An Illustrated Proof of the CAP Theorem](https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)
* [CAP Twelve Years Later: How the "Rules" Have Changed](https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/)
* [CAP:  O Teorema que todo desenvolvedor tem que saber](https://www.youtube.com/watch?v=syLXIvnUg0k)
