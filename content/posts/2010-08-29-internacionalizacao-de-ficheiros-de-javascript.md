---
title: Internacionalização de ficheiros de JavaScript
author: Miguel Alho
type: post
date: 2010-08-29T17:43:05+00:00
url: /internacionalizacao-de-ficheiros-de-javascript/
wordbooker_options:
  - 'a:8:{s:18:"wordbook_noncename";s:10:"48ead89ac2";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";s:23:"wordbook_default_author";s:1:"2";s:23:"wordbook_extract_length";s:3:"256";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";}'
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - 'C#'
  - i18n
  - Javascript
  - l10n
  - T4

---
Os últimos posts no meu blog têm sido acerca do tema da internacionalização de aplicações, nomeadamente:

  * <a title="Permanent Link to ASP.Net – Método alternativo de internacionalização (i18n)" href="http://miguelalho.com/?p=1146" rel="bookmark">ASP.Net – Método alternativo de internacionalização (i18n)</a>
  * <a title="Permanent Link to Localizar Enumerações" href="http://miguelalho.com/?p=1100" rel="bookmark">Localizar Enumerações</a>
  * <a title="Permanent Link to Adicionar novas línguas ao Windows (edições Home)" href="http://miguelalho.com/?p=1097" rel="bookmark">Adicionar novas línguas ao Windows (edições Home)</a>

O post de hoje contínua o sobre o método alternativo com ASP.NET, recorrendo a ao <a href="http://www.gnu.org/software/gettext/manual/gettext.htm" target="_blank">GetText </a>do GNU. Como vimos, a aplicação do método _() para substituir texto localizável tornava-se um recurso simples e eficiente na internacionalização de ASP.NET. Em especial, é um método adoptado pela restantes linguagens e evita o esforço extra necessário para introduzir o texto em ficheiros .resx.

<!--more-->

Procurei uma solução semelhante para Javascript, uma vez que a aplicação que estou a internacionalizar recorre muito a funcionalidade em javascript. Em especial, há mensagens de erro específicos e mensagens de confirmação contextualizadas. Especifico a ASP.Net, não encontrei nada de útil. Até achei muito estranho não detectar nada de muito útil nesse campo.

De qualquer forma, há alguns métodos genéricos a aplicar, incluíndo plugins em jQuery (<a href="http://plugins.jquery.com/project/gettext" target="_blank">http://plugins.jquery.com/project/gettext</a>), mas gostei da descrição dada em <a href="http://24ways.org/2007/javascript-internationalisation" target="_blank">24ways.org</a>. Simplesmente introduzi a função para _() no meu ficheiro core de javascript (é uma pequena biblioteca e funções e abstracções comuns a todas as páginas das minhas aplicações, e incluído em todas as páginas):

<pre lang="javascript">function _(s) {
	if (typeof(i18n)!='undefined' && i18n[s]) {
		return i18n[s];
	}
	return s;
}
</pre>

Com isto é esperado existir uma variável _i18n_ definido com a lista de traduções em pares chave-valor em JSON, do tipo:

<pre lang="javascript">var i18n = {
"" : "",
"Hello" : "Olá",
"Goodbye" : "Adeus",
//(...)
}
</pre>

O método em 24ways.oprg refere ainda funções para formatar strings que podem ser uteis. Eu no entanto já tinha uma implementação de string.format para javascript (<http://www.geekdaily.net/2007/06/21/cs-stringformat-for-javascript/>).

Portanto, com a função no core, e a variável i18n adicionada, nos diversos ficheiros de javascript, onde fosse usado uma string, este deveria ser englobado por _():

<pre lang="javascript">if (confirm(_('Confirma a alteração de estado nos processos seleccionados?') )) {
//...
}
</pre>

Assim, quando executado, e porque o método _() está sempre presente, o texto será substituído pelo existente no dicionário ou se não encontrado, o próprio texto usado como argumento da função.

Resta apenas dois passos &#8211; gerar os ficheiros .po, que deve seguir um esquema semelhante ao mencionado no artigo de ASP.Net, usando instruções póst-build e o gettext, e depois transformar os registos dos ficheiros .po em json. O primeiro dos passos é relativamente directa, apenas necessitando de construir uma nova lista de ficheiros (apenas de .js) e adicionar as entradas envolvidas em _() no .pot.

O segundo passo é mais &#8220;dificil&#8221;. Não há nada para o fazer directamentem senão um script em perl (<a href="http://jsgettext.berlios.de/doc/html/po2json.html" target="_blank">http://jsgettext.berlios.de/doc/html/po2json.html</a>), e como não me servia, decidi construir um script T4 simples, para usar no processo.

O script assume alguns pressupostos, pelo que, se utilizar, deve de os ter em conta e alterar consoante o necessário. O primeiro é que os ficheiros resultantes, quer os .po, quer os .js vão estar cada um numa pasta dedicada à língua específica como por exemplo:

/Scripts/locale/xx/messages.po  
/Scripts/locale/xx/i18n.js

Também, considero que o ficheiro .po será gerado e unido com recurso a msgmerge, e portanto apenas devo transformar os ficheiros .po em ficheiros .js

Assim, o po2json.tt, colocado na pasta (/Scripts/locale/) pode ser executado após o build. Ele procurará o ficheiro .po de cada língua no array, e transformarará em um ficheiro i18n.js. Eu decidi definir as linguas manualmente, mas com pequenas alterações, podem ser detectadas automáticamente.

O T4 é o seguinte:

<pre lang="csharp">&lt;#@ Template Language="C#v3.5" Hostspecific="True" #&gt;
&lt;#@ Output Extension=".js" #&gt;
&lt;#@ Import Namespace="System.IO" #&gt;
&lt;#@ Import Namespace="System.Collections.Generic" #&gt;
&lt;#@ Import Namespace="System.Text.RegularExpressions" #&gt;
&lt;#@ Include File="MultiOutput.t4" #&gt;
var i18n = {
&lt;#
    string localeFolderPath = @"D:\Codigo\ProjectoWeb\Scripts\locale\";
    string[] suportedLang = {"pt"};
    string pofileName = "messages.po";
    string msgid = "msgid";
    string msgstr = "msgstr";
    string txtREGEX = "\"(.*)\"";
      
    foreach (string lang in suportedLang)
    {
        string[] lines = File.ReadAllLines(localeFolderPath + "\\" + lang + "\\"  + pofileName);
        List&lt;KeyValuePair&lt;string, string&gt;&gt; entries = new List&lt;KeyValuePair&lt;string, string&gt;&gt;();
        int currentLine = 0;

        while (currentLine &lt; lines.Length)
        {
            string line = lines[currentLine];
            if (line.StartsWith(msgid))
            {
                string keyString = "";
                keyString += Regex.Match(line, txtREGEX).Groups[1].Value;
                
                //iterate while still a msgid
                line = lines[++currentLine];
                while(line.StartsWith("\""))
                {
                    keyString += Regex.Match(line, txtREGEX).Groups[1].Value;
                    line = lines[++currentLine];
                }

                if (line.StartsWith(msgstr))
                {
                    string valueString = "";
                    valueString += Regex.Match(line, txtREGEX).Groups[1].Value;
                    
                    if (++currentLine &lt; lines.Length)
                    {
                        line = lines[currentLine];
                        while (line.StartsWith("\""))
                        {
                            valueString += Regex.Match(line, txtREGEX).Groups[1].Value;
                            line = lines[++currentLine];
                        }
                    }
                    //add pair to entries list
                    entries.Add(new KeyValuePair&lt;string, string&gt;(keyString, valueString));
                }     
            }

            currentLine++;
        }
        
        //build outoput
        for( int i = 0; i&lt; entries.Count; i++)
        {
            KeyValuePair&lt;string, string&gt; entry = entries[i];
        
#&gt;
"&lt;#=  entry.Key #&gt;" : "&lt;#= entry.Value #&gt;"&lt;#=    i
&lt;#
        }  
    SaveOutput(localeFolderPath + "\\" + lang + "\\"  + "i18n.js"); 
    
    }
#&gt;
};
</pre>

Procura cada &#8220;msgid&#8221; e processa as línguas seguintes convenientemente. Se houver erros, o processo será abandonado. O tt recorre ainda a um MultiOutput.t4, para gerar mais que um ficheiro a partir do .tt, convenientemente localizado. Não necessita de estar incluído no projecto (para não obrigar a incluir as dependências de TextTempleting).

Para incluir o ficheiro na lista de scripts a puxar, basta regista-lo no ScriptManager, no Page_Load da MasterPage :

<pre lang="csharp">ScriptManager.RegisterClientScriptInclude(this, typeof(string), "i18n", Page.ResolveUrl("~/Scripts/locale/" + lang + "/i18n.js"));
</pre>

onde a variável lang é uma string com as letras da língua, para identificar a pasta onde será armazenado. Eu obtenho a a língua para a aplicação (transversal), mas outra possibilidade seria definir a língua pelas preferências do utilizador. Também, o caminho poderia ser proveniente de uma chave de configuração no web.config, se necessário.

Creio que está é uma solução simples, que pode, se necessário ser melhorado com mais funções (especialmente de formatações e plural / singular) como descrito no artigo do 24ways.org. Para já, esta é me suficiente.

[Download po2json t4][1]

 [1]: http://miguelalho.com/wp-content/uploads/2010/08/po2json.zip