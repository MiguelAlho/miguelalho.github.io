---
title: Redução de Ruído de um Servidor 1U – parte 3 (cooling do processador)
author: Miguel Alho
type: post
date: 2009-11-14T16:45:10+00:00
url: /reducao-de-ruido-de-um-servidor-1u-parte-3-cooling-do-processador/
categories:
  - 'Code &amp; IT'

---
Pois é, já vai algum tempo desde o último artigo da saga de redução de ruído. A parte 3 refere-se ao cooling do processador, já que da fonte conseguimos resolver com a ligação externa a uma fonte silenciosa.

Restava, então, arrumar a questão do servidor com o corte de ruído no sistema de arrefecimento do processador, que era composto por um dissipador passivo sobre o processador (Pentium 4) e uma ventoinha que injectava ar nas laminas do dissipador. A ventoinha era, definitivamente o problema já que a rotação criava um nível de ruído na ordem dos 40db (segundo o datasheet da mesma).

Para esta terceira parte, adquiri um dissipador novo &#8211; um <a href="http://www.gelidsolutions.com/products/index.php?lid=2&#038;cid=12&#038;id=40" target="_blank">GELID Silence 775</a>, de baixo perfil. Infelizmente, a board do servidor ( SuperMicro &#8230; ) não o suportava. De qualquer forma, antes de tomar qualquer decisão sobre como avançar, decidi fazer uns testes. Fiz uma curta análise sobre os efeitos da ventilação na temperatura do processador. Os processadores P4 são, definitivamente, quentes, o que obriga a um sistema de arrefecimento eficiente para manter o correcto funcionamento. A análise que efectuei foi, simplesmente, ligar o servidor, carregando ou a tela da BIOS, ou um Linux, e analisar a variação da temperatura. Foi realizado em períodos de 10 segundos durante até 5 minutos, e em diversas configurações. Eis os resultados:

<a href="http://miguelalho.com/wp-content/uploads/2009/11/cpuTempTests.png"  target="_blank"><img src="http://miguelalho.com/wp-content/uploads/2009/11/cpuTempTests-300x154.png" alt="cpuTempTests" title="cpuTempTests" width="450" class="aligncenter size-medium wp-image-1013" srcset="http://www.miguelalho.pt/wp-content/uploads/2009/11/cpuTempTests-300x154.png 300w, http://www.miguelalho.pt/wp-content/uploads/2009/11/cpuTempTests-1024x527.png 1024w, http://www.miguelalho.pt/wp-content/uploads/2009/11/cpuTempTests.png 1035w" sizes="(max-width: 300px) 100vw, 300px" /></a>

Inicialmente comecei por testar a temperatura com o estado normal (dissipador passive e ventoinha barulhenta), com caixa aberta e fechada. Esta configuração é estável em termos de temperatura, com a temperatura a variar entre os 31 e 35 graus, em que a tampa fechada representa mais 2 ou 3 graus, imagino pela redução de ar novo. De seguida testei sem a ventoinha. O dissipador claramente não era suficientemente grande para funcionar sozinho, e comprovou-se com um aumento muito rápido da temperatura &#8211; cerca de 3º a cada 10 segundos. A chegar aos 2 minutos bateu um ponto critico de 60º e terminei a experiência.

Decidi então efectuar algumas experiências com a redução da frequência do relógio para visualizar o efeito sobre a temperatura. Os primeiros testes forma realizados com a ventoinha no lugar, e denotou-se uma quebra grande na temperatura (abaixo dos 30º) e um crescimento muito lento e estável. A hipótese que punha, tendo em conta a quebra de temperatura com a redução da frequência do relógio é que uma solução passiva poderia eventualmente resultar. A medição seguinte foi realizada sem a ventoinha, mas novamente cresceu rapidamente.

Um último teste &#8211; será que uma solução com uma ventoinha &#8220;regular&#8221; de caixa de 80mm conseguiria arrefecer o processador? eventualmente poderia tentar encaixar um ou vários de modo a criar um fluxo de ar que fosse útil. Eu tinha um disponível (silencioso) de 2000 RPM e fiz a experiência com a ventoinha colocada, ao alto, no lugar da ventoinha do servidor. Nesta posição, a temperatura cresceu a um ritmo regular e terminei a experiência com 55º. Claramente o fluxo de ar era insuficiente. Mas antes de desligar, experimentei colocar a ventoinha directamente sobre o dissipador, extraindo o ar das laminas. A temperatura desceu rapidamente, estabilizando nos 37º. Efectivamente é equivalente ao comportamento &#8220;normal&#8221; de um dissipador de processador com ventoinha.

A extracção do ar do dissipador, seria uma possibilidade para resolver o barulho. Infelizmente a dimensão do dissipador em conjunto com a ventoinha superavam a altura da caixa 1U. Numa 2U caberia certamente. Haveria certamente outras possibilidades com a abertura de um buraco na tampa da caixa, ou afins, mas por enquanto não quero causar esse tipo de destruição.

O que fazer agora? Bem, efectivamente quero aproveitar a caixa (penso que o preço do conjunto justificava a caixa e discos facilmente). Decidi adquirir uma board (ZOTAC G31 micro ATX) e processador (Dual Core E5300) novos, que à partida permitirão utilizar o dissipador da GELID, e até ter um consumo energético menor que o P4.  
Mais testes (e resultados) brevemente!

  * [Redução de ruído de um servidor 1U &#8211; part 1 (Intro)][1]
  * [Redução de ruído de um servidor 1U &#8211; part 2 (Fonte de Alimentação)][2]
  * [Redução de ruído de um servidor 1U &#8211; part 3 (Cooling do Processador)][3]
  * [Redução de ruído de um servidor 1U &#8211; part 4 (Conclusão)][4]

 [1]: http://www.miguelalho.com/?p=988
 [2]: http://miguelalho.com/?p=993
 [3]: http://miguelalho.com/?p=1012
 [4]: http://miguelalho.com/?p=1019