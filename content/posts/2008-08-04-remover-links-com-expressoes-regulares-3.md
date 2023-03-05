---
title: Remover links com expressões regulares
author: Miguel Alho
type: post
date: 2008-08-04T08:11:22+00:00
url: /remover-links-com-expressoes-regulares-3/
categories:
  - 'Code &amp; IT'
tags:
  - Regex

---
Numa aplicação que tenho em desenvolvimento, uma das funcionalidades que existe é a exportação de tabelas para Excel. Há algum tempo que tenho desenvolvido uma pequena classe que que exporta objectos do tipo GridView, DetailsView e agora Table de uma pagina ASP.Net para Excel. A classe renderiza o código do controlo e transforma-o numa worksheet de Excel, com um controlo por folha. A limitação existente é que o ficheiro gerado (que é XML) apenas é legível no Microsoft Excel. Outros editores como o OpenOffice não interpretam correctamente o XML (nem consegue gerar um XML correcto para abrir no Excel).

Um detalhe/feature que adicionou agora foi a extracção de links. Quando a célula da tabela incluía um link, o HTML desse link era escrito na célula da folha excel. Por exemplo, poderia ter:

`2 / EP / <a href="/APP/form.aspx?queryparam=1" alt="">1</a>`

Em vez do simples `2 /EP / 1` que desejava na celula.

Porque a string que deve ser removida (ou substituída) é algo complexa de encontrar, o uso de Regex é o caminho a seguir para a sua substituição.

Não estou muito habituado a trabalhar com REGEX, portanto nada melhor que usar um aplicativo auxiliar. Neste caso o [RegexBuddy][1] que é MARAVILHOSO para esta tarefa. O RegexBuddy permite criar e testar expressões regulares. Mas o forte do aplicativo é a capacidade didáctica que tem (no painel de criação, a expressão que introduzimos é explicado bloco a bloco, detalhe a detalhe), e ainda gera o código necessário para o usar na linguagem favorita, incluindo o C#. A experimentar, sem dúvida!

[<img class="aligncenter size-full wp-image-532" title="regex11" src="http://miguelalho.com/wp-content/uploads/2008/08/regex11.jpg" alt="" width="500" height="322" />][2]  
No caso da substituição do, criei a seguinte: 

<pre lang="reg">&lt;a\s[^>]*&lt;([^>]*)&lt;/a></pre>

O primeiro grupo <a\s[^>]\*> procura o texto <a &#8230;.> (começa com <a e um espaço, depois pode ter quais quer caracteres até ao > e finalmente encontra o > ). O segundo grupo ([^<]\*) é próprio texto do link e que me interessa conservar. De notar, que estando entre parentes, pode ser recuperado por &#8220;backreference&#8221;; O Terceiro grupo é a tag que fecha o link, ou seja o &#8220;<\a>.

O RegexBuddy, &#8220;buddy&#8221; como realmente é, ainda cria o código para o colcoar no C#:

<pre lang="csharp">ResultString = Regex.Replace(SubjectString, "&lt;a\\s[^>]*&lt;([^>]*)&lt;/a>", "$1");</pre>

que devolve a string que substitui os links existentes pelo texto no próprio link, e substitui todas as instâncias de link existentes no mesmo. O $1, no ultimo parâmetro, representa a primeira &#8220;backreference&#8221;, que neste caso é o texto devolvido pelo ([^<^]*).

 [1]: http://www.regexbuddy.com/
 [2]: http://miguelalho.com/wp-content/uploads/2008/08/regex11.jpg