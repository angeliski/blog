# Equals e HashCode : O que é, porque usar e como implementar

Uma das duvidas mais comuns para os programadores que estão começando em java é sobre a dupla _"equals e hashCode"_.

O hashCode é uma ferramenta da JVM usada para montar a tabela de hash de modo correto.

"Ham? Do que você ta falando velho?"

Tabela Hash \[também conhecida como Tabela de Dispersão ou Tabela de Espalhamento\] é uma tabela onde as informações são armazenadas conforme um **"numero hash"** calculado com base nas propriedades da informação. Isso permite que seja muito rápido recuperar uma informação na tabela.

Vamos exemplificar o negócio: você tem uma tabela com as informações de todos os pacientes de um hospital. Se você fosse buscar um paciente em especial iria demorar um tempo (O(n) numa busca linear ou O(log N) para buscas binarias) o que pode ser extremamente ruim em uma situação real onde existe um volume de dados gigantescos. Usando uma tabela hash a busca reduz seu tempo de busca (O(1)) para qualquer situação, bastando apenas o calculo do valor hash na entrada e na saída da informação.

Um ultimo exemplo pra quem ainda não compreendeu, imagine que a tabela hash é um armário cheio de gavetas, todas com um numero na frente. Toda vez que você vai guardar alguma coisa nesse armário, primeiro se calcula um hash dessa informação \[o como a gente explica depois\] e com esse número nós colocamos na gaveta especifica. Se alguns anos mais tarde precisar dessa informação, apenas calculando o hash nós podemos ir direto na gaveta certa!!!

"O LOKO CARA! Entendi essa parada mas… e se tiver mais de uma informação nessa gaveta?!"

Se entendeu a parada mesmo. A ideia de uma boa função hash é garantir que somente uma informação esteja em uma determinada "gaveta", só que infelizmente isso nem sempre é possível. Quando mais de uma informação acaba em um mesmo **bucket** \[apenas uma outra forma de se referir a "gaveta"\] acontece o que chamamos de colisão na tabela hash. Ainda assim é a melhor estratégia, pois temos que procurar apenas entre poucos objetos ao invés daquela volumosa quantidade de antes.

Toda essa explicação de hash é algo independente da linguagem, tabelas hash podem ser usadas em diversos cenários onde exista a informação para armazenar e recuperar informações de modo otimizado e eficiente. O java se utiliza desses conceitos para melhorar suas estruturas e prover melhores buscas.

"Angeliski, se falou muito desse tal hash… onde entra o equals?"

Lembra que eu disse que pode haver uma colisão de informações? ou seja, podem ter dois dados num mesmo **bucket**. Através do método equals você pode determinar se os objetos são iguais. A ideia do equals é garantir que dois objetos são "significativamente iguais", ou seja, que os objetos tem uma valor igual dadas as suas propriedades.

> "Falar é fácil, me mostre o código."
> 
> Linus Torvalds

Como implementar?
-----------------
%[https://gist.github.com/angeliski/548d5aa2a4fc12f18b77c909417af8fa#file-pessoa-java]

Como podemos definir que duas pessoas são "significativamente iguais"? Vamos dizer que toda pessoa com o mesmo nome e com a mesma idade é igual, modificando nossa classe ela fica assim:

%[https://gist.github.com/angeliski/773603a77ff8426bd8bb4ff87a2255a8#file-pessoa-java]
Essa é uma implementação muito simples do método, não fazemos verificações de nulo por exemplo, mas ela é bem didática.

O importante é que com esse método garantimos que um objeto com mesmo nome e idade vai ser sempre considerado igual. "Olha, você ta explicando e tal…mas isso pode não dar muito certo… e se o sobrenome for diferente? Não vai ser a mesma pessoa."

Exatamente gafanhoto! Um dos "problemas" em se implementar o **equals** é garantir que as informações usadas vão garantir que os objetos são os mesmos, por isso a ideia do "valor significativo", cabe ao desenvolvedor decidir isso.

Outra coisa importante é: **hashCode** e **equals** andam juntos! Se você implementar um **DEVE** fazer o mesmo com o outro. Não vai dar erro se não o fizer, mas a chance de alguma lógica dar errado por conta disso é muito grande.

![Mais uma coisa!](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445912078/24euZ87hr.jpeg)Mais uma coisa!

Segundo a Documentação do Java, o contrato(Regras) de equals() segue as seguintes diretrizes:

> *   É reflexivo: para qualquer valor de referência x, x.equals() deve retornar true;
> *   É simétrico: para qualquer valor de referência x e y, x.equals(y) deve retornar true se, e > somente se, y.equals(x) retornar true.
> *   É transitivo: para qualquer valor de referência de x, y e z, se x.equals(y) retornar true e > y.equals(z) também retornar true, então, x.equals(z) deve retornar true.
> *   É consistente: para qualquer valor de referência de x e y, múltiplas chamadas de x.equals(y) > retornarão consistentemente true ou consistentemente false, contanto que nenhuma informação > usada nas comparações do objeto de equals tenha sido alterada.
> 
> Para qualquer valor de referência x que não seja null, x.equals(null) deve retornar false.

Como dito, são diretrizes, você não as precisa seguir. Só que sem seguir elas, sua implementação do equals não vai ser eficiente nem vai atender suas necessidades.\[siga as diretrizes cara, vai por mim.\]

"Angeliski, eu não tenho ideia de como fazer um equals e hashCode!! E agora?"

A maneira mais simples (que pode não ser a mais eficiente) é pedir para a IDE gerar isso pra você. Veja o código gerado para a nossa classe:

%[https://gist.github.com/angeliski/358fbe8004e1c49b48761a6cde6e252e#file-pessoa-java]

Por padrão as classes tem a implementação do equals da classe Object, que apenas verifica se os dois objetos tem o mesmo endereço de memória, nem sempre isso é util para nosso código.

Porque usar
-----------

A maior vantagem em sobrescrever esses métodos é melhorar a busca dos objetos. Vamos a um exemplo:

%[https://gist.github.com/angeliski/3e1fd2985b44429785ffb884d8421254#file-buscapessoa-java]

Como você pode ver nesse código, temos necessariamente que percorrer a lista para poder localizar nosso objeto, agora vamos ver um modo mais eficiente de fazer isso:

%[https://gist.github.com/angeliski/f3607074bc3681a33ea729259a666bc3#file-buscapessoa-java]

O método **.indexOf** da interface List retorna o indice do objeto passado, mas isso se torna "inviável" sem uma implementação correta do **equals** e **hashCode**. Isso é só uma situação, existem diversos "problemas" acarretados pela não implementação desses métodos, quando usamos uma datatable no JSF existe algumas situações em que os objetos se comportam de modo incorreto graças a falta dos métodos sobreescritos.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski

* * *

### Referências:

ps: Eu removi o último link pois a Caelum não disponibiliza mais o conteúdo