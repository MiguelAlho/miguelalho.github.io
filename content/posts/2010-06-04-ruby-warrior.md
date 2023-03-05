---
title: Ruby Warrior
author: Miguel Alho
type: post
date: 2010-06-04T00:23:04+00:00
url: /ruby-warrior/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'
tags:
  - AI
  - IA
  - inteligência artificial
  - Ruby
  - Ruby Warrior

---
Acabei há instantes o nível intermédio (o segundo dos dois níveis) do <a href="http://github.com/ryanb/ruby-warrior" target="_blank">Ruby-Warrior</a>, um jogo criado para ensinar inteligência artificial com base em Ruby. O jogo é muito simples e muito divertido, especialmente para quem é _geek_ (lol) e gosta de programar. Aqui no escritório temos o desafio interno de terminar o jogo sem recorrer a ajudas externas. Felizmente consegui sem externas e quase sem usar as pistas que o jogo nos dá quando morremos.

O jogo é muito simples &#8211; visualmente é texto na linha de comandos &#8211; e temos de mover a nossa personagem pelos diversos níveis da torre, para chegar ao topo e salvar a princesa. Pelo meio vamos encontrando diversos inimigos e pessoal que precisa de ser salvo. No jogo, programamos a nossa personagem de modo a tomar as decisões correctas para ultrapassar os inimigos e chegar à escadaria de cada nível.

O modo iniciante é jogado apenas a uma dimensão (um eixo apenas) enquanto o segundo modo (intermédio) já decorre a duas dimensões tornando necessário implementar decisões de prioridade relativo a direcções em que devemos actuar. Assim, somos obrigados a analisar o meio envolvente e prioritizar as acções. 

O jogo é muito giro, e permite ensinar bastante acerca do Ruby, especialmente a definição de métodos e mecanismos de controlo (if /elsif /else e array e for). Também ajuda a descubrir formas de criar decisões em código.

Recomendo vivamente experimentarem &#8211; o código pode ser puxado <a href="http://github.com/ryanb/ruby-warrior" target="_blank">do GitHub do Ryan Bate</a>. Tinha pensado colocar aqui o meu código, mas achei melhor não.. é melhor experimentar e descobrir. Se bem que posso partilhar a minha solução com quem quiser ver 😀