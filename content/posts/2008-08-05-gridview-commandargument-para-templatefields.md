---
title: GridView CommandArgument para TemplateFields
author: Miguel Alho
type: post
date: 2008-08-05T16:48:49+00:00
url: /gridview-commandargument-para-templatefields/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - CommandArgument
  - gridview
  - TemplateFields

---
Um problema que gera alguma confusão no uso de _GridViews_ é quando é necessário utilizar controlos do tipo _Button_ ou _ImageButton_ dentro de um _TemplateField_. O problema principal é que, enquanto no postback a informação do _CommandName_ passa correctamente, o do _CommandArgument_ não passa correctamente &#8211; é necessário fazer o Bind explicitamente.

Um exemplo: Tenho um _GridView_ com botões no fim (não são do tipo edit, delete&#8230;) de cada linha. Cada linha apresenta dados de um registo e cada um dos botões atribui um valor distinto a um campo desse registo (botão 1 coloca &#8220;A&#8221; no campo, o botão 2 coloca &#8220;B&#8221; no campo, etc&#8230;).

Enquanto que, com _CommandFields_, podemos obter o número do registo pelo número de linha/índice (passo no _GridViewCommandEventArgs_), com o uso do _TemplateField_, o _CommandArgument_ não passa. É necessário atribuí-lo explicitamente, adicionando o valor directamente:

<pre lang="asp">(...)
&lt;asp:TemplateField>
  &lt;ItemTemplate>
    &lt;asp:Button ID="btt1" runat="server" Text="A" 
      CommandArgument='&lt;%# Bind("NS_Registo") %>' CommandName="A"/>
    &lt;asp:Button ID="btt2" runat="server" Text="B" 
      CommandArgument='&lt;%# Bind("NS_Registo") %>' CommandName="B"/>
    (...)
  &lt;/ItemTemplate>
&lt;/asp:TemplateField>
</pre>

Ou associando o numero do índice da GridView através de

<pre lang="asp">&lt;asp:Button (...) CommandArgument="&lt;%# Container.DataItemIndex %>;" />
</pre>

Assim, no postback, teremos disponível um valor (um valor do registo, explicito, ou o índice da linha do GridView) para usar.