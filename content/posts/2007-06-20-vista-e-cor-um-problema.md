---
title: Vista e cor… um problema?
author: Miguel Alho
type: post
date: 2007-06-20T10:07:00+00:00
url: /vista-e-cor-um-problema/
categories:
  - photography
tags:
  - equipamento
  - gestão de cor
  - técnica

---
No fim de semana decidi tentar calibrar, dentro do possível, o monitor do portátil que tenho, e que está com Vista. Escrevi &#8220;tentar&#8221; porque 1) o monitor é quase incalibrável (LCD e muda facilmente o contraste consoante o ponto de vista, infelizmente e 2) não encontrei os controlos de calibração do Adobe Gamma, que estava habituado a usar no desktop e 3) não tenho nenhum equipamento de calibração. Na verdade o único controlo que encontrei foi o da placa gráfica, no painel de controlo da NVidia.

De qualquer forma, tentei procurar informação sobre a calibração de cor no Vista, a ver onde encontraria o Gamma no meio daquilo e encontrei um artigo/post no <a href="http://www.outbackphoto.com/tforum/viewtopic.php?TopicID=2518" target="_blank">Digital Outback Photo</a>. Trata-se de um artigo publicado inicialmente no Cromix Newsletter #26 (escrito por Steve Upton) em que o autor apresenta informação sobre o tratamento de cor nos sistemas MS Windows (passado e presente). <a href="http://www.outbackphoto.com/tforum/viewtopic.php?TopicID=2518" target="_blank">O artigo</a> merece uma leitura atenta, especialmente se estiveres atento a questões de cor e calibração da cor no teu workflow de imagens.

Resumidamente, a Microsoft apresenta no novo S.O. um novo sistema de gestão de cor (CMS) &#8211; Windows Color System (WCS). O WCS vem substituir o CMS anterior &#8211; Image Color Management (ICM) e que era baseado em perfis ICC. O WCS tem sido desenvolvido pela Microsoft em parceria com a Canon. O WCS não é compatível com ICC (são estruturas de informação diferentes) mas é possível a conversão da informação. No Vista, ainda é possível usar perfis ICC desde que todos os perfis em uso são ICC. Caso contrario, WCS é usado e os ICCs são convertidos para a estrutura WCS. Mais, os perfis WCS podem ser embutidos nas estruturas do ICC. (Confusão, né?&#8230; Eu sei&#8230;) As conversões no WCS são calculadas &#8220;on the fly&#8221;, sendo rápido e eficiente. Os perfis WCS também armazenam informação do ponto negro.

No entanto, apesar das vantagens que o WCS apresentaa (e estão listadas no artigo), há problemas associados&#8230; que aparentam ser mais do Vista do que própriamente do WCS. O primeiro que salta á vista no artigo é a dificuldade em carregar as curvas de calibração dos monitores. É um problema de conflitos de carregamento das Tabelas de consulta (lookup tables &#8211; LUT). O Windows sempre dependeu de aplicações externas para carregar esses dados (nos MAC é carregado no arranque desde o OS 8). Como tal, duas aplicações podem ter LUTs diferentes carregadas. 

O segundo problema que chama a atenção é o &#8220;bug&#8221; da autorização. Se já usas ou usaste o Vista, é natural que tenhas ficado farto do contínuo pedido de autorização para efectuar uma tarefa administrativa. E quando isso acontece, o ecrã escurece. Pois é, esse escurecimento é algo que altera os perfis de cor carregados! Ou seja entras no Vista, carregas a calibração do monitor com uma aplicação externa, e 5 minutos depois num pedido de acção administrativa, a calibração é alterada na gráfica (para o escurecimento) e nunca é reposto&#8230; é facil perder confiança na calibração que tens, com o Vista. Felizmente é possivel eliminar a alteração desligando o UAC e o escurecimento do ecrã (ver: <a href="http://www.howtogeek.com/howto/windows-vista/disable-user-account-control-uac-the-easy-way-on-windows-vista/" target="_blank">disable user account control uac the easy way on windows vista</a> & <a href="http://www.howtogeek.com/howto/windows-vista/make-user-account-control-uac-stop-blacking-out-the-screen-in-windows-vista/" target="_blank">make user account control uac stop blacking out the screen in windows vista</a> )

Naturalmente o WCS parece interessante e um avanço no tratamento de cor no Windows. Mas os problemas que o Vista apresenta são problemáticos e até &#8220;corrigidos&#8221;, poderá ser problemático usar o WCS eficientemente, especialmente a um nível profissional onde é necessário garantir precisão na côr. Mas convém ler o artigo completamente para ter a correcta percepção da questão da gestão da cor no Vista. 

Agora já só falta calibrar o monitor&#8230;.