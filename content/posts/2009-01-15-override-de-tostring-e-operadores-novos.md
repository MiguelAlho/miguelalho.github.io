---
title: Override de ToString() e operadores novos
author: Miguel Alho
type: post
date: 2009-01-15T16:01:07+00:00
url: /override-de-tostring-e-operadores-novos/
categories:
  - 'Code &amp; IT'
tags:
  - 'C#'
  - Classes
  - operadores
  - override
  - ToString()

---
Ontem estive numa curta discussão acerca das potencialidades de herança nas classes, em programação por objectos, em especial no .NET, com um amigo que está a estagiar comigo. Bastou um pequeno exemplo para demonstrar alguns dos princípios.

Comecei por recordar o principio da herança com um diagrama muito simples:  
[<img src="http://miguelalho.com/wp-content/uploads/2009/01/main.jpg" alt="" title="Base-derivado" width="299" height="260" class="alignnone size-medium wp-image-837" />][1]

Neste exemplo ClasseBase, como o nome sugere, é base de ClasseDerivado1 e ClasseDerivado2, e tendo propriedades publicas e métodos publicos, essas propriedades estarão, tambem, presentes em nas classes derivadas. Ou seja

_ClasseDerivado1_ tem (publicamente) acessível as propriedades _prop1, prop2_ e _prop3_, e ainda os metódos meth1 e meth2.  
_ClasseDerivado2_ tem (publicamente) acessível as propriedades _prop1, prop2_ e _prop4_, e ainda os metódos meth1 e meth3.

A conversa depois seguiu para o facto de poder fazer overide de métodos da base na classe derivada. Em concreto falamos do ToString(). Todos os objectos criados em .NET derivado da base Object, que, entre outras, tem o método ToString(). Por defeito este devolve o valor da instancia (quando é um tipo de valor, por exemplo, int, byte, bool, char..). Quando se trata de um tipo que é referência (exceptuando a string), é devolvido o nome completo da classe (namespaceCompleto.nomeDaClasse). No entanto, nada me impede de redefinir o ToString(). Por exemplo, numa classe &#8220;Pessoa&#8221;, posso querer que o ToString() devolva a concatenação do primeiro e último nome.

Como conseguir? 

<pre lang="csharp">class QQcoisa
    {
        private string _myVar = "olá";
        [XmlAttribute]
        public string myVar
        {
            get { return _myVar; }
            set { _myVar = value; }
        }

        public override string ToString()
        {
            return base.ToString() + " " + myVar;
        }

        public static string operator +(QQcoisa in1, QQcoisa in2)
        {
            return in1.myVar + in2.myVar;
        }
    }
</pre>

Usando a declaração &#8220;public overide string ToString()&#8221;, posso redefinir o funcionamento do método. Se necessitar de recorrer, por alguma razão, ao ToString() da base, posso através da instrução

<pre lang="csharp">base.ToString();</pre>

No exemplo em cima, o método devolve o nome da class (&#8220;QQcoisa&#8221;) concatenado com um espaço e por fim o valor em myVar (que por defeito é &#8220;olá&#8221;).

É um exemplo simples do poder e flexibilidade de OOP.

A conversa continuou com a menção da criação de operadores para as classes. As potencialidades criadas pro esta funcionalidade é enorme. Por exemplo posso no meu código definir o que é uma soma de dois objectos (que não são valores). Por exemplo posso definir que somar dois QQcoisa é somar os valores presentes em myVar de cada um das instâncias. 

Nem todos os operadores permitem redefinição. A l<a href="http://msdn.microsoft.com/en-us/library/8edha89s(VS.80).aspx" target="_blank">ista dos operadores que podem ser redefinidos estão no MSDN</a>. Para redefinir, usa-se a declaração:

<pre lang="csharp">public static string operator +(QQcoisa in1, QQcoisa in2)</pre>

Simples, até.

 [1]: http://miguelalho.com/wp-content/uploads/2009/01/main.jpg