---
title: Instalação do JIRA + Confluence e integração com IIS
author: Miguel Alho
type: post
date: 2009-11-29T11:46:19+00:00
url: /instalacao-do-jira-confluence-e-integracao-com-iis/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'

---
Estive durante um curto período a analisar alguns sistemas colaborativos de gestão de projectos, e acabei por escolher o <a href="http://www.atlassian.com/jira" target="_blank">JIRA</a>. O Jira é um gestor de projectos bem orientado a software, com acompanhamento de casos e analise de velocidade de desenvolvimento, com acções colaborativas. Não é o único no mercado, mas é, dentro daquilo que procurava, muito completo e funcional e a um preço bastante acessível (Com os starter packs, são apenas $10 para 10 utilizadores, alojado localmente). Um vantagem que esta aplicação tem é o facto de ter inúmeros módulos interessantes a integrar, nomeadamente o <a href="http://www.atlassian.com/software/greenhopper/" target="_blank">Greenhoper</a>, para uma gestão de projecto Agile, e o <a href="http://www.atlassian.com/software/fisheye/" target="_blank">Fisheye </a>para integrar o [Subversion][1] no sistema. 

Ainda da Atlassian, há um sistema de Wiki completo, pássivel de ligar ao Jira, chamado <a href="http://www.atlassian.com/software/confluence/" target="_blank">Confluence</a>. Como qualquer dos módulos que mencionei, o curtos do starter pack é de $10, para 10 utilizadores. E por apenas $40, consegue-se um sistema de gestão de projecto muito completo e funcional, que correctamente utilizado, permitirá manter uma visão correcta dos projectos e objectivos.

As aplicações mencionadas são todas web apps, baseado em JSPs, que é uma tecnologia que estou completamente alheio a. Sou todo .NET, e portanto o a configuração e suporte preocupava-me um bocado. Felizmente, o site tem documentação MUITO BOA, o que auxiliou em muito a instalação, e configuração. A instalação do JIRA, com o instalador fornecido, é directo &#8211; é só seguir os passos de integração com a base de dados (podendo escolher entre uma série deles, como o PostgreSQL). Já com o Confluence, tive muito mais dificuldades, mas é bem explicado &#8211; 1º não estou habituado ao Tomcat e JAVA; 2º porque não segui correctamente as instruções.

Quer o Jira, quer o Confluence, correm em instâncias individuais de um Tomcat Server. Acredito que seja possível integra-las na mesma instalação (deve ser as instalações EAR/WAR mencionados). Para já, no entanto, estão instaladas em instâncias individuais, e como serviços no Windows (o que, no entanto, não me deixa completamente satisfeito porquer cada um desses serviços esta a comer 250 &#8211; 300MB de RAM). Porque o IIS não suporta os JSP directamente, a integração possível é o encaminhamento do pedido para os serviço, usando filtros ISAPI.

Porque tive dificuldades com o Confluence, gostava de deixar aqui um pequeno tutorial de como instalar o sistema, pelo menos com uma visão macro, dos passos.

**Visão Geral**  
De uma forma geral, a instalação pode seguir os seguintes passos:

  1. <a href="http://confluence.atlassian.com/display/JIRA/Installing+JIRA+Standalone+on+Windows" target="_blank">Instalar o Jira standalone </a>(usando o installer, é directo e sem chatices)
  2. <a href="http://confluence.atlassian.com/display/JIRA/Connecting+JIRA+to+a+Database" target="_blank">Integrar o Jira na base de dados preferido</a>
  3. <a href="http://confluence.atlassian.com/display/DOC/Installing+Confluence+Standalone+on+Windows+from+Zip+File" target="_blank">Instalar o Confluence a partir do zip </a>(por alguma razão não recomendam o uso do standalone) e configura-lo para não colidir com o Jira, a nível de portos de ligação, e para utilizar uma base de dados (que não o HSQL original), e arrancar como serviço do Windows
Nesta fase, deve ser possível aceder às duas aplicações, localmente, no browser, por portos diferentes. Para todos os efeitos, são dois servidores Tomcat activos e independentes a actuar. Agora, para integrar no IIS, é preciso integrar os filtros isapi, que encaminham os pedidos dum site no IIS, para os portos correctos na qual o Tomcat está a ouvir.

  4. <a href="http://confluence.atlassian.com/display/JIRA/Integrating+JIRA+with+IIS" target="_blank">Integrar com o IIS</a>

Aquilo que notei neste processo é que TODOS os portos nas instalações tinham de variar. No caso do JIRA, usei:  
**/Jira/conf/server.xml**

  * **server** : port=8005;
  * **Connector coyoteConnector** : port=8080 redirectPort=8443
  * **Context** : <Context path="/jira" docBase="${catalina.home}/atlassian-jira" reloadable="false">
  * **Connector AJP** : port=8009 redirectPort=8443

Para o Confluence:  
**/Confluence/conf/server.xml**

  * **server** : port=8006;
  * **Connector coyoteConnector** : port=8081 redirectPort=8444
  * **Context** :<Context path="/confluence" docBase="${catalina.home}/confluence" debug="0" reloadable="false">
  * **Connector AJP** : port=8010 redirectPort=8444

Depois no conector (jakarta), o uriworkermap.properties tem:

> /jira/*=worker1  
> /confluence/*=worker2

e o workers.properties.minimal tem:

> worker.list=worker1, worker2
> 
> #for jira  
> worker.worker1.type=ajp13  
> worker.worker1.host=localhost  
> worker.worker1.port=8009
> 
> #for confluence  
> worker.worker2.type=ajp13  
> worker.worker2.host=localhost  
> worker.worker2.port=8010

Se tudo correr bem.. fica a funcionar! Fora do servidor, não é possível aceder directamente aos ports 8080 e 8081, que por defeito, são bloqueados pela firewall (e bem). Todos os pedidos são necessariamente encaminhados pela porta 80, e resolvidos pelo IIS.

 [1]: http://subversion.tigris.org