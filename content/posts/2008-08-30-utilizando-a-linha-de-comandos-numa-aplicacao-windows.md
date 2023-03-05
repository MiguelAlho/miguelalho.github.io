---
title: Utilizando a linha de comandos numa aplicação Windows
author: Miguel Alho
type: post
date: 2008-08-30T09:54:39+00:00
url: /utilizando-a-linha-de-comandos-numa-aplicacao-windows/
categories:
  - 'Code &amp; IT'
tags:
  - Aplicações
  - Code Project
  - command prompt
  - linha de comando
  - Windows
  - windows foms

---
Depois de ver o <a href="http://miguelalho.com/?p=661" target="_blank">Ubiquity</a>, creio que se torna claro o poder de uma linha de comandos numa interface aplicacional, especialmente quando os comandos estão bem construídos. Se bem que muitos há muito terão anunciado a morte da linha de comandos em detrimento de interfaces gráficas, a verdade é que ainda não morreu e encontra-se frequentemente novas aplicações para ela. Repare que o Linux continua a ser fortemente dependente da linha de comandos, existe uma versão do Windows &#8211; Windows Server Core &#8211; que é completamente dependente da linha de comandos, o motores de busca geralmente aceitam parametros de uma forma similar a uma linha de comandos e agora temos o Ubiquity que facilita tarefas através de uma linha de comandos com uma linguagem natural e facilmente adaptável.

Associamos a linha de comandos a tarefas de sistema operativo, normalmente, mas é possível associá-lo a uma aplicação concreta. E não refiro os comandos de inicialização da aplicação; estou a falar de inserir uma linha de comandos dentro de uma aplicação, de modo a desenvolver tarefas aplicacionais e que tipicamente exigiria meia dúzia de cliques. Porque há tarefas que se tornam fatídicas com uma interface gráfica &#8211; para uma tarefa por vezes é necessário deslocar o rato constantemente de um ponto para outro, aceder a um menu, seleccionar um elemento, preencher uma caixa e clicar. E se poder simplesmente escrever uma linha de texto que descreve completamente a tarefa?

Imaginei que para o gerador de código, esse elemento poderia ser uma mais valia. Há tarefas que acredito que poderão ficar simplificadas através de uma comandos texto. Por exemplo, para criar um objecto, poder escrever algo do género:

add bo NomeDoObjecto

em vez de ter que seleccionar um módulo, clicar com o botão direito, clicar na opção do menu de contexto, preencher o campo do formulário e clicar em OK. É possível, e creio que será mais rápido &#8211; menos movimento de rato, menso cliques, menos transição entre teclado e rato.

Algo que reparei é que informação sobre estratégias de implementação de linhas de comandos são relativamente escaças na web (ou então segui o caminho errado em termos de palavras de pesquisa), em especial a inclusão de uma consola de comandos num formulário windows. Encontrei depois de algum tempo um componente muito interessante no <a target="_blank" href="http://www.codeproject.com/">Code Project</a> desenvolvido por um indiano &#8211; o <a target="_blank" href="http://www.codeproject.com/KB/miscctrl/commandprompt.aspx">Command Prompt Control</a> do _vikashparida_.

O control é algo simples e tem bastante funcionalidade. Apresenta a listagem de comandos inseridos (e resultados) como na consola tradicional, permite modificar o texto do _prompt_, e automáticamente divide o texto do comando pelos delimitadores que ele define (e que podem ser alterados). Tem três eventos associados, sendo o principal a manipular é o _Command_ que representa a inserção de um comando completo, mas tem outros que permitem controlar a aparição de _tooltips_ e afins. A implemantação do handler acaba por ser algo do género (por exemplo para um comando &#8220;date&#8221; que apresenta a data actual:

<pre lang="csharp">private void prompt1_Command(object sender, CommandEventArgs e)
{    
   if (e.Command == "date")    
   {         
	e.Message = "  The current date is : " + DateTime.Now.ToLongDateString();
	return;    
   }    

   // Show error message    
   e.Message = "'" + e.Command + "' is an unrecognized command";    
   // Don't store the command    
   e.Record = false;
}
</pre>

como demonstra o autor. A assemblagem está completa, e pode ser incorporado na toolbox do Visual Studio para inclusão em projectos.