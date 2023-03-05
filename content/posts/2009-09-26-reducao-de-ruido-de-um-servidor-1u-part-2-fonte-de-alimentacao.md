---
title: Redução de ruído de um servidor 1U – part 2 (Fonte de alimentação)
author: Miguel Alho
type: post
date: 2009-09-26T13:18:19+00:00
url: /reducao-de-ruido-de-um-servidor-1u-part-2-fonte-de-alimentacao/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'

---
O primeiro elemento de ruído que vou atacar é a proveniente da fonte de alimentação. A fonte é de 250W, com forma própria para caixas 1U. 250W pode parecer pouco para os standards comuns de hoje, mas considerando os poucos elementos envolvidos, não é problemático. O maior consumidor de energia é sem dúvida o processador que nos Pentium 4 rondam uns incríveis 110W. Fora disso, há o cooler (blower no caso deste server) e os dois discos. O server não tem nenhum leitor óptico, nem placas PCI, nem uma placa gráfica dedicada.

O ruído da fonte é bastante elevado, incrivelmente. Provem das duas ventoinhas de 40mm que estão no chassis da fonte &#8211; um para entrada de ar e um para saída. ventoinhas pequenas significam mais rotações para obter o mesmo fluxo de ar. Mais rotações significam ruído elevado. Não tenho como medir, mas imagino que o ruído da fonte ronde os 30-40db, o que é bastante incómodo.

Soluções?

  * &#8211; substituir a fonte por um menos ruidoso ou até sem ventilação (solução passiva).
  * &#8211; substituir as ventoinhas na fonte por ventoinhas reconhecidamente menos ruidosas
  * &#8211; utilizar uma outra fonte, mesmo que não seja adequado 
      * **fonte silenciosa**
        
        Existem séries de fontes para 1U que tem em conta o problema do ruído. A <a href="http://www.seasonicusa.com/1u.htm" target="_blank">Seasonic tem fontes 1U</a> com baixo nível de ruído (25db), mas além da dificuldade em encontrar na Europa, o preço não é muito convidativo &#8211; rondam os 100 dolares. <a href="http://www2.computeruniverse.net/products/e90312758/d2/specifications/shuttle-pc60-300-watt-silentx.asp" target="_blank">Outras fontes</a> um 1U, mais ruidosas, rondam os 80 euros (mais portes). A substituição da fonte é uma possibilidade, mas não é a primeira escolha, de facto.
        
        **Substituição das ventoinhas de 40mm**
        
        <div id="attachment_995" style="width: 460px" class="wp-caption aligncenter">
          <img aria-describedby="caption-attachment-995" src="http://miguelalho.com/wp-content/uploads/2009/09/015-1.jpg" alt="fonte 1U com 2 ventoinhas de 40mm" title="015-1" width="450" height="191" class="size-full wp-image-995" />
          
          <p id="caption-attachment-995" class="wp-caption-text">
            fonte 1U com 2 ventoinhas de 40mm
          </p>
        </div>
        
        A fonte tem 2 ventoinhas nos extremos, que provocam o fluxo de ar no interior da fonte que é bastante preenchido. Felizmente, os fios das ventoinhas são ligados com conectores em vez de estarem soldados ao circuito impresso, o que facilitara a tentativa de substituição. Penso que será uma solução válida, desde que haja redução do ruído das ventoinhas, sem afectar gravemente a temperatura na fonte, e portanto, do interior da caixa. Ventoinhas de 40mm são baratas, portanto não deve ser problemático. Resta testar. Mas antes disso, a terceira possibilidade:
        
        **Substituir por uma fonte &#8220;normal&#8221;**
        
        Este ponto tem uma vantagem clara: Fontes de PC normais são baratas e silenciosas. Têm espaço para arrefecer, ventoinhas maiores e menos ruidosas, e o mesmo tipo de conexão. O problema é que não cabem na caixa 1U &#8211; tem de ficar do lado de fora. Para fixar no bastidor, a fonte terá de ficar &#8220;pendurado&#8221; com o auxilio de outro elemento, o que n será bom ou até possível para fixar e remover do bastidor, ou terá de ficar pousado sobre a caixa e com um intervalo que permita encaixar no bastidor. Eu testei esta solução, sem qualquer problema, com uma fonte de 300W. Removida a fonte antiga, bastou passar os cabos para dentro da caixa pelo buraco da interface, ligar os conectores à board e aos discos, _et voilá_, fonte silenciosa ligada à caixa. No meu caso, a fonte até é de 20 pinos, em vez dos 24 pinos, mas é possível ligar na mesma. Simplesmente é necessário ligar o conector de 4 pins extra, também à board, na tomada própria.
        
        Para melhorar, há uma solução que penso que é viável, barato, mas que exigirá um pequeno esforço. A ideia é estender os cabos de 20/24 pinos da fonte. O ideal é mesmo que na caixa seja fixa uma interface para ligar o conector de 2o/24 pinos, fichas dos discos, e o de 4 pins (caso necessário).esta tomada de interface ligará desde a interface aos dispositivos internos; depois a fonte pode ser conectado à interface, sem ter que estar fisicamente dependente da caixa do servidor &#8211; poderei por a fonte na base do bastidor, pendura-lo num suporte do bastidor, etc. Acho uma ideia interessante e viável.
        
        <div id="attachment_996" style="width: 460px" class="wp-caption aligncenter">
          <img aria-describedby="caption-attachment-996" src="http://miguelalho.com/wp-content/uploads/2009/09/021.jpg" alt="Fonte normal ligado ao server pelo exterior" title="021" width="450" height="600" class="size-full wp-image-996" />
          
          <p id="caption-attachment-996" class="wp-caption-text">
            Fonte normal ligado ao server pelo exterior
          </p>
        </div>
        
        Já agora, e para que seja minimamente perceptível a diferença de nível de som das fontes, veja o vídeo &#8211; o primeiro é o de 1U, e o segundo de um PC normal; ambos forma gravados com musica de fundo ao mesmo nível sonoro.
        
        
        
        Resta agora o processo de cooling do processador!
        
          * [Redução de ruído de um servidor 1U &#8211; part 1 (Intro)][1]
          * [Redução de ruído de um servidor 1U &#8211; part 2 (Fonte de Alimentação)][2]
          * [Redução de ruído de um servidor 1U &#8211; part 3 (Cooling do Processador)][3]
          * [Redução de ruído de um servidor 1U &#8211; part 4 (Conclusão)][4]

 [1]: http://www.miguelalho.com/?p=988
 [2]: http://miguelalho.com/?p=993
 [3]: http://miguelalho.com/?p=1012
 [4]: http://miguelalho.com/?p=1019