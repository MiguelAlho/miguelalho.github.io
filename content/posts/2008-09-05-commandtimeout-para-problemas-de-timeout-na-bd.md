---
title: CommandTimeout para problemas de timeout na BD
author: Miguel Alho
type: post
date: 2008-09-05T16:20:09+00:00
url: /commandtimeout-para-problemas-de-timeout-na-bd/
categories:
  - 'Code &amp; IT'
tags:
  - SQLServer

---
Um pequeno problema que resolvi hoje, que deve ser comum com alguns procedimentos pesados em bases de dados, é da própria base de dados lançar um _timeout_ num procedimento. Para todos os efeitos, as bases de dados como SQL Server geralmente definem um período finito de tempo máximo para a ligação, de modo a evitar potenciais _deadlocks_ ao sistema. O &#8220;erro&#8221; por _timeout_ eleva uma excepção que no SQL Server 2005 é do tipo _SQLException_:

> _System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. at System.Data.SqlClient.SqlInternalConnection_

Entre a base de dados e o .NET, há algumas formas em como podemos alterar o período máximo de execução. O primeiro é na própria base de dados, onde podemos alterar o valor pro defeito. No caso do SQL Server (Express 2005) o valor é de 600s. Se colocarmos o valor 0, o timeout é anulado e a base só termina quando finalizar o procedimento que tem em curso, o que pode ser perigoso. 

Em termos de .NET e no caso de estar a utilizar a classe SQLCommand para enviar os comandos à base de dados, uma hipotese é na _connection string_ usada para ligar à base de dados inserir um Connect Timeout do tipo:

<pre lang="xml">&lt;add key="DBConnection" value="server=LocalHost;uid=sa;pwd=;database=DataBaseName;Connect Timeout=200; pooling='true'; Max Pool Size=200"/> </pre>

Isto naturalmente impõe o tempo de _timeout_ para todas as ligações efectuadas com a string, o que poderá ter ou não interesse. Se a alteração interessar apenas a um comando ou um conjunto restrito de comandos, será de todo o interesse estabelecer o timeout para apenas o comando em questão. Inicializando um _SqlCommand_ sobre uma conecção, é possível definir o timeout:

<pre lang="csharp">SQLConnection con = new SQLConnection(connectionString);
//definir a query se necessário
SQLCommand cmd = new SQLCommand(query, con);
cmd.CommandTimeout = 500;
//(...)
</pre>

É também importante notar que o _Command Timeout_ é diferente de um _Connection Timeout_. _Connection Timeout_ está ligado ao processo de tentativa de ligação, enquanto o _Command Timeout_ está ligado ao procedimento em si, e o tempo que demora a completar. Definir o _CommandTimeout_ resultou para o meu problema onde adicionei a linha de código com um tempo suficientemente alto (e definido no ficheiro de configuração).

Mais info pode ser encontrado em:  
<a href="http://forums.asp.net/t/903456.aspx" target="_blank">Forums.ASP.Net</a>  
<a href="http://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx" target="_blank">CommandTimeout no MSDN</a>  
<a href="http://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlconnection.connectiontimeout.aspx" target="_blank">ConnectionTimeout no MSDN</a>