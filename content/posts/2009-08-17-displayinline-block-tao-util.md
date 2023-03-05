---
title: display:inline-block .. tão útil!
author: Miguel Alho
type: post
date: 2009-08-17T19:05:41+00:00
url: /displayinline-block-tao-util/
categories:
  - 'Code &amp; IT'
tags:
  - CSS
  - html
  - Javascript
  - templates

---
Pois é, esta semana e a passada é a do frenesim das interfaces. Mas ainda bem. Resumindo, tive uma certa &#8220;infelicidade&#8221; em comprar os controlos da DevExpress. Ainda por cima comprei uma licença Enterprise. Não são de todo maus, mas na realidade conseguem ser bastante. Tipo, têm funcionalidade boa compactada no controlo, e o aspecto visual e funcionamento acaba por ser relativamente intuitivo para o utilizador final. O problema é a API (quer server quer cliente) que é demasiado complexo e muitas vezes nada intuitivo (o que me tem causado muiiiitoo tempo perdido e muitas dores de cabeça). A coisa que mais me chateia naquilo é o HTML que é renderizado&#8230; meu Deus.. TABELAS POR TODO O LADO!!! . Género, para desenhar um select box são necessárias mil tabelas encaixadas umas nas outras. É código feio, muito feio, e difícil depois de fazer o que quer que seja com Javascript e jQuery, porque simplesmente não sabes onde actuar (o menu da aplicação, renderizado no cliente, ocupava umas 1500 linhas de código &#8211; agora são 160, incluindo javascript, e consigo facilmente encontrar as coisas que preciso!). Os controlos tem todos uma API do lado cliente, mas até hoje ainda não percebi nada daquilo, e a documentação daquilo é fraquinho.

O bom é que o serviço de apoio é bom, e o CodeRush é um excelente produto, e esse recomendo sem dúvida. É um auxilio fantástico. Mas os controlos podem esquecer. A menos que o projecto seja muito simples, sem camadas e objectos e camadas e afins, (tipo o ui conectar-se à BD), aquilo acaba por dar muitas dores de cabeça. Mais fácil gerar código e fazer as interfaces à mão com HTML e controlos ASP.Net, e ainda combinar CSS com jQuery. é o que tenho feito ultimamente com o meu colaborador, e acredita, estou bem mais satisfeito, e acredito que tenho uma interface bem mais agradável e funcional. Caí um bocado no &#8220;hype&#8221; daquilo, e infelizmente, hoje ando a refazer interfaces e designs&#8230;

Portanto, agora, tenho criado umas interfaces bem mais limpas e bem mais funcionais. Ainda mantenho alguns dos controlos em locais em que não &#8220;estragam&#8221; nada e mantém funcionalidade válida. Mas onde possível e onde se justifica, estou a limpar. E como é claro, há uma série de coisas que estou a aprender às custas disso (e uma biblioteca de javascript com funcionalidade universal às interfaces, que tem sido muito útil).

Então uma revisão rápida às &#8220;coisas&#8221; aprendidas:

  * É possível criar templates do lado cliente para renderizar dados vindos em chamadas AJAX. Tenho testado duas formas, com bons resultados em ambos, sendo mais ou menos útil numa ou noutra situação. Um á muito boa quando a resposta AJAX é uma lista. Neste caso é a solução do script do tipo &#8220;text/html&#8221; com o template de renderização em html, e os campos a preencher em entre tags <#= #> (semelhante ao T4). É depois usado uma função javascript para correr o template e substituir o os campos com o resultado do pedido AJAX. O método está descrito no blog do <a href="http://weblogs.asp.net/dwahlin/archive/2009/04/17/minimize-code-by-using-jquery-and-data-templates.aspx" target="_blank">Dan Whalin</a> e ainda no do <a href="http://www.west-wind.com/WebLog/posts/300754.aspx" target="_blank">Rick Strahl</a>. Recomendo uns testes. É muito rápido na execução (a latência é reduzida por haver poucos dados na transmissão) e é óptimo para soluções de paginação.O segundo método é o de ter uma div escondida, com a estrutura de apresentação dos dados e os campos bem identificados, e usar o clone() do jquery, para criar uma cópia do bloco, preenche-lo, e apresentar no local onde necessitar. O clone é opcional e depende do que é pretendido, mas é uma solução muito útil para apresentar detalhe e blocos de inserção / edição. E é facil de implementar e perceber. Arisco dizer mais fácil que soluções compradas&#8230;Sério.
  * JSON é fantástico. Dados bem estruturados e que ocupam muito pouco espaço. Muito menos que XML. Diria mais limpo, até. Mas não é um mar de rosas total. Por exemplo, a serialização de Datas é muito estranho, porque a data é em si uma estrutura de dados complicado de representar. Alías, no javascript, tens uma forma única que é através do construtor &#8220;new Date()&#8221;. Problema é que JSON é transmitida como texto e a deserialização dá barraca.Solução? O serviço .NET envia uma string do tipo &#8220;/Date(1241796300465)/&#8221; O numero no interior com 13 digitos, representa o numero de milisegundos desde 1 de Janeiro de 1970, até à data ali representada. para transformar isso, nada como um pouco de código Javascript: <pre lang="js">function GetJSDateFromNET(netDateString) {
   var myregexp = /\d{13}/;
   var match = myregexp.exec(netDateString);
   if (match != null) {
      result = match[0];
   } else {
      result = "";
   }

   var foo = new Date(parseInt(result));
   return foo;
}
</pre>
    
    Assim, procuro os 13 digitos via regex, e crio a data a partir do número no encontrado, que é efectivamente suportado pelo construtor de Datas do javascript. Também evito um eval que poderia ser de alguma forma perigosa, imagino.</li> 
    
      * Prototypes. Muito bom! Melhora a escrita no javascript permitindo encadear funções de instâncias (métodos), e extender as funcionalidades de tipos basicos como strings, inteiros e datas. Já tinha visto, mas nunca tinha percebido ou notado o potencial. Agora, sim. Por exemplo, para a função anterior, já posso aplicar uma função directamente ao tipo string para criar a data, e se quiser, encadiar uma transformação para string formatada: <pre lang="js">msg.d.CampoDeDataDoJSON.toDateJsonNet().toYMDString();</pre>
        
        que resultaria numa string com o formato &#8220;yyyy-MM-dd&#8221;. Para isso tenho:
        
        <pre lang="js">String.prototype.toDateJsonNet = function() { return GetJSDateFromNET(this); }

function DateToYMDString(date) {
    return date.getFullYear() + "-" + (date.getMonth() + 1).toString().lpad('0', 2) + "-" + date.getDate().toString().lpad('0', 2);
}

Date.prototype.toYMDString = function() { return DateToYMDString(this);}</pre>
        
        De uma forma simples, o prototype acrescenta funcionalidade ao tipo (aqui é com um tipo de dados base, mas podia ser um tipo que eu criei). Também te deixa o código mais limpo e legível, o que é sempre importante!</li> 
        
          * display:inline-block. Esta é fantástica. E resolve um problema &#8220;clássico&#8221; de colunas, sem recorrer a tabelas nem floats. Tem algumas manhas, e dependências de browsers que aplicam incorrectamente (mais IE hacks, mas simples), e bem explicadas <a href="http://blog.mozilla.com/webdev/2009/02/20/cross-browser-inline-block/" target="_blank">aqui</a>, <a href="http://www.tjkdesign.com/articles/float-less_css_layouts.asp" target="_blank">aqui</a>, e <a href="http://www.satzansatz.de/cssd/onhavinglayout.html" target="_blank">aqui</a>. Para o meu caso quero duas colunas, lado a lado, baseados em divs. <pre lang="html"><div class="col1">
  ...
  
</div>


<div class="col2">
  ...
  
</div></pre>
            
            <pre lang="css">.col1{width:70%; display:inline-block; vertical-align:top;*display:inline;*zoom:1}
.col2{width:29%; display:inline-block; vertical-align:top;*display:inline;*zoom:1 }</pre>
            
            O segredo está na combinação &#8220;display:inline-block&#8221; com &#8220;vertical-align:top;&#8221; o primeiro permite colocar divs lado a lado, mas verticalmente ficam alinhados pela base do texto contido nas divs. para isso, serve o vertical-align:top. Isto funciona bem no FF3.5. Para resolver para IE8, basta o &#8220;hack&#8221; do asterisco como prefixo no display:inline e zoom:1, que são interpretados apenas pelo IE. Apenas testei no IE8 (que é o q necessitava) e resulta bem. O que mais gosto disto é que a div que suporta as duas colunas 8um género de contentor clássico para centrar tudo) cresce quando qualquer das duas colunas cresce, o que não acontece com as soluções de float, como tinha anteriormente. Esta fui uma bela revelação para mim, que já tive este problema diversas vezes!</li> </ul> 
            
            E por hoje chega. Ficam assim aqui algumas das fantásticas revelações destas semanas!!! 😀