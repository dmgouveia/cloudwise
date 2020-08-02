Hoje vamos falar um pouco sobre load balancers. Para ajudar na explicação, vou usar a arquitetura abaixo.

[Imagem aqui]

Essa é um exemplo de uma arquitetura qu você deve encontrar frequentimente. Nesse caso especifico temos uma unica VPC dentro de uma região da AWS. Dentro dessa VPC temos duas regiões e dentro de cada região temos duas subnets, uma privada e outra publica. Vamos suporte que nesse exemplo cada instacia está rodando um servidor que está servindo uma página web.

Essas duas instancias estão dentro de uma auto scaling group que pode lançar novas instancias dentro das duas diferentes zonas de disponibilidade. Temos também um load balancer que está entre os usuários e os servidores. Não se preocupe que vamos falar de load balancer em outro momento.

Geralmente damos o nome de elasticidades para ambientes que podem aumentar e diminuir de tamanho conforme a carga de trabalho. A função do auto scaling é exatamente essa, te ajudar a aumentar ou diminuir seu ambiente a partir de métricas que você configura. 

Antes da gente falar de auto scaling, acho que precisamos falar de provisionamento. Quando você lança uma instancia, a não ser que você utilize uma já pre-configurada ou do marketplace, ela geralmetne só vem com o sistema operacional e com alguma opção para você acessar a instancia. Se for linux, SSH ou Windows RDP habilitado. Logo, para você servir páginas web a partir de uma instancia, você precisa configurar essa instancia, usando apache ou nginx por exemplo. 

Nesse caso, se você tem uma unica instancia, você só precisar acessar ela e instalar, porém esse processo não é escalavel, ou seja, se o auto-scaling group lançar instancias, você não quer ter que entrar em cada instancia para configura seu servidor web. Você vai querer que o auto-scaling faça o lançamento desses recursos já configurados. 

Uma das opções é, a partir de uma instancia já configurada, você pode criar uma AMI e utilizar essa AMI para fazer o lançamento de mais instancias usando o auto-scale. Essa solução funciona, porém ela não é facil de dar manutenção, pois, se por exemplo, saiu uma nova versão do seu servidor HTTP ou você precisar modificar alguma coisa na configuração das máquinas, você precisa lançar uma instancia em separado, fazer as atualizações necessárias, criar uma nova AMI e reconfigurar o auto-scaling group. 

Uma outra opção é utilizar o bootstrap disponivel, que no caso de EC2 é o que a AWS chama de user data. 
Quando você provisiona uma instancia, você pode informar um script para que a instancia execute esse código após a AWS fazer o lançamento do EC2. Esse é um exemplo user data que você pode usar para instalar o apache.

```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www
```

Esse processo é totalmente diferente do que criar uma AMI, pois ele se torna mais flexivel para atualizações, pois se for necessário fazer alguma modificação nas instancias, você pode simplesmente modificar o user-data e lançar novas instancias. Porém, tudo sempre vem com um lado negativo, e nesse caso a instancia deve levar mais tempo para terminar de ligar, pois além de lançar a instancia, temos que esperar o sistema operacional executar cada passo que foi solicitado no user-data.


Por isso, se você tiver que escolher entre AMI customizadas ou user-data, lembre-se que com user-data você ganha flexibilidade, porém você perde performance se comparado com AMI customizada. 

Por enquanto é isso, nos próximos posts vamos falar mais de auto-scaling.