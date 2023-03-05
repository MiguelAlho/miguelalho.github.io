---
title: '.Less (Dot Less) -> “CSS on Steroids”'
author: Miguel Alho
type: post
date: 2010-06-03T18:46:41+00:00
url: /less-dot-less-css-on-steroids/
categories:
  - 'Code &amp; IT'
tags:
  - .Less
  - .net
  - CSS
  - T4

---
Ainda no outro dia, falava com os meus colegas de como seria muito bom que o CSS tivesse estruturas de herança incorporados e variáveis.. especialmente variáveis, para não ter de definir constantemente as mesmas coisas.

E hoje enquanto lia o <a href="http://imar.spaanjaars.com/536/using-less-to-change-the-way-you-write-your-css" target="_blank">blog do Spaanjaars</a>, descubir o <a href="http://dotlesscss.com/" target="_blank">.Less</a>, uma biblioteca dedicada a extender o CSS, introduzindo variáveis e esquemas de herança. Soa bem? Se soa!! 😀

Com o .Less, podemos escrever codigo com variáveis:

<pre lang="css">@brand_color: #4D926F;

#header {
  color: @brand_color;
}

h2 {
  color: @brand_color;
}
</pre>

Ou nestings de selectores:

<pre lang="css">#header {
    color: red;
    a {
       font-weight: bold;
       text-decoration: none;
    }
}
</pre>

Ou misturas de elementos:

<pre lang="css">.rounded_corners(@radius: 5px) {
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
  border-radius: @radius;
}

#header {
  .rounded_corners;
}

#footer {
  .rounded_corners(10px);
}
</pre>

Lindo, né? Originalmente, o <a href="http://lesscss.org/" target="_blank">Less foi construído para o Ruby</a> (é um gem para o Ruby), mas um grupo converteu-o para .Net, criando o .Less. E ainda bem! No .Net, funciona como um HTTPHandler. Os ficheiros de CSS são ficheiros .less que são transformados aquando do pedido da página. É também possível <a href="http://haacked.com/archive/2009/12/02/t4-template-for-less-css.aspx" target="_blank">usar templates T4</a> para criar versões estáticas do CSS.

A experimentar!