# Backstage — O que é isso?

Uma das temáticas que tem ganhado cada vez mais força é a tal da experiência do desenvolvedor, ou [Developer Experience](https://medium.com/rd-shipit/developer-experience-chapter-one-11d3f3499899) (para os mais chegados no inglês).

Dentre muitas dos assuntos que podemos falar sobre esse tema, um que tem ganhado cada vez mais destaque é o Internal Developer Portal (IDP), ou portal do desenvolvedor interno.

Se eu fosse resumir do que se trata um IDP, eu diria que é uma ferramenta interna usada para abstrair complexidades de operação (seja de infraestrutura, tooling, ou padronização) permitindo que o desenvolvimento seja mais focado no negócio em si.

Não se preocupe se isso não ficou tão claro, ao longo do artigo nós vamos explorar melhor essa definição.

%[https://twitter.com/kelseyhightower/status/851935087532945409]

I'm convinced the majority of people managing infrastructure just want a PaaS. The only requirement: it has to be built by them.

Mas porque diabos eu to falando dessa IDP num texto sobre o Backstage?

### O que é o Backstage do Spotify?

Eu vou deixar esse vídeo oficial porque ele é bonito demais e merece ser visto (infelizmente ele é apenas em inglês):

<iframe src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2F85TQEpNCaU0%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D85TQEpNCaU0&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2F85TQEpNCaU0%2Fhqdefault.jpg&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" frameborder="0" scrolling="no"><a href="https://medium.com/media/04a66eee94047e4b5a652dd45d61fd33/href">https://medium.com/media/04a66eee94047e4b5a652dd45d61fd33/href</a></iframe>

Se você acessar o [site oficial](https://backstage.io/), vai encontrar logo de cara: (tradução livre) “Uma plataforma aberta para a construção de portais de desenvolvedores”.

Agora as coisas começaram a se conectar, certo? Nós podemos usar o Backstage para construir nosso IDP!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280395963/-0wSYe00G.gif)

Homem falando “Wow”

Mas para a gente começar a fazer isso, precisamos primeiro entender suas principais funcionalidades (e quais problemas elas resolvem)

### Quem é o dono desse serviço?

Uma das primeiras coisas que começam a acontecer em uma empresa conforme ela vai crescendo é o surgimento da tal “planilha de donos”, que diz qual time é responsável por qual serviço, quem faz parte do time, quem você pode procurar para tirar dúvidas.

No começo é muito legal, mas com o tempo a manutenção dela começa a ficar inviável. Novos serviços não aparecem por lá, antigos donos não atualizam informações, pessoas responsáveis deixam a empresa e o nome continua lá.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280399070/O0bf9RaKw.gif)

Homem gritando com raiva

Uma das coisas que o Backstage ataca é esse problema, com uma funcionalidade chamada [Software Catalog](https://backstage.io/docs/features/software-catalog/software-catalog-overview), o famoso Catálogo de Serviços.

O catálogo é o coração das funcionalidades que o Backstage entrega, ele reúne diversas informações e relaciona isso de maneira a entregar para o usuário uma maneira fácil e simples de achar **quem é o dono.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280401642/D3rbyMNVU.png)

Visão do Catalogo de Serviços em [https://demo.backstage.io/](https://demo.backstage.io/)

O catálogo pode ser **alimentado (**o que a gente chama de ingestão**)** com dados de diversas fontes: Github, Gitlab, Okta, Azure, LDAP e por aí vai. Existem muitas [integrações](https://backstage.io/docs/integrations/) prontas já, mas mesmo quando você não encontrar o que precisa, adicionar um novo mecanismo de ingestão é bem simples (no futuro eu posso fazer um texto fazendo algo do tipo).

### Como eu crio um serviço novo?

Você faz um estudo de solução e chega a conclusão: Precisamos de um novo serviço em produção. Mas por onde começar? E mesmo que você tenha essa resposta, como garantir segurança, observabilidade, que ele segue os padrões da área de engenharia,e que ao final disso tudo ainda vai ser adicionado no Backstage com as informações necessárias para identificar os donos dele?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280404139/weXvi1Ywh.gif)

Sherek falando “oh boy”

Uma das funcionalidades que o Backstage tem é chamada de [Software Templates](https://backstage.io/docs/features/software-templates/software-templates-index).

A ideia é simples: você tem um template, define parâmetros de entrada, os passos e o Backstage cria o novo serviço para você.

Spotify tem um conceito chamado “[Golden Path](https://engineering.atspotify.com/2020/08/how-we-use-golden-paths-to-solve-fragmentation-in-our-software-ecosystem/)” que tira muito valor disso: o melhor caminho para uma determinada tarefa.

Pensa aí na sua empresa: existe uma linguagem dominante (Java, Ruby, JavaScript, Go ou qualquer outra), existem algumas ferramentas padrões (docker, maven, yarn), bibliotecas internas e externas (log, trace, autenticação) e uma série de boas práticas que precisam ser feitas toda as vezes, normalmente conhecido como Boilerplate.

Agora imagina um mundo onde você só diz o nome do serviço e ganha tudo pronto, rodando até em produção em poucos minutos. Esse é o objetivo aqui, o software template te permite criar templates de acordo com a necessidade da sua empresa.

### Onde está a documentação desse serviço?

Você criou seu serviço, escreveu uma documentação explicando a arquiteta, como rodar, colocou informações relevantes da Stack, uma seção de problemas, tudo maravilhoso.

Mas as pessoas ficam o tempo todo te mandando mensagem perguntando as mesmas coisas que estão na doc.

Depois de perguntar se elas olharam na documentação, você recebe a seguinte indagação: “Mas que documentação? Onde está isso?”

Você mostrou o lugar e todos felizes. Meses depois você volta na doc para rodar seu serviço localmente, mas não funciona.

A maneira como o serviço funcionava mudou, mas ninguém atualizou a documentação.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280406140/wNcrhXs0f.gif)

Image de umc cachorro em em um incendio falando “ta tudo ótimo”

Você sabe que agora é a hora que eu falo do Backstage, a funcionalidade que ele tem se chama [TechDocs](https://backstage.io/docs/features/techdocs/techdocs-overview).

A ideia é simples: mantenha a documentação relevante para o serviço perto dele.

Através de arquivos Markdown, você mantém os documentos no mesmo repositório e garante que isso vá ser atualizado junto com alterações que impactam a documentação. Além disso, ele permite que na página do serviço seja exibido um website renderizado a partir desses arquivos tornando ainda mais fácil as pessoas encontrarem essa informação junto ao serviço.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280408658/vMrsJKsyZ.png)

Visão da da aba de documentação em [https://demo.backstage.io/](https://demo.backstage.io/)

Vou inclusive deixar um vídeo da Kubecon onde o time do Spotify fala mais sobre essa ideia, ele é bem curtinho e vale a pena:

<iframe src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FaIURaocR5D8%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DaIURaocR5D8&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FaIURaocR5D8%2Fhqdefault.jpg&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" frameborder="0" scrolling="no"><a href="https://medium.com/media/d4dd9f5c94ef9c2a7e4c92afcd6cdfd3/href">https://medium.com/media/d4dd9f5c94ef9c2a7e4c92afcd6cdfd3/href</a></iframe>

### Eu tenho que **saber** onde tá tudo isso?

Você colocou o Backstage no ar, mas tem um problema: como as pessoas encontram as coisas?

Como achar o serviço, a documentação, o template, ou até mesmo aquela informação que não está no Backstage mas em um sistema diferente.

Encontrabilidade é um fator chave para o sucesso de uma iniciativa de produtividade. Não adianta sua ferramenta ser a melhor, se ninguém achar nada nela.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280410629/VoSVqdHUF.gif)

Barbosa concordando com a afirmação

O Backstage resolve isso com o [Search](https://backstage.io/docs/features/search/search-overview). Imagine você ter o Google para procurar coisas na sua empresa. Essa é ideia aqui, você tem recursos para indexar diferentes fontes de informação e permitir uma busca centralizada no Backstage.

Além disso, ele fornece indexadores para o seu catálogo, então fica fácil achar a informação que já está ali dentro.

Até aqui você percebe que tem muita coisa legal no Backstage. Mas e se eu te disser que tem mais?

<iframe src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fgiphy.com%2Fembed%2Fl0Exdm9UbTHAFcJi0%2Ftwitter%2Fiframe&amp;display_name=Giphy&amp;url=https%3A%2F%2Fmedia.giphy.com%2Fmedia%2Fl0Exdm9UbTHAFcJi0%2Fgiphy.gif&amp;image=https%3A%2F%2Fi.giphy.com%2Fmedia%2Fl0Exdm9UbTHAFcJi0%2Fgiphy.gif&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=giphy" width="435" height="241" frameborder="0" scrolling="no"><a href="https://medium.com/media/c4b50e2f38a4231273ecb55ba5a3b95a/href">https://medium.com/media/c4b50e2f38a4231273ecb55ba5a3b95a/href</a></iframe>

### Conheça o sistema de plugins

Uma das coisas que torna o Backstage uma ferramenta tão poderosa é seu sistema de plugins.

Com uma interface clara e bem definida, ele permite que você adicione funcionalidades apenas instalando novos plugins.

Quer exibir os PRs do Github? Tem [plugin](https://github.com/backstage/backstage/tree/master/plugins/github-pull-requests-board)

Os builds do CircleCI? Tem [plugin](https://github.com/backstage/backstage/tree/master/plugins/circleci)

Bitbucket? Todo list? Verificações de qualidade?

[Tem plugin](https://backstage.io/plugins) para isso e muito mais.

E se você não achar o plugin, é bem simples desenvolver um! Com um pouco de conhecimento de [express](https://expressjs.com/) ou [react](https://reactjs.org/) você consegue criar novas funcionalidades (nos plugins para o front-end você usa react e nos plugins para backend você usa express).

Além disso, a [comunidade](https://backstage.io/community) é extremamente receptiva. Tem uma [documentação](https://backstage.io/docs/overview/what-is-backstage) legal (e melhorando), [Github](https://github.com/backstage/backstage) muito ativo e até uma [central de aprendizado](https://backstage.spotify.com/learn/)

### O Backstage vale a pena?

Essa resposta depende da maturidade da sua empresa. A [developer experience](https://medium.com/rd-shipit/developer-experience-chapter-one-11d3f3499899) tem cada vez mais se tornado essencial para que o valor do negócio possa emergir do time de desenvolvimento **mais rápido, com maior qualidade e segurança**.

O Backstage é uma ferramenta que, sem dúvidas, pode te ajudar nisso. Se sua empresa ainda não tem um time dedicado para isso, recomendo dar uma olhada no [Roadie](https://roadie.io/), uma solução Saas do Backstage que pode te ajudar a reduzir custos de manutenção (e acelerar seu início, pois em apenas alguns minutos você consegue tudo rodando sem maiores dificuldades).

Aqui na RD Station o Backstage já é uma realidade para nosso time de engenharia. Nosso catálogo está 100% mapeado, e nosso time faz uso dos templates tanto para criar novos serviços [Rails](https://github.com/backstage/backstage/tree/master/plugins/scaffolder-backend-module-rails), quanto [Micro Frontends](https://martinfowler.com/articles/micro-frontends.html).

Claro, ainda temos um longo caminho a evoluir (como time e com a ferramenta), além de dificuldades ainda não solucionadas (não é trivial garantir que o catálogo não fique desatualizado com a realidade da empresa por exemplo).

Mesmo assim, nós já vimos a engenharia sair de quase dois meses para criar um novo serviço para menos de uma semana. E só vamos parar quando levar menos de cinco minutos.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280413074/VljBCznYA.gif)

Homem falando “volte em 5 minutos”

Existem [outras ferramentas](https://platformengineering.org/platform-tooling) no mercado de IDP, e o seu cenário pode te fazer optar por diferentes escolhas, mas em termos de flexibilidade nenhuma delas vai te oferecer o que o Backstage oferece. Em contrapartida, você pode se ver obrigado a construir coisas internamente para se adequar ao seu caso de uso.

Como tudo em engenharia, são [escolhas](https://medium.com/swlh/decision-management-in-software-engineering-ca60f9d40e02).

Me conta, o que você quer saber mais do Backstage?

Referencias:

*   [How we are improving developer experience at QuintoAndar with backstage.io | by Gabriel Dantas](https://medium.com/quintoandar-tech-blog/how-we-are-improving-developer-experience-at-quintoandar-with-backstage-io-fa1ab70b75cb)
*   [Tech Talks — Como desenvolvemos o developer portal usando o Backstage.io](https://www.youtube.com/watch?v=Y57gUwb1v3g)
*   [I spent two years trying to do what Backstage does for free — Stack Overflow Blog](https://stackoverflow.blog/2022/09/19/i-spent-two-years-trying-to-do-what-backstage-does-for-free/)
*   [BackstageCon Community Summary](https://docs.google.com/document/d/1r1kVPvRWlx4U12ljW0pcLF5s0NNuOnUgkkottmOSl0o/edit)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280414983/6jqGALK1A.gif)
