---
title: O snippet “prop” no Visual Studio 2008
author: Miguel Alho
type: post
date: 2008-08-16T10:59:34+00:00
url: /o-snippet-prop-visual-studio-2008/
categories:
  - 'Code &amp; IT'
tags:
  - 'C# Visual Studio 2008'
  - prop
  - snippets

---
Um dos _snippets_ mais usados em C# é o &#8220;prop&#8221;, que escreve o código base de uma propriedade de uma classe, com a declaração do campo privado, a propriedade publica e a definição dos métodos de _get_ e _set_. No Visual Studio 2005&nbsp; e para o C#2.0, o snippet tinha a aparência do tipo:

<pre lang="csharp">private $type$ $field$;

public $type$ $property$
{
   get { return $field$;}
   set { $field$ = value;}
}
</pre>

Este até é o do _snippet_ usado pelo Visual Studio 2005. escrevendo &#8220;prop&#8221; e carregando em _Tab_ duas vezes, o IDE automaticamente geava este código, onde era necessário mudar apenas o tipo, o nome do atributo e o da propriedade.

No Visual Studio 2008, este código mudou. O 2008 aproveita a auto-implementação das propriedades, definida no C# 3.0, em que o a definição necessita de ser:

<pre lang="csharp">public $type$ $property$ { get; set; }
</pre>

Muito mais reduzido e muito mais simplificado &#8211; basta definir o tipo e o nome da propriedade; a parte privada e a lógica de atribuição é criada pelo compilador. Este código é agradável, quando a implementação pretendida é simples. O código tb ocupa muito menos espaço, o que pode ajudar à leitura da estrutura da classe. Infelizmente, não é perfeito, visto que é por vezes útil trabalhar a lógica de atribuição, ou se o tipo é nullável, a lógica de atribuição tem de ser obrigatoriamente trabalhada.

A solução? Definir novos _snippets_ :D. Essencialmente basta definir um (ou vários novos snippets) para suportar as variantes, e o assunto fica resolvido. Eu aproveitei a oportunidade para implementar o do 2.0 em duas formas. A primeira é a clássica, com a definição do atributo e dos métodos de _get_ e _set_. Também, habitualmente defino o elemento privado com o prefixo &#8220;_&#8221; e o mesmo nome da propriedade; Também costumo incluir um valor predefinido, o que era chato de inserir com o _snippet_ original (obrigava a sair do bloco para poder retornar, senão incluía o valor no nome do atributo&#8230;); Também tenho interesse em inserir a informação de serialização. Por fim, fiz igual, mas considerando o tipo nullável.

O resultado:

<pre lang="xml"><?xml version="1.0" encoding="utf-8" ?>
&lt;CodeSnippets  xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  &lt;CodeSnippet Format="1.0.0">
    &lt;Header>
      

<Title>
  prop 2.0
</Title>
      &lt;Shortcut>propc&lt;/Shortcut>
      &lt;Description>Code snippet for property and backing field&lt;/Description>
      &lt;Author>Miguel Alho&lt;/Author>
      &lt;SnippetTypes>
        &lt;SnippetType>Expansion&lt;/SnippetType>
      &lt;/SnippetTypes>
    &lt;/Header>
    &lt;Snippet>
      &lt;Declarations>
        &lt;Literal>
          &lt;ID>type&lt;/ID>
          &lt;ToolTip>Property type&lt;/ToolTip>
          &lt;Default>int&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>field&lt;/ID>
          &lt;ToolTip>The variable backing this property&lt;/ToolTip>
          &lt;Default>myVar&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>defVal&lt;/ID>
          &lt;ToolTip>The default value&lt;/ToolTip>
          &lt;Default>-1&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>xmlType&lt;/ID>
          &lt;ToolTip>The XML serialization attribute&lt;/ToolTip>
          &lt;Default>XmlAttribute&lt;/Default>
        &lt;/Literal>
      &lt;/Declarations>

      

<Code Language="csharp">
        private $type$ _$field$ = $defVal$;
        [$xmlType$]
        public $type$ $field$
        {
            get { return _$field$;}
            set { _$field$ = value;}
        }
        $end$
      </Code>
    &lt;/Snippet>
  &lt;/CodeSnippet>

  &lt;CodeSnippet Format="1.0.0">
    &lt;Header>
      

<Title>
  prop null
</Title>
      &lt;Shortcut>propn&lt;/Shortcut>
      &lt;Description>Code snippet for property and backing field of nullable types&lt;/Description>
      &lt;Author>Miguel Alho&lt;/Author>
      &lt;SnippetTypes>
        &lt;SnippetType>Expansion&lt;/SnippetType>
      &lt;/SnippetTypes>
    &lt;/Header>
    &lt;Snippet>
      &lt;Declarations>
        &lt;Literal>
          &lt;ID>type&lt;/ID>
          &lt;ToolTip>Property type&lt;/ToolTip>
          &lt;Default>int&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>field&lt;/ID>
          &lt;ToolTip>The variable backing this property&lt;/ToolTip>
          &lt;Default>myVar&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>defVal&lt;/ID>
          &lt;ToolTip>The default value&lt;/ToolTip>
          &lt;Default>null&lt;/Default>
        &lt;/Literal>
        &lt;Literal>
          &lt;ID>xmlType&lt;/ID>
          &lt;ToolTip>The XML serialization attribute&lt;/ToolTip>
          &lt;Default>XmlAttribute&lt;/Default>
        &lt;/Literal>
      &lt;/Declarations>

      

<Code Language="csharp">
        private $type$? _$field$ = $defVal$;
        [$xmlType$]
        public $type$? $field$
        {
           get { if(_$field$.HasValue) return _$field$; else return null;}
           set { if(value.HasValue) _$field$ = value.Value; else _$field$ = null;}
        }
        $end$
      </Code>
    &lt;/Snippet>
  &lt;/CodeSnippet>
&lt;/CodeSnippets>

</pre>

Basta criar um ficheiro, tipo props.snippet, e no Visual Studio importar o ficheiro para os _snippets_ de C#. A partir daí, as declarações &#8220;propc&#8221; e &#8220;propn&#8221; ficam disponíveis, sendo o primeiro para o tipos &#8220;normais&#8221; e o &#8220;propn&#8221; para propriedades baseados em tipos anulláveis. Também podem <a href="http://miguelalho.com/wp-content/uploads/2008/08/prop_c_n.snippet" target="_blank">descarregar o ficheiro daqui</a>. Customize at will!

Também podem ver mais algumas notas em <a href="http://blogs.msdn.com/wriju/archive/2007/10/04/visual-studio-2008-automatic-property-is-the-default-snippet.aspx" target="_blank">http://blogs.msdn.com/wriju/archive/2007/10/04/visual-studio-2008-automatic-property-is-the-default-snippet.aspx</a>