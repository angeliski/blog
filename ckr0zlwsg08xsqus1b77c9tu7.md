# Developer Experience — Chapter One

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116165198/e7bV7_FHE.gif)

Um dos assuntos que eu mais venho estudando nos últimos meses é essa tal de “Developer Experience” (experiência do desenvolvedor) e hoje eu vou começar a compartilhar alguns desses aprendizados.

São muitas coisas e para não ficar um texto gigantesco, eu vou separar ele em algumas partes.

Hoje a gente vai colocar o pé na água e começar o básico sobre esse assunto.

### Muitos nomes para uma mesma coisa

Existem diversas maneiras de se referir a esse assunto:

*   Developer Experience
*   Developer Productivity
*   Developer Effectiveness
*   Developer Entregando Mais
*   Developer Sofrendo Menos

Mas seja qual for o nome que você preferir, elas sempre vão tentar traduzir uma ideia simples: Queremos tornar o processo de desenvolvimento de software mais **eficiente** para as pessoas desenvolvedoras.

Essa ideia passa por várias coisas: produtividade, eficiência, padronização, previsibilidade, boas ferramentas, boa documentação, boa comunicação e tudo aquilo que podemos usar para construir um ambiente mais eficiente.

Sempre que nós estamos produzindo algo, nesse caso software, nós temos um processo (seja ele qual for) que nos ajuda a sair do ponto A e ir até o ponto B.

Vamos pegar o TDD (Test-driven development) como uma referência, é um processo que parte do teste, passa por escrever um código e ter um feedback sobre esse código.

Esse fluxo é o que nós podemos chamar de [Feedback Loop](https://martinfowler.com/articles/developer-effectiveness.html#FeedbackLoops), que basicamente significa quanto tempo leva para você saber se o que você fez tá certo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116167693/W-99ajbH4.gif)

Quanto menor for esse loop, mais provável é de que a pessoa desenvolvedora vá executá-lo.

Sabe aquela suíte de testes que demora 18h pra rodar na sua máquina?

Esse é um feedback loop muito demorado, se você pudesse rodar tudo em 1s, jamais delegaria isso para o seu CI rodar.

Esse loop está presente em todos os processos que você faz, desde o seu teste unitário até aquele PR indo para produção. O tempo que o feedback demora para chegar muda totalmente a sua relação com a tarefa.

### Flywheel Death Star

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116170312/UHdb-9mU2i.gif)

Uma das coisas que contribuem para esse feedback loop ser ruim é a roda da morte da experiência do desenvolvedor:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116172768/7-YN_BhDO.png)

Fonte: [https://martinfowler.com/articles/developer-effectiveness/negative-flywheel.png](https://martinfowler.com/articles/developer-effectiveness/negative-flywheel.png)

A ideia é simples: eu quero chegar hoje na empresa e levar X tempo para colocar meu primeiro código/serviço em produção. **O Sonho**.

Você vai passar pelas três etapas da roda da morte (provavelmente):

### Não sei que não sei

Tem aquele monte de coisa que você não sabe (_porque é uma empresa nova_) e tem coisa que você nem sabe que não sabe.

Qual é o repositório? Como roda o projeto? Usa Github? Gitlab? Gitflow?

Falhou o build. Ah, você esqueceu de fazer X.

Falhou de novo. Ah, nesse caso aí tem de fazer Y, por causa de W.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116174686/dUBJhKT07.gif)

### Informação fragmentada

Se você é como eu, provavelmente deve ter perguntado “Mas tem uma documentação?” e recebeu um link (_com sorte_).

Depois de ter lido a documentação, você começa a tarefa com mais confiança.

O build falha de novo. Não tem nada na tal documentação.

“Fulano, deu esse erro aqui, mas não tem nada na doc”

“Ah sim, essa informação tá aqui (recebe outro link)”

Mais uma leitura. Mais uma falha. Mais um link.

“Ah, verdade, isso não tem documentação. Da um ping no **DevDeDuzentosAnosDaEmpresa** que ele te ajuda”

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116176777/0lIB65xtr.gif)

### Difícil descobrir as coisas

Você venceu as barreiras, gastou uma semana trabalhando naquele serviço, fez todos os testes, fez o CI passar (_finalmente_) e tá pronto pra revisar.

Ai o **DevDeDuzentosAnosDaEmpresa** manda no seu PR:

“Legal isso, mas o serviço _MandaEmailBacana_ já dispara e-mail, você pode só implementar o template lá”

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116179055/7R7WqCXPU.gif)

Aí você volta lá no começo da roda da morte: Qual é o repositório?

### Mas isso importa?

Essa é sempre uma dúvida que bate, então vamos pensar em um exemplo.

Imagina que você precisa desenvolver um serviço novo que lista as postagens do Facebook de um usuário.

Simplificando bastante a ideia, é pegar um usuário, bater na API do Facebook, tratar possíveis erros e devolver a informação.

Aqui é onde você escuta aquela famosa frase: “É só um botão, né?”

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116183485/dQyTs3k9_.gif)

Só que esse seu serviço novo precisa ter uma série de coisas que você não leva em consideração ao pensar apenas na funcionalidade, tais como:

*   Autenticação
*   Autorização
*   Logs
*   Métricas
*   Tracing
*   Circuit Break
*   Alertas
*   Rate Limit
*   Continuous Integration
*   Continuous Delivery (ou só Delivery mesmo)

Isso só é importante se você está preocupado com a qualidade do seu serviço em produção. Fazer software de qualquer jeito **é rápido no curto prazo**, mas cobra seu preço no longo prazo.

E essas são só algumas coisas, em um serviço mais complexo essa lista pode aumentar ainda mais.

O grande problema disso é que uma funcionalidade que poderia sair em 1 semana, leva 3 meses para ser entregue.

E nessa, sai todo mundo perdendo:

*   A pessoa desenvolvedora, que não aguenta mais mexer naquele mesmo caso de uso.
*   A empresa, que não pode atrair mais clientes com a funcionalidade em produção.
*   O cliente, que fica insatisfeito por “uma coisa simples demorar tanto para sair”.

Quando estamos falando da experiência do desenvolvimento, queremos habilitar a pessoa desenvolvedora a entregar valor o mais rápido possível em produção **com qualidade**.

Imagina o seguinte cenário: Você aperta uns 3 botões em uma tela, e recebe um serviço pronto para rodar em produção, só falta implementar a sua funcionalidade.

3 meses viraram 3 minutos e seu time pode entregar valor muito mais rápido e todo mundo sai ganhando (tem uma ferramenta para te ajudar com isso, mas esse é assunto de outro post).

### A estrada para eldorado

Agora que você se convenceu que o investimento vai se pagar, a dúvida que bate é: E agora, o que eu faço?

Eu quero te apresentar uma ideia que o Netflix apresentou como [The Paved Road](https://pt.slideshare.net/diannemarsh/the-paved-road-at-netflix) (a estrada pavimentada), onde a ideia é investir em asfaltar os caminhos mais percorridos.

Imagine uma tarefa que seu time de desenvolvimento faz toda semana, por exemplo, criar um controller, adicionar um registro de DNS, criar um novo serviço em produção.

Seja lá o que for, o caminho para executar essa tarefa tem de ser o mais asfaltado possível. Se leva 10 horas, você precisa investir em fazer levar 5h, 3h, 10min.

Quando você investe em construir essa estrada, você permite que seu time entregue numa velocidade ainda maior.

Mas é importante você não limitar as pessoas a “andar nessa estrada” pois isso pode inibir seu time a inovar. Nós estamos num mundo onde inovação e velocidade precisam andar lado a lado.

(E se eu soubesse a resposta pra fazer isso funcionar o tempo todo, eu tava escrevendo esse texto da minha mansão em Miami)

Eu te expliquei esse conceito porque eu quero deixar clara uma coisa: Você não é o Netflix, não tente resolver seus problemas como eles.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116187352/rUz4mVEce.gif)

Empresas do tamanho do Netflix tem um jeito particular de resolver seus problemas, primeiro porque eles tem uma quantidade enorme de recursos (financeiros e de pessoas para trabalhar no problema), segundo que eles tem o tamanho que torna alguns problemas muito relevantes.

O conceito de estrada pavimentada é maravilhoso, mas ao tentar resolver os problemas do mesmo jeito que eles você vai gastar um recurso que **não tem**, em um problema que não **existe ainda**.

Isso não quer dizer que não vamos fazer nada, vamos separar nossa iniciativa em alguns pilares:

*   Documentação
*   Bibliotecas
*   Serviços

Vamos falar um pouco sobre cada um desses pilares e entender como ele se encaixa no tamanho da sua empresa.

### Documentar ou não documentar, eis a questão

Esse assunto por si só merece um texto à parte, mas eu não podia falar da experiência da pessoa desenvolvedora, sem tocar nesse assunto.

São muitos os exemplos do seu dia que vão ressaltar o quão feliz ou triste pode ser encontrar uma documentação:

*   Quando você pega aquele README completo, com todos os detalhes que você precisa saber,
*   Aquele framework/biblioteca/serviço que você acessa e tem uma documentação completa, explicando em detalhes como fazer as coisas, possíveis problemas e até com uns trechos de código que é só copiar e ser feliz.

Também tem aquelas documentações de API que você acessa e só sente vontade de chorar.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116190038/DQoQiWBVW.gif)

O mesmo acontece na sua empresa, só que você não pode simplesmente fechar o site e desistir da tarefa, já que você está sendo pago por isso.

Dai você gasta 5 dias para descobrir como fazer o maldito projeto rodar na sua máquina, configurando duzentos arquivos e dependências diferentes que você nem sabia que existia, só porque ninguém se deu ao trabalho de escrever dez linhas de documentação que resolveriam o problema em meia hora.

E sabe qual é o pior?

Depois de sofrer 5 dias, você faz o projeto funcionar e não se dá ao trabalho de escrever as tais dez linhas para resolver a vida do próximo colega.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116193515/3lLYRm4bg.gif)

O segredo é esse: deixa mais arrumado do que encontrou.

E incentive as pessoas a fazerem o mesmo.Viu a thread, já manda:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116195881/nB61h3Gv8.png)

A documentação é um dos pilares que vai bem **em qualquer tamanho de empresa**, ela sempre vai ser necessária.

Quando você é pequeno, documentar agiliza o processo de entrada de novos integrantes, quando você é grande ela é essencial para o bom funcionamento da empresa.

O único cuidado aqui é com relação a como descobrir essa documentação, conforme a sua empresa vai crescendo, isso pode começar a ficar complicado. Então invista nisso quando começar a sentir essa dor!

Aqui na RD Station a gente começou utilizando as [wikis do Github](https://docs.github.com/pt/communities/documenting-your-project-with-wikis/about-wikis), mas a pesquisa deixava muito a desejar, então nós migramos para o [Grav](https://getgrav.org/) e tem nos atendido bem (até agora).

### Bibliotecas

Independente de qual a sua stack, ela provavelmente tem algum tipo de gerenciador de dependências que te permite instalar algum tipo de biblioteca.

Um exemplo disso é o npm, um gerenciador de pacotes para a linguagem de programação JavaScript.

Esse poder te permite instalar sim biblioteca de terceiros, mas te permite construir as suas bibliotecas para reutilizar nos seus projetos.

Aqui a vantagem para empresas menores é baixa, pois a chance de múltiplos projetos estarem rodando é baixa.

Mas encapsular em bibliotecas funcionalidades comuns como fluxos de autorização e autenticação pode acelerar muito a adoção de novos projetos.

Além disso, bibliotecas são ótimas fontes de desacoplamento, pois você pode construir abstrações que te tragam vantagens na troca de uma solução.

Internamente na RD nós temos uma biblioteca que abstrai as nossas interações com o [PubSub](https://cloud.google.com/pubsub), então se nós precisarmos adicionar alta disponibilidade com uma alternativa (Kafka por exemplo) o esforço é menor.

Mas cuidado, não vai sair criando biblioteca de tudo também. Nem todo problema é um problema genérico.

### Serviços: Todo mundo quer ter, ninguém quer cuidar

Serviços são coisas complicadas, eles trazem benefícios, mas também trazem responsabilidades adicionais. Você pode entregar uma experiência melhor para quem vai usar, mas com isso acaba adicionando uma carga de complexidade e de operação no time que vai cuidar desse produto.

Imagine que você construiu um serviço de email na sua empresa e chamou ele de **Dispatch**. As pessoas começam a usar ele, e de repente alguém pergunta “Tem uma SDK?”.

Você rapidamente desenvolve uma SDK e entrega ela deixando seus clientes satisfeitos.

O uso do serviço vai crescendo e começa a apresentar lentidão.

Enquanto você realiza alguns ajustes, alguém lança no Slack “Nossa, o e-mail caiu”.

Te bate o desespero. Seu serviço está fora, é um incidente.

Você vai acessando dashboards, vendo gráficos até se dar conta que é o serviço de e-mail do Google que caiu, não o seu.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116199925/l3xwdl6SCo.gif)

Essa história é para destacar o quão complexo o serviço pode ser. Para o time de desenvolvimento ele pode ser maravilhoso (o que todo mundo quer), como a experiência de usar o Heroku é: apenas alguns comandos e você tem a sua aplicação no ar.

Mas esse tipo de abordagem requer mais investimento, tanto inicial (pois se tratam de abstrações mais de alto nível para atender mais clientes), como ao longo do tempo (pois o serviço passa a ser um produto interno que precisa de constante evolução), então muito cuidado ao escolher essa abordagem para acelerar seu time.

Se a sua empresa é pequena, evitar essa abordagem pode ser uma boa ideia.

### Por onde eu começo?

Eu também me fiz essa pergunta seis meses atrás e depois de ficar algum tempo procurando sobre o assunto eu criei o [Awesome Developer Experience](https://awesome-exp.dev/) para reunir recursos sobre o assunto e ser um ponto de partida para quem quer saber mais do assunto.

Além disso, internamente nós atacamos duas frentes: facilitar o nosso processo de adequação ao PRR (Production Readiness Review), que é uma ferramenta para garantir uma entrega de qualidade em produção, e começamos um processo de pesquisa e entrevista com toda engenharia.

Outra coisa que fizemos foi começar a buscar qual seria nossa [métrica norte](https://blog.vindi.com.br/north-star-metric-estrela-guia/) para acompanhar ao longo do ano e medir o sucesso da nossa iniciativa.

Minha sugestão para você que está começando é buscar entender quais são os problemas atuais que o seu time está passando e procurar iniciativas que entreguem valor em curto prazo.

Mais para frente eu vou trazer um artigo falando sobre métricas e como o uso pode te ajudar a visualizar e entender o que atacar.

### Pra encerrar a conversa

Mesmo depois de falar tanto, a gente ainda tem muito o que conversar. O texto de hoje foi só uma introdução sobre as coisas que acho que são um bom ponto de partida.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116206150/vprjj1CWO.gif)

Mas claramente temos ainda que nos aprofundar em coisas como documentação, métricas e sobre ferramentas que podem facilitar tanto seu trabalho quanto acelerar as entregas do seu time.

Se você ficou com alguma dúvida, deixe um comentário ou me manda uma mensagem que eu gosto de conversar sobre o assunto.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116209946/8xB3OUiJG.gif)

Estamos desenvolvendo coisas incríveis para nossos clientes aqui na RD Station. Se você se preocupa com estabilidade, escalabilidade e quer grandes desafios, estamos contratando: [https://bit.ly/3d7JucV](https://bit.ly/3d7JucV)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1626116212087/XMVCy-M6B.gif)

* * *

[Developer Experience — Chapter One](https://medium.com/rd-shipit/developer-experience-chapter-one-11d3f3499899) was originally published in [Ship It!](https://medium.com/rd-shipit) on Medium, where people are continuing the conversation by highlighting and responding to this story.