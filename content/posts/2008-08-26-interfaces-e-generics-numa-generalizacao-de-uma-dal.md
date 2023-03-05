---
title: Interfaces e Generics numa generalização de uma DAL
author: Miguel Alho
type: post
date: 2008-08-26T10:30:27+00:00
url: /interfaces-e-generics-numa-generalizacao-de-uma-dal/
categories:
  - 'Code &amp; IT'
tags:
  - 'C#'
  - code generator
  - custom DAL
  - DAL
  - generics
  - 'gerador de código c#'
  - interfaces

---
Nas ultimas semanas tenho ponderado e escrito algum código relativo ao <a href="http://miguelalho.com/?p=589" target="_blank">gerador de código</a>. Até agora consegui uma interface básica com uma _treeview_ para navegar pelos objectos e um painel de propriedades para editar as propriedades dos objectos que defini. Muito arcaico mas ainda é só uma base. Pelo menos já gera o código dos _business objects_ como utilizo, e escreve a configuração para um ficheiro XML, usando uma classe de serialização que eu já tinha.

Agora ando a pensar e preparar a DAL, que é talvez a parte mais complexa em termos de estrutura. É mais complexo, em especial, devido à modulariedade que desejo que tenha. Em especial, o conjunto de requisitos que desejo para a DAL são:

  * possibilitar trabalhar com um conjunto de sistemas de bases de dados (SQLite, SQLServer, MySQL, Access, &#8230;) para poder facilmente gerar o código necessário para qualquer um destes sistemas.
  * permitir _loggar_ as operações efectuadas, também para um conjunto de repositórios diferentes (bases de dados, ficheiros texto, ficheiros XML, etc..)
  * permitir gerar o código base para manipular os dados (GetItem, GetList, Save, Delete&#8230;) para os diversos sistemas de armazenamento.
  * permitir extender as classes, adicionando-lhes funcionalidade ou sobrepondo a existente na base sem afectar a geração de novo código.
  * aproveitar código já criado e as opções que o .NET oferece. 

Basicamente, necessito de um género de _framework_ para a DAL. Modelei o conceito no papel e verifiquei que iria ter que prototipar alguns conceitos para garantir o funcionamento e aperfeiçoar o método. Em especial, necessitei de testar e verificar as limitações dos _generics_ e _interfaces_ no C#.

<!--more-->

**O Modelo / Código**

Aproveitei o snippet compiler para escrever algum código simples para testar o esquema de interfaces e generics. Imaginei que o meu gerador de código deveria gerar apenas as classes da DAL com os métodos para manipular os dados, sem ter de preocupar com o sistema de base de dados a utilizar. Compreendo que a geração de queries pode ser problemático (há ligeiras variações de sintaxe entre sistemas), mas é algo que irei procurar generalizar e resolver.

Á partida (e já era algo que tinha de outros projectos) tenho uma classe por tipo de base de dados de accesso a dados que expõe métodos de obtenção (GetItem, GetList), escrita (Save) preenchimento (Fill) de objectos na base de dados. Geralmente os métodos aceitam como parametros a query e a connection string e devolvem os objectos preenchidos ou códigos de successo. Os métodos tratam de abrir a ligação, loggar o inicio da operação, executar a query, preencher o objecto de retorno (loggando sucesso ou erros) e devolvendo o resultado. As operações são as mais &#8220;básicas&#8221; e servem para muitos tipos de operações (por exemplo, porque o meu &#8220;Save&#8221; é baseado num ExecuteNonQuery(), este serve normalmente para Inserts, Updates, e Deletes). Naturalmente, para garantir a modulariedade, as várias classes tem de ter a mesma interface.

<img src="http://miguelalho.com/wp-content/uploads/2008/08/interfaces.jpg" alt="" title="interfaces" width="500" height="718" class="aligncenter size-full wp-image-639" /> 

Da mesma forma que o sistema deve permitir o acesso a dados, com uma class por tipo de base de dados, também deverá ter para cada tipo de repositório de registos (_log_). Naturalmente, devem ter a mesma interface.

<pre lang="csharp">namespace InterfaceTemplateTest.DALAccess
{
    /// <summary>
    /// Data Access interface - defines data access 
    /// </summary>
    public interface IDataAccess
    {
        void Read();
        //(interface de metodos CRUD
    }

    public class TxtDataAccess : IDataAccess 
    {
        public void Read()
        {
            Logger.Write("stuff");
            Console.Write("TxtDataAccess Read");
        }

        // (...) restante conjunto de métodos CRUD
    }

    public class SQLDataAccess : IDataAccess
    {
        public void Read()
        {
	    Logger.Write("stuff");
            Console.Write("SQLDataAccess Read");
        }

        // (...) restante conjunto de métodos CRUD
    }
	
    /// <summary>
    /// A interface de aceso generica - é usado para garantir que o DataAccess<T> implementa todos os metodos de IDataAccess
    /// </summary>
    /// <typeparam name="T">Classe Derivado de IDataAccess</typeparam>
    public interface IDataAccessGen<T> : IDataAccess where T:IDataAccess {}

    /// <summary>
    /// Classe genérica de accesso a dados
    /// </summary>
    /// <typeparam name="T"></typeparam>
    public class DataAccess<T>:IDataAccessGen<T> where T:IDataAccess, new ()
    {
        public void Read()
        {
            T t = new T();
            t.Read();
        }
    }
	
    #region Logging Classes and Methods
    public interface ILog
    {
        void Write(string msg);
    }

    public class TxtLog : ILog
    {
        public void Write(string msg)
        {
            Console.Write("TxtLog Write" + msg);
        }
		
	//outros métodos de leitura e escrita
    }

    public class SQLLog : ILog
    {
        public void Write(string msg)
        {
            Console.Write("SQLLog Write" + msg);
        }
    }

    public interface ILogGen<U>:ILog where U:ILog {}

    public class LogAccess<U> where U:ILog, new()
    {
        public static void Write(string msg)
        {
            U u = new U();
            u.Write(msg);
        }
    }

    #endregion
}

</pre>


<p>
  O código acima apresenta a estrutura muito básica para a estrutura modular. Quer para o acesso a dados quer para o Log, são definidas as interfaces (IDataAccess e ILog). As interfaces definem os métodos que terão de ser implementados nas classes, de modo a que outros possam utilizar os mesmos métodos, independentemente do tipo de repositório utilizado. Para cada tipo de repositório, são definidos os métodos da interface (sendo cada classe derivado da interface, como o <i>public class SQLDataAccess : IDataAccess</i>). Por fim é definido uma interface com generics e a classe genérica de acesso. Estes dois elementos são essenciais para implementar um nível de abstracção para as restantes classes da DAL.
</p>


<p>
  Usando como exemplo as classes de acesso a dados:
</p>


<pre lang="csharp">
    public interface IDataAccessGen&lt;T> : IDataAccess where T:IDataAccess {}

    public class DataAccess&lt;T>:IDataAccessGen&lt;T> where T:IDataAccess, new ()
    {
        public void Read()
        {
            T t = new T();
            t.Read();
        }
    }
</pre>


<p>
  cria uma interface IDataAccessGen<T> com os mesmos métodos da interface IDataAccess. DataAccess, que deriva desta interface, implementa os métodos da interface (que também são os de IDataAccess) e que utiliza um tipo de IDataAccess que implementa os métodos. Note que é introduzido o constragimento na definição onde T deve ser derivado de IDataAccess, de modo a garantir que o DataAcces utilize apenas classes que implementam os métodos da interface. Posso aceder ao dados em qualquer tipo de repositório através da classe. O acesso é efectuado com instruções do tipo :
</p>


<pre lang="csharp">
DataAccess&lt;SQLDataAccess> da = new DataAccess&lt;SQLDataAccess>();
da.Read();
</pre>


<p>
  Facilmente posso trocar o tipo de suporte de dados, mudando o tipo declarado na instância de DataAccess<>. No entanto, esta inicialização seria utilizada constantemente. Para generalizar a declaração em cima, é preferível criar umas novas classes que pouco mais servem senão para gerar um género de <i>alias</i>:
</p>


<pre lang="csharp">
namespace InterfaceTemplateTest.DALDEF
{
    public class DBAccesser : DataAccess&lt;TxtDataAccess>{}
    public class Logger : LogAccess&lt;TxtLog>{}
}
</pre>


<p>
  Deste modo, DBAccesser é sempre um DataAccess<> de determinado tipo, e posso instanciar o DBAccesser em vez de DataAccess<>. Enquanto que as classes de acesso podem estar presentes numa biblioteca e utilizados múltiplos vezes nos projectos, se necessitar (em principio) de alterações, estas novas classes devem ser definidas por projecto (específicos aos repositórios de dados) e podem ser gerados automaticamente pelo gerador de classes. Neste caso defini dois nomes, o DBAccessor e o Logger que derivam das classes genéricas mencionadas (DataAccess<> e LogAccess<>). Assim, as classes da DAL especifica ao modelo dos dados pode efectuar chamadas na forma:
</p>


<pre lang="csharp">
namespace InterfaceTemplateTest.DAL
{
    public class SomeDAL
    {
        public static void SomeMethod()
        {
	    DBAccesser da = new DBAccesser();
            da.Read();
        }
    }
}
</pre>


<p>
  O uso de interfaces impede a definição dos métodos de DataAccess<> como <i>static</i>, mas as da DAL já podem ser efectivamente <i>static</i>, de modo a que no programa principal (ou num 3-tier o BLL) execute o método no formato:
</p>


<pre lang="csharp">
SomeDAL.SomeMethod();
</pre>


<p>
  Com isto, o meu gerador de classes, ao nível da DAL, terá apenas de criar (teoricamente) as classes de acesso com os métodos CRUD específicos ao objecto, utilizando o DataAccess<T> para abstrair o tipo de base de dados e repositório de registos (log) utilizado. Para generalizar ainda mais, poderei introduzir um esquema de geração automática da query a partir de uma serie de parâmetros, de modo a que a query seja especifico ao sistema de BD usada, e que pode pertencer à interface do IDataAccess. 
</p>


<p>
  O <a href="http://miguelalho.com/wp-content/uploads/2008/08/interfacetester.zip">código esta disponível (interfacetester.cs zippado)</a>, podendo ser executado no Snippet Compiler. Tem os namespaces e classes todas no mesmo ficheiro.
</p>