---
title: 'Strings de formatação de DateTime em C# (VB)'
author: Miguel Alho
type: post
date: 2008-09-02T20:16:44+00:00
url: /strings-de-formatacao-de-datetime-em-c-vb/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - 'C#'
  - data hora
  - datetime
  - datetime format string
  - português
  - string

---
É uma tarefa comum formatar instâncias de data e hora nas aplicações para correcto armazenamento ou apresentação. Felizmente, o objecto _DateTime_ tem na sua definição métodos que permitem formatar o valor nos mais diversos formatos, o que simplifica o processo de apresentação de valores, mas não só. Por exemplo, numa base de dados SQLite (e não só), é conveniente enviar o valor da data e hora no formato correcto, para evitar erros nas operações.

O _DateTime_, como a generalidade das classes do framework, tem um método de _ToString()_, com diversos overloads, onde um deles é:

<pre lang="csharp">string DateTime.ToString(string format); 
</pre>

Este método admite uma _string_ de formatação do valor que retorna, no formato de _string_. Se bem que a construção da string não é de todo difícil, é importante escolher a correcta e ter o cuidado de não confundir alguns casos de maiúsculas e minúsculas. [Produzi um pequeno exemplo][1] no Snippet Compiler para demonstrar o conceito, e as diferenças entre cada caracter ou combinação:

<pre lang="csharp">public static void RunSnippet()
	{

		DateTime agora = DateTime.Now; //02-09-2008 19:43:25
		
		//ToString por defeito
		WL("null :" + agora.ToString()); //02-09-2008 19:43:25		
		
		//date / time completos
		WL("d : " + agora.ToString("d")); //02-09-2008
		WL("D : " + agora.ToString("D")); //terça-feira, 2 de Setembro de 2008
		WL("t : " + agora.ToString("t")); //19:43
		WL("T : " + agora.ToString("T")); //19:43:25
		WL("s : " + agora.ToString("s")); //2008-09-02T19:43:25
		WL("f : " + agora.ToString("f")); //terça-feira, 2 de Setembro de 2008 19:43
		WL("F : " + agora.ToString("F")); //terça-feira, 2 de Setembro de 2008 19:43:25
		WL("g : " + agora.ToString("g")); //02-09-2008 19:43
		WL("G : " + agora.ToString("G")); //02-09-2008 19:43:25
		WL("r : " + agora.ToString("r")); //Tue, 02 Sep 2008 19:53:20 GMT
		WL("u : " + agora.ToString("u")); //2008-09-02 19:53:20Z
		WL("U : " + agora.ToString("U")); //terça-feira, 2 de Setembro de 2008 18:53:20
		
		//formato de dia
		WL("dd : " + agora.ToString("dd")); //02
		WL("ddd : " + agora.ToString("ddd")); //ter
		WL("dddd : " + agora.ToString("dddd")); //terça-feira

		//formato do mês
		WL("m : " + agora.ToString("m")); //2-9
		WL("M : " + agora.ToString("M")); //2-9
		WL("MM : " + agora.ToString("MM")); //09
		WL("MMM : " + agora.ToString("MMM")); //Set
		WL("MMMM : " + agora.ToString("MMMM")); //Setembro
		
		//formato do ano
		WL("y : " + agora.ToString("y")); //Setembro de 2008
		WL("yy : " + agora.ToString("yy")); //08
		WL("yyyy : " + agora.ToString("yyyy")); //2008
		
		//horas
		WL("hh : " + agora.ToString("hh")); //07
		WL("HH : " + agora.ToString("HH")); //19
		
		//minutos
		WL("mm : " + agora.ToString("mm")); //45

		//segundos
		WL("ss : " + agora.ToString("ss")); //28
		WL("ff : " + agora.ToString("ff")); //04
		WL("fff : " + agora.ToString("fff")); //044
		WL("ffff : " + agora.ToString("ffff")); //0446
		WL("fffff : " + agora.ToString("fffff")); //04463 ...
		
		//outros
		WL("gg : " + agora.ToString("gg")); //D.C.
		WL("tt : " + agora.ToString("tt")); // (deveria ser PM)
		WL("zz : " + agora.ToString("zz")); //+01
		WL("zzz : " + agora.ToString("zzz")); //+01:00
		
		//toString parametrizados e separadores
		WL("HH:mm:ss:ffff : "+ agora.ToString("HH:mm:ss:ffff")); //19:43:25:0446
		WL("dd/MM/yyyy : "+ agora.ToString("dd/MM/yyyy")); //02/09/2008
		WL("dd 'de' MM em yyyy : "+ agora.ToString("dd 'de' MM 'em' yyyy 'às' HH e mm")); //02 de 09 em 2008 às 19 e 43
		WL(agora.ToString("dd-MM-yyyy HH:mm:ss"));	//02-09-2008 19:43:28
		
	}
</pre>

Naturalmente, dada a especificidade dos nomes dos dias e meses, e mesmo a formatação das horas, os valores retornados pelo _ToString()_ depende fortemente do da informação de cultura definido no _System.Globalization_ do sistema onde corre as instruções. Este caso é para Português de Portugal.

Revendo o exemplo:

  * Sem parmetro de formatação, obtemos um valor de data e hora perfeitamente valido para uso comum.
  * Existe um conjunto de caracteres singulares que produzem uma data e hora completa. Dessas, apenas o &#8216;s&#8217; e o &#8216;u&#8217; produzem saídas ordenáveis. 
  * O &#8216;U&#8217; tem comportamento estranho e deve ser evitado. 
  * Os componentes individuais tem diversas formatações possíveis, dependendo do número de vezes que o caracter é usado.
  * &#8216;f&#8217; representa fracções de segundo. Cada &#8216;f&#8217; acrescentado representa uma casa decimal extra na representação
  * É muito importante não confundir &#8216;M&#8217; de meses com &#8216;m&#8217; de minutos. É um erro que nem sempre é fácil de detectar ou cujo resultado é evidentemente errado.
  * É possível combinar os parâmetros de modo a criar novos que nos sirvam. Como separadores, podem ser usados o mais diversos caracteres, como espaços, dois pontos, barras, ou palavras. No caso de palavras, quando contem caracteres já definidos para formatação (como o &#8216;d&#8217;, o &#8216;m&#8217;, etc.) é importante envolver o(s) caracter(es) em aspas como &#8216;às&#8217;. Basta envolver caracter crítico (como à&#8217;s&#8217;) mas é mais legível envolver a palavra completa.

E assim fica. Para consultar mais info sobre o assunto, o <a href="http://blog.stevex.net/index.php/string-formatting-in-csharp/" target="_blank">Steve Tibbett tem no blog uma página dedicado a formatação com strings</a>, incluindo outros tipos de dados, e há sempre a <a href="http://msdn.microsoft.com/en-us/library/az4se3k1.aspx" target="_blank">documentação da MSDN</a>.

 [1]: http://miguelalho.com/wp-content/uploads/2008/09/datetimeformattest.zip