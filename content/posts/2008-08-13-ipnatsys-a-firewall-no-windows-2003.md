---
title: 'IPNat.sys & a Firewall no Windows 2003'
author: Miguel Alho
type: post
date: 2008-08-13T15:27:52+00:00
url: /ipnatsys-a-firewall-no-windows-2003/
categories:
  - 'Code &amp; IT'
tags:
  - firewall
  - IPNat
  - Server
  - windows 2003

---
Um problema de hoje &#8211; estava a tentar reconfigurar o BackupPC para efectuar backups de clientes Windows. Neste caso, decidi experimentar o rsyncd em vez do Samba. No entanto enquanto tentava abrir a FireWall do windows, para abrir uma excepção, surgia a seguinte mensagem:

<pre>O Firewall do Windows não pode ser executado pois está sendo executado outro programa ou serviço que pode usar o componente de conversão de endereço de rede (Ipnat.sys)&lt;/pre&gt;

Fartei-me de ligar e desligar serviços e mesmo parar o Ipnat via o commando 

&lt;pre&gt;sc stop ipnat</pre>

sem sucesso. Finalmente encontrei <a href="http://techrepublic.com.com/5208-6230-0.html?forumID=102&#038;threadID=208530&messageID=2555741" target="_blank">este post num forum</a> que resolveu. Bastou desligar o &#8220;Encaminhamento e Acesso Remoto&#8221; em _Meu Computador -> Gerir -> Serviços e Aplicações_ para voltar a ter acesso à Firewall.