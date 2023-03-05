---
title: Artigos de t4 no Olegsych.com
author: Miguel Alho
type: post
date: 2009-01-11T11:48:19+00:00
url: /artigos-de-t4-no-olegsychcom/
categories:
  - 'Code &amp; IT'
tags:
  - Olg Sych
  - T4 templates

---
Eu já me referi aos templates t4 neste blog, num post entitulado &#8220;<a target="_blank" href="http://miguelalho.com/?p=752">VSX &#8211; DSLs ->T4</a>&#8220;. Confesso que os templates t4 tem sido uma das amis importantes tecnologias que tenho usado nos últimos meses em termos de desenvolvimento. Tem me permitido analisar as aplicações que estou a desenvolver de outra forma, podendo cosntruir uma estrutura mais coerente e com melhroes capacidades de gestão. Tem tambem permitido melhorar a capacidade de code-reuse e retirado o típico esforço de trabalho repetitivo, comun na construção de DALs.

Um dos blogs que mais informação tem sobre estes templates é o de <a target="_blank" href="http://www.olegsych.com">Oleg Sych</a>. Há uma panóplia de artigos associados ao conceito e tema, e inclui muita experiência na exploração dos templates. Eu recomendo a todos os que desenvolvem aplicações em .NET a analisarem os templates e o poder que oferece no workflow. É verdadeiramente espantoso na geração de código, customizado às necessidades e dinâmico!

Naturalmente o site do Oleg tem uma lista infindável de artigos, mas vou listar alguns dos mais importantes e uteis e que pode ser um óptimo ponto de partida. A partir daí é saltar de link em link&#8230;.

</p> 

  * [How to create a simple T4 template][1] &#8211; Além da apresentação simplificada do potencial dos templates, e o modo de uso, ainda demonstra como passar valores de um ficheiro para outro.


  * [Why use T4 Toolbox?][2] &#8211; T4 Toolbox é um conjunto de templates que o Oleg desenvolveu e que disponibiliza, com uma série de tarefas já integradas, como criação de métodos CRUD, ficheiros .config, decorator e AzMan, além de funções para criar outputs multi ficheiros, e diferentes abordagens nos templates.


  * [How to use T4 to generate CRUD stored Procedures][3]


  * [How to use T4 to generate strongly-typed AzMan wrapper][4] &#8211; AzMan, ou Authorization Manager é um módulo que implementa o modelo RBAC de autorização em aplicações, podendo gerir autorização ao nível do perfil e até da operação numa aplicação. 


  * [<span>How to use T4 to generate Decorator classes</span>][5] &#8211; o poder de introspecção


  * [How to generate multiple outputs from single T4 template][6] &#8211; Uma das tarefas complicadas é criar multiplos ficheiros a partir de um template (por exemplo um ficheiro por classe criado por um template), porque o designer cria um ficheiro de saída (geralmente .cs) automáticamente. Um método de tmeplate resolve o assunto.


  * [<span>T4 Template Design: Merged Template Class</span>][7] &#8211; óptimo para criar blocos mais pequenos e inserir em templates mais complexos. Um&nbsp; pouco ao estilo da extracção de métodos em C#.


  * [T4 Template Design: Template Method][8] &#8211; óptimo para criar métodos uteis a mais ficheiros e templates.


  * [T4 Template Design: Standalone Template][9] &#8211; utilizando o CallContext para obter dados a incluir no template.


  * [T4 Template Design: Nested Template Class][10] &#8211; OOP aplicado a templates!

 [1]: http://www.olegsych.com/2007/12/how-to-create-a-simple-t4-template/
 [2]: http://www.olegsych.com/2008/12/why-use-t4-toolbox/
 [3]: http://www.olegsych.com/2008/01/how-to-use-t4-to-generate-crud-stored-procedures/
 [4]: http://www.olegsych.com/2008/12/t4-toolbox-strongly-typed-azman-wrapper-generator/
 [5]: http://www.olegsych.com/2007/12/how-to-use-t4-to-generate-decorator-classes/
 [6]: http://www.olegsych.com/2008/03/how-to-generate-multiple-outputs-from-single-t4-template/
 [7]: http://www.olegsych.com/2008/04/t4-template-design-merged-template-class/
 [8]: http://www.olegsych.com/2008/04/t4-template-design-template-method/
 [9]: http://www.olegsych.com/2008/04/t4-template-design-standalone-template/
 [10]: http://www.olegsych.com/2008/04/t4-template-design-nested-template-class/