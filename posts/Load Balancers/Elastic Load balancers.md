Elastic Load balancers

Esse é o primeiro post de uma série sobre Elastic Load Balancers (ELB). 
Um load balancer aceita trafego de clientes e faz o roteamento desses requests para alvos. Na AWS, o uso mais comum é ser feito o balanceamento de trafego para instancias EC2 em uma ou mais AZs.

O load balancer também monitora a saúde dos seus alvos e quando é detectado um alvo que não está saudavel, ele para de enviar requests para esse alvo. 

A AWS tem atualmente três tipos de load balancers:


* Application Load Balancers (ALB)
* Network Load Balancers (NLB)
* Classic Load Balancers (CLB)

Existe algumas diferenças de como esses load balancers funcionam e como são configurados. Uma delas é que tanto o ALB e o NLB, os alvos são registrados em grupos e faz o roteamento das requisições para esses grupos. Com CLB você registra as instancias direto no load balancer.

** Load balancers e seus nós

Para cada AZ que você habilitar o load balancer, a AWS cria um nó dentro dessa AZ. Se você tiver instancias dentro dessa zona mas não tiver habilitado aquela zona no seu load balancer, esses alvos não deveriam receber trafego. Existe uma função chamado cross-zone load balancing que vou explicar mais a frente. 

O ideal é que para cada zona que você habilita o load balancer, você tenha pelo menos uma instancia. A recomendação é que você habilite multiplas AZs no seu load balancer, e para alguns tipos como no ALB, é mandatório que você habilite multiplas AZs. O motivo disso é que se houver uma falha em uma AZ, o load balancer pode continuar enviando trafego para as outras AZs se elas tiverem destinos saudaveis. 

** Cross-zone Load Balancing.

Como já falei, os nós do load balancer distribui requisições dos clientes para os alvos registrados. Com o cross-zone, cada nó do balanceador distribui trafego entre todos os alvos em todas as AZs e quando o cross-zone está desativado, cada nó distribui trafego só para os alvos registrados em sua AZ.

O diagrama abaixo mostra o efeito de ter essa função habilitada. Nesse cenário temos duas AZs habilitadas com o load balancer. Nesse caso temos as instancias distribuidas de forma desigual, com 2 instancias na zona A e 8 na zona B. Quando o cliente envia um request, o DNS vai responder os requests com o IP de cada nó, dessa forma cada nó vai receber 50% do trafego vindo dos clientes, e cada load nó do balanceador vai distribuir esse trafego entre os alvos registrados. 

Se o cross-zone está habilitado, cada alvo vai receber 10% do trafego. Isso porque cada nó do balanceador vai distribuir sua carga entre todos os alvos. Porém o trafego que sair da AZ-A para os alvos na AZ-B será cobrado.

[Balanceamento Igual Cross-AZ.jpg]

Se o cross-zone estiver desabilitado, os dois alvos na zona A devem receber 25% do trafego cada uma, enquanto os oitos alvos na zona B recebera 6.25% do trafego. Porém, como cada nó só vai falar com as instancias que estão registradas dentro da sua AZ, não haverá a cobrança de trafego entre AZs.

[Balanceamento desigual.jpg]

Por enquanto é isso, de uma olhada nos próximos posts que vamos explorar um pouco mais cada opção de load balancer.

Obrigado e até mais.