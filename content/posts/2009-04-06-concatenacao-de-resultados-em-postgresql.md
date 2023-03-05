---
title: Concatenação de resultados em PostgreSQL
author: Miguel Alho
type: post
date: 2009-04-06T23:22:23+00:00
url: /concatenacao-de-resultados-em-postgresql/
categories:
  - 'Code &amp; IT'
tags:
  - array_to_string
  - base de dados
  - COALESCE
  - concatenação
  - PostgreSQL
  - to_tsvector

---
Hoje enquanto tentava solucionar uma funcionalidade de uma aplicação que estou a desenvolver, surgiu a necessidade de encontrar forma de concatenar, em string, os valores retornados por várias linhas de uma pesquisa. A ideia era poder juntar num só campo, um vector léxico (tsvector do PostgreSQL) os campos texto de várias tabelas referentes à mesma entidade, para permitir uma pesquisa por texto livre centralizada.

Por exemplo supomos uma tabela simples &#8220;pessoa&#8221; com campos id\_pessoa, nome e localidade (os últimos dois são varchars). Supomos também uma segunda tabela &#8220;notas&#8221;, com campos id\_nota, id\_pessoa, e texto, onde id\_pessoa é chave externa e texto é do tipo text. Assumimos ainda que o campo notas pode ter mais que um registo por pessoa, como também pode não existir nota. A minha intenção é que consiga concatenar os vários campos de texto e vectorizar esse texto, para pesquisar nele usando o TSearch2 do PostgreSQL.

O PostgreSQL tem algumas funções úteis para isto, nomeadamente to\_tsvector, COALESCE, array\_to_string, array e o operador de concatenação ||.

Analisando primeiro pessoa, para concatenar o texto, teria qualquer coisa do género:

<pre lang="sql">SELECT nome || ' ' || localidade FROM pessoa
</pre>

o operador || efectua a concatenação do texto das colunas (que são dum tipo que represente texto). A operação anterior é efectuada para cada linha da tabela. Para vectorizar, aplico o to_tsvector:

<pre lang="sql">SELECT to_tsvector((SELECT nome || ' ' || localidade FROM pessoa WHERE id_pessoa = xxx))
</pre>

O to_tsvector vectoriza a string léxicamente, mas exige que a entrada seja um campo (texto) apenas, daí indicar um WHERE clause que garante um campo apenas. O segundo par de parenteses também é necessário, em torno da subquery. Teria que executar a query num ciclo qualquer para poder manipular várias linhas, individualmente. 

Imagina que no campo nome tinha &#8220;Miguel da Silva Alho&#8221; e em localidade tinha &#8220;actualmente em Murtosa, Portugal&#8221;, o vector resultante seria: 

<pre>'alho':4 'silv':3 'actual':5 'miguel':1 'murtos':7 'portugal':8
</pre>

com a eliminação das palavras de paragem e a redução às raizes das palavras exemplificado por &#8220;actual&#8221;, &#8220;silv&#8221; e &#8220;murtos&#8221;.

Para efectuar o mesmo para as notas, a operação é semelhante, mas necessito de ter o cuidado e evitar um retorno nulo. Para tal aproveito a função COALESCE presente na maioria dos sistemas de base de dados. O COALESCE permite analisar diversos valores, retornando o primeiro não nulo.

Se tivese a certeza que apenas tinha uma linha por pessoa, seria:

<pre lang="sql">SELECT to_tsvector(COALESCE(SELECT texto FROM notas WHERE id_pessoa = xxx), '')
</pre>

Com COALESCE, se o SELECT retornar um valor válido, é efectivamente esse que é vectorizado. Se no entanto o resultado do SELECT for nulo, é testado a nulidade do valor seguinte, neste caso uma string vazia (mas não nula). Como é não nulo, este é vectorizado (como se fosse to\_tsvector(&#8221;) ). O truque surge no entanto, se quero juntar o texto das diversas notas de uma pessoa. Nesse caso necessito de recorrer ao array\_to_string() e fico com :

<pre lang="sql">SELECT to_tsvector(COALESCE((
	SELECT array_to_string(
	  array(SELECT texto FROM notas WHERE id_pessoa = xxx), ' ')
	),''))
</pre>

Em array, a lista de resultados é convertido para um array, e com o array\_to\_string, cada item do array é concatenado, separado apenas pro um espaço (como definido no segundo parâmetro da função). Novamente , tenho um COALESCE para prevenir um valor null do conjunto.

Tendo agora os dois vectores, posso concatena-los:

<pre lang="sql">SELECT	to_tsvector(
		(SELECT nome || ' ' || localidade FROM pessoa WHERE id_pessoa = xxx)
	) || 
	to_tsvector(COALESCE(
               (SELECT array_to_string(
	  	  array(SELECT texto FROM notas WHERE id_pessoa = xxx), ' ')
		),'')
       )
</pre>

Repara que tenho a concatenação dos vectores através de ||. se o segundo fosse nullo, o resultado seria tb nullo, independentemente do resultado do primeiro vector. Daí a importância do COALESCE.

E aí está.

<div class="zemanta-pixie">
  <img class="zemanta-pixie-img" src="http://img.zemanta.com/pixy.gif?x-id=660020dc-a1c3-8613-ae29-f6863d193f4b" />
</div>