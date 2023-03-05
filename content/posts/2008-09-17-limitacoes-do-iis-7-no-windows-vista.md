---
title: Limitações do IIS 7 no Windows Vista
author: Miguel Alho
type: post
date: 2008-09-17T13:02:34+00:00
url: /limitacoes-do-iis-7-no-windows-vista/
categories:
  - 'Code &amp; IT'
tags:
  - desenvolvimento web
  - IIS 6
  - IIS 7
  - Windows Vista
  - XP

---
É pelo menos a segunda vez que tenho que enfrentar problemas de limitações do sistema operativo, e é nesta altura que mais detesto a Microsoft e o seu sistema operativo. Mas enfim, são as regras do jogo que nos impõe. E é nesta altura que gostava que o mundo fosse o Ubuntu.. hehehe

Desta vez, no desenvolvimento de uma aplicação que faz grande uso de serviços, e até encadeamento de acessos. Em termos de produção, trata-se de um sistema distribuído, mas todo o desenvolvimento é efectuado numa só máquina, com os diversos serviços integrados em directorias virtuais com se fossem servidores distintos. Uma abstracção que permite-me ter alguma flexibilidade no desenvolvimento da(s) aplicação(ões). 

Esta abordagem tem, no entanto, problemas associadas. Cada chamada a um serviço é uma chamada na contagem, e porque o processo exige pedidos síncronos, o anterior tem que aguardar pela resposta do seguinte. Após horas a batalhar com a questão e após um esforço grande de logging, encontrei o local do problema &#8211; uma operação que teoricamente é simples e rápida, mas que exigia um pedido a um serviço. O tempo de execução entre a chamada anterior e a seguinte era sempre de 1m30s. Só depois deste periodo (em que a aplicação dava um timeout, nas camadas superiores) é que os pedidos em espera eram executados (quase instantaneamente).

O giro disto tudo, e o que me levou a desconfiar do IIS / Vista foi o facto de este problema surgir apenas com o acesso à aplicação via IIS. Usando o servidor de desenvolvimento do Visual Studio, o pedido passava sempre, sem problemas. Detectado o ponto de falha, e com este conhecimento da potencial falha, um pouco de pesquisa no Google apresenta a causa. IIS 7 em Vista Home Premium.

O Home Premium admite sevrir páginas e desenvolvimento para aplicações simples, sem dúvida. E até agora só encontrei dois problemas com ele em termos de desenvolvimento. Mas para algumas aplicações mais complexa como o caso da minha torna-se problemático e é muito díficil encontrar o bicho que causa o problema.

Em <a target="_blank" href="http://www.stuffthatjustworks.com/2008/02/25/LimitationsOfIIS7OnVista.aspx">Stuff that Just Works, de Cal Zant</a> há uma página que apresenta o conjunto de limitações que as versões do Windows apresentam. Com o XP Pro, e IIS 6, a limitação era de 1 site e 10 ligações concorrentes. O XP, versões básicas, não permitiam o IIS. Com o vista, mais ou menos a mesma situação surge &#8211; para as versões Business e superiores, o numero de ligações concorrentes são ilimitadas, mas passou a haver limite no numero de pedidos simultâneos, e a restrição do número de sites desapareceu. 

Já com o Home Basic Premium, que é o unico dos Home a suportar o IIS, a intenção é suportar as necessidades de desenvolvimento casual e portanto entram as limitações &#8211; processamento de três pedidos em simultâneo e sem o suporte a configurações de segurança avançadas &#8211; exactamente os dois pontos problemáticos para mim&#8230; 

Em termos de mudança relativo ao IIS 6 no XP Pro, enquanto neste era possível que apenas 10 utilizares dessem inicio a sessão (mas podendo fazer quantos pedidos quisesse), com o IIS 7 em Vista, podem estar ligados quantos quiserem, mas apenas 10 (ou 3) pedidos estarão a ser processados em qualquer momento e as restantes permanecem em fila de espera o que permite que os utilizadores se mantenham ligados à aplicação, mas eventualmente com demora na resposta. No meu caso, como os 3 em execução estavam dependentes de um 4 que estava em espera, apenas o timeout do primeiro permitiu a continuação da execução.

Acredito que sejam limitações impostas pela Microsoft para de alguma forma puxar as empresas a comprarem as versões de servidor (em termos de ambiente de utilização) e para o programador mover-se para um SO &#8220;mais completo&#8221;. Mas que são situações chatas (e caras), lá isso são!!!

Em IIS.Net, há ainda a <a target="_blank" href="http://learn.iis.net/page.aspx/479/iis-70-features-and-vista-editions/">lista completa das diferenças</a> do IIS entre versões do Vista&#8230;