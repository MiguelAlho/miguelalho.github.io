---
title: FileUpload, iframes, e actualização de listas.
author: Miguel Alho
type: post
date: 2009-08-21T12:07:02+00:00
url: /fileupload-iframes-e-actualizacao-de-listas/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - fileupload
  - iframes
  - Javascript

---
Andei à procura de uma solução para um problema de envio de ficheiros, e não estava a encontrar nas pesquisas uma solução válida, dentro do contexto que estava a desenvolver. Felizmente lembrei-me duma que resultou e que vou descrever. E até é bastante simples! lol&#8230;

Então o contexto. Tenho uma página complexa (tem imensos blocos de funcionalidade ortogonais). Um das secções / blocos representa um conjunto de documentos associados a uma entidade. A funcionalidade da página está bastante dependente de AJAX para evitar carregamentos de página e ter uma interface mais fluída. Porque o bloco de adição dos documentos requer um upload, queria que o upload também fosse efectuado sem refresh da página.

Aparentemente, não é possível fazer o envio directamente com uma chamada a um método por AJAX. A solução então passa por usar um iframe na página, e o conteúdo desse iframe ser um novo formulário de envio. Segui esse caminho &#8211; implementei o iframe e criei a página de upload (o formulário tem um control de Fileupload, uma DropDownList, e um botão de envio). Neste caso, a funcionalidade de upload está contido neste formulário e este é o único responsável pelo upload. e funciona correctamente como esperado. O postback é contido no formulário do iframe e apenas nesse ponto é que há refresh. E é aceitável.

O problema que surge agora é que na página original, tenho a lista de documentos associados à entidade, e necessito de actualizar essa lista com o novo documento associado. A minha lista de documentos é carregada por chamadas AJAX, usando os métodos de templates javascript que mencionei [no penúltimo post][1] e portanto bastaria chamar novamente a função que preenche a lista para conseguir actualiza-la. Procurei então um método de comunicação entre frames, para que o a acção de click do botão ou do refresh da página actuasse sobre a página pai. Má ou impossível solução. No entanto, no meio disto, lembrei-me &#8211; &#8220;bem, se cada vez q é feito um upload, há um postback e a página do iframe é recarregada, será que dá para simplesmente apanhar o evento de &#8216;load&#8217; do iframe?&#8221;. E a verdade é que sim, sempre que a página do iframe é carregado, o load do iframe dispara. Assim, fiquei com :

<pre lang="javascript">$(document).ready(function() {
      $('#docuploadframe').bind('load', function() { LoadDocList(); });
 });</pre>

Conclusão: em cada upload de documento, o load do iframe é disparado (após upload, ou mesmo erro), e então recarrego a lista de documentos após esse evento, via AJAX. Simples!

 [1]: http://miguelalho.com/?p=958