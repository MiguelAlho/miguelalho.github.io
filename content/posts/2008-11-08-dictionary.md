---
title: 'Dictionary<,>'
author: Miguel Alho
type: post
date: 2008-11-09T00:08:21+00:00
url: /dictionary/
categories:
  - 'Code &amp; IT'
tags:
  - .net
  - 'C#'
  - Classes
  - Dictioanry

---
O tipo Dictionary do framework .NET é uma classe muito útil para armazenar listas de objects que queremos que sejam identificáveis por uma chave. O tipo pertence ao namespace System.Collections.Generic da framework e partilha alguns métodos das listas genéricas.

O tipo Dictionary, para cada item, contem dois valores &#8211; TKey, que é uma chave e que pode ser de diversos tipos (string, int, ..) e o TValue que é o valor para para a chave. Cada chave tem de ser único, naturalmente para que o registo possa ser referenciado correctamente. A chave não pode ser nulo, mas o valor pode. O Dictionary implementa ainda uma série de interfaces, nomeadamente o IDictionary, ICollection e IEnumerable, da qual podemos aproveitar as potencialidades.

Tive de utilizar esta class hoje, no desenvolvimento de uma class, e a verdade é que simplficou muito o trabalho que estava a fazer. De uma forma simplificada, estava a tentar extrair palavras de um texto, e que estavam entre um conjunto de palavras chave. Tratava-se de dados pessoais em currículos, e onde eu queria extrair, por exemplo, o nome, a data de nascimento, a nacionalidade, o sexo, etc&#8230; Então após a extracção do conjunto de palavras, armazenei-os num Dictionary<string, string> &#8211; a chave era a string da palavra chave que precedia o texto que procurava, e o valor era o texto que procurava.

Depois de instanciar o Dictionary:

<pre lang="csharp">Dictionary&lt;string, string&gt; dictionary1 = new Dictionary&lt;string, string&gt;();
&lt;/pre&gt;

podemos adicionar pares chave / valor:

&lt;pre lang="csharp"&gt;
dictionary1.Add("chave1","valor da chave1");
dictionary1.Add("maçãs","a chave pode ser o nome de um item, e o valor a descrição, por exemplo");
dictionary1.Add("além","disso a chave pode conter acentos e outros tipos de carácteres");
&lt;/pre&gt;

O Dictionary, como as listas genéricas permitem ordenação:

&lt;pre lang="csharp"&gt;
dictionary1.Sort();
&lt;/pre&gt;

onde podemos usar comparadores. Mas mais importante, podemos obter um valor através da chave como também usar métodos da interface IDictionary, como o "Contains" ou "TryGetValue"

&lt;pre lang="csharp"&gt;
string valor1 = dictionary1["além"];

if(dictionary1.Contains("chave1")) 
	Console.WriteLine("chave1 existe no dicionário");

Dim value As String = ""
If openWith.TryGetValue("maçãs", out value) Then
        Console.WriteLine("For key = \"tif\", value = {0}.", value)
</pre>

Com estas características, quando é necessário um par chave-valor, o Dictionary é uma óptima escolha, como também é exemplo da potencialidade do framework .NET.