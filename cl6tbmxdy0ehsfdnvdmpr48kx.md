# Dotfiles - Como fazer sua vida mais fácil

Uma coisa que todo desenvolvedor tem (ou teve) que fazer é configurar o ambiente dele. Isso inclui, configurações de Git, SSH, instalar zsh, atalhos do sublime e por ai vai.

Ai você faz isso na sua casa. Tem que repetir tudo no trabalho. Mudou de emprego? Tudo de novo. Trocou o computador pessoal? Bora lembrar quais eram as configurações que tinham de ser feitas. E se você tem o hábito de criar alias para tarefas é quase um parto. Faz em casa, tem que fazer no trabalho e em todo computador que você for trabalhar. E se esquecer? Fica sem o atalho até lembrar como era pra ser feito ou precisar muito e sair pesquisando. Você ai deve estar pensando:

_"E se existisse um jeito de deixar isso automático? Fácil e rápido pra usar?"_

Existe jovem: **dotfiles.**

O que é isso?
-------------

Um padrão do sistemas unix hoje em dia é guardar as mais diversas configurações em arquivos iniciados com ponto: .gitignore, .bash, .bashrc, .profile, .m2, .zsh e por ai vai.

Isso nasceu a muito tempo e eu não vou entrar na discussão de bom ou ruim. (Se você estiver curioso a respeito, pode ler mais [aqui](https://mexapi.macpress.com.br/ordem-nos-dotfiles).)

O negocio é que temos que conviver com isso e aceitando isso, podemos tornar nossas vidas mais fácil. Os mais diversos arquivos de configuração são armazenados nos chamados **dotfiles.**

Facilitando a vida
------------------

A primeira coisa que precisamos fazer é definir quais arquivos podemos e queremos salvar. É comum em sistemas linux, adicionarmos diversas configurações no arquivo **_.bashrc_** (ou no .zshrc se você estiver usando zsh como eu), nele colocamos alias, funções customizadas, até mesmo exportação de variáveis. Esse é um arquivo que é muito bom ter a mão quando configuramos nossa outra máquina, num instante temos nossos atalhos e configurações customizadas. Se você quer simplesmente ter a mão isso, pode salvar uma cópia dele em algum serviço de armazenamento online como o Dropbox. Isso não é um problema, desde que você só salve um arquivo. Mas e quando você começar a salvar vários arquivos? De diversos programas, em diversas pastas, como gerenciar isso e pior, como restaurar isso tudo depois?

_"Você não disse que ia facilitar a vida?"_

Vou facilitar, mas primeiro você precisa entender qual é o problema que queremos resolver agora. Primeiro definimos que vamos salvar nossos _**dotfiles,**_ mas esbarramos no segundo problema: _**como gerenciar isso?**_

Código é _quase_ _sempre a resposta_
------------------------------------

A primeira coisa que vamos definir é que precisamos de um controle de versão. Por se tratarem de arquivos que mudam com o tempo, é importante ter um registro e um controle dessas mudanças. Então, git neles. Você pode ficar livre para escolher onde colocar seus arquivos, mas eu recomendo o [Github.](http://github.com/) Porque? Por se tratar de um lugar onde os mais diversos códigos são compartilhados. Se você não conhece nada de Git ou de Github, pode fazer um curso fantástico [aqui](https://www.udemy.com/git-e-github-para-iniciantes/).

Se você é desenvolvedor, quando eu falei em código duas coisas certamente passaram pela sua cabeça:

1 - Vou fazer um código animal para controlar meus dotfiles; Ou

2 - Vou procurar algum programa que controla dotfiles para mim

Eu não vou entrar (agora) na discussão se você deve ou não criar o seu próprio programa para isso, o que é importa é definirmos o intuito: ter um código que seja capaz de controlar seus arquivos. Onde eles vão, como eles tem que ser dispostos, precisa rodar algum script? Eu uso um carinha chamado [dotbot.](https://github.com/anishathalye/dotbot) Você pode ler a documentação se tiver o intuito de usar ele, mas a ideia por trâs é simples: Você tem um arquivo que descreve sua configuração, definindo onde ele deve criar os links simbolicos, se tem de executar algum script e outros detalhes. Quando você precisar ter seus dotfiles em outro lugar, só precisa rodar um .**_/install_** na raiz e pronto. Tudo funcionando. Você ver como ficaram os meus **dotfiles**  [aqui.](https://github.com/angeliski/dotfiles)

Não use o dotfile dos outros
----------------------------

Quando eu comecei nesse mundo de dotfiles, a primeira coisa que eu pensei foi:

"Cara, precisa copiar o dotfiles de alguém fantástico! Assim eu vou ser fantástico também!"

E qual é o problema nisso? Acontece que, seja quem for, as maneira de trabalho são muito diferentes. Você e o Martin Fowler tem maneiras diferentes de pensar e isso muda tudo. Como você vai pensar num atalho que ele usa? Mesmo que você leia os arquivos, não vai ser natural pra você, vai rolar um esforço, o que remove o propósito de produtividade. Não me entenda errado, não estou dizendo pra você não copiar, olhar, duplicar dotfiles dos outros, mas quero que você reflita sobre isso. Aquilo que você está usando, faz sentido pra você? Existem companhias por exemplo, que usam esses arquivos de configuração para manter um padrão nos editores e afins. Isso tem muita utiliade em alguns casos, mas cautela. Antes de usar os arquivos de configuração de outra pessoa, lembre-se que isso é algo extremamente pessoal e por consequência, tem muito  a ver com a maneira de trabalho de cada um.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski