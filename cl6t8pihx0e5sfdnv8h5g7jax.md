# Maven : O que é, porque usar e por onde começar

Imagine que seu chefe passou um projeto pra você, sua grande chance! Começam as configurações, as bibliotecas utilizadas são definidas e a busca pela internet começa. Dois dias de procura, muitos forums visitados  exaustivamente, mas você conseguiu achar todas as libs! Vitória!! Mas será que era necessário todo esse trabalho? Imagine se houvesse um jeito de buscar "MAGICAMENTE" todas as libs que você.. oh GOD! Isso chama [MAVEN](http://migre.me/w9Bs9)!!

Maven é um [Projeto da Apache](http://migre.me/w9Bue) que tem como foco um gerenciamento de bibliotecas entre outras responsabilidades, ele permite constuir projetos, gerenciar dependencias, gerar documentações. Se você quiser se aprofundar tem um link maroto [aqui](http://migre.me/w9Bs9).

"Ok Angeliski, tudo muito legal pelo que você falou, mas pra que eu vou usar isso?"

Porque usar o Maven?! Eu poderia simplesmente dizer que ele é fantástico e tava acabado, mas vamos justificar... Uma das responsabilidades do Maven é gerenciar as dependências do projeto, ou seja, aquelas bibliotecas que você ficou DIAS procurando na internet (que também podem não ser as oficiais...) estão em um lugar só, seja um [repositório local](http://migre.me/ev6s5) ou um [repositório "direto"](http://migre.me/ev6u1). Isso garante que sejam quantas forem as equipes no projeto, ou estejam onde elas estiverem, as bibliotecas vão ser as mesmas, os comportamentos vão ser os mesmos (isso garante que não vai haver um "funciona na minha maquina!"). Alem disso o Maven entra como "substituto" do [ANT](http://migre.me/ev6Db) na parte de "compilar e juntar" os projetos distribuídos uma pratica cada vez mais comum nas aplicações empresariais que usam os EAR. Isso tudo falando em um ambiente corporativo, que provavelmente já deve ter seu "Maven instalado" quando você chegar, por isso vá se atualizando, mas qual seria o motivo de usar isso em casa? Bom, primeiro o motivo obvio que você vai aprender uma tecnologia que vai ser utilizada nas empresas, ou seja, já começa com vantagem sem precisar ficar todo perdido, além disso quando você trabalha com as dependências gerenciadas você tira as LIBs do projeto, se você precisar compartilhar  esse projeto ele estaria muito mais leve.

"Cara, isso parece até ser boa idéia... mas vamo começar por onde?!"

O primeiro passo é entrar no site do [Maven](http://migre.me/ev6TU) e baixar o zip (atualmente na versão 3.0.5), lá tem o passo a passo pra configurar ele na sua maquina, é simplesmente a configuração de duas variáveis de ambiente. Além disso é provavel que você esteja usando uma IDE (Eclipse, por favor, Eclipse. Sem influências.. husahash), então você vai precisar configurar o  plugin na IDE para utilizar em conjunto.

Resumo da História: Maven é um projeto fabuloso, que otimiza seu trabalho, nesse post só foi abordado o Maven pelo lado do Gerenciamento das Dependências, mas o Maven é muito mais que isso.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski