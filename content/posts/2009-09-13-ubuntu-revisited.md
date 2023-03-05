---
title: Ubuntu, revisited
author: Miguel Alho
type: post
date: 2009-09-13T05:47:15+00:00
url: /ubuntu-revisited/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'

---
Este site tem sido alojado nos últimos 4 anos num serviço prestado pela <a href="http://hostingportugal.pt/home.php" target="_blank">HostingPortugal</a>. Não tenho razão especial de queixa, que não tem havido qualquer conflito. A única coisa que não gosto é do uso do Helm, como gestor de conta, mas dado que tinha escolhido uma conta Windows (para servir ASP.NET), não tive como fugir a ele. Versões mais recentes do painel parecem interessantes, mas mesmo assim deixa a desejar. Estou demasiado habituado a usar a interface do Windows Server para gerir, que aquilo acaba por saber a pouco. Também sou um &#8220;control freak&#8221; e por isso sinto a limitação.

Decidi então alojar localmente a minha página. Sei que há alguns riscos inerentes à mudança, e mesmo de capacidade de servir (com o volume de tráfego que tenho, não deve ser problemático). Estou a reabilitar uma máquina para servir esta função. Tenho um servidor Windows activo internamente, mas não quero que seja o web-server, pelo menos para já. Gostava de ter o webserver numa máquina separada, e neste caso, visto que a página actual é puramente PHP, vou experimentar um LAMP server, com base no Ubunto Server. Mais tarde, se não ficar satisfeito, sigo o caminho do Windows Server, mas é pouco provável.

Como é habitual quando inicio uma instalação no Linux, tendo a visitar um post antigo: <a href="http://miguelalho.com/?p=770" target="_blank">Instalação de Ubuntu Server 8.10</a>. Tem sido um guia base para introduzir um GUI nas edições de servidor, e que fica leve. Não é que a panóplia de aplicações presentes no Gnome ou KDE não sejam interessantes, mas não necessito de quase nada daquilo, e é escusado ocupar o espaço. O comando base usado foi:

`sudo apt-get install xorg xfce4 gdm synaptic firefox ntfs-config thunar-volman`

Neste caso adicionei o gdm para ter o login do gnome e permitir que algumas aplicações como o synaptic aparecessem correctamente nos menus. Também, adicionei 3 itens <a href="http://disambiguation.wordpress.com/2008/08/19/minimal-ubuntu-tweaking-some-settings/" target="_blank">mencionados no blog Disambiguation</a>, que são:

\# xfce4-goodies (alguns add-ons uteis ao xfce)  
\# xfce4-mcs-plugins-extra (permite adicionar aplicações ao arranque)  
\# xfce4-taskmanager (permite ver as aplicações em execução, como no windows)

De seguida, para uma administração remota e web do servidor, instalei o Webmin, seguindo os passos em [ubuntugeek.com][1]:

`<br />
$ sudo aptitude install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl libmd5-perl</p>
<p>$ wget http://garr.dl.sourceforge.net/sourceforge/webadmin/webmin_1.441_all.deb</p>
<p>$ sudo dpkg -i webmin_1.441_all.deb<br />
` 

O Webmin tem uma série de opções bastante interessantes, com a administração do server, das opções de rede na mesma (através da qual fixei o IP de rede interna do server), das diversas bases de dados, backups e cron jobs, até tem uma interface para a shell (se bem q uma ligação por SSH será mais eficiente, mas é bom, mesmo assim). Na verdade, com o Webmin, o desktop é praticamente desnecessário, nesta aplicação. Se realmente não for, eliminarei o passo numa futura instalação para este fim.

Agora resta transferir a instalação do WordPress para o server, e mapear o DNS e NAT como deve ser para apontar correctamente ao servidor LAMP.

ACTUALIZAÇÃO:  
porque a estrutura de endereços no sourceforge sofreu uma alteração (penso eu), o endereço no comando wget é diferente. deve então experimentar: 

wget http://switch.dl.sourceforge.net/sourceforge/webadmin/webmin/1.500/webmin\_1.500\_all.deb

ou ainda

wget http://dourceforge.net/projects/webadmin(files/webmin/1.500/webmin\_1.500\_all.deb/download

 [1]: http://www.ubuntugeek.com/ubuntu-serverinstall-gui-and-webmin-in-ubuntu-810-intrepid-ibex-guide.html