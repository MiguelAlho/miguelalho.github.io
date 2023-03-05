---
title: Uns tópicos para terminar o ano…
author: Miguel Alho
type: post
date: 2008-12-28T21:10:10+00:00
url: /uns-topicos-para-terminar-o-ano/
categories:
  - 'Code &amp; IT'
tags:
  - google reader
  - Javascript
  - jQuery
  - sb2developer blog
  - scriptmanager

---
Pois é, o Natal já lá vai (e espero que tenha sido bom para todos) e o novo está por aí a vir. E como tal, o blog estava a merecer um postzito, pelo menos. Portanto, vai em jeito de rapidinha&#8230;

**Javascript/Jquery e Master Pages**

Uma das dificuldades em utilizar MasterPages em .Net é manter caminhos correctos nas referencias, especialmente se necessitas de reflectir esses caminhos em páginas de conteúdos ou controlos de utilizador não referenciados na mesma pasta que a master page. No caso de Javascripts, o caminho passa por usar o ScriptManager:

<pre lang="asp">&lt;asp:ScriptManager runat="server" LoadScriptsBeforeUI="true"&gt;
	&lt;Scripts&gt;
            &lt;asp:ScriptReference Path="~/Scripts/jquery-1.2.6-vsdoc.js" /&gt;
            &lt;asp:ScriptReference Path="~/Scripts/universal.js" /&gt;
        &lt;/Scripts&gt;
&lt;/asp:ScriptManager&gt;
</pre>

Neste caso, o ScriptManager gere a inclusão dos tags de script na página. Por outro lado, também seria possível usar o método **Page.ResolveUrl()** para obter a referencia correcta do caminho.

Se for necessário uma referência no code behind, a solução apresentada no <a href="http://www.gotjeep.net/Blogs/CommentView,guid,4be2f278-12e4-40d5-b154-0e8ecaf18fac.aspx" target="_blank">offroadcoder.com</a> que utiliza o método RegisterStartupScript() do ScriptManager.

**Sb2DevelopersBlog**  
A equipa de desenvolvimento da SB2 (francesa) mantém um blog (em inglês) muito muito com muitas amostras de código, como por exemplo o:

  * <a href="http://blog.sb2.fr/post/2008/12/28/Pluggable-Generic-ASPNET-Cache-Manager.aspx" target="_blank">Pluggable Generic ASP.NET Cache Manager</a>
  * <a href="http://blog.sb2.fr/post/2008/12/25/Generic-EventHandler-Raising-Extension-Method.aspx" target="_blank">Generic Eventhandler Raising Extension Method</a>
  * <a href="http://blog.sb2.fr/post/2008/12/25/Storing-ViewState-On-Server-Instead-Of-Client-With-ASPNET.aspx" target="_blank">Storing ViewState On Server Instead Of Client With ASP.NET</a>

Entre muitos outros. É um feed a manter no <a href="http://feeds.feedburner.com/sb2/JSrW" target="_blank">RSS</a>

**Google Reader**

E pro falar em RSS, decidi (e após muita insistência do Paulo Carrasco) começar a usar o Google reader, e verdade seja dita, até estou relativamente satisfeito. Gosto muito do GreatNews, mas o <a href="http://reader.google.com" target="_blank">reader do Google</a> está igualmente bom, e a partilha de posts é algo que representa uma mais valia.

os meus items partilhados estão em : <a href="http://www.google.com/reader/shared/03649456030336896792" target="_blank">http://www.google.com/reader/shared/03649456030336896792</a>. Pode eventualmente haver pro lá umas coisas NSFW ligadas à fotografia&#8230; mas é principalmente programação.

E para já, tá se bem. Há tanto que poderia escrever, mas tem de ser aos bocadinhos. Tenho andado &#8220;a correr&#8221; com as tarefas e efim. é código + código + código&#8230; Bom Ano!