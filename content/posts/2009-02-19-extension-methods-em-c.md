---
title: 'Extension Methods em C#'
author: Miguel Alho
type: post
date: 2009-02-19T11:15:14+00:00
url: /extension-methods-em-c/
categories:
  - 'Code &amp; IT'
tags:
  - 'C# 3.0'
  - extension methods

---
Um conceito muito interessante e novo no C# 3.0 e que tem aparecido bastante em blogs ultimamente é o conceito de métodos de extensão. Penso que já todhttp://miguelalho.com/wp-admin/post.php?action=edit&post=867&message=1&\_wp\_original\_http\_referer=http%3A%2F%2Fmiguelalho.com%2F%3Fp%3D867os nós tivemos que escrever um ou outro conjunto de métodos estáticos auxiliares para manipular objectos, normalmente agrupados em classes com o nome de &#8220;Helper&#8221;. Métodos de extensão são essencialmente isso mesmo, mas com uma sintaxe mais amigável.

Um exemplo é certamente a melhor forma de explicar.

Supomos um método de manipulação de strings, que não está presente na classe String do .NET, por exemplo, que conte o número de caracteres de espaço na string. Sem métodos de extensão, eu primeiro criava uma classe auxiliar com o método estático para efectuar o passo:

<pre lang="csharp">public class StringHelper
{
	public static int GetSpaceCount(string s)
	{
		int output = 0;
		foreach(char c in s)
		{
			if(c==' ')
				output++;
		}
		return output;
	}
}
</pre>

GetSpaceCount é estático, recebe uma string (a que tem os espaços que queremos contar), e retorna o inteiro com o número de espaços. Para o utilizar, posso fazer o seguinte, por exemplo:

<pre lang="csharp">string text = "Olá tudo bem?";
Console.WriteLine(StringHelper.GetSpaceCount(text));
</pre>

Onde o método é chamado da forma _&#8220;nome da classe&#8221;.&#8221;nome do método estático&#8221;(&#8220;parâmetro&#8221;)_.

O métodos de extensão oferecem uma forma mais elegante e natural de chamar o método, como se pertencesse à classe da instância, ou seja, poderia escrever a mesma linha da seguinte forma:

<pre lang="csharp">Console.WriteLine( text.GetSpaceCount());
</pre>

Repara que text.GetSpaceCount() é bem mais simples, legível e intuitivo que StringHelper.GetSpaceCount(text) . Para criar o método de extensão temos passos semelhantes ao da criação do helper, mas com uns detalhes a ter em conta. Primeiro, a própria classe de suporte é estático. Segundo, o método é também estático. Terceiro, na lista de parâmetros, o primeiro parâmetro é do tipo sobre a qual trabalhamos e deve ser precedido da palavra &#8220;this&#8221;. Portanto a mesma classe reescrita fica:

<pre lang="csharp">public static class StringExtension
{
	public static int GetSpaceCount(this string s)
	{
		int output = 0;
		foreach(char c in s)
		{
			if(c==' ')
				output++;
		}
		return output;
	}
}</pre>

É necessário que o projecto tenha a referência ao System.Core.dll. A partir daqui, o método fica disponível para trabalhar directamente sobre strings e com suporte intellisense e tudo.

O conceito é óptimo para construir classes que possam ser reaproveitados em diversos projectos e incluídos num framework, e ajudar a manter um código limpo e legível. Basta depois referir a DLL e/ou namespace que contem os métodos.

Como referencias, seguem alguns links:

  * <a href="http://aspnet.4guysfromrolla.com/articles/021809-1.aspx" target="_blank">4 Guys from Rolla</a>
  * <a href="http://blog.gadodia.net/extension-methods-in-vbnet-and-c/" target="_blank">Extension Methods in Vb.Net and C# @ Habitually Good</a>
  * <a href="http://weblogs.asp.net/dwahlin/archive/2008/01/25/c-3-0-features-extension-methods.aspx" target="_blank">C# 3.0 Features: Extension Methods </a>
  * <a href="http://goneale.wordpress.com/2009/02/17/15-helpful-net-extension-methods-to-increase-productivity/#" target="_blank">15 Helpfull .NET Extension Methods to Increase Productivity</a>

<div class="zemanta-pixie">
  <img class="zemanta-pixie-img" src="http://img.zemanta.com/pixy.gif?x-id=ed4ed356-71c5-474c-b832-8b66ddc73b70" alt="" />
</div>