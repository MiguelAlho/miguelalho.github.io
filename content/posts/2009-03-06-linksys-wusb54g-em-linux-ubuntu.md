---
title: Linksys WUSB54G em Linux (Ubuntu)
author: Miguel Alho
type: post
date: 2009-03-06T11:55:03+00:00
url: /linksys-wusb54g-em-linux-ubuntu/
categories:
  - 'Code &amp; IT'
tags:
  - linksys
  - Linux
  - wireless

---
Não gosto nada da Linksys. Lol. Até brinco com o facto com colegas que ficam pro vezes admirados, já que os equipamentos da Linksys são considerados muito bons e até são feitos pela Cisco. Tudo bem, mas os que me aparecem à frente só dão chatices. Enfim.

De qualquer forma, tenho tido contacto com algumas peças da marca numa instalação, nomeadamente o adaptador WUSB54G, que permite um PC ligar-se à rede através da porta USB. É uma peça um pouco antiquado mas funciona. Também estão a ser usados em maquinas muito idosas, já. Instalar no Linux é que, infelizmente, não é um processo imediato. Tenho encontrado alguma info em foruns, mas as respostas às vezes são tão complexas que até assustam. 

Não é, no entanto, assim tão díficil de instalar no Ubuntu. Basta ir buscar uns elementos via o Synaptics. Eles são:

  * ndiswrapper (permite usar drivers wireless do Windows no Linux)&nbsp;
  * ndisgtk (para ter uma interface gráfica)
  * ntfs-3g e ntfs-config (caso ainda n esteja instalado &#8211; permite ler ficheiros do sistema NTFS de uma pen ou disco externo no Linux)
  * gnome-mount (para poder montar automáticamente um disco externo.

As últimas duas são auxiliares, se pretender copiar os ficheiros necessários através de uma pen ou assim a partir do windows. No entanto, são uteis de ter para uma qualquer de chuva como hoje.

Convém puxar os drivers do site da linksys, porque será necessário o ficheiro de extenção .inf para efectaur a instalação do driver. Para o WUSB54G o link é <a target="_blank" href="http://www.linksysbycisco.com/US/en/support/WUSB54G/download">http://www.linksysbycisco.com/US/en/support/WUSB54G/download</a>

Depois de sacar, extrai os ficheiros e copie-os para uma pen para aceder na máquina linux (naturalmente só faz sentido se estiver a puxar os ficheiros de a partir de outro computador)

Abre o NDisgtk (Sistema -> Administração -> Windows Wireless Drivers), precione o botão de instalação do driver, e localiza o ficheiro .inf (no meu caso, estava na pasta <pen:>\linksys driver\WUSB54Gv4_20051110\Drivers\WUSB54Gv4\rt2500usb.inf ). 

E já está.. até é simples 😀

<div class="zemanta-pixie">
  <img class="zemanta-pixie-img" src="http://img.zemanta.com/pixy.gif?x-id=6a626983-8095-4426-85fc-6e9e501b387a" />
</div>