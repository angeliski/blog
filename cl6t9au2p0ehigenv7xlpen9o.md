# Git e a dificuldade em renomear um arquivo

Quem trabalha com Git ou usa ele em casa, provavelmente já teve esse problema.

Quer dizer, existe alguns requisitos pra isso acontecer... Primeiro você não pode usar linux (não me odeie, eu uso linux em casa!) ou seja, isso só costuma acontecer no Mac (Eu uso no trabalho) e no Windows.

O problema é relativamente simples, você renomeia um arquivo, mas você só altera as letras maiúsculas e minúsculas, por exemplo, você pega o seu arquivo _QuitacaoParcial**D**ECompras.java_ e altera para _QuitacaoParcial**d**ECompras.java_

Pronto! Ai você comita e segue a vida. Você não vai ter nenhum problema até que baixe esse projeto novamente ou que outra pessoa de um pull na  máquina dela.

_"Angeliski, se tá bricando né? Porque isso daria problema?"_

O que acontece é que o git por padrão tem uma configuração de case insensitive... Isso quer dizer que pra ele TESTE.java, teste.java, TesTe.java, TEsTe.java **são a mesma coisa.** E isso é um problema, porque quando você altera o nome do arquivo ele não salva essa alteração... O que resulta num problema no próximo pull que for dado, já que você tem um arquivo com o nome errado e acha que ele está certo. Normalmente em projetos Java isso dá problema, por conta da estrutura de pacotes que a linguagem usa (Porque isso se aplica a nome de pastas também).

 _"Mas como eu resolvo isso? Tem algum jeito?"_

A primeira vez que eu me deparei com esse problema eu resolvi da maneira mais porca simples que se possa imaginar. Mudei o nome do arquivo para uma coisa aleatória como A.java, fiz um commit, depois alterei o nome do arquivo para o que eu realmente queria, _QuitacaoParcial**d**ECompras.java. Só que isso sempre me incomodou, parece uma solução extremamente gambiarrenta grosseira. Depois de umas pesquisas, achei na documentação do git a solução:_

> [core.ignoreCase](ftp://www.kernel.org/pub/software/scm/git/docs/git-config.html)
> 
> If true, this option enables various workarounds to enable Git to work better on filesystems that are not case sensitive, like FAT. For example, if a directory listing finds "makefile" when Git expects "Makefile", Git will assume it is really the same file, and continue to remember it as "Makefile".
> 
> The default is false, except [git-clone(1)](ftp://www.kernel.org/pub/software/scm/git/docs/git-clone.html) or [git-init(1)](ftp://www.kernel.org/pub/software/scm/git/docs/git-init.html) will probe and set core.ignoreCase true if appropriate when the repository is created.

 Nem preciso dizer que eu chorei pulei de alegria não? O que acontece é simples, apesar da propriedade core.ignoreCase ser false por padão o git-clone faz o favor de colocar ela como padrão quando é executado. Isso ai atrapalha tudo! Mas uma vez que a gente sabe qual é o problema é simples de resolver:

    git config --global core.ignorecase false

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski