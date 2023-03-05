---
title: Redução de ruído de um servidor 1U – part 1 (Intro)
author: Miguel Alho
type: post
date: 2009-09-26T16:03:21+00:00
url: /reducao-de-ruido-de-um-servidor-1u-part-1-intro/
categories:
  - 'Code &amp; IT'
tags:
  - 1u
  - 4U
  - caixa
  - cooling
  - moding
  - ruído
  - servidor

---
Recentemente adquiri um novo servidor. Decidi alojar a minha página localmente, substituindo o serviço de hosting que era suportado externamente há 4 anos. A página não tem um peso de movimento que justifique o custo do alojamento externo, e tendo internamente, posso explorar melhor a administração do server Linux.

Há, no entanto, um problema. Contextualizando, tenho um server em Windows (com processador quad-core e vários discos em RAID), que uso internamente &#8211; backups, desenvolvimento, repositório, etc. Tenho-o numa caixa 4U de rack, e montado num bastidor D.I.Y.. Não é um bastidor perfeito, mas para o que necessito no momento, serve. Futuramente comprarei um em metal quando encontrar um a bom preço e quando for financeiramente viável. O server está montado no espaço de trabalho, e felizmente é bastante silencioso. O som das ventoinhas (do painel frontal, fonte, e saída) e dos motores dos discos são audíveis, mas bastante baixo. Não é incómodo. Um pouco de musica na sala, mesmo baixo, resolve completamente. Nunca medi, mas imagino que pelas características das ventoinhas, o nível não ultrapassa os 20dB.

Inicialmente, a minha opção de server web era a recuperação de um PC que já não utilizava a correr um Ubunto com LAMP instalado. O PC era uma board simples com um Athlon XP2000, um disco IDE de 80Gb e 2Gb de RAM. Instalei tudo lá, (rapidamente) e estava tudo ok. Também, pouco ou nada audível. O meu problema com esta configuração era a falta de uma caixa. Entra o primeiro dos dilemas&#8230;

<div id="attachment_989" style="width: 460px" class="wp-caption aligncenter">
  <img aria-describedby="caption-attachment-989" src="http://miguelalho.com/wp-content/uploads/2009/09/035.jpg" alt="Bastidor D.I.Y com server em caixa e server aberto montado" title="035" width="450" height="600" class="size-full wp-image-989" />
  
  <p id="caption-attachment-989" class="wp-caption-text">
    Bastidor D.I.Y com server em caixa e server aberto montado
  </p>
</div>

A primeira opção era comprar mais uma caixa 4U, o que implicaria um investimento de 100€, aproximadamente. A caixa que comprei para o server Windows foi <a href="http://www.inforlandia.pt/catalogo/detalhes_produto.php?id=901219" target="_blank">um 4U / 19&#8243; da CodeGen, na Inforlândia</a>. É uma caixa que procurava há algum tempo, e estou bastante satisfeito. Tenho os 4 discos bem posicionados e ventilados, e há espaço para instalar tudo correctamente e convenientemente. 

A outra opção era adquirir um server usado num anuncio que encontrei. O server é proveniente de um datacenter no Algarve e era um P4 a 3Ghz, com 2Gb de RAM, 2 discos de 160Gb, duas portas de rede Ethernet gigabit e tudo isto numa caixa 1U. por 100€ + IVA + portes.. (150€, portanto). Como devem imaginar, esta ultima opção, tendo em conta as características e o facto de ter a caixa 1U, para colocar no bastidor, aliciou-me bastante. E foi esta opção que escolhi.

<img src="http://miguelalho.com/wp-content/uploads/2009/09/003.jpg" alt="Novo servidor P4 em caixa 1U. Denote o &quot;blower&quot; no centro." title="003" width="450" height="338" class="size-full wp-image-990" /> 

A verdade é que está tudo ok com a máquina encomendada. Entrega rápida, óptimas condições. Há é um problema que não imaginava, e deve-se muito ao facto de nuinca ter trabalhado com materiais de bastidor, directamente. 1U é sinónimo de pouco espaço e portanto desafios de controlo de temperatura importantes. Processadores P4 (especialmente a serie Prescott) são bem conhecidos pelo consumo de energia e a consequente aumento de temperatura. Temperatura que tem de ser combatida com dissipadores e ventoinhas. Dado o perfil baixo da caixa, ventoinhas de 40mm e &#8220;blowers&#8221; são comuns. E porque são pequenos, tem geralmente de rodar rápido. E porque rodam rápido, fazem uma barulheira infernal. Sério! Num datacenter, imagino que não seja problemático já que as máquinas podem ser isoladas numa sala. Mas aqui no escritório, simplesmente não é possível. O barulho é incapacitante, e imagino que a longo prazo criaria problemas de audição. Seria o mesmo que ter um aspirador a trabalhar 24/7. Muito barulho.

Restam duas opções, então.

  1. volto à primeira solução: adquiro uma caixa 4U, instalo o hardware velho, ou o novo na caixa, e esqueço o assunto.
  2. procuro uma maneira de silenciar o bicho e fico satisfeito com o trabalho realizado (e a história que fica para contar aos poucos interessados)

Vou para a segunda opção. Tenho que solucionar o problema do ruído, minimizando-o tanto quanto possível, com investindo o mínimo possível. Também quero uma solução não destrutiva &#8211; não quero abrir buracos na caixa, nem nada desse género. Já identifiquei os pontos de ruído. São eles a fonte e o blower central, e são estes os problemas a resolver. O som dos discos é mínimo e não apresentam qualquer problema.

Vou dividir a solução que encontrar em vários posts, mostrando cada um à medida que forem implementados.

  * [Redução de ruído de um servidor 1U &#8211; part 1 (Intro)][1]
  * [Redução de ruído de um servidor 1U &#8211; part 2 (Fonte de Alimentação)][2]
  * [Redução de ruído de um servidor 1U &#8211; part 3 (Cooling do Processador)][3]
  * [Redução de ruído de um servidor 1U &#8211; part 4 (Conclusão)][4]

 [1]: http://www.miguelalho.com/?p=988
 [2]: http://miguelalho.com/?p=993
 [3]: http://miguelalho.com/?p=1012
 [4]: http://miguelalho.com/?p=1019