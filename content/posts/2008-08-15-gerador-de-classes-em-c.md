---
title: 'Gerador de Classes em C#'
author: Miguel Alho
type: post
date: 2008-08-15T13:05:09+00:00
url: /gerador-de-classes-em-c/
categories:
  - 'Code &amp; IT'
tags:
  - 'C#'
  - Classe Generator
  - Classes
  - Gerador de classes
  - MyGeneration
  - N-Tier

---
Das funcionalidade mais eficientes que pode haver é a possibilidade de autogerar código. Especialmente durante a fase inicial do coding da aplicação, gerar código das classes é uma seca. Mais se for multi-camada. Interfaces constantemente criadas, nomes repetidos constantemente e código que, de aplicação em aplicação, mantém-se sempre igual&#8230;

Os últimos projectos que desenvolvi permitiu observar muitos padrões em uso &#8211; o uso de objectos anuláveis frequentemente, o tipo de código de acesso aos dados constante, os métodos base existentes nas classes, etc. Algumas classes que desenvolvi já permitem a reutilização projecto a projecto, o que é óptimo. Por exemplo o de acesso a dados da BD SQLServer e SQLite (que ainda têm margem para melhorias), ou de escrita de dados em Excel, ou o de serialização para XML. 

Mas para geração do código base é que ainda não tenho nada, e até agora tenho estado dependente do Visual Studio e algumas funcionalidades existentes. As versões Express ainda não tem esse tipo de funcionalidade implementada, mas tem pequenos truques que ajudam. O Sparx, que utilizei num projecto, é completo e resulta bem, mas pelo menos a versão que utilizei não resolveu todos os problemas. 

O que preciso é de uma aplicação que gere os meus _BO_ (_business objects_), a _DAL_ (_Data Acces Layer_) e que aproveite correctamente os métodos de acesso que já gerei, o _BLL_ (_Business Logic Layer_) caso o projecto necessite, e que me gere o SQL também, já agora. Preferencialmente, tudo adaptado ao meu _workflow_ e padrão de código, similar e adaptado do esquema que o <a target="_blank" href="http://imar.spaanjaars.com/QuickDocId.aspx?quickdoc=416">Imar Spaanjaars</a> apresenta no site dele.

Há um tempo que imaginava tentar construir algo, que aproveitasse bem o que já desenvolvi até agora, para gerar código base para qualquer aplicação que eu venha a desnvolver. Algo do género do <a target="_blank" href="http://www.csharpfriends.com/demos/csharp_class_generator.aspx?ftd=2">C# Classe Generator</a> apresentados no <a target="_blank" href="http://www.csharpfriends.com/">CSharpFriends.com</a>, que permite a inserção do _Namespace_, do classe, das propriedades, e que gere o código dos objectos e da DAL. Mas ao pesquisar para este post, encontrei algo interessante no site do Spaanjaars &#8211; um link para o <a target="_blank" href="http://www.mygenerationsoftware.com/portal/default.aspx">MyGeneration</a>. O MyGeneration é um gerador de código muito completo, e que permite introduzir _templates_; Suporta várias linguagens de programação (C#, VB.NET, JScript&#8230;), vários sistemas de BD (SQL Server, Oracle, Acces, SQLite, Firebird,..), conceito de projecto, e suporte a diversas arquitecturas ORM. O IDE parece bastante completo. E complexo.

Portanto surge o probelma normal de &#8220;Usar o que existe e poupar tempo (ou não)&#8221; vs. &#8220;Construir algo costumizado, e talvez mais simples (ou não)&#8221;. Vou ter de estudar melhor o MyGeneration a ver se encaixa.