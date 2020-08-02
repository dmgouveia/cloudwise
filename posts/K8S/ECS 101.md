# Virtualização VS Contêiners

Hoje vamos falar um pouco sobre contêiners e virtualização.

Um contêiner é um ambiente isolado preparado para rodar uma aplicação, que é lançado em um sistema operacional usando um sistema de container como o Docker. Geralmente dentro de um container você vai encontrar a aplicação e também sua dependencias e bibilioteca.

Ao contrário de um contêiner, as máquinas virtuais (VMs) executam um sistema operacional completo, incluindo o próprio kernel. 

https://images.contentstack.io/v3/assets/blt300387d93dabf50e/bltb6200bc085503718/5e1f209a63d1b6503160c6d5/containers-vs-virtual-machines.jpg 

---

## Diferenças

Executar seu kernel e S.O. tem suas vantagens, e a segurança do isolamento é uma das principais vantagens.
Da perspectiva do Hypervisor não tem como saber o que está sendo executado dentro de uma máquina virtual, porém como os containers compartilham o mesmo Kernel, existe um chance de que um container ganhe acesso ao sistema operacional do servidor. Com VMs isso é mais dificil de acontecer porque o kernel não é compartilhado, o que reduz a chance de um ataque desse tipo.

## Quando usar contêiners?

Contêiners são uma boa opção para uma grande gama de aplicações, entre elas:

### Tempo de inicialização

Contêiners são geralmente lançados em alguns segundos, enquanto uma VM pode levar alguns minutos. Isso pode ser importante principalmente em um momento que você precisa escalar sua aplicação de forma rápida.

### Utilização de recursos.

Como contêiners compartilham recursos com o sistema operacional, eles precisam de menos requisitos quando comparado com VMs. Um contêiner geralmente precisa de menos espaço em disco, menos RAM e CPU. Por causa disso geralmente você consegue executar várias aplicações simultaneas em um unico host.


## E porquê eu devo considerar VMs?

Como já foi dito acima, VMs tem um nivel de segurança a mais no nivel de isolamento entre as máquinas virtuais e o Hypervisor. Isso porque cada sistema operacional tem seu próprio Kernel. Por essa razão VMs são mais seguras que contêiners.

Atualmente a maioria dos Hypervisors conseguem que você inicie máquinas virtuais rodando qualquer sistema operacional, ou seja, você consegue lançar uma máquina virtual Linux em um Hypervisor Windows e vice-versa.
Um contêiner não tem essa portabilidade, apesar de que você poder lançar uma máquina Debian e um contêiner em Ubuntu, você não consegue lançar uma contêiner Linux em um Docker Windows e/ou vice-versa.

### Rollback

A grande maioria dos sistemas de virtualização é possível fazer um snapshot da máquina virtual e se algo der errado, é possível retornar a máquina a um estado anterior.

Contêiners não oferecem essa opção. Você pode fazer o roll back de uma imagem porém na maioria dos casos os contêiners guardam dados fora da imagem e por isso fazer um roll back de uma imagem não vai ajudar a recuperar dados perdidos durante um problema.

## E qual devo usar?

Existem diversas outras diferenças entre os dois sistema, por isso é dificil dizer se é melhor usar contêiners ou máquinas virtuais. A maioria das empresas acabam optando por um ambiente misto, ganhando as vantagens de ambos os ambientes. É importante também levar em consideração o ambiente humano, o time de TI que vai administrar o ambiente precisa saber o básico de ambos os ambientes, principalmente para saber o que esperar de cada um devido as suas limitações.