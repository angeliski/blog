# Utilizando o @ConversationScoped no VRaptor 4

O VRaptor em sua versão 4 utiliza o CDI 1.1 como base. E uma das considerações a serem feitas quando usamos o CDI é o escopo de vida dos objetos. Sempre é preciso escolher com cuidado o escopo, levando em consideração qual vai ser a aplicação do objeto. Utilizar o _@RequestScoped_ onde é necessário um _@ApplicationScoped_ pode ser desastroso. Um escopo muito util nesse cenário é o _@ConversationScoped_ que nos permite um controle maior conforme a nossa necessidade. A aplicação apresentada nesse tutorial está disponivel no GitHub: [ScoreCDI](https://github.com/angeliski/ScoreCDI). Abaixo nosso Controller:


```java
package br.com.angeliski.controller;

import java.io.Serializable;
import java.util.Random;

import javax.enterprise.context.Conversation;
import javax.enterprise.context.ConversationScoped;
import javax.inject.Inject;

import br.com.caelum.vraptor.Controller;
import br.com.caelum.vraptor.Get;
import br.com.caelum.vraptor.Post;
import br.com.caelum.vraptor.Result;

@Controller
@ConversationScoped
public class HomeController implements Serializable {

	private static final long serialVersionUID = 943045823176068998L;
	private Result result;
	private Conversation conversation;
	private Integer resultado;
	private int total;

	/**
	 * @deprecated CDI eyes only
	 */
	protected HomeController() {
		this(null, null);
	}

	@Inject
	public HomeController(Result result, Conversation conversation) {
		super();
		this.result = result;
		this.conversation = conversation;
	}

	@Get
	public void index() {
	}

	@Get
	public void game() {
		if (conversation.isTransient()) {
			conversation.begin();
		}
		result.include("cid", conversation.getId());
		gerarPergunta();
	}

	private void gerarPergunta() {
		Random random = new Random();
		Integer primeiroValor = random.nextInt(100);
		Integer segundoValor = random.nextInt(100);

		resultado = primeiroValor * segundoValor;

		result.include("primeiro", primeiroValor);
		result.include("segundo", segundoValor);
	}

	@Post
	public void game(Integer resposta) {
		if (resultado.equals(resposta)) {
			total++;
		}
		gerarPergunta();
		result.include("cid", conversation.getId());
	}

	@Get
	public void fim() {
		if (!conversation.isTransient()) {
			conversation.end();
		}
		result.include("total", total);
	}

} 
``` 


A primeira distinção desse Controller é a anotação _@ConversationScoped_ na classe. Isso indica para o CDI qual vai ser o escopo, mas cuidado: só isso não é suficiente para que a sua classe tenha um escopo maior que o escopo request. Para que o escopo se torne persistente, não perdendo os dados a cada requisição, é necessário injetar um objeto _Conversation_ e chamar seu método _begin_. Isso pode ser observado aqui:

    	@Get
    	public void game() {
    		if (conversation.isTransient()) {
    			conversation.begin();
    		}
    		result.include("cid", conversation.getId());
    		gerarPergunta();
    	}

Nesse método a conversação é iniciada. O _Conversation_ tem um estado padrão conhecido como \*transient\*. Quando o método _begin_ é acionado ele passa para o \*long-running\* mantendo os dados até que seja invocado o método _end_ do _Conversation_. Só que um detalhe importante deve ser observado. O CDI não tem como saber a qual objeto você está tratando. O único modo dele saber isso é você informando qual o identificador daquela conversação e isso é feito através do pârametro \*\*cid\*\*. É necessário informar na url esse pârametro para que o CDI consiga recuperar o objeto correto. Observe que o pârametro cid é incluido no result para que seja possível utilizar ele na pagina. O nome utilizado para incluir o \*id\* do _Conversation_ no result não é fundamental. Ele poderia ser qualquer um, desde que fosse recuperado corretamente na pagina. Só é fundamental utilizar o pârametro \*\*cid\*\* quando a requisição for enviada para o servidor. Observe como ficou a nossa pagina que vai utilizar esse parâmetro.


```jsp
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
<head>
<title>Score CDI</title>

</head>
<body>
	<h4>Sessão atual ${cid}</h4>
	<p>Quanto é ${primeiro} * ${segundo}?</p>
	<form action="${linkTo[HomeController].game}?cid=${cid}" method="POST">
		<input name="resposta" type="number" required>
		<button type="submit">Continuar jogo!</button>
	</form>
	
	<a href="${linkTo[HomeController].fim}?cid=${cid}">Finalizar!</a>

</body>
</html>
```

Você pode observar que a action do formulário faz o uso correto do **cid**. Quando essa requisição for enviada ao servidor, vai enviar junto o identificador correto. Enquanto a requisição for feita enviando o **cid** o número exibido em Sessão atual vai continuar o mesmo, pois a conversação vai se manter. Você pode fazer um teste acessando diretamente '/home/game' sem enviar o cid no pârametro. Vai ser iniciada uma nova conversação e o cid irá se modificar.

Por fim temos o método que encerra o _long-running_ através do método _end_:

    	@Get
    	public void fim() {
    		if (!conversation.isTransient()) {
    			conversation.end();
    		}
    		result.include("total", total);
    	}

Aqui é valido observar que caso você não encerre uma conversação, ela não vai se manter em memória por muito mais que dois minutos, que é o tempo padrão. Isso garante que diferente do _@SessionScoped_ os recursos vão ser liberados antes, caso a aplicação não encerre o estado daquele objeto. O timeout pode ser ajustado através do método _setTimeout_ conforme a necessidade, mas normalmente não é preciso.

Dúvidas? Gostou? Me acha um idiota?

Comenta ai!!

Angeliski