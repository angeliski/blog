# Deploy no Now com Travis CI

Recentemente eu precisei fazer algo que eu achei que era simples. Configurar um projeto no Github, para fazer o build no [Travis](https://travis-ci.org/) e o Deploy no [Now](https://zeit.co/now). Depois de três dias procurando informações de como fazer isso:

![Pato Donald surtando](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445778884/RDxzsLiTI.gif)Pato Donald surtando

Se você já teve que fazer isso, ou está tentando fazer, entende bem do que eu estou falando. Borá lá ver como fazer isso?

O Projeto
---------

Antes de começar vamos falar sobre o projeto que eu vou usar de exemplo. É o projeto de uma pequena API para consume de eventos de frontend, usando node, express e mocha/chai para os testes. Você pode dar uma olhada no [repositório](https://github.com/angeliski/frontendbr-eventos-api) se quiser.

A parte fácil
-------------

Claro, tem uma parte que foi fácil. Configurar o Travis CI para fazer o build, foi relativamente simples. É só criar um arquivo **.travis.yml** na raiz com o seguinte conteudo:

    language: node_js
    node_js:
    - "6.11.0"
    script: mocha --timeout 10000 --reporter nyan
    before_script:
    - npm install
    after_success:
    - npm run coveralls

Além disso, basta ativar o seu projeto no Travis. Isso já te garante 50% do caminho.

Onde começa a treta
-------------------

Dai, você decide publicar seu projeto no [Now](https://zeit.co/now). É fácil, simples, um comando. Só que, como você faz isso pelo Travis? Vai lá, procura ai como faz, vamos ver o que você acha (sem contar esse post):

![Chorando por não achar nada na busca](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445781723/-Ts-1rpVV.gif)Chorando por não achar nada na busca

Depois de muito tempo procurando como fazer isso, eu descobri que você pode [gerar um token no now](https://zeit.co/account/tokens) e usar isso para fazer o deploy. Com esse token configurado como uma variável de ambiente no Travis (não vai me comitar ele!) você pode atualizar seu arquivo **.travis.yml** para:

    language: node_js
    node_js:
    - "6.11.0"
    script: mocha --timeout 10000 --reporter nyan
    before_script:
    - npm install
    after_success:
    - npm run coveralls
    - now -t $TOKEN_NOW --public

Dai é só comitar e corre pro push!

Mas e se eu quiser um Alias?
----------------------------

Se você conhece o Now, sabe que é possível criar um alias para o seu site, seja para um endereço pago ou para solicitar um alias para o domínio padrão, algo assim: **[https://seuprojeto.now.sh](https://seuprojeto.now.sh/)**

**Mas e se eu quiser fazer isso no deploy do Travis?**

![Power Ranger com medo](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445784499/SS0kAgZkB.gif)Power Ranger com medo

Calma jovem, vou te dizer que é simples agora que eu aprendi essa budega.

Mude seu arquivo **.travis.yml** para isso aqui:

    language: node_js
    node_js:
    - "6.11.0"
    script: mocha --timeout 10000 --reporter nyan
    before_script:
    - npm install
    after_success:
    - npm run coveralls
    - now -t $TOKEN_NOW --public
    - now -t $TOKEN_NOW --public alias

E o seu package.json agora tem que ter as propriedades que vão dizer qual é o alias:

    "now": {
       "alias": "seuprojeto"
    }

Pronto, isso garante que quando seu deploy for feito, vai criar um alias 100% funcional.

Mas e se eu quiser só a Master?
-------------------------------

Claro, chegou aquela hora que você fala: "Show! Eu quero tudo isso, mas eu só quero que isso aconteça quando for na master", afinal de contas você não quer que toda branch que você tem vá pro ar, não é?

![Menina do Jurassic Park com muito medo](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445786700/TRvTrK4FA.gif)Menina do Jurassic Park com muito medo

Você percebendo que a branch zoada quase entrou em produção

Sem panico, é mais simples do que o anterior agora que eu gastei um mês pra descobrir isso.

O seu arquivo  **.travis.yml** vai ficar assim:

    language: node_js
    node_js:
    - "6.11.0"
    script: mocha --timeout 10000 --reporter nyan
    before_script:
    - npm install
    after_success:
    - npm run coveralls
    - test $TRAVIS_BRANCH = "master" && test $TRAVIS_PULL_REQUEST = "false" &&
    now -t $TOKEN_NOW --public
    - test $TRAVIS_BRANCH = "master" && test $TRAVIS_PULL_REQUEST = "false" &&
    now -t $TOKEN_NOW --public alias

Feito!

Com isso o seu comando de publicação só é executado na branch master e quando não for um pull-request (Os pull-requests para master tem como branch a master)

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski