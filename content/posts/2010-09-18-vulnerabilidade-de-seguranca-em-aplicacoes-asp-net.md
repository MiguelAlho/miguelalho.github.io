---
title: Vulnerabilidade de Segurança em Aplicações ASP.NET
author: Miguel Alho
type: post
date: 2010-09-18T15:16:10+00:00
url: /vulnerabilidade-de-seguranca-em-aplicacoes-asp-net/
wordbooker_options:
  - 'a:8:{s:18:"wordbook_noncename";s:10:"d5e0c3857b";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";s:23:"wordbook_default_author";s:1:"2";s:23:"wordbook_extract_length";s:3:"256";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";}'
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - segurança

---
Foi <a href="http://http://www.microsoft.com/technet/security/advisory/2416728.mspx" target="_blank">anunciado ontem pela Microsoft uma vulnerabilidade no ASP.Net</a>, associado a códigos de erro enviados pelo servidor para o cliente, e que poderiam servir para o utilizador aceder a ficheiros como o web.config.

No blog do Scott Guthrie, ele refere como funciona a vulnerabilidade e como prevenir. Essencialmente, é necessário reeditar os ficheiros web.config, e configurar de modo a ter os &#8220;custom errors&#8221; activos. Ele recomenda ainda ter apenas uma página de erro, e injectar algum código no Page_load dessa página:

<pre lang="asp">&lt;%@ Page Language="C#" AutoEventWireup="true" %>
&lt;%@ Import Namespace="System.Security.Cryptography" %>
&lt;%@ Import Namespace="System.Threading" %>






    

<div>
  An error occurred while processing your request.
      
</div>


</pre>

O detalhe completo em: <a href="http://weblogs.asp.net/scottgu/archive/2010/09/18/important-asp-net-security-vulnerability.aspx" target="_blank">http://weblogs.asp.net/scottgu/archive/2010/09/18/important-asp-net-security-vulnerability.aspx</a>