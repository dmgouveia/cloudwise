Quando a gente fala de usar auto scaling group, você precisa também configurar aonde essas instancias serão lançadas, tamanho, Keypair, security group, etc. Para passar essas configurações, existem dois metodos, usando launch configuration ou launch template. Vamos abordar as duas formas nesse post.

O Launch Configuration é a forma legada de se passar esses parametros para o auto-scaling group. Quando você cria um launch configuration, ele não vai poder ser modificado, ou seja, é um modelo imutavel, se você resoler, por exemplo, mudar o tamanho da instancia, será necessário criar um novo launch configuration.

[Imagem Launch Configuration 1]

Quando você for criar um launch configuration, será necessário colocar um nome, selecionar qual AMI você quer usar, no caso da imagem acima estou selecionado a AMI publica de Amazon Linux 2. Na sequencia, é necessário selecionar a familia da instancia. Nesse caso estou escolhendo a menor instancia possível só para efeitos de demonstração.

Na sequencia você pode configurações adicionais, é aqui que você pode configurar o launch configuration para utilizar spot instances, profile de IAM entre outras coisas. Um parametro importante de se configurar nesse momento é o user-data que falamos no post anterior. Para encontrar o user-data você precisa clicar em advanced details.

[Imagem Launch Configuration 2]

Na sequencia você ainda precisa customizar, se quiser, os discos que serão lançados junto da instancia, selecionar o security-group que será asignado a instancia e por ultimo o key pair para login na instancia. 

Feito isso só clicar em Creata launch configuration.

Uma dica, caso você esteja estudando para a prova de SA Professional, quando falamos em auto-scaling, toda configuração relacionada a instancia, será configurado no launch configuration ou launch template.

Como eu já comentei, launch configuration é a forma mais antiga de se configurar os parametros das instancias lançadas pelo auto-scaling. Atualmente a forma recomendada pela AWS é a utilização de launch templates. A configuração de um launch template é similar ao do launch configuration, você vai precisar das mesmas informações basicas, porém você pode passar alguns parametros mais avançados.

[Launch Template 1]

A primeira mudança entre o launch configuration e o launch template é que no launch template você pode configurar tags para o launch template além de usar um outro template como modelo, se você usar um outro template de modelo, ele vai popular alguns campos abaixo baseado no seu modelo. No nosso caso não vamos mudar nada ali.

[Launch Template 2]

Na sequencia, você precisa escolher a AMI, instance type e key pair, assim como no launch configuration.

[Launch Template 3]

Depois vamos passar os parametros de redes, aqui você deve utilizar VPC, EC2-Classic é um padrão de rede que é antigo e só deve ser usado se você tiver certeza do que está fazendo. Selecione também o security group que será associado a instancia. Como é mostrado na imagem você pode também alterar as configurações de disco da instancia.

[Launch Template 4]

Também é possível criar as tags para os recursos. Veja que essa tag é diferente da primeira, que é a tag do launch template. Você pode também adicionar mais ENIs se a instancia assim permitir.

Por ultimo, vamos ter em Advanced Details muitas outras opções de configuração, entre elas se você gostaria de usar Spot Instance, shutdown behaviour, IAM Profile e no final da página, entre outros parametros, você vai encontrar a opção para configurar o user-data. Quando estiver pronto, clice em create launch template.

Agora, lembra que eu falei que você pode modificar o launch template? Pois bem, com um template pronto você pode cliar em modify template.

[Launch Template 5]

Aqui o launch template vai estar preenchido com as informações que você configurou anteriormente e vai permitir que você modifique qualquer parametro. Ao terminar, só confirmar as modificações no final da pagina.

Veja que quando você modificar o template, você criou uma nova versão desse template, e se você achar necessário, pode escolher qual será a versão padrão quando você usar esse template.

[Launch Template 6]

Agora que temos o nosso launch template criado, vamos finalmente criar o auto scaling group.

A primeira parte você vai precisar passar um nome, escolher o template e a versão (se houver).

[Auto Scaling 1]

Na sequencia você pode optar por usar as opções já configuradas no launch template com relação a compra das instancias ou você pode combinar instancias on-demand e spot. Muito cuidado quando você for configurar a utilização de on-demand e spot, pois você pode perder as instancias spots caso o preço pelas instancias fique maior do que aquele que você está disposto a pagar. No meu caso, vou deixar para usar as configurações já passadas ao launch template.

Além disso, aqui você pode escolher a VPC e as subnets/AZs que você quer usar.

[Auto Scaling 2]

Na sequencia, você pode selecionar um load balancer para usar com as instancias que serão lançadas pelo auto scaling. Novamente aqui recomendo você usar Aplication Load balancer ou Network Load balancer. Classic load balancer é uma forma antiga que só deve ser usada se você tiver certeza do que você está fazendo.

Health Checks também é configurado nessa parte. Esse health check vai verificar se a instancia está saudavel, por exemplo que sua página web está sendo fornecida. Se o auto scaling identificar que a instancia não está saudavel, ela vai lançar uma nova instancia.

Você pode também habilitar o health check no ELB, isso vai fazer com que o load balancer cadastre essa instancia como uma das disponiveis para receber o workload. Fique atento com o grace period, pois se você estiver usando user-data, pode levar um tempo para que a instancia comece a responder os requests do health check.

[Auto Scaling 3]

No próximo passo você consegue dimensionar a quantidade de máquinas que o auto scaling pode lançar e também o scaling policy. Nesse caso por exemplo você pode colocar que se a CPU estiver acima de 50%, o auto scaling pode lançar mais instancias. Não se esqueça também de colocar quanto tempo a instancia precisa para que o auto scaling possa começar a coletar as métricas. 

Depois disso você pode configurar notificações, como por exemplo você pode enviar uma mensagem ao SNS para avisar cada vez que o auto scaling toma alguma ação e adicionar tags.

Com isso você tem um auto scaling group, com um scaling policy e usando launch template.