---
title: 'Three C# 2.0/3.0 Syntaxes That You Didn’t Know But Were Afraid to Ask'
author: Miguel Alho
type: post
date: 2008-10-06T10:02:21+00:00
url: /three-c-2030-syntaxes-that-you-didnt-know-but-were-afraid-to-ask/
categories:
  - 'Code &amp; IT'
tags:
  - 'C# 3.0'
  - sintáxe

---
Um link interessante, com algumas novidades do C# 3.0. Enquanto que o primeiro ponto apresentado na página fala da declaração de propriedades, que já referi aqui anteriormente, os dois seguintes para mim são novidade. Gostei especialmente da novidade no operador ternário ?:, que num teste a _null_, em vez do formato do C# 2.0 :

<pre lang="csharp">Objecto obj1 = null;
Objecto obj2 = (obj1 == null ? new Object() : obj1);
</pre>

pode ser escrito como :

<pre lang="csharp">Objecto obj1 = null;
Objecto obj2 = (obj1 ?? new Object());
</pre>

As três demonstrações de sintaxe no <a href="http://www.adamtibi.net/post/2008/09/30/Three-C-Sharp-Syntaxes-That-You-Didnt-Know-But-Were-Afraid-to-Ask.aspx" target="_blank">site de Adam Tibi</a>.