---
title: Recuperação de arranque em Windows Server 2008
author: Miguel Alho
type: post
date: 2011-01-03T10:32:05+00:00
url: /recuperacao-de-sectores-de-boot-em-windows-server-2008/
categories:
  - 'Code &amp; IT'
tags:
  - boot
  - bootrec
  - diskpart
  - recuperação
  - Windows Server 2008

---
Pois. Este é um post que representa o meu dia de ano novo. A passagem foi excelente. O tarde após a passagem foi bastante assustador. Aproveitei a &#8220;paragem&#8221; para fazer uma limpeza ao meu servidor e introduzir uns discos novos. E assim se fez o caos! Passo a explicar&#8230;

O meu servidor é relativamente simples e construído às necessidades. É um setup simples com dois discos de ITB em RAID 1 para sistema e repositórios de ficheiros importantes, e mais 3 discos de 640GB para &#8220;tralha&#8221;. Decidi fazer um upgrade, e acrescentar algum espaço &#8211; Ia juntar um disco de 640GB que estava na prateleira parada com uma das existentes num RAID 1 dedicado a backups, e acrescentar um disco de 1TB para mais tralha. Assim, ficaria com um total de 7 discos, representando cerca de 3TB de espaço, estando metade montado em RAID1. Uma unidade separada e dedicada a backups parece-me uma boa aposta, e quero implementar uma forma mais automatizada de os fazer, quer do servidor, da minha máquina e a dos meus colaboradores.

Portanto abri e limpei algum (pouco, felizmente) pó do servidor, e reorganizei a barra de discos que agora ficou completamente cheia. Introduzi o disco antigo que esteve parado (e que ainda tinha uma instalação de OS anterior do servidor, como backup), arranquei a máquina, criei um novo RAID e&#8230; O servidor entrou no OS antigo em vez da instalação actual. Arrancou pelo segundo disco em vez do primeiro. E assim o caos começou.

<!--more-->

Obviamente não era isso que eu queria. Queria o arranque pelo mesmo OS que deveria estar intacto no DISCO 0. Infelizmente esta troca alterou o esquema de letras de unidades e o sector de arranque do disco principal., Não compreendi porque, infelizmente, mas tornou-se impossível arrancar pela partição de sistema do disco original, mesmo sendo a única unidade fisicamente ligada. Algo&#8230; assustador!

Para corrigir tive que arranjar algum método de corrigir, que não envolvê-se reinstalar. Felizmente encontrei alguns comandos que se podem executar para reconstruir o o sector e informação de arranque que passo a explicar. Há &#8220;ordem&#8221;, de certa forma, nos passos a executar q passo a descrever. Este foi o meu caso e raciocínio, mas obviamente pode mudar caso a caso.

O primeiro passo é introduzir e arrancar o servidor com o disco de instalação do Windows Server 2008. Ele tem ferramentas de recuperação, que podem ser executadas a partir da linha de comandos. Depois de arrancar, deve escolher a opção de reparação. Surge uma janela de escolha de unidade de sistema a recuperar. Infelizmente no meu caso, pelo menos inicialmente, não reconhecia nenhuma partição com uma instalação de sistema. De qualquer forma, escolhendo &#8220;NEXT&#8221; permite chegar às opções seguintes e nas opções de ferramentas, escolhi a linha de comandos para abrir uma consola. Foi interessante verificar que, mesmo n conseguindo arrancar pelo OS, na consola, o windows ainda reconhecia a estrutura de partições e unidades presente no disco. Escolhendo a unidade C do sistema (>c:) executei o seguinte comando:

`bootrec.exe /fixmbr`

O comando repara o Master boot record. Em muitas situações este comando ou a combinação com o seguinte:

`bootrec.exe /fixboot`

Deveria ser suficiente para reparar o arranque. Infelizmente no meu caso não o foi, pois o /fixboot respondeu com a mensagem de erro &#8220;Element Not Found&#8221;. Este erro significa, essencialmente, que a partição de sistema não está &#8220;activa&#8221;, e portanto deve ser activado, para o recuperador o encontrar. para tal, uso o DISKPART:

`<br />
>diskpart<br />
DISKPART>list disk<br />
...<br />
DISKPART>select disk 0<br />
...<br />
DISKPART>list partition<br />
...<br />
DISKPART>select partition 1<br />
...<br />
DISKPART>active<br />
DISKPART exit<br />
` 

Esta sequência demonstra alguns comandos do diskpart. os &#8220;list&#8221; listam os discos ou partições disponíveis no disco seleccionado. O &#8220;select&#8221; permite seleccionar e disco e partição desejada. &#8220;Active&#8221; activa a selecção. Depois de executar estes comandos, e com a partição activa, o bootrec.exe /fixboot executa correctamente e pode ser executado ainda o comando

`bootrec.exe /rebuildbcd`

Se tudo correr bem, deverá ser suficiente. Infelizmente, no meu caso, não o foi. Isto porque, no arranque, sugiu nova mensagem de erro, mas desta vez a indicar que o BOOTMGR estava em falta : &#8220;BOOTMGR IS MISSING&#8221;. Mais uma volta&#8230; desta vez foi suficiente executar o reparador de arranque no CD de instalação. Infelizmente, e ao contrario do Vista, esta opção não aparece na lista de ferramentas. Para aceder é necessário entrar na consola de comandos e executar:

`X:\sources\recovery\StartRep.exe`

Este foi o toque final necessário para ficar tudo OK. Pude finalmente concluir e ligar os restantes discos sem problemas dentro do servidor.

Refs:  
<http://www.lancelhoff.com/how-to-fix-vista-mbr-repair-broken-vista/>  
<http://www.pcstats.com/articleview.cfm?articleid=2264&page=8>  
[http://www.networksteve.com/forum/topic.php/VPN\_created\_using\_modem\_-\_unable\_to_modify/?TopicId=9937&Posts=2][1]

 [1]: http://www.networksteve.com/forum/topic.php/VPN_created_using_modem_-_unable_to_modify/?TopicId=9937&Posts=2