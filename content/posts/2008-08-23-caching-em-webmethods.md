---
title: Caching em WebMethods
author: Miguel Alho
type: post
date: 2008-08-23T07:57:20+00:00
url: /caching-em-webmethods/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - 'C#'
  - Cache
  - Caching
  - HttpRuntime
  - Webmethod
  - WebServices

---
O objecto _Cache_ do _System.Web.Caching_ é muito funcional e poderoso na gestão de dados aplicacionais no ASP.Net. Uso-o frequentemente em aplicações, como modo de persistir dados nas camadas superiores permitindo acelerar alguns processos, especialmente quando os dados vão ser usados continuamente durante um curto período de tempo e em diversas páginas ou com operações de paginação e exportação de dados de uma _GridView_. 

Há naturalmente vantagens e desvantagens em usar a Cache do .Net para armazenar dados. A _Cache_ é efectivamente rápida. Muito mais rápida que o _ViewState_, por exemplo, e faz uma melhor gestão de grandes quantidades de dados que a objecto de _Session_. Permite também gerir o tempo de vida e modo de remoção dos dados. No entanto, a cache está disponível a todos os utilizadores, pelo que é necessário ter algum cuidado a gerir as chaves usada para armazenar os valores, se bem que esta característica até é vantajosa em muitas situações.

Enquanto que em páginas (ou melhor, classes herdadas da classe _Page_) o objecto _Cache_ está imediatamente disponível, o mesmo não é tão verdade para _WebServices_. _WebServices_ permite algumas estratégias de _Caching_, sendo a principal a cache de saída (_output caching_), em que a resposta do _WebMethod_ é armazenado em cache durante um determinado período. Por exemplo:

<pre lang="csharp">[WebMethod(CacheDuration=60)]
public string DevolveString()
{
   string palavra
   //processamento (...)
   return palavra;
}
</pre>

Irá devolver a mesma string durante os 60 segundos posterior à chamada, e determinado pelo uso de &#8220;_CacheDuration=60_&#8221; no atributo do _WebMethod_.

Mas e se quisermos utilizar a cache da mesma forma que é usada nas páginas, com recurso a métodos (estáticos?) para inserir e obter valores? Enquanto que escrever:

<pre lang="csharp">Cache.Insert("objectoAinserir", obj); //insere a instância obj na cache 
</pre>

é válida nas classes das páginas, o mesmo não é verdade num _WebMethod_. Aliás, no VS, para que o IDE reconheça o objecto Cache, é necessário inserir a referencia ao _System.Web.Caching_, e mesmo assim os métodos não são disponibilizados pelo IDE. Instanciar a cache com algo do género

<pre lang="csharp">System.Web.Caching.Cache cache = new System.Web.Caching.Cache();
</pre>

é válido quer no IDE, quer em _runtime_, qualquer tentativa de atribuir ou obter um valor usando _cache.Get(&#8230;)_ ou _cache.Insert(&#8230;)_ levantam excepções.

A solução passa emtão por ir buscar o objecto de Cache a outro local, nomeadamente ao HttpRuntime. Deste modo, no WebMethod, já é possivel inserir e obter valores dentro de um método (que não é necessáriamente a resposta do método) e persisti-lo para utilizar com outros métodos ou até outros utilizadores.

<pre lang="csharp">[WebMethod]
public void TestMethod()
{
   int a = 1;
   HttpRuntime.Cache.Insert("A", a);
}

[WebMethod]
public int TestMethod2()
{
   return (int)HttpRuntime.Cache.Get("A");
}
</pre>

O código anterior é exemplo de como utilizar a Cache do HttpRuntime. No _TestMethod()_ é armazenado um valor na cache, e considerando que este é executado antes do _TestMethod2()_, ao chamar o _TestMethod2()_ o valor presente na cache será recuperada e retornada, permitindo persistir dados entre métodos, especialmente quando volumosos, e que pode evitar trocas de dados lentos entre camadas aplicacionais ou serviços.