# Developer Experience — Chapter Two

[Ano passado o time se formou e com ele veio um desafio](https://medium.com/rd-shipit/developer-experience-chapter-one-11d3f3499899): Como vamos medir a evolução das nossas iniciativas? Quais métricas a gente vai olhar para saber que estamos no caminho certo? Uma empresa que tem como valor [Data-Driven](https://www.cultura.rdstation.com/) precisa tomar decisões baseadas em evidências sólidas e seguras.

**Precisamos medir a produtividade**

Esse com certeza foi o nosso primeiro pensamento e se você caiu nessa armadilha deve ter descoberto: Essa é uma métrica muito complexa para ser definida, porque medir a coisa errada pode levar a um problema maior do que o de apenas não medir, como já diziam os agilistas: **métricas moldam comportamentos** (ou na economia, pessoas respondem a incentivos)**.**

Isso quer dizer que se você focar em medir número de commits por dia, por exemplo, as pessoas vão entender que elas **precisam commitar todo dia** e isso pode ser qualquer coisa, não apenas código certo para o problema certo.

Ao mesmo tempo que se o foco está em medir entregas, você pode gerar diversas entregas que não geram valor real para o cliente, mas que geram números.

Nenhuma das métricas está necessariamente errada, afinal de contas nós queremos escrever mais código e queremos entregar mais. A grande problemática dessa métrica é como ela é exposta, acompanhada e divulgada. E **quais são as outras métricas que a acompanham**.

Esse [texto](https://medium.com/rd-shipit/m%C3%A9tricas-em-times-%C3%A1geis-as-3-m%C3%A9tricas-fundamentais-que-voc%C3%AA-precisa-saber-e-dominar-816ffb6a53c5) do Caco traz alguns insights valiosos sobre métricas ágeis e eu recomendo a leitura.

Mas nós continuamos com um problema: Como vamos medir a evolução das nossas iniciativas? Como vamos saber se a nossa engenharia realmente está mais produtiva?

**Como o mercado mede isso?**

Sem dúvida nós não devemos ser os primeiros a enfrentar esse problema, então vamos descobrir como outras empresas têm acompanhado a evolução dessa produtividade.

Após [muita leitura sobre o assunto](https://awesome-exp.dev/)(e de montar um compilado com essas informações) encontramos um conjunto de métricas que vem ganhando força na área de maneira a demonstrar a performance de um time de desenvolvimento de software: as [DORA Metrics](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance)

**O que são DORA Metrics?**

Essa também foi nossa dúvida. Um dos grupos de pesquisa do Google realiza há alguns anos um estudo sobre as práticas de DevOps que permitem uma empresa ir “melhor ou pior”, esse relatório é chamado de The DevOps Research and Assessment (DORA).

Durante esse estudo, ficou claro que existiam 4 métricas primárias que avaliavam a performance de entrega de uma empresa. Cada uma dessas métricas tem uma separação de 4 níveis, o que coloca ela em um grupo de maior ou menor impacto.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280422118/hsMD4xkla.png)

As métricas são (tradução livre):

*   **Frequência de deploy (Deployment frequency)** — Quão frequente é colocado código em produção? Ou a liberação de novas features?
*   **Tempo de espera entre mudanças (Lead time for changes)** — Quanto tempo leva entre você começar a fazer algo e aquilo estar pronto rodando em produção?
*   **Tempo para recuperação do Serviço (Time to restore service)** — Quanto tempo demora para que você se recupere de incidentes? Incluindo o tempo que leva **para descobrir que está acontecendo um incidente.**
*   **Taxa de mudanças com falha (Change failure rate)** — Qual o percentual de mudanças que você faz que geram um erro/bug/incidente?

Começar a olhar essas métricas e ler os [relatórios que o google produziu](https://awesome-exp.dev/docs/researchs) nos trouxeram muitos insights sobre como medir e como avançar nas nossas iniciativas.

Mas mesmo com esse avanço, nós ainda tínhamos um problema: o tempo entre nossa entrega e o reflexo na métrica era muito grande! Nós estávamos entregando coisas, mas o reflexo no número demorava muito para aparecer.

Para explicar isso, vamos falar sobre pensamento analítico crítico (um conceito que o grande [Eduardo Magalhães](https://www.linkedin.com/in/dudumagalhaes/) apresentou na RD) e sobre suas três dimensões em relação às métricas:

*   Camada de Controle
*   Camada de Influência
*   Camada de Interesse

### Camada de Controle

Essa camada é a que está mais perto de você e do seu time. Aqui nós temos as métricas que vocês têm **controle** e **independência** para mudar. Quando você faz alguma ação, o reflexo disso leva pouco tempo para aparecer. O número de fatores que podem impactar nessa camada é menor e o time tem mais controle sobre eles.

Exemplo: Podemos olhar para quanto tempo leva para a nossa bateria de testes rodar

**Qualquer** alteração que o time faça para otimizar esse tempo vai rapidamente dar resultado na métrica do tempo (vai aumentar ou diminuir imediatamente).

### Camada de influência

Essa camada está um pouco mais distante e normalmente ela envolve mais contextos (e times), além de um número muito maior de fatores que impactam as métricas presentes nessa camada.

Isso quer dizer que para mudar essa métrica você tem **menos controle e independência**, mas ela tende a ser um reflexo de um conjunto de métricas de controle (de diversos times e contextos). Se as suas métricas de controle vão mal, sua métrica de influência dificilmente vai melhorar.

Essa camada é importante porque ela conecta iniciativas, elas contam uma história sobre como as iniciativas estão indo e se correlacionando (a sua métrica está boa, mas o **todo** não funciona)

Seguindo o nosso exemplo, podemos pensar em quanto tempo leva para rodar nosso [CI (Continuous Integration)](https://www.atlassian.com/br/continuous-delivery/continuous-integration).

Diferente dos testes, existem mais coisas envolvidas que não necessariamente estão no nosso controle. Isso significa que o tempo **total** que o CI leva para passar, tem diversas influências (etapas diferentes do processo que mudam esse tempo) e fatores que podem gerar uma variação nesse tempo.

Melhorar o tempo dos testes **pode** ajudar no tempo do CI, mas se algum outro processo estiver ruim (o tempo que leva para criar a imagem por exemplo) ainda assim o **todo vai estar ruim.** Como time nós podemos olhar para o recorte do tempo que os testes rodam (**controle)**, mas o tempo total é composto de muitas outras métricas (**influência).**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280423431/9TveBLLOS.png)

Imagem baseada no CI para demonstrar como existem muitas relações e influências na relação do tempo total de execução

### Camada de interesse

É aqui que a gente tá querendo chegar! Todas as camadas se combinam para chegar em algo maior, uma métrica muito distante da sua realidade, mas invariavelmente **muito importante.** Aqui existem muitas influências, de áreas diferentes, de times, domínios, contextos que criam e compõem um resultado final.

No nosso exemplo, nenhuma das outras métricas é “realmente” importante. Eu não quero testes mais rápidos, nem um CI com tempo menor, eu quero um **Tempo de espera entre mudanças(Lead time for changes)** menor!

O que acontece é que o “Lead time for changes” é uma métrica distante da nossa realidade, ela é influenciada por muitas outras (tempo de execução do CI, Review, Testes, Deploy), a independência do time em atacar outros pontos pode até ser menor (o deploy não está no nosso escopo por exemplo), e com isso se torna fácil ficar perdido sem ver esse número se mexer quando você realiza uma entrega.

Daí vem a importância dessas camadas: elas vão te ajudar a ter uma resposta mais rápida sobre as suas decisões (eu deveria estar dando prioridade para isso agora?) e ainda assim garantir que o resultado final é o que você está buscando. A dica é:

Foque na camada de **controle**, observe a **influência** e entenda o impacto que elas têm na camada de **interesse**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280424885/8gqQ_vD_L.png)

Mas entenda que isso é complexo, porque as camadas e as métricas vão interagir formando o que podemos chamar de árvore de métricas do seu negócio. E normalmente, você vai atuar numa métrica de controle **bem distante do topo** (considerando que o topo é o que faz a sua empresa ganhar dinheiro).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280427113/62wtPLAop.png)

### Revisitando as métricas

Depois dessa aula que o [Eduardo Magalhães](https://www.linkedin.com/in/dudumagalhaes/) nos ofereceu, nós tivemos a oportunidade de revisitar nossas definições e conectar melhor as coisas. Vou deixar abaixo um exemplo de como passamos a refletir sobre as métricas do time:

Pequeno contexto: capybot é nossa ferramenta interna que usamos atualmente para ajudar no deploy do nosso monolito. Mas outro dia eu conto essa historia melhor :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280429155/Nz-TmTB7d.png)

Desenho de uma pequena arvore de métrica no contexto do time de developer experience

As [DORA Metrics](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) sem dúvida são um objetivo a ser alcançado, mas nossas iniciativas precisam de uma resposta mais rápida (estamos acertando no que estamos fazendo?) sem perder de vista o objetivo maior (tornar a nossa engenharia realmente mais produtiva) e gerando retorno para a empresa ([MRR](https://conteudo.movidesk.com/o-que-e-mrr/) ainda maior, ou seja, ganhar mais dinheiro).

E como a gente passou por isso, separei algumas dicas para você:

*   A camada de controle tem de estar perto de você. Se a sua iniciativa/tarefa/projeto não está mexendo diretamente nesse número, provavelmente tem alguma outra métrica que você deveria estar olhando. Vale refletir também se a métrica que você escolheu é “a certa” ou se a iniciativa deu certo. Isso é possível de ser feito de maneira mais rápida, pois esses fatores estão dentro do seu controle.
*   As métricas contam uma história, mas não pode ser a **única história**: Feedback dos seus usuários é essencial para te dar clareza se você está acertando nas escolhas que está fazendo.
*   Você vai errar de primeira, na segunda vai errar só um pouco menos — A dura realidade é que esse é um processo iterativo, nós estamos a cada semestre melhorando nossas métricas e para quais pontos estamos olhando, o importante é ter clareza do **motivo** que te levou a olhar para esses números. O objetivo desse framework é te ajudar a errar de maneira mais enxuta e com aprendizados, assim o seu erro tem um custo menor (comparado com só sair fazendo as coisas sem uma métrica de feedback rápido) e te dá um retorno maior (já que mesmo quando existe um erro na hipotese, você consegue criar um aprendizado relevante).

### Fechando o papo

Para encerrar nosso papo sobre as métricas, nosso time cresceu muito, desde que ele foi fundado, nesse aspecto. Estamos melhorando, ajustando e revisitando essas métricas a cada semestre, de modo a entender quais são as métricas que queremos estar **controlando** e **influenciando** e como elas **se desdobram no nosso interesse que são** as DORA Metrics.

E qual tem sido o desafio para você medir na sua empresa? Manda nos comentários e bora trocar uma ideia sobre isso.

Estamos desenvolvendo coisas incríveis para nossos clientes aqui na RD Station. Se você se preocupa com estabilidade, escalabilidade e quer grandes desafios, [estamos contratando](https://grnh.se/44d568232us)!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670280431024/NuL9UaKCj.gif)