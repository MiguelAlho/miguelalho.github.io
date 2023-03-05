---
title: Configurar o gateway no Linux
author: Miguel Alho
type: post
date: 2009-09-14T18:10:02+00:00
url: /configurar-o-gateway-no-linux/
categories:
  - 'Code &amp; IT'
tags:
  - gateway
  - Linux
  - rede
  - route
  - router

---
Estive preso com um problema de configuração do LAMP server &#8211; conseguia conectar-me às interfaces web através da rede interna, mas não pelo exterior (ou domínio), usando o NAT do _router_ para encaminhar correctamente. Na própria máquina, conseguia pingar a endereços internos mas não a externos. O problema? Ao mudar de um endereço IP dinâmico (DCHP) para um estático, o _gateway_ não ficou definido. Confirmo na lista dada pelo comando &#8220;route&#8221;, que o _gateway_ não existe. A solução?

<pre>route add default gw 192.168.0.1 eth0</pre>

ou seja, adicionei o endereço do _router_ como _default gateway_ à lista de rotas para a placa de rede eth0.

Alguma ajuda daqui: <http://serverfault.com/questions/65206/fowarding-http-to-lamp-server-through-router-nat>  
e daqui: [http://www.linuxhomenetworking.com/wiki/index.php/Quick\_HOWTO\_:\_Ch03\_:\_Linux\_Networking][1]

 [1]: http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch03_:_Linux_Networking