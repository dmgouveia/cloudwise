# Monolitos & Microserviços.

Ultimamente temos presenciado uma virada nas aplicações moniliticas e frequentimente temos visto desenvolvedores utilizarem microserviços para suas aplicações. Mas o que realmente é cada coisa?

https://3ovyg21t17l11k49tk1oma21-wpengine.netdna-ssl.com/wp-content/uploads/2017/08/the-dangers-of-a-monolithic-app.jpg

## Aplicações monoliticas.

https://gautambiztalkblog.files.wordpress.com/2016/06/image1.png

Uma aplicação monilitica nada mais é do que uma solução que está instalada em um único local, por exemplo, um serviço web que serve várias páginas web. No exemplo abaixo por exemplo, temos a representação de um serviço web que prove o serviço web para vários negócios. 

https://gautambiztalkblog.files.wordpress.com/2016/06/image2.png

Um dos problemas desse tipo de solução é sua dificuldade de atualização, ou seja, se o desenvolvedor fez uma alteração que afeta só os produtos desse site, provavelmente teremos que deixar indisponivel todo o site e não só aquela sessão. 

Não só isso, cada alteração de uma parte desse site terá que ser testado por todos os outros segmentos, para validar que as alterações da sessão de inventário, por exemplo, não está afetando a operação da sessão de produtos.

Um outro desafio que temos com esse tipo de solução é como administrar o workload de cada parte do site.
Vamos supor que estamos em Black Friday e temos uma grande demanda dos clientes acessando o site para fazer suas compras, da forma que está, vamos ter que aumentar a capacidade dos serviços do serviço web, consequentemente, vamos aumentar a capacidade de todo o site, e não só a sessão de promoções por exemplo.

Além disso, em aplicações moniliticas, geralmente precisamos fazer o scale-up, geralmente adicionando CPU, memória e disco. Apesar de hoje a utilização de serviços em Cloud permitirem esse tipo de escala de forma fácil, o aumento de CPU e memória ainda tem um limite. 

## Microserviços

https://gautambiztalkblog.files.wordpress.com/2016/06/image3.png

O conceito de microserviço é ter componentes independentes que trabalham em conjunto para entregar a funcionabilidade da aplicação. Cada microserviço geralmente é encapsulado em simples e especificas funções. Esse componentes podem escalar e também ser atualizado de forma independente.

## Stateless X Stateful

https://www.xenonstack.com/images/insights/xenonstack-what-are-stateful-and-stateless-applications.png

### Stateless 

Em aplicações stateless, o banco de dados é externo ao serviço, geralmente em um banco SQL ou NoSQL como DynamoDB ou MongoDB. 

O serviço em sí pode ser stateful, ou seja, no caso do exemplo do serviço web, você quer guardar a sessão do seu cliente mas geralmente faz isso fora do serviço web, em um banco de dados em memória ou em um sistema de dados compartilhado.

Uma das vantagens de guardar a sessão fora do serviço web é que em um momento que seja necessário fazer o scale-in ou scale-out da sua sessão, não vai importar qual nó ou servidor web o seu cliente vai fazer a requisição, porque o serviço web vai consultar se a sessão ainda que está guardado externo ao serviço web.

https://miro.medium.com/max/1400/1*HJbZhbowe_wlrblyqwTlUQ.png

### Stateful

Em uma solução stateles, adicionamos um tempo de resposta devido a consulta a um serviço externo. Talvez essa latencia não seja aceitavel na sua aplicação. Nesse caso talvez seja melhor trabalhar com uma aplicação stateful. 

Geralmente serviços como back ends de jogos, bancos de dados como serviço ou qualquer outro cenário que é necessário de uma resposta de baixa latencia, são exemplos de aplicações que mantém informações dentro do seu serviço.

Um desafio em aplicações stateful é quando precisamos escalar o serviço. Pois cada vez que vamos fazer o scale-out dessa aplicação precisamos replicar as informações que estão no serviço para o novo nó. Essa funcionalidade geralmente depende de um banco de dados externo para gerar as replicas do status do serviço.
