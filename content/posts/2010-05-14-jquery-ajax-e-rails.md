---
title: jQuery, Ajax, e Rails
author: Miguel Alho
type: post
date: 2010-05-14T15:31:39+00:00
url: /jquery-ajax-e-rails/
categories:
  - 'Code &amp; IT'
tags:
  - .net
  - AJAX
  - jQuery
  - JSON
  - Rails
  - Ruby
  - Webmethod

---
Para além das lides habituais com o desenvolvimento em ASP.Net com C# e  muito Javascript, tenho tido nas últimas semanas oportunidades para trabalhar com Ruby on Rails. É uma linguagem e framework bastante interessante, sem dúvida, e uma abordagem diferente à construção de páginas, visto que aplica o padrão MVC.

Ainda estou a aprender muito do muito básico, mas tenho conseguido com alguma comunicação com os colegas, aplicar conceitos que já aplicava no .Net, especialmente no que diz respeito ao uso de javascript no browser e a troca de dados por Ajax.

<!--more-->

Em termos de Ajax, tenho-me concentrado essencialmente em usar o jQuery para efectuar os pedidos e manipular os dados. Muito raramente (mais para o nunca), uso os controlos da biblioteca de Ajax da MS. não é que sejam menos bons, simplesmente gosto de usar o jQuery e construir de baixo para cima os componentes. Por vezes os truques escondidos dos controlos estragam a experiência de desenvolvimento. Aprendi isso com os controlos da DevExpress&#8230;

Com jQuery, o pedido por Ajax é realizado simplesmente usando o $.ajax() , com as opções devidamente preenchidas. Até já tinah construido uma função numa biblioteca &#8220;core&#8221; pessoal para abstrair alguns detalhes como a definição de JSON como formato de pedido e resposta:

<pre lang="javascript">function GetAjaxData(DTO, serviceAddress, webMethodName, successFuntion, errorFunction) {
    $.ajax({
        type: "POST",
        url: serviceAddress + "/" + webMethodName,
        contentType: "application/json; charset=utf-8",
        data: JSON.stringify(DTO),
        dataType: "json",
        success: successFuntion,
        error: errorFunction
    });
}
</pre>

### Asp.Net Way

O GetAjaxData é apenas um género de wrapper em torno do $.ajax(), fixando alguns parâmetros e aplicado os argumentos como os dados a transmitir, o caminho e nome do método a executar, e os callbacks. Por exemplo, se tiver um método de página (gosto de usar assim) que deva chamar, teria algo do género:

<pre lang="csharp">public class QQPagina: Page
{
    //...
    
    [WebMethod]
    public static int MetodoAChamar(int id)
    {
        return id*1000;
     }

</pre>

O método apresentado é algo ridículo, mas serve para explicar. Temos um método da classe da página (supomos &#8220;QQPagina.aspx&#8221;), que se chama &#8220;MetodoAChamar&#8221;, recebe um parâmetro inteiro &#8220;id&#8221; e devolve um inteiro. É importante notar que o método é static (portanto ao nível da classe e não instanciado &#8211; não que acesso às propriedades instanciadas da classe). Também, apesar de usar aqui um método numa página Aspx, podia ter colocado o mesmo numa classe de WebService. Ambos os métodos teriam o atributo &#8220;[WebMethod]&#8221;, mas o método no WebService não é static.

Para executar o método, podia fazer algo do género:

<pre lang="javascript">function ChamarMetodo(id){
     GetAjaxData(
             {"id":5},
             "QQPagina.aspx", 
             "MetodoAChamar", 
             function(msg){ alert(msg.d); },
             function(msg){ alert(msg.responseText) }
     );
}
</pre>

A função no JavaScript ChamarMétodo efectua o pedido Ajax, e apresenta o resultado num alerta.reparem que os dados são preparados no argumento DTO e devidamente formatado em JSON. O endereço da página é relativa à actual.

Se tudo estiver OK, o resultado seria uma caixa de mensagem a aparecer com 5000 escrito e um botão de OK.na verdade, o retorno pode ser objectos complexo (devolvidos estruturados em JSON) para depois construir parte da interface, o HTML directo para reconstruir uma parte da página. É de lembrar que os dados retornados são encapsulados numa propriedade base do resultado que é o &#8220;d&#8221;.

O que gosto deste método é o facto de poder usar um código mais limpo no cliente, e mais controlo sobre o momento e operações de pedidos.

### Ruby on Rails Way

Para Ruby On Rails (RoR), há uma serie de formas de conseguir, e a framework tem uma integração interessante com o Prototype. Pessoalmente estou treinado no jQuery, e portanto procurei formas de conseguir usar este.

A maior dificuldade que encontrei foi perceber o mecanismo da chamada, já que o uso do padrão MVC supõe algumas considerações. O endereço à qual é efectuado o pedido deve ter em conta o esquema de routing, por exemplo, e é necessário ter atenção à forma como é renderizado o layout &#8211; não deve ser renderizado normalmente, visto que só queremos transmitir uma pequena porção de HTML ou os dados para o construir.

Começando pelo função de JavaScript que chama o método do controlador, mantém-se praticamente idêntico, excepto que, devido à aplicação do routing, é conveniente escrever o endereço e o método de uma só vez. O rails até ajuda nesse aspecto:

<pre lang="javascript">function CallAjaxMethod(DTO, serviceAddress, successFuntion, errorFunction) {
    $.ajax({
        type: "POST",
        url: serviceAddress,
        contentType: "application/json; charset=utf-8",
        data: JSON.stringify(DTO),
        dataType: "json",
        success: successFuntion,
        error: errorFunction
    });
}
</pre>

Tem outro nome neste momento, e menos um parâmetro, e pode ser introduzido numa biblioteca de funções base. O Rails permite injectar código para chamar código remoto, usando blocos como o &#8220;link\_to\_remote&#8221; ou o &#8220;forms\_remote\_tag&#8221;. Neste caso, na View, vou introduzir o código entre tags de script explicitamente, em vez de depender do Rails para o gerar:

<pre lang="javascript">function FuncaoQueUtilizaAjax(id){
     //processa uma serie de coisas...
     CallAjaxMethod(
          {"id": id, "nome": "Miguel Alho"},
          "&lt;%= url_for(:action =&gt; 'doAjaxOperation') %&gt;",
          function(msg){ alert(msg) },
          function(msg){ alert(msg.responseText); }
     );
}
</pre>

É em muito semelhante ao ChamarMetodo() que usamos no exemplo para o .Net, mas neste caso é o rails a dar-nos o endereço do URL a que faremos o pedido usando o &#8220;<%= url_for(:action => &#8216;doAjaxOperation&#8217;) %>&#8221;, onde doAjaxOperation é o nome do método a executar no controlador. Assim, no Controlador, devemos ter:

<pre lang="ruby">class ConceitoActualController &lt; ApplicationController
     #...

     def doAjaxOperation
          # criar um novo objecto com os argumentos enviados em POST
          postData = ActiveSupport::JSON.decode(request.raw_post())

          # processamento ....
          output =  "Olá " + postData["nome"] + ", " + postData["id"]
          render (:json =&gt; output)
          # alternativamente podia ser 
          # render :text =&gt; output.to_json
     end

</pre>

Assim &#8216;doAjaxOperation&#8217; é um método da classe do controlador actual (deve ser publico!) e porque tem o &#8220;render(:json &#8230;) ou o render :text, o método irá ignorar o layout, evitando reconstruir toda a página, e apenas lançar aquilo que necessitamos (que neste caso seria &#8220;Olá Miguel Alho, 1&#8221;). outra opção para não renderizar o layout seria utilizar &#8220;render :layout => false&#8221; dentro do método.

Os argumentos que enviamos, vão no POST da mensagem (é interessante ver isto no FireBug!), e para os decifrar, usamos ActiveSupport::JSON.decode(request.raw_post()). postData passa a ter o objecto nele e podemos ter acesso as propriedades do POST enviadas no pedido.

No fim, deve ser tido em conta que, ao contrario do .Net, os dados da resposta são entregues directamente no argumento da função do sucesso, e não encapsulados numa propriedade &#8220;d&#8221;. Portanto, em vez de termos um &#8220;msg.d&#8221; com a string &#8220;Olá Miguel Alho, 1&#8221;, o próprio msg teria essa informação.

Para mim, a transição não foi de todo fácil ,mas confesso que não tenho ainda boas bases nem do Ruby, nem do Rails, para facilitar. Felizmente, com o conhecimento adquirido no .Net com esta matéria, foi pelo menos ter uma boa base de arranque e encontrar o caminho.Espero que esta informação possa ajudar alguém!