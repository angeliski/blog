# Mapeando views com Hibernate no spring-data

Um framework sensacional que existe para persistência de dados é o [Hibernate](http://hibernate.org/). Ele simplifica uma série de operações através da sua abstração, permitindo que você realize interações com o banco sem necessariamente escrever SQL. Mas o assunto não é sobre ele nem sobre outra maravilha que é o [spring-data](http://projects.spring.io/spring-data/). Não se preocupe se você não conhece essas ferramentas, vamos falar  delas em outro post.

O assunto de hoje é sobre uma particularidade que acontece quando você coloca os dois juntos (bem comum) e tenta mapear uma view (não tão comum).

Bora lá ver como resolver isso!

O problema
----------

Representar uma view como entidade do **Hibernate** não é nada complexo, você apenas cria uma classe com base na sua view e anota ela de acordo. Acontece que a view no hibernate não tem uma anotação especial, é uma entidade  como as outras e por conta disso,  precisa que um atributo esteja anotado com **@Id.** Só que você já sabe que sua view (normalmente) não tem um id,  já que ela é uma representação de outros dados. Então você resolve anotar qualquer atributo com **@Id** e seguir em frente.

Só que quando você recuperar registros dessa view e houver duas linhas com o mesmo valor, só uma delas será retornarda. Confuso? Vamos ver um exemplo.

Imagine que nossa aplicação tem duas tabelas, uma de pessoas e outra de endereços. Além disso, temos uma view que nos retorna a pessoa e seu endereço completo. Nossa tabela de pessoa tem um _**Bruce Wayne**_ cadastro com dois endereços e um _**Berry Allen**_ com só um endereço. Vamos ver como fica nosso código.


```java
@Entity
public class Pessoa {

    @Id
    private Long id;
    private String nome;
    private Integer idade;

    @OneToMany(mappedBy = "pessoa")
    private List<Endereco> enderecos;
    //getter e setters
}

@Entity
public class Endereco {

    @Id
    private Long id;
    private String logradouro;
    private String bairro;
    private String cep;
    @ManyToOne
    @JoinColumn(name = "pessoa_id")
    private Pessoa pessoa;
}

@Entity
public class PessoaComEndereco {

    @Id
    private Long pessoaId;
    private Long enderecoId;
    private String nome;
    private String enderecoCompleto;

}
``` 


Como você pode ver,  a view teve seu id anotado no id da pessoa, acontece que quando tentarmos recuperar os dados de Bruce dentro da aplicação através da nossa view, vamos ter um problema. Veja só:

![Intellij mostrando o registro duplicado](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445855975/kVin8rTaw.png)Intellij mostrando o registro duplicado

Se você executar o teste **carregarEnderecosEsperandoDuplicidade** presente na classe **ViewHibernateSpringDataApplicationTests** vai obter o mesmo resultado: O mesmo endereço é carregado duas vezes.

_"Mas Angeliski, como isso é possível? Eu verifiquei o banco e o select, tá tudo certo?"_

Obs.  Eu não testei isso sem usar o **spring-data** para verificar se esse erro ocorre também.

O problema está claramente no _**@Id**_ que  especificamos na view. Vamos pensar na solução agora.

1º Solução - EmbeddedId
-----------------------

Quando falamos do _**@Id**_ você pode logo pensar em resolver isso gerando um Id composto para a view. Então vamos criar uma classe para testar esse cenário:

    @Entity
    @Table(name = "PESSOA_COM_ENDERECO")
    public class PessoaComEnderecoIdEmbeddable {
    
        @EmbeddedId
        private PessoaEnderecoId pessoaEnderecoId;
        private String nome;
        private String enderecoCompleto;
    }

Agora sim, nosso resultado está correto quando realizamos a busca:

![Intellij mostrando apenas um registro](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445857380/gy6dI8B2r.png)Intellij mostrando apenas um registro

Isso resolve o nosso **atual** problema, só que nossas views quase nunca são tão enxutas e provavelmente não terão um conjunto de identificação único que sempre faça sentido.

_"Angeliski, se o EmbeddedId não pode resolver sempre, como faz?"_

2º Solução - Gerando um id para a View
--------------------------------------

É isso ai. Nós, programadores, vamos providenciar um identificador para essa view.

Primeiro vamos ver nossa classe, que representa isso:

    @Entity
    @Table(name = "PESSOA_COM_ENDERECO")
    public class PessoaComEnderecoIdGerado {
    
        @Id
        private String id;
        private Long pessoaId;
        private Long enderecoId;
        private String nome;
        private String enderecoCompleto;
    }

Se você notar, nós acrescentamos o campo Id nesse caso, para anotar ele como o @Id dessa view. Mas como vamos gerar um identificador no banco? Dê uma olhada na nossa declaração da view:

    CREATE OR replace VIEW pessoa_com_endereco 
    AS 
      SELECT Uuid()                                                 AS ID, 
             p.id                                                   pessoa_id, 
             e.id                                                   endereco_id, 
             p.nome, 
             Concat(e.logradouro, ', ', e.bairro, ' - CEP:', e.cep) 
             endereco_completo 
      FROM   pessoa p 
             inner join endereco e 
                     ON p.id = e.pessoa_id; 

Se você observar, o primeiro item do select é o id, usando a função [UUID()](https://dev.mysql.com/doc/refman/5.7/en/miscellaneous-functions.html#function_uuid) (para o Mysql!).

Essa função vai ser responsável por gerar nosso identificador.Se você se incomodar pelo identificador se uma String, pode usar a função [UUID\_SHORT()](https://dev.mysql.com/doc/refman/5.7/en/miscellaneous-functions.html#function_uuid-short) no lugar e mudar para Long.

Ou se estiver usando um database Oracle, pode usar o [rownum](https://www.techonthenet.com/oracle/functions/rownum.php) para gerar seu identificador.

Conclusões
----------

As duas abordagens tem suas desvantagens.

Criar identificadores compostos no JPA é verboso e muitas vezes não faz sentido, ainda mais se tratando de uma view. Além disso, por ser uma view, o conjunto de dados que torna a linha única, pode não estar presente no retorno, ou não ter a mesma consistência para os dados, o que torna inviável criar essa classe composta.

Criar uma coluna que que representa seu Id pode ser um problema quando você está mexendo em uma base legada por exemplo, nem sempre é possível alterar a view para adicionar essa coluna que você tanto necessita. Além disso, essa solução se parece muito com um contorno a limitação do JPA no mapeamento de Views.

Eu disponibilizei o código desses teste neste [repositório](https://github.com/angeliski/view-hibernate-spring-data), assim você pode reproduzir e testar por si mesmo.

Sugestões e melhorias no código são bem vindas, basta abrir uma issue/pull request.

Qual estratégia você vai usar vai depender do seu contexto, eu só apresentei as abordagens. E você já teve um problema parecido? Abordou de outra forma?

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski