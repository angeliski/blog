# Yarn - O NPM não serve mais?

A pouco tempo o Facebook disponibilizou para a comunidade de desenvolvimento o **Yarn,** um gerenciador de dependências parecido com o **npm**. Tão parecido que a única diferença na estrutura que ele gera é o **yarn.lock**,  mas vamos explicar o que tudo isso significa.

### Package Manager

Uma das coisas que toda aplicação tem são dependências. E para resolver o nosso problema de um milhão de dependências nem sempre temos um gerenciador de dependências para cuidar disso.  A responsabilidade desses gerenciadores é garantir que você não precisa ficar procurando onde baixar essas dependências e que suas dependências são as mesmas para todos desenvolvedores em teoria. No Java existe o [Maven](https://maven.apache.org/),  [Gradle](https://gradle.org/),  que ilustram como isso pode ser. No Javascript/Node existe o **[npm](https://www.npmjs.com/)**. Mas acontece  que o **npm** tem também um repositório onde essas dependências ficam salvas e outros Gerenciadores (conhecidos aqui como Package Manager) vão recuperar os itens necessários para sua aplicação funcionar, além do **npm** podemos citar o ied, pnpm, npm-install, npmd e agora também o [Yarn](https://yarnpkg.com/).

_"Tá,  mas existem todos esses Angeliski,  pra que outro? "_

Eu poderia dizer que o Facebook gosta de reinventar a roda compartilhar novidades, mas não é tão simples, acontece que o **Yarn** vem para resolver outros problemas.

### Yarn install

A primeira coisa que chama a atenção nele é a velocidade que ele faz o download das dependências do projeto. Quem mexe com aplicações **Node** sabe como é demorado  rodar uma aplicação a primeira vez. Acontece que o **npm** demora para baixar as dependências e disponibilizar elas na pasta _node\_modules_. O **Yarn** tem um algoritmo otimizado pra isso,  que além de tudo costuma paralelizar os downloads. Mas a verdadeira magia esta no **cacheamento de dependências**. Quando você instala uma dependências no **Yarn**,  ele faz um cache local dessa dependência, assim quando você fizer isso dessa dependência em qualquer outro lugar, o **Yarn** não precisa buscar na Internet,  por conta disso ele **funciona offline** para as dependências cacheadas também.

Do meu ponto de vista, essa é a maior vantagem do **Yarn** sobre todos os outros. Até hoje o download dessas dependências pelos gerenciadores era sempre demorado e se você apagava o node\_modules (seja porque precisou ou porque deu um git clean sem querer) você tinha que esperar baixar **TUDO DE NOVO.** Além disso, o Yarn resolve de uma maneira melhor a questão do lock de dependências com o yarn.lock

_"Falhou Angeliski, o Node tem o shrinkwrap!"_

### yarn.lock

Sim, o Node tem o _shrinkwrap,_ mas o primeiro problema dele é que ele não é gerado por padrão, ao contrário do yarn.lock, ou seja, se você esquece de gerar ele passa a ter a opção de ter problemas quando for atualizar as dependências. Se você mexe com software, sabe como é fácil esquecer de rodar aquele comando X, que só dá erro beeem depois que você não está mexendo mais com a tarefa. Visando evitar isso, o **Yarn** tem o yarn.lock gerado sempre que existe uma modificação nas dependências, isso garante que onde quer que o projeto seja baixado, quando o yarn install for executado, as mesmas dependências serão executadas, seja na maquina do lado, na homologação, no servidor de testes ou até na casa do dev que está fazendo remoto hoje.

As vantagens que o **Yarn**traz logo de cara são bacanas, mas o mais interessante é que não existe um "processo de migração" do **npm** para o **Yarn.** Como a estrutura padrão é a mesma, você pode usar na sua maquina sem que ninguém mais do seu time use... Levando em conta o custo disso claro, como nem todos do time usam, a chance do yarn.lock ficar errado/desatualizado é enorme. Só que o que conta é que você não precisa fazer grandes mudanças no seu projeto para começar a usar o **Yarn.** É apenas sair usando!

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski