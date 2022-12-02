# Deletando devagar no Oracle?

Salve Galera! Hoje eu venho trazer uma dica rápida mas que me ajudou no trabalho.

Atualmente a aplicação que eu desenvolvo tem seus dados salvos numa base de dados Oracle. A persistência é abstraída pelo nosso Hibernate, mas no fim do dia é no Oracle que a coisa acontece. Recentemente tivemos problema com uma deleção de registro que demorava muito tempo. Era uma tabela pequena, com no máximo cem registros, mas que demorava cerca de dois minutos para deletar.

![Sério?](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445769993/FW8ehFyhn.gif)Sério?

Pois é. Como pode um negocio desses? Depois de muita procura e testes eu descobri o motivo. Durante uma deleção de registro o Oracle procura todas as referências daquela tabela e valida antes de executar. Em outras palavras, sempre que você executa um comando delete numa tabela, o Oracle pesquisa cada tabela que tem uma Foreign Key que aponta para para a tabela que você está tentando deletar. No meu caso, existia uma tabela imeeeeensa que tinha uma FK vinculada. Quando eu tentava deletar, o Oracle tinha que realizar um Full Scan para garantir que o registro que eu queria apagar não tinha nada vinculado. A Solução nesse caso foi criar um index que indexava somente a coluna da FK. Feito isso meu delete passou a ter um tempo de espera de 10s.

![Galera comemorando](https://cdn.hashnode.com/res/hashnode/image/upload/v1660445772507/YAXFy9qmD.gif)Galera comemorando

Claro, esse é um caso bem especifico. Existem ainda outras situações que você pode querer deletar muitos registros, nesse caso pode ser mais rápido, criar uma tabela apenas com os registros que você vai manter... Mas isso é tema de outro post, outro dia.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski