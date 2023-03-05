---
title: 'FATAL:  could not reattach to shared memory (key=…, addr=…): 487'
author: Miguel Alho
type: post
date: 2011-01-25T10:07:55+00:00
url: /fatal-could-not-reattach-to-shared-memory-key-addr-487/
categories:
  - 'Code &amp; IT'
tags:
  - base de dados
  - PostresSQL
  - Windows 2008 server

---
`FATAL:  could not reattach to shared memory (key=276, addr=01F20000): 487`

Este é um erro que aparenta aparecer em algumas instalações PostgreSQL em Windows, em especial depois de existir algum update do OS. Uma instalação minha teve este problema, com o erro a ser registado periodicamente nos logs do PostgreSQL. Infelizmente o erro é problemático para aplicações dependentes da base de dados, uma vez que ele interrompe conecções entre a aplicação e a base de dados. É causador de um tipico erro de .NET com mensagem pouco esclarecedora:

`[SocketException (0x2746): An existing connection was forcibly closed by the remote host]<br />
   System.Net.Sockets.NetworkStream.Read(Byte[] buffer, Int32 offset, Int32 size) +232`

Poderia ser de uma coneção externa, mas o pilha de chamadas tinha claramente a biblioteca NpgSQL &#8211; o _provider_ PostgreSQL para .NET na lista. Não era uma conecção entre cliente e servidor, mas sim entre a aplicação e o serviço de base de dados (que é efectuado sobre TCP, mesmo estando na mesma máquina).

A solução é a actualização da instalação do serviço de base de dados &#8211; este caso passei do 8.4.0 para o 8.4.6. (a correcção deve ter sido introduzido no 8.4.1). Esta versão terá o _patch_ para a correcção deste erro. A actualização é simples: para o serviço e correr o instalador mantendo os dados e configurações intactas (poderá ser necessário recolocar o serviço a arrancar pelo sistema utilizador LOCAL SYSTEM, se necessário).

Info sobre o erro ou patch:  
<a href="http://blog.hagander.net/index.php?url=archives/149-Help-us-test-a-patch-for-the-Win32-shared-memory-issue.html#feedback" target="_blank">http://blog.hagander.net/index.php?url=archives/149-Help-us-test-a-patch-for-the-Win32-shared-memory-issue.html#feedback</a>  
<a href="http://www.postgresql.org/docs/8.4/static/release-8-4-1.html" target="_blank">http://www.postgresql.org/docs/8.4/static/release-8-4-1.html</a>