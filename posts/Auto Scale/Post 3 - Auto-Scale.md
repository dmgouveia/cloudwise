Após a configuração de auto scaling, a Amazon vai tentar lançar os recursos que você configurou. Vale a pena lembrar que a Amazon sempre vai tentar equilibrar o lançamento das instancias, ou seja, se você selecionou 3 AZs e o desired capacity para 12 instancias, se tudo estiver correto você deve ter 4 instancias por AZ. 

Não da pra afirmar que sempre será assim, pois a cada momento que você lança uma instancia, vários fatores podem fazer que essa ação falhe. A Subnet daquela AZ pode estar sem IP, a AWS pode não ter hardware disponivel para lançar sua instancia ou até uma falha maior na propria AZ. Por isso mantenha em mente que nem sempre vamos ter esse balanceamento entre AZs, mas o auto scaling vai tentar sempre equilibrar a quantidade de instancias por zonas.



So now I'm going to edit this again. I'm going to set the desired capacity to three and save it now, because the desired capacity is three what that's going to do is attempt to start on additional EC2 instance, it always attempts to maintain the desired number of instances that you've set on an Auto Scaling Group and attempts to do so where there was instances distributed across all of the subnets that you've configured. 

A gente falou no post anterior sobre health checks, o health check é usado nesse caso para que o ASG identifique se todas as instancias estão funcionando conforme o esperado, caso uma instancia tenha algum problema, por exemplo se o serviço web parar de funcionar, o ASG vai marcar essa instancia como unhealthy e então lançar uma nova instancia. Você pode revisar o termination policies para identificar a forma que o ASG vai terminar os recursos. 

Uma outra opção para escalar o ambiente mais rápido é através do steps. Imagina que se você está acompanhando a quantidade de conexões para lançar mais instancias e você configurou que quando tivermos 200 conexões, lançaremos uma instancia. Vale lembrar que quando lançamos essa instancia, o ASG entra em um periodo de cool down antes de lançar mais uma. Porém se a quantidade de conexões sairam de 200 para 20000, talvez você queria lançar 5 ou 10 instancias de uma vez, para isso você também pode configurar a opção de steps. Com o steps, você pode tanto fazer o scale-out ou scale-in mais rápido. 

Now monitoring is provided as part of auto scaling groups will be injecting auto Scaling Group specific metrics into CloudWatch logs. You are able to enable group metric collection for a given auto scaling group, and that allows EC2 to send information into CloudWatch that defines the auto scaling group as a group.

Você também pode utilizar o CloudWatch para monitorar o seu auto scaling group. Por exemplo, você criar dashboards para acompanhar a quantidade de instancias saudaveis dentro do ASG e então enviar notificações caso você chege em um numero critico. 

Espero que você tenha gostado dessa revisão sobre auto scaling groups da AWS.

Mais um vez, obrigado pela visita!