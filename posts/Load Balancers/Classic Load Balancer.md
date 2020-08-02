Nesse post vamos falar de Classic Load balancers. 

Antes de mais nada, esse modelo de balanceador é um modelo antigo, que tem algumas limitações quando comparado com o network load balancer (NLB) ou Application load balancer (ALB).

Apesar disso, usar o CLB tem algumas vantagens comparada com ALB:

* Suporte a EC2-Classic.
* Suporte a TCP e SSL listeners
* Suporte a sticky sessions usando os cookies gerados pela aplicação.

Quando você lançar um CLB, você precisa selecionar qual VPC esse balanceador vai ficar, além de também informar se é um um balanceador interno ou não, se você selecionar essa opção, o balanceador não vai ter acesso a internet. 

[DefineCLB_LBName]

Na mesma página inicial, será necessário criar o listener. O Listener nada mais é do que uma relação de 1 para 1 entre a porta e protocolo que o balanceador vai receber o pacote do cliente e para qual porta e protocolo o balanceador deve encaminhar o pacote.

Esse é um dos limitadores do CLB quando comparado com ALB. No CLB, usando o site https://cloudwise.com.br de exemplo, eu não consigo configurar que https://cloudwise.com.br/blog seja encaminhado a um destino e https://cloudwise.com.br/contato seja encaminhado para outro, pois o CLB não opera na camada 7 do modelo OSI.

Após a configuração do listener, você precisa selecionar pelo menos umaa subnets onde esse CLB vai operar e então configurar o security group (SG). Esse SG é aplicado somente aos pacotes que tem como destino o CLB, ou seja, praticamente todo trafego que for entrar nesse balanceador vai passar por esse security group.

Quando for configurar health check, você tem algumas opções, entre elas HTTP, ou seja, o CLB vai fazer um request nas instancias que você tiver registrado e se uma delas não responder no tempo configurado, essa instancia vai ser marcada como unhealthy e o balanceador vai parar de enviar trafego para ela.

No CLB você pode registrar diretamente quais instancias EC2 devem fazer parte do pool, porém se você tiver um auto scaling group configurado (ASG), você pode deixar essa opção em branco, pois você pode configurar que toda instancia lançada pelo ASG seja registrada no CLB.

Uma outra opção de configuração que você deve ter em mente é se sua aplicação é stateless ou stateful, pois se for stateful, talvez você precise configurar a função de sticky session do CLB, dessa forma quando for estabelecido uma sessão, o balanceador vai sempre te encaminhar para esse servidor.

Outra função do CLB é que você pode configurar HTTPS nele, logo a comunicação entre o cliente o e CLB pode ser feito com HTTPS e o CLB pode fazer a comunicação com suas instancia em HTTP. Isso quer dizer que todo o overhead que suas instancias teriam com a criptografia, será transferiado para o load balancer, além de que, se você configurar HTTPS no CLB, nesse caso você só precisa de um certificado e não será necessário ter um certificado por instancia. 

Por enquanto é isso, até o próximo post!

