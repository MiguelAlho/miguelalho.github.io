---
title: Esconder colunas em GridViews para impressão
author: Miguel Alho
type: post
date: 2008-08-06T08:30:17+00:00
url: /esconder-colunas-em-gridviews-para-impressao/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - colunas
  - CSS
  - gridview

---
Uma questão comum que surge com _GridViews_ é ter a possibilidade de mostrar ou esconder colunas aquando da impressão. Uma forma simples de resolver a questão é utilizar uma stylesheet de impressão, aplicada sempre que o browser recebe o comando de impressão e que inclui uma classe do género:

<pre lang="css">.noprint{display:none;}</pre>

Todos os elementos que tiverem esta classe atribuída poderão ser visíveis no ecrã, mas na impressão desaparecerão.

No caso da _GridView_, se tivermos uma coluna (_TemplateField_) com botões, por exemplo, e não quisermos que os botões sejam visíveis, basta no _TemplateField_ definir _HeaderStyle-CssClass_ e _ItemStyle-CssClass_ con a classe &#8220;noprint&#8221;

<pre lang="asp">&lt;asp:TemplateField (...) HeaderStyle-CssClass="noprint" ItemStyle-CssClass="noprint">
   &lt;ItemTemplate>
	(...)
   &lt;/ItemTemplate>
&lt;/asp:TemplateField>
</pre>

Só definir o _ItemStyle-CssClass_ elimina os botões, mas preserva a coluna. Atribuindo a classe a ambos esconde a coluna. Se houver um _Footer_, será necessário efectuar o mesmo para o _FooterStyle-CssClass_.