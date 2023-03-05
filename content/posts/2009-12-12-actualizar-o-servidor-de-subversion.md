---
title: Actualizar o servidor de Subversion
author: Miguel Alho
type: post
date: 2009-12-12T16:29:30+00:00
url: /actualizar-o-servidor-de-subversion/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'
tags:
  - subversion
  - svn

---
Um dos problemas que enfrentei nos últimos dias com o Subversion foram os chamados &#8220;tree-conflicts&#8221;. São essencialmente conflitos que surgem quando é feito a actualização de um nome de ficheiro e o ficheiro é movido, e alguem depois tem no seu &#8220;working copy&#8221; uma versão ainda anterior a essa operação. Quando tentam efectuar o commit, vão naturalmente encontrar um erro, por tentarem actualizar um ficheiro que não está presente na revisão.

Segundo encontrei, a versão mais recente (1.6) do SubVersion tem melhor suporte para este tipo de erros agora. Mas primeiro, antes de actualizar, justifica (sempre, mesmo que digam que não é preciso) fazer um backup. O backup é um simples &#8220;dump&#8221; das revisões para um ficheiro e efectuado pelo comando:

<pre>>svnadmin dump X:\caminho_do_repositorio X:\Caminho_e_nome_do_ficheiro_do_backup
</pre>

A extensão do ficheiro gerado pode ser um qualquer, mas recomendo o uso de a data. O commando executa uma descarga completa das bases com as diversas revisões para o ficheiro (binário). O ficheiro tende a crescer muito (nalguns casos centenas de MBs) mas é altamente comprimível.

Infelizmente, como podem ver, é uma execução repositório a repositório. Nada como criar um script para realizar a tarefa (ou um programazito para gerir a lista de repositórios e os dumps!) . Este é um passo que deve ser frequente, para quem tem amor à vida e ao trabalho que realiza&#8230; a redundância nestas coisas nunca é demais!

O próximo passo é a actualização do serviço (Windows do servidor de SVN). É conveniente reinicializar o serviço no fim do processo de actualização. Também , dependendo de como instala, pode acabar por ter duas instalações do mesmo serviço (por exemplo eu tinha um _server_ de Subversion inicial a correr, e actualizei com o _installer_ do CollabNet. Fique com dois serviços instalados no servidor).

O último passo da actualização é actualizar os repositórios em si, utilizando o comando _upgrade_:

<pre>>svnadmin upgrade X:\caminho_do_repositorio
</pre>

Novamente um script ou uma pequena aplicação serão muito úteis para auxiliar este processo para o caso de haver muitos repositórios.

Por fim, actualize tb o cliente (Tortoise, etc.) para que trabalhem bem com o server actualizado. Os working copies no cliente são actualizados automaticamente pelo Tortoise assim que houver o contacto.