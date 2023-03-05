---
title: Construir um ubuntu ( ou uma distribuição própria, por assim dizer..)
author: Miguel Alho
type: post
date: 2008-10-28T22:36:00+00:00
url: /construir-um-ubuntu-ou-uma-distribuicao-propria-por-assim-dizer/
categories:
  - 'Code &amp; IT'
tags:
  - configuração
  - Gnome
  - Linux
  - Ubuntu
  - XFCE

---
Nestes últimos dias, a minha mente tem estado algo dorido com a quantidade de info nova que tenho estado a adquirir. Tenho planeado há algum tempo construir um pequeno servidor em próprio para manter em casa, especialmente destinado a armazenar dados &#8211; SVN, máquinas virtuais, servidor de testes, backups, NAS&#8230; enfim.. um multifunções centralizado.

Finalmente surgiu a oportunidade de adquirir alguns dos elementos necessários &#8211; já tinha dois discos SATA de 640Gb que encontrei a bom preço (até subiram de preços quando desde que as adquiri), e investi no restante hardware, para suportar tudo. Conta com um processador Quad Core (Intel Q6600) que estava a um bom preço, importado, e componentes para misturar com alguns já existentes&#8230; uma maquina para servir a função durante uns bons anos, espero. Naturalmente e gostando do DIY como gosto, a caixa será um projecto de trabalho manual.

Mas depois vem o problema maior.. configurar o SO para manter isto tudo. E aqui começa a confusão. A ideia de virtualizar parece-me bem e útil, mas o como é q me está a deixar algo perplexo. Confesso que não estava a par das tecnologias de virtualização (para além de clientes como o VMWare, Virtualbox e VirtualPC). A palavra Hypervisor passou a fazer parte do meu vocabulário, o VMWare ESXi, o Xen Server, e o KVM pareceram ser opções interessantes, para usar como uma camada entre o hardware e os SOs.

Aliás parte da confusão vem daí &#8211; usar um sistema destes, hipervisor, mínimo, e correr N SOs sobre ele, ou optar por um SO Linux base e abrir os VMs que necessito, quando necessitar? Usar algo como um hipervisor, como base e depois outros por cima, preocupa-me&#8230; se houver avaria de configurações (e tendo em conta o q Murphy diz e o pouco que conheço desta tecnologia) que risco corre os dados nos SOs implementados. Tendo em conta que uma função importante do sistema é o de backup, não quero arriscar perder os dados por não poder aceder a eles por o hipervisor base ter dado o berro&#8230;. Possível? Arriscado? Antes de optar pela solução de SO &#8220;normal&#8221; com VMs, devo testar o esquema de hipervisor. O ESXi é gratuito mas tem algumas limitações, que não sei se são graves ou não. Xen parece interessante e tenho um colega que já usou e a quem vou certamente cravar informação. O KVM também pode ser ideal.

De qualquer forma, optando por um esquema &#8220;tradicional&#8221;, resta escolher o sabor de Linux a usar. Infelizmente, na minha opinião, há demasiado por onde escolher. Achava o esquema de licenciamento do Windows, chato (Home, Home premium, Business, Ultimate&#8230;), mas mesmo sendo gratuito, o Linux consegue ser complicado. Base Debian vs. Slax vs. outros; Gestor Gnome vs. KDE vs. XFCE vs&#8230;; etc, etc, etc. A possibilidade de escolha é bom, não haja duvida, mas demasiado acho que não é de todo o ideal. Por exemplo, se no caso do gestor de janelas houvesse apenas 1 geral, &#8220;themed&#8221;, talvez a gestão de tudo o resto (e aplicações compatíveis, etc) podesse ser melhorado. Também acho que o KDE tem o problema da letra &#8220;k&#8221;, especialmente antes dos nomes dos aplicativos. Deviam ter escolhido uma vogal, como a Apple&#8230; 😀

Decidi num portatil velho testar o Ubuntu Server e encaixar um ambiente gráfico. Vi-me grego para meter o Gnome a que estou habituado, e mesmo assim n consegui passar da linha de comandos, por mais info que achasse na web. Até que hoje, encontrei isto:

<li<a href="http://disambiguation.wordpress.com/2008/08/15/new-life-for-my-old-computer/" target="_blank">>A new life for and old computer @ disambiguation</a></li> 

  * <a href="http://disambiguation.wordpress.com/2008/08/17/a-minimal-ubuntu-install-the-basics/" target="_blank">A minimal ubuntu install</a>
  * <a href="http://disambiguation.wordpress.com/2008/08/18/minimal-ubuntu-panels-firefox-and-a-launcher/" target="_blank">panels, firefox and a laucher</a>
  * <a href="http://disambiguation.wordpress.com/2008/08/19/minimal-ubuntu-tweaking-some-settings/" target="_blank">Minimal Ubuntu &#8211; tweaking soime settings</a>
  * [Minimal Ubuntu &#8211; multimedia and a nicer launcher][1]

Trata-se de uma série de posts, que tem por base uma instalação do Ubuntu mínimo &#8220;Ubunto Minimal Install&#8221;, instalado a partir de um ISO de 9.5MB. A base do Ubuntu é instalado (pelo que vi numa VM, ocupa 667MB &#8211; certamente com pacotes puxados da net). O autor nos posts simplesmente configura o sistema ao seu gosto. Vale a pena a leitura, mas o que queria apresentar, principalmente, é como adicionar um dos ambientes gráficos possíveis a esta instalação, ou a uma instalação como o server.

sudo apt-get install xorg xfce4 gdm synaptic

Este commando deve resolver (pode ser necessário um &#8220;sudo apt-get update&#8221; previamente). Instala o Xorg com o sistema gráfico, xfce4 para o gestor de janelas, o _GNOME display manager_ ao nivel do arranque gráfico e login, e o Synaptic para instalar programas no ambiente gráfico. o commando &#8220;startx&#8221;, arranca o sistema, naturalmente. A partir daí é adicionar ao gosto do freguês. 😀

 [1]: http://disambiguation.wordpress.com/2008/08/22/minimal-ubuntu-multimedia-and-a-nicer-launcher/