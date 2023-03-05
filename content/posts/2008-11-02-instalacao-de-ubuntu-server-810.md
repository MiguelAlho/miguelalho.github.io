---
title: Instalação de Ubuntu Server 8.10
author: Miguel Alho
type: post
date: 2008-11-02T15:32:08+00:00
url: /instalacao-de-ubuntu-server-810/
categories:
  - 'Code &amp; IT'
tags:
  - configuração
  - instalação
  - Linux
  - servidor
  - ubuntu 8.10
  - XFCE

---
Dando continuidade ao post &#8220;<a href="http://miguelalho.com/?p=767" target="_blank">Construir um ubuntu ( ou uma distribuição própria, por assim dizer..)</a>&#8221; que escrevi há dias, decidi escrever aqui um passo-a-passo de como efectuar uma possível configuração, mínima, e com ambiente gráfico.

Então para isso vou seguir com o Ubuntu-server, que me interessa (e instala alguns pacotes na qual tenho interesse, como o servidor LAMP), e vou adicionar um ambiente gráfico e um conjunto de aplicações que considero necessário para o meu uso. Vou efectuar esta instalação numa máquina virtual do <a href="http://www.virtualbox.org/" target="_blank">VirtualBox</a>, que prefiro a outras aplicações de virtualização como o VMWare.

Para começar, criei uma nova máquina virtual para Ubuntu com 756MB de RAM e um novo disco de 8GB, dinamicamente expansível. Antes de iniciar, é util efectuar uma operação que resulta das características do novo kernel. Porque o kernel 2.6 do linux tem algumas dependências do processador, no Virtualbox, é necessário activar o suporte à virtualização e o PAE (extensão de endereço físico). Para tal, depois de criar a maquina virtual,com o botão direito selecciona Definições -> Geral -> Avançado e seleccione &#8220;Activar VT-x / AMD-V&#8221; e &#8220;Activar PAE/NX&#8221;, como mostra a figura:

[<img class="aligncenter size-medium wp-image-774" title="vbox0012" src="http://miguelalho.com/wp-content/uploads/2008/11/vbox0012-500x391.jpg" alt="" width="500" height="391" />][1]

Se não o activar, ao reiniciar a máquina após a instalação, irá aparecer uma mensagem de panico do kernal:

<pre>this kernel requires the following features not present on this CPU
pae
Unable to boot. Please use a kernal apropriate for your CPU</pre>

Activando o PAE, esta situação desaparece, e naturalmente o hardware usado tem de suportar a função. Esta solução esta descrita no site <a href="http://tombuntu.com/index.php/2008/08/06/upgrading-virtualbox-and-virtualizing-the-ubuntu-810-alphas/" target="_blank">Tombuntu</a>.

A máquina está pronta para instalar o ubuntu-server, faltanso apenas carregar (montar) o ISO, para a instalação. Este pode ser efectuado novamente nas definições da máquina virtual, em CD/DVD ROM, escolhendo uma imagem ISO do server (ou então o alternate CD que tb tem imensas opções de instalação, sem instalar o sistema ubuntu completo com os programas).

[<img class="aligncenter size-medium wp-image-775" title="ubuntu-server-install001" src="http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install001-500x426.jpg" alt="" width="500" height="426" />][2]

A instalação é bastante &#8220;directa&#8221;, bastando seguir os passos descritos. E até é um processo bastante rápido. Basta seguir os passos (é quase sempre &#8220;next&#8221;).  Ao nível do disco e partições, escolhi &#8220;guiado &#8211; usar disco inteiro e &#8230; LVM&#8221;, mas podes utilizar o que é mais conveniente para o teu teste. Neste caso não tinha necessidade de qualquer outro tipo de alteração.

[<img class="aligncenter size-medium wp-image-776" title="ubuntu-server-install002" src="http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install002-500x426.jpg" alt="" width="500" height="426" />][3]

No diálogo de selecção de software, escolhi adicionar o servidor LAMP, Sevridor OpenSSH, e host de Maquinas virtuais. A esolha vem de algumas situações que quero testar. Ao fim de cerca de 10 minutos, o SO está instalado e pronto a utilizar (quase). Resta desmontar o ISO e reiniciar a VM.

Naturalmente, ao iniciar, não teremos nenhum ambiente gráfico. O Ubuntu Server é essencialmente &#8220;shell-based&#8221; e dependente da linha de comandos. Vamos então adicionar o ambiente gráfico para termos um ambiente mais &#8220;comun&#8221; e agradável.

No post &#8220;<a href="http://miguelalho.com/?p=767" target="_blank">Construir um ubuntu ( ou uma distribuição própria, por assim dizer..)</a>&#8221; mostrei como adicionar o XFCE, como gestor de janelas. Vou continuar a usar o XFCE para este exemplo. Já testei o GNOME, mas foram demasiados elementos a serem introduzidos para o meu gosto, e o XFCE é levezinho e rápido, o que é optimo.

Então no terminal deve ser exectutado o seguinte comando para actualizar a lista de repositórios da instalação:

<pre>sudo apt-get update</pre>

Agora segue a instalação de alguns pacotes necessários e uteis:

  * **xorg** &#8211; o sistema gráfico para o linux e que permite ter um ambiente gráfico. Inclui bibliotecas, fonts, suporte para imagens,  e afins do x-server; _(Down: 32.1 MB / Espaço Ocupado: 93MB)_
  * **xfce4** &#8211; o gestor de janelas do XFCE _(D: 44MB / EO: 177MB)_
  * **synaptic** &#8211; gestor de pacotes e de instalação de aplicações _(D: 13.5MB / EO: 81.5MB)_
  * **firefox** &#8211; browser, quase obrigatório incluir, até para poder pesquisar qualquer situação. A instalação inclui o ubufoz que permite adicionar extensões através do synaptics. _(D: 23MB / EO: 84,5MB)_
  * **ntfs-config** &#8211; o ubunto tem suporte para o sistema de ficheiros NTFS integrado (através do ntfs-3g) mas para poder escrever correctamente para lá, esta aplicação permite activar a opção de escrita. _(D: 45kB / EO: 442kB)_
  * **thunar-volman** &#8211; thunar é o explorador de ficheiros do XFCE. O _thunar-volman_ permite uma melhor gestão e integração de discos amovíveis, como pens e afins. Importante para a escrita em NTFS. _(D: 73,1kB / EO: 471kB)_
  * **virt-manager** &#8211; gestor gráfico de maquinas virtuais baseado no KVM, que foi instalado no server. _(D: 1.5MB / EO: 7.3MB)_

Para isto lanço o comando:

<pre>sudo apt-get install xorg xfce4 synaptic firefox ntfs-config thunar-volman virt-manager</pre>

ou instalar cada pacote individualmente, de pretender algum controlo. No mínimo é conveniente ter o xorg, xfce4 e o synaptic, permitindo instalar o resto através do synaptic.

Instalado, resta iniciar o ambiente grafico escrevendo o comando

<pre>startx.</pre>

[<img class="aligncenter size-medium wp-image-779" title="ubuntu-server-install003" src="http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install003-500x416.jpg" alt="" width="500" height="416" />][4]

Executando a aplicação gráfica de adicionar e remover programas (o menu surge clicando com o botão direito do rato no desktop, e a aplicação está no menu &#8220;sistema&#8221;). podemos adicionar mais algumas aplicações, nomeadamente:

  * **disk analyser** &#8211; aplicação gráfica com informação sobre o disco e espaço ocupado, com base no Baobab e algumas bibliotecas do Gnome
  * **gedit** &#8211; bloco de notas do GNOME, que estou habituado a usar.
  * **monitor de sistema** _&#8211;_ monitor do sistema, CPU e memória, semelhante ao gestor de tarefas do Windows.
  * **7zip** &#8211; pacote para ver e abrir ficheiros compactados, também existente para o Windows. (a alternativa gratuita ao WinZip e WinRar).

Uma analise efectuada executando o analisador de disco (Menu -> Accessorios -> Analisador de Utilização do disco), podemos ver o estado da instalação do disco:

[<img class="aligncenter size-medium wp-image-780" title="ubuntu-server-install004" src="http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install004-500x416.jpg" alt="" width="500" height="416" />][5]

São utilizados cerca de 1.4GB do disco, onde cerca de 450MB são programas novos instalados, e 188MB são ficheiros na cache do programa apt (/var/cache/apt), que podem ser removidos se desejados.

Num próximo post, irei referir a configuração do ambiente gráfico, do aspecto, menus e afins. O XFCE é efectivamente interessante e eficiente.

 [1]: http://miguelalho.com/wp-content/uploads/2008/11/vbox0012.jpg
 [2]: http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install001.jpg
 [3]: http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install002.jpg
 [4]: http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install003.jpg
 [5]: http://miguelalho.com/wp-content/uploads/2008/11/ubuntu-server-install004.jpg