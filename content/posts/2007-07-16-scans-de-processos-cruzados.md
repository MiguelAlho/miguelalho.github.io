---
title: Scans de processos cruzados
author: Miguel Alho
type: post
date: 2007-07-16T23:01:00+00:00
url: /scans-de-processos-cruzados/
categories:
  - Photography
tags:
  - analógico
  - Aplicações
  - Photoshop
  - técnica

---
Acho que posso contar pelos dedos o numero de vezes que consegui uma imagem decente com o processo revelado com o processo cruzado. E sobretudo penso que se trata de problemas de processo. 

O processamento cruzado tem por base a revelação de película em química &#8220;inadequada&#8221;, nomeadamente a revelação de película de slides (positivos que requer químicos do processo E-6) em químicos usados para revelar negativos a cores (processo C-41). Na realidade o processo funciona, no entanto o resultado é um negativo. E dependendo da película, esta terá uma core base diferente da habitual laranja/castanho dos negativos a cores. No caso do Fuji Astia, o resultado é um negativo verde.

Deste modo, apesar de termos uma imagem presente, temos as cores trocadas e contrastes alterados. Tradicionalmente, a impressão seria feita normalmente no laboratório, como para um negativo, mas com um ajusto de filtragem mais trabalhada para tentar aproximar as cores. E os resultados tendem a ser fabulosos, com cores ricas e fortes.

Scanar é que mais &#8220;doloroso&#8221; em termos de processo. O facto prende-se que a generalidade do software procura a cor alaranjada da película para corrigir,e como não o encontra, os resultados são&#8230; esquisitos. E os softwares do scanner geralmente não tem uma opção de escolha para o ajusto automático das cores, porque não existe propriamente um ajusto correcto &#8211; a química destruiu essa opção, basicamente. Mesmo assim tem de haver uma forma de obter resultados minimamente satisfatórios para preparar a restante edição. Numa pesquisa, encontrei este <a href="http://photo.net/bboard/q-and-a-fetch-msg?msg_id=00IsEo" target="_blank">thread</a> no fórum do <a href="http://www.photo.net" target="_blank">photo.net</a> com uma sugestão de scanar como se fosse película preto e branco. Decidi experimentar e fazer comparações.

[<img src="http://farm2.static.flickr.com/1293/832015778_1df503a921.jpg" width="450" height="225" alt="cross_posScan" />][1]  
[<img src="http://farm2.static.flickr.com/1162/832015770_caf3c744fb.jpg" width="450" height="225" alt="cross_negScan" />][2]  
[<img src="http://farm2.static.flickr.com/1003/832015734_53af5d889d.jpg" width="450" height="225" alt="cross_bwScan" />][3]

As três imagens em cima são as previsualizações para scans como película positivo, negativo e preto e branco, respectivamente. À esquerda são scans sem qq ajusto de cor (&#8220;reset&#8221; no software da Epson) e à direita é o scan com o ajusto automático. Como é visivel, como positivo temos a película no seu estado original &#8211; verde! Como negativo de cores, é feito um ajusto automático e que reforça em muito o contraste. O scan a preto e branco, ignora o cast alaranjado, e a correcção é mais ténue, mas mais &#8220;flat&#8221;. O típico dos negs neste caso é a existência de uma componente arroxada em toda a imagem, ou pelo menos muito notável nos céus azuis fortes, especialmente nas transições. 

[<img src="http://farm2.static.flickr.com/1369/832015864_f7d9ce7e2e.jpg" width="450" height="225" alt="cross_bwAdjust" />][4]

Seguindo as indicações do post, efectuei os ajustes de pretos e highlights e gama (o que não é nada fácil na janela tão pequena do preview do Epson Scan). Efectivamente melhorou a imagem. Um contraste mais forte e uma ligeira saturação (10) finalizou o processo de ajusto para o scan (imagem de cima). A imagem scannada dava efectivamente uma boa base de arranque para a restante edição.

No entanto decidi experimentar, parralelamente, outra ideia. E se scannado o negativo como positivo ou como negativo ou como película preto e branco, fizesse os restantes ajustes no PS, sobre a imagem base? O Neg é uma opção horrível &#8211; é muito difícil de efectuar os ajustes (pelo menos sem correcção automática).

No caso da imagem positiva (verde) a imagem produzida ficou muito claro e arosado. Aplicando ajustos no histograma (ponto preto, e ponto branco), apareceram as cores &#8220;correctas&#8221; (ou pelo menso muito próximo do que poderia esperar). Duas curvas ajustaram o contraste para o seguinte resultado:

[<img src="http://farm2.static.flickr.com/1394/832015842_18b275f99d.jpg" width="450" height="401" alt="cross_posPSadjust" />][5]

Um dos problemas do Epson scan, e que considero grave, é q o crop é sempre automático. É util, mas volta e meia há uma imagem da qual o software não consegue detectar correctamente, e é recortado demasiado da imagem. Nesse aspecto, o <a href="http://www.silverfast.com/scanner-software/" target="_blank">Silverfast</a> é muito mais atractivo, e até permite manter as margens! Neste caso esse problema surgiu com a versão positiva&#8230;

A outra tentativa foi com a versão scannado como preto e branco (a 24bits de cor).O processo ignora a inversão das cores, visto que já está correctamente colorido, e restringe-se à correcção de tons &#8211; levels para corrigir o ponto negro e curvas para o contraste. A curva do Cross-process do PS3 n resulta, infelizmente. duas curvas com o preset &#8220;Strong Contrast&#8221; resulta bem. O resultado com uma e duas curvas, respectivamente:

[<img src="http://farm2.static.flickr.com/1400/832015880_2750a9b735.jpg" width="450" height="452" alt="cross_bwPSadjust" />][6]  
[<img src="http://farm2.static.flickr.com/1297/831675071_1419785fce.jpg" width="450" height="452" alt="cross_bwPSadjust2sc" />][7]

Com 2 curvas aplicadas, é possível que o contraste esteja demasiado agressivo. tem piada ver que com um black and white aplicado (e filtro azul, e amarelo ajustado) a coisa tb fica engraçada! 

[<img src="http://farm2.static.flickr.com/1136/831188153_03dee7479d.jpg" width="450" height="452" alt="cross_bwPSadjustBW" />][8]

 [1]: http://www.flickr.com/photos/mytymyky/832015778/ "Photo Sharing"
 [2]: http://www.flickr.com/photos/mytymyky/832015770/ "Photo Sharing"
 [3]: http://www.flickr.com/photos/mytymyky/832015734/ "Photo Sharing"
 [4]: http://www.flickr.com/photos/mytymyky/832015864/ "Photo Sharing"
 [5]: http://www.flickr.com/photos/mytymyky/832015842/ "Photo Sharing"
 [6]: http://www.flickr.com/photos/mytymyky/832015880/ "Photo Sharing"
 [7]: http://www.flickr.com/photos/mytymyky/831675071/ "Photo Sharing"
 [8]: http://www.flickr.com/photos/mytymyky/831188153/ "Photo Sharing"