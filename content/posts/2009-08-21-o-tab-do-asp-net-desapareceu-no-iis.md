---
title: O tab do ASP.NET desapareceu no IIS!!??
author: Miguel Alho
type: post
date: 2009-08-21T11:36:05+00:00
url: /o-tab-do-asp-net-desapareceu-no-iis/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - IIS

---
Não aconteceu comigo, mas ontem foi, talvez, a segunda vez que ouvi falar desta situação. um colega ontem teve o mesmo problema e ajudei a encontrar uma solução para o problema. Aparentemente, até é um problema comum e parece estar relacionado com um parâmetro de configuração de módulos 32bits em 64bits, e especialmente de Windows Server 2003 instalado num VMWare Server. A solução está descrita em vários locais da web &#8211; vou apenas transcrever/traduzir a solução para que possa ser útil a mais pessoal.

  1. Pare o serviço do IIS
  2. Localize o ficheiro &#8220;metabase.xml&#8221; em _c:\windows\system32\inetsrv_ e abra-o no notepad ou outro editor
  3. Procure a linha que tenha a chave &#8220;Enable32BitAppOnWin64&#8221; e elimine-o
  4. Guarde o ficheiro
  5. Reinicie o IIS (e restantes serviços que possam ter parado)
  6. Verifique que a tab existe

Esta solução está em <http://www.bobnedved.com/post/2007/12/My-ASPNET-Tab-is-missing-in-IIS!!.aspx>