---
title: Application Load Balancer
output: post
---

O Application Load Balancer (ALB) opera na __camada sete do modelo OSI__. Isso quer dizer que depois que o balanceador recebe um request, ele avalia suas regras em usando as prioridades configuradas para determinar qual regra ele deve aplicar, e então seleciona o um recurso do grupo para encaminar a solicitação. 

É possível configurar as regras que encaminham as solicitações para diferentes grupos baseado no conteudo da solicitação. O Roteamento é feito de forma independente, mesmo quando um alvo está registrado com multiplos grupos. Essas regras podem ser configuradas como path ou host-based. Isso quer dizer que você pode encaminhar, por exemplo http://cloudwise.com.br/post para um grupo e http://contato.cloudwise.com.br para outro.

Se você usa HTTPS, usar o ALB também pode ser muito util, como existe a possibilidade de configurar o roteamento das conexões via por host, o ALB permite que você configure multiplos listeners e para cada listener em HTTPS, você pode colocar um certificado diferente. Se fosse estivesse usando um CLB, nesse momento seria necessário criar um novo CLB.

Uma outra grande diferença é no health check, usando ALB você pode configurar o código que você esperar de retorno da sua aplicação, ou seja se sua aplicação retorna 200, 301 e 302 por exemplo, você pode configurar o ALB para aceitar esses códigos e marcar a instancia como saudavel, caso receba esses retornos. 

Além de EC2, você também pode relacionar um App load balancer com outros serviços como, ECS, WAF e também Lambda, ou seja, você pode invocar um lambda para cada conexão que chegar no balanceador.

Por enquanto é isso, no próximo post vamos tratar de Network Load Balancer.

Obrigado e até mais.