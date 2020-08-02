---
title: Network Load Balancer
output: post
---

Nesse post vamos falar um pouco de Network Load Balancer na AWS.

Esse tipo de balanceador opera na camada quatro do modelo OSI, ou seja, é um balanceador que operada na camada TCP. Isso é importante pois como o balanceador não precisa chegar até a camada sete, ele tende a responder mais rapido as solicitações do que o ALB.

Além disso, diferente do CLB e do ALB que só é possível chegar nele através do nome, com o NLB é possível acessar pelo IP do balanceador.

Uma outra importante diferença é como cada tipo de balanceador lida com a criptografia. No ALB o cliente estabelece uma conexão com o balanceador, essa é uma conexão que termina no balanceador e então o balanceador faz uma outra conexão para o destino, dependendo da configuração do listener. 

Logo, se você tiver configurado HTTPS na porta 443, você vai ter uma conexão entre o cliente e o load balancer na porta 443 e então o balanceador vai fazer uma nova conexão até o destino. Se você tiver configurado o forward rule em HTTP na porta 80, então o balanceador vai fazer o offload do SSL. Se você tiver feito o forward rule em HTTPS, logo o load balancer vai fazer uma nova conexão usando esse protocolo.

Veja que usando ALB, não existe em nenhum momento, uma unica conexão entre o cliente e o servidor.

Com o NLB, como ele trata os pacotes na camada 4 do modelo OSI, o balanceador nada faz com a criptografia, ele simplesmente analisa o cabeçalho TCP, determina a informação do endereçamento e encaminha a conexão para os destinos sem nenhuma modificação. Logo esse é o unico balanceador na AWS disponivel nesse momento que permite uma conexão ininterrupta. 

Nessa URL você terá acesso a comparação entre os tipos de balanceadores, para uma visão geral. 
https://aws.amazon.com/elasticloadbalancing/features/#compare 

É isso pessoal, muito obrigado e até a próxima.