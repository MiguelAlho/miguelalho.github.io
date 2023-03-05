---
title: Localizar Enumerações
author: Miguel Alho
type: post
date: 2010-07-05T23:46:19+00:00
url: /localizar-enumeracoes/
categories:
  - 'Code &amp; IT'
tags:
  - enum
  - enumeração
  - globalization
  - locale
  - ToString()

---
Continuando no desafio de localizar a minha aplicação, estou agora a atacar as enumerações. Esta também é uma óptima oportunidade de efectuar algum refactoring às classes e estruturas existentes.

Parte das enumerações usadas são automaticamente escritas com uma estrutura que era óptima.. até agora. A aplicação é _N-layer_, dividido em camadas lógicas. Por enquanto, e porque ainda não foi necessário mais, mantém-se _monotier_. As camadas lógicas neste caso são UI, BLL (_Business Lógic Layer_) e DAL (_Data Access Layer_), mais os repositórios de dados. Comum a todas as camadas há duas bibliotecas &#8211; MAAPPFramework que é um conjuntos de classes base, interfaces base, e muitos helpers comuns a várias aplicações (O MAAPP significa Miguel Alho Application : D ) e a camada de BO (_Business Objects_) que inclui as classes conceptuais e DTOs (_Data Transfer Objects_) do domínio. Os objectos nesta biblioteca são &#8220;dummy objects&#8221;, no sentido que não tem funcionalidade (senão algumas ordenações e extensões). Apenas suportam dados e estão devidamente decoradas para serem serializadas. Esta biblioteca é transformada em dll e utilizada por todas as camadas. Esta arquitectura está muito bem exemplificado pelo Imar Spaanjars nos seus artigos <a href="http://imar.spaanjaars.com/416/building-layered-web-applications-with-microsoft-aspnet-20-part-1" target="_blank">Building Layered Web Applications with Microsoft ASP.NET 2.0</a>.

<!--more-->

As enumerações que eu tinha incluíam sempre um valor numérico definido explicitamente (evitando súbitas alterações de referências para com a base de dados numa nova geração de código) e uma descrição com texto legível, em vez de um texto colado em CamelCase. um exemplo:

<pre lang="csharp">namespace Namespace.BO
{
    [Serializable]  
    public enum EnumTipoContacto
    {	
        [Description("Telefone")]
	Telefone = 1,
        [Description("Telemóvel")]
        Telemovel = 2,
        [Description("Email")]
        Email = 3,
        [Description("Fax")]
        Fax = 4,
        [Description("Outro Tipo de Contacto")]
        Outro = 5
    }
}
</pre>

A descrição escrita como atributo, recorrendo ao _System.ComponentModel.DescriptionAttribute_ permite em runtime ter uma versão legível do nome da enumeração, óptima para apresentação na UI, em vez do tipo valor devolvido pelo método _ToString()_. No exemplo em cima, O caso do &#8220;Outro&#8221; demonstra a possível diferença. Para obter os valores, na minha _framework_ tenho um método numa _helper class_ que permite obter o valor da descrição, bem como uma _extension method_ para ajudar:

<pre lang="csharp">public class EnumDescription
{
        public static string GetDescription(Enum value)
        {
            FieldInfo fieldInfo = value.GetType().GetField(value.ToString());
            DescriptionAttribute[] attributes = (DescriptionAttribute[])fieldInfo.GetCustomAttributes(typeof(DescriptionAttribute), false);
            return (attributes.Length &gt; 0) ? attributes[0].Description : value.ToString();
        }

        //(...)
}

public static class EnumExtension
{
    public static string GetDescription(this Enum e)
    {
         return EnumDescription.GetDescription(e);
    }
}
</pre>

Considerando que o código é gerado a partir de um modelo que controlo, é uma óptima solução, desde que localizado a uma definição linguística. Seria possível criar múltiplos atributos (costumizados) para suportar descrições localizadas, por exemplo, mas a manutenção e separação não me parece optimizado. Procurei outra solução que me permitiria utilizar ficheiros de recursos (.resx).

Uma das técnicas que encontrei parece simples e óptima. A descrição passa a ser definida no ficheiro de recursos, e a chave é o nome qualificado da enumeração. Optei pelo nome completo porque é me fácil de gerar com _T4_. Assim, a minha enumeração fica simplificada:

<pre lang="csharp">[Serializable]
public enum EnumTipoContacto
{	
	Telefone = 1,
        Telemovel = 2,
        Email = 3,
        Fax = 4,
        Outro = 5,
}
</pre>

e o meu ficheiro .resx terá um conjunto de chaves valores qualificados, que pode ser localizado na versão do ficheiro para outra língua:

<pre lang="xml">(...)

    Telefone
  
 
    Telemóvel
  
 
    Email
  
 
    Fax
  
 
    Outro Tipo / Desconhecido
 
(...)
</pre>

Para obter o valor da chave, a operação é ligeiramente diferente:

<pre lang="csharp">public static string GetLocalizedDescription(EnumTipoContacto tipo)
{
      ResourceManager resources = Namespace.BO.Properties.Enum.ResourceManager;

      string resourceKey = String.Format("{0}.{1}", tipo.GetType(), tipo);
      string localizedDescription = resources.GetString(resourceKey);

      if (localizedDescription.IsNullOrEmpty())
           return tipo.ToString();
      else
           return localizedDescription;
}
</pre>

Neste exemplo, _tipo.GetType()_ devolve _Namespace.BO.EnumTipoContacto_, permitindo construir a chave. é necessário ter em atenção que linha

<pre lang="csharp">ResourceManager resources = Namespace.BO.Properties.Enum.ResourceManager;
</pre>

obriga-me a que a assemblagem gerada seja o _Namespace.BO.dll_ e o ficheiro de recursos é o _Enum.resx_ ou _Enum.xx.resx_ onde _xx_ representa o código cultural (&#8220;es&#8221;, por exemplo). Novamente, porque o meu código é em boa parte autogerado, tenho controlo sobre esse formato.

Outro método muito útil, para usar como _DataSource_ para preencher _DropDownLists_ e controlos do género é a seguinte:

<pre lang="csharp">public static List&lt;KeyValuePair&lt;string, string&gt;&gt; GetDDLList()
{
            List&lt;KeyValuePair&lt;string, string&gt;&gt; kvPairList = new List&lt;KeyValuePair&lt;string, string&gt;&gt;();
            ResourceManager resources = Namespace.BO.Properties.Enum.ResourceManager;

            foreach (Enum enumValue in Enum.GetValues(typeof(EnumTipoContacto)))
            {
                string resourceKey = String.Format("{0}.{1}", enumValue.GetType(), enumValue);
                string localizedDescription = resources.GetString(resourceKey);
                
                if(localizedDescription.IsNullOrEmpty())
                    kvPairList.Add(new KeyValuePair&lt;string, string&gt;(enumValue.ToString(), enumValue.ToString()));
                else
                    kvPairList.Add(new KeyValuePair&lt;string, string&gt;(enumValue.ToString(), localizedDescription));
            }

            return kvPairList;
}
</pre>

Semelhante à anterior, permite obter uma lista de _KeyValuePairs_ aceites como _DataSource_ dos controlos.

Este foi, numa fase inicial, o tipo de estrutura que mais me preocupava, no sentido que os atributos poderiam complicar a globalização, e obrigar a um refactoring interno profundo. Felizmente, um simples _workaround_ e _refactoring_ evitou grandes mexidas. Também, porque nem os nomes dos métodos (nomeadamente o _GetDDLList_() ) nem os tipos de dados foram alterados, a camada superior continua a funcionar sem ter sido afectado.