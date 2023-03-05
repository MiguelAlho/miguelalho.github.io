---
title: ASP.Net – Método alternativo de internacionalização (i18n)
author: Miguel Alho
type: post
date: 2010-08-25T00:41:32+00:00
url: /asp-net-metodo-alternativo-de-internacionalizacao-i18n/
wordbooker_options:
  - 'a:8:{s:18:"wordbook_noncename";s:10:"b2f17b66cd";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";s:23:"wordbook_default_author";s:1:"2";s:23:"wordbook_extract_length";s:3:"256";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";}'
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - i18n
  - internactionalização
  - l10n

---
A internacionalização de aplicações (internationalization &#8211; i18n) para posterior localização (localization &#8211; l10n) é um processo multi-passo, que envolve a detecção de texto que deve ser apresentado em múltiplas línguas, bem como a preparação da base de código para que o texto possa ser correctamente traduzido.

Cada tecnologia tem a sua própria forma de implementar mecanismos que auxiliam a tradução. .Net recorre a recursos compilados na forma de ficheiros .resx . Os ficheiros .resx são de uma forma geral pares chave-valor, estruturados em XML, que são compilados para dlls carregados no inicio da aplicação.

<!--more-->

Apesar de uma solução interessante, é extremamente tediante usa-los em ASP.Net. Com as bibliotecas de uma API, ok. O suporte a nível de código C# (ou VB ) torna o uso relativamente simples. Já com ASP.Net, não é possível dizer o mesmo. As razões estão bem descritas em &#8220;<a href="http://www.expatsoftware.com/articles/2010/03/why-internationalization-is-hopelessly.html" target="_blank">Why Internationalization is hopelessly Broken in ASP.Net</a>&#8220;, mas resumindo, tudo tem de ser controlos ASP.Net com o runat=&#8221;server&#8221;, não há suporte simplificado a texto estático (escrito directamente na página), a referência de chaves das strings obriga a inserir meta:resoursekey=&#8221;&#8221; em todas os controlos, e os ficheiros .resx não são facilmente legíveis por pessoal não técnico.

A alternativa? Seguir o método do &#8220;resto do mundo&#8221; e usar ficheiros de strings em formatos simplificados, nomeadamente os ficheiros .po. Quem já explorou um projecto open-source ou projectos aplicacionais com add-ins linguísticos, muito provavelmente já se cruzaram com este formato de ficheiros. São ficheiros de texto simples, com texto assoiado a uma chave (msgid) e texto associado à tradução (msgstr). Por exemplo:

<pre lang="text">msgid "Pesquisar Candidatos"
msgstr "Search Candidates"</pre>

De notar neste exemplo que a chave está em PT e a tradução em EN. Não é recomendado esta abordagem, uma vez que as línguas latinas tem acentos que não são óptimos para chaves.

Continuando, o uso de ficheiros .PO, noutras frameworks, é suportado por por uma função gettext(), onde é passo a chave (geralmente o próprio texto, na língua base) e retorna a versão traduzida, conforme as preferências de utilizador. Melhor, há geralmente uma forma simplificada da função escrito _(). Assim, cada string a analisar chama a função gettext(), obtendo a versão traduzida do texto. Bem mais simples que os .resx e meta:resourcekey do .NET.

## Usando Fairlylocal

O <a href="http://www.fairtutor.com/fairlylocal/" target="_blank">FailyLocal </a> é um projecto opensource iniciado por Jason Kester, e que implementa a funcionalidade do gettext e ficheiros PO para ASP.Net. Como descrito no link do projecto, para pedir a tradução, na página basta utilizar:

<pre lang="asp">&lt;%= _("texto a traduzir") %&gt;
</pre>

ou no codebehind:

<pre lang="csharp">controlo.InnerHtml = _("texto a traduzir");</pre>

Tão simples e eficaz. Em termos de biblioteca, basta adicionar os ficheiros ao projecto da aplicação (têm de ser um projecto de aplicação web, em vez de um projecto de website, uma vez que o segundo não <a href="http://www.fairtutor.com/fairlylocal/fairlylocal-details#basepage" target="_blank">permite evento post-build, que o primeiro permite, e que constrói dinamicamente os ficheiros PO</a>).

Em termos de execução, os ficheiros PO são carregados no arranque da aplicação, para um dicionário estático em memória, onde depois são realizadas as consultas do gettext(). Neste caso, todas as paginas devem herdar de InternationalPage ou InternationalMaster (ou a página base da aplicação derivar destas) em vez de Page directamente, para que _() fique disponível.

Para as minhas necessidades, efectuei pequenas alterações. Primeiro, para facilitar o uso em mutlipos projectos, adicionai os ficheiros ao projecto da minha framework, para que tenha sempre disponível. Segundo, porque necessito de aceder aos métodos, quer para a página instanciada, quer quando estou a usar WebMethods, tornei a propriedade LanguageCode e o método _() static. Finalmente, porque necessito de definir a língua ao nível da aplicação e não para cada utilizador, modifiquei o getter de LanguageCode para obter o código de lingua directamente da thread, em vez da sessão do utilizador:

<pre lang="csharp">public static string LanguageCode
{
   get
   {
       if (_languageCode == null)
       {
           _languageCode = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName.ToLower();
	}
	return _languageCode;
    }
    set
    {
	_languageCode = value;
    }
}</pre>

Incluíndo os scripts post-build, os ficheiros .PO são actualizados após cada build, podendo ser traduzidos de seguida, ou directamente ou recorrendo a programas como o <a href="http://www.poedit.net/index.php" target="_blank">Poedit</a>. Ferramentas como o <a href="http://pepipopum.dixo.net/" target="_blank">Pepipopum </a>permitem traduzir automaticamente (via Google translate API) o ficheiro PO. As chaves devem estar em inglês para funcionar correctamente.

Finalmente, convém referir que o sistema tem fallback automático para o texto da própria chave &#8211; se o testo não estiver traduzido no ficheiro .po, o texto a usar é o próprio argumento do _(), o que ajuda muito se tiver a internacionalizar uma aplicação já existente.

## Uma macro util

Ainda assim, com esta simplificação, internacionalizar uma aplicação existente não deixa de ser um trabalho tediante. Localizar o texto a traduzir e envolver no método _() é &#8220;chato&#8221;.

Para ajudar, criei uma macro:

<pre lang="vb">Public Module GetTextSurrond

    '' surround text with &lt;%= _(" and ") $&gt;
    ''
    Sub ASPXSurround()
        Dim textSelection As EnvDTE.TextSelection

        textSelection = DTE.ActiveDocument.Selection()
        textSelection.Text = "&lt;%= _(""" + textSelection.Text + """) " ''%&gt;"
    End Sub

End Module
</pre>

Associando esta Macro a um atalho de teclas, por exemplo Ctrl+G, Ctrl+T, ajudará a usar a macro, bastando seleccionar o texto e executar o comando (nos ficheiros .aspx).

## Conclusão

Pela curta experiência que tenho com esta alternativa de tradução, não deixa de me parecer N vezes mais vantajoso que o uso de ficheiros .resx no ASP.Net. Os ficheiros .PO são apenas strings, portanto para recursos binários outra estratégia deve ser adoptada.