---
title: DSL-Tools – Ferramentas para contruir Ferramentas
author: Miguel Alho
type: post
date: 2008-09-10T13:06:33+00:00
url: /dsl-tools-ferramentas-para-contruir-ferramentas/
categories:
  - 'Code &amp; IT'
tags:
  - Domain-Specific Languages
  - DSL
  - DSL-Tools
  - Visual Studio

---
Os DSL (_Domain Specific Language_s) existem há muito para nos facilitar a vida. São linguagens que nos permitem manipular processo em domínios específicos. SQL e HTML são bons exemplos de DSLs &#8211; SQL permite desenvolver métodos de manipulação de dados no domínio das bases de dados (e não serve para mais) como também o HTML permite formatar dados para visualização num _browser_ mas pouco mais. Contrariamente, C#, C++ entre outros são linguagens mais generalizadas, que permitem trabalhar em muitos domínios.

Nos últimos dias tenho estado a pesquisar alguns conceitos e componentes e de alguma forma tropecei nos DSL Tools para o Visual Studio. Até recentemente, as DSLs eram puramente textuais, mas agora começa a ser mais frequentemente ter DSLs gráficos. A vantagem é que, além de permitirem expressar melhor conceitos de um domínio específico, as suas representações poderem ser expressas por código de uma forma automatizada (com geradores de código). Um bom exemplo deste processo creio ser os diagramas de classes que o VS suporta actualmente onde a manipulação do gráfico gera código especifico ao domínio das classes, ou os diagramas de bases de dados do SQL Server, que tem o mesmo comportamento.

Agora, o VS suporta a criação de DSLs através da DSL-Tools &#8211; são projectos <a target="_blank" href="http://www.microsoft.com/DOWNLOADS/details.aspx?familyid=30402623-93CA-479A-867C-04DC45164F5B&displaylang=en">incluídos no SDK de extensão do VS</a>, e que permitem criar aplicações semelhantes ao VS, mas específicos ao domínio da aplicação que se pretende construir. Um diagrama de classes como no UML nem sempre é capaz de demonstrar os conceitos que uma DSL consegue. E a capacidade de gerar código é uma mais valia, sem dúvida. E logo pensei no gerador de código&#8230; porque não tentar implementa-lo no DSL, onde o domínio é, para efeitos concretos, a framework que pretendo implementar?

Como é natural, começar com conceitos novos não é nada simples, mas achei um artigo/tutorial muito interessante no <a target="_blank" href="http://www.linhadecodigo.com.br">Linha de Código</a>. O artigo &#8220;<span id="ctl00_ContentPlaceHolder1_lblTitulo1"></span>[DSL Tools &#8211; Melhore sua produtividade através de linguagens visuais de domínio-específico no Visual Studio.NET][1]&#8221; (vou a meio) apresenta muito bem os conceitos das DSLs esclarecendo muitas questões que me surgiram nos primeiros artigos que encontrei. Muito completo, muito explicado, e com um exemplo simples, mas completo. Mas não é unico, naturalmente. Um outro artigo de interesse é o <a target="_blank" href="http://www.code-magazine.com/Article.aspx?quickid=0710072">Domain-Specific Development in Visual Studio no Code-Magazine</a>. E há sempre o <a target="_blank" href="http://msdn.microsoft.com/pt-br/vsx/bb980955%28en-us%29.aspx">Learn Visual Studio Extensability do MSDN</a>.

Parece-me uma área muito interessante e com verdadeiro potencial em termos de produtividade&#8230;

 [1]: http://www.linhadecodigo.com.br/Artigo.aspx?id=1456