---
title: 'Byte[] para String e vice-versa'
author: Miguel Alho
type: post
date: 2008-09-30T20:18:10+00:00
url: /byte-para-string-e-vice-versa/
categories:
  - 'Code &amp; IT'
tags:
  - 'Code &amp; IT'
  - conversão
  - string

---
Algo que geralmente me faz confusão é a conversão de tipos. Especialmente coisas que são supostamente simples, como a conversão de um _array_ de bytes para string, ou uma string para um _array_ de bytes. Éra de, à primeira vista, e tendo em conta a forma como é armazenado um caracter de uma string &#8211; é geralmente um byte, em que o valor do byte é o código do caracter &#8211; considerar que a convesão poderia ser feita directamente, através de um cast. Algo como:

<pre lang="csharp">string a = (string)b; //em que b é um byte[]
</pre>

Pois, mas não dá. E a explicação até é simples &#8211; a codificação dos caracteres pode ser do mais diverso (ASCII, UTF7, UTF8, UTF16, UTF32&#8230;), e é importante que o programa conheça qual. Assim sendo, é de aproveitar os _encoders_ que o .NET oferece no _namespace_ System.Text.

Para converter de string para byte[]:

<pre lang="csharp">System.Text.ASCIIEncoding  encoding = new System.Text.ASCIIEncoding();
byte[] bArray = encoding.GetBytes(str);
</pre>

e de byte[] para string:

<pre lang="csharp">System.Text.ASCIIEncoding enc = new System.Text.ASCIIEncoding();
string str = enc.GetString(bArray);
</pre>

Neste caso é usado a mapeamento de valores para caracteres ASCII.