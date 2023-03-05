---
title: Arrancar servidores sem teclado
author: Miguel Alho
type: post
date: 2009-09-13T13:08:57+00:00
url: /arrancar-servidores-sem-teclado/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'
tags:
  - boot
  - Keyboard
  - on erro

---
A maioria das boards, no arranque, exigem por defeito que haja um teclado ligado á porta PS2 ou USB. Mas há situações, especialmente em servidores montados num rack, onde não faz sentido haver teclado ligado fisicamente e permanentemente. A gestão é geralmente feito remotamente.

Ao arrancar um PC ou servidor sem o teclado, aparece a habitual mensagem (com uma certa ironia na mesma) do &#8220;No keyboard detected &#8211; press F1 to continue&#8221;. Na verdade, a verificação da existência do teclado é um dos passos executados no arranque e que levanta um erro, tal como as falhas de discos entre outras. no entanto na configuração da BIOS, há geralmente uma ou outra mensagem que permite contornar a paragem.

Nalguns, a opção está na opção &#8220;Halt on&#8221;, que indica sobre que erros deve parar o arranque. Opções por defeito são All (para sobre qualquer erro), mas há também o &#8220;none&#8221;, &#8220;all but keyboard&#8221;, &#8220;all but disk&#8221;, &#8220;all but disk/keyboard&#8221;. Estas são as que encontrei numa board (ASUS A7V8X para AMD).

Mas a opção &#8220;Halt on&#8221; não é única. Há semelhantes. Para um Asus P5Q-E, a opção refere &#8220;Press F1 on Errors&#8221;, que deve ser desabilitado. Certamente com boards e versões de BIOS diferentes, os nomes mudarão mas a intenção é a mesma &#8211; saltar por cima do erro.