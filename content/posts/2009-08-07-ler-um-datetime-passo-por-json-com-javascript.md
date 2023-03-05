---
title: Ler um DateTime, passo por JSON, com Javascript
author: Miguel Alho
type: post
date: 2009-08-07T14:02:43+00:00
url: /ler-um-datetime-passo-por-json-com-javascript/
categories:
  - 'Code &amp; IT'
tags:
  - datetime
  - Javascript
  - JSON
  - WCF
  - webservice

---
O título é algo confuso, mas o problema é pertinente. Quando se utiliza JSON para transmitir dados entre servidor e browser-cliente, enquanto a generalidade de objectos serializáveis são convertidos correctamente (um inteiro é um inteiro, uma string é uma string&#8230;), os valores de datas não são convertidos como um tipo Date do javascript, nem DateTime do .NET. A generalidade dos valores passos no JSON são convertidos para uma forma literal correspondente &#8211; os números numa sequência de caracteres numéricos, as strings como sequência de caracteres, etc. Mas para o tipo que representa uma data, simplesmente não existe um literal único &#8211; basta pensar que há dezenas de formas de representar uma data &#8211; só a parte do calendário, ou com hora, ou com variações de barras e hifens, ou com a troca de texto e ordem com base em culturas variadas.

Na serialização JSON do .NET, a Microsoft decidiu adoptar uma convenção para enviar os dados num formato literal &#8211; a data é representada pelo número de milissegundos desde a data de referência de 1 de Janeiro de 1970. O formato em que é enviado é:

/Date(xxxxxxxxxxxxx)/

Evidentemente, esta string não é uma data por si só, apenas uma representação literal (e independente de constrangimentos culturais). Necessitamos de converter os ticks numa data. No javascript o único método de conversão que temos é a função Date(), que funciona como construtor do objecto da Data. Dos vários overloads que existem, há um que recebe o número de milisegundos desde 1 de Janeiro de 1970, tal como o número que recebemos da resposta JSON!

Sendo assim, podemos usar a seguinte função para obter a estrutura da Data, a partir do literal devolvido pelo servidor:

<pre lang="javascript">function GetJSDateFromNET(netDateString) {
    var myregexp = /\d{13}/;
    var match = myregexp.exec(netDateString);
    if (match != null) {
        return new Date(parseInt(match[0]));
    } else {
        return null;
    }
}
</pre>

Assim, com o valor devolvido pode-se fazer algo do género:

<pre lang="javascript">...
var data = GetJSDateFromNET(valorDataJSON);
alert(data.getYear());
...
</pre>

A função, reutilizável, permite ler correctamente o literal de data enviado pelo servido, e processar a data do lado do cliente.

referencias:  
[An Introduction to JSON @ MSDN][1]  
[Encosia][2]

 [1]: http://msdn.microsoft.com/en-us/library/bb299886.aspx
 [2]: http://encosia.com/2009/04/27/how-i-handle-json-dates-returned-by-aspnet-ajax