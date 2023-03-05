---
title: 'Tech Week #1 – Backups – II'
author: Miguel Alho
type: post
date: 2007-06-05T22:25:00+00:00
url: /tech-week-1-backups-ii/
categories:
  - 'Code &amp; IT'
tags:
  - Aplicações
  - equipamento
  - Tech Week
  - técnica

---
Continuando da <a href="http://mytymyky.blogspot.com/2007/06/tech-week-1-backups-i.html" target="_blank">primeira parte</a>, vamos olhar a algumas opções de armazenamento de dados disponíveis, e recomendáveis para processos de backup. Mas antes de ver os componentes específicos, vamos ver alguns conceitos:

  * <span style="font-weight:bold;">Armazenamento on-line/off-line</span>  
    O conceito de on-line e off-line ao nível do armazenamento prende-se com a forma como é guardado o hardware que suporta os dados. On-line refere-se ao meio ligado à máquina de armazenamento. Off-line significa que é removível do computador, mas mantém os dados. Off-line é uma boa sugestão para backups. Armazenarenas os dados no equipamento, e depois guardas noutro local que consideras seguro. Os dados ficam armazenados e sem o risco de serem mexidos. 
      * <span style="font-weight:bold;">Tecnologias e equipamentos</span>  
        Apesar de existirem diversas tecnologias para o armazenamento de dados, as três principais em uso são os meios ópticos, magnéticos e electrónicos. 
        Dos três, talvez o mais importante seja o magnético. Antigamente eram as disquetes, hoje são discos duros de grande capacidade e as cassetes de armazenamento. As cassetes são um meio típico em uso em empresas, mas tem um custo relativamente elevado. As cassetes ou fitas magnéticas tem capacidade para algumas largas dezenas de gigabytes de capacidade. No entanto a escrita é lenta e o equipamento de escrita e leitura é cara, e portanto geralmente fora do alcance e desnecessário para utilizadores comuns. 
        
        Já os discos duros estão bem mais em uso, com capacidades elevadíssimas &#8211; das largas dezenas ao milhar de gigabytes &#8211; e com o preço a descer à velocidade de um avalanche. Um disco de 500GB, externo, já só custa 100-140€ &#8211; menos de 30 cêntimos por Gigabyte. Há com três &#8220;sabores&#8221; &#8211; &#8220;Micro-drives&#8221;, mais parecidas com cartões de memória compact flash; discos de 2,5&#8243; típico dos computadores portáteis; e discos de 3.5&#8243; mais comuns nos computadores. Os mais pequenos são naturalmente mais caros e com menos capacidade, no entanto são bem mais portáteis, e geralmente as versões com caixa externa não necessitem de alimentação própria, bastando a alimentação de uma porta USB. No caso de backups, o ideal será o formato maior, quer pelo custo, quer pela característica estática do equipamento (não faz muito sentido arriscar a destruição da tua cópia de segurança com transportes&#8230;).
        
        Dos sistemas ópticos, o CD e DVD estão mais que implementados no mercado, sendo o DVD o &#8220;rei&#8221;. Tratam-se de bolachas plásticas em que os dados são queimados na superfície do disco através de um laser e a leitura é feita também por um laser. Os DVDs suportam cerca de 4.7 gigabytes por disco com um custo reduzido (0.30€ &#8211; 1€ por disco), se bem que há deles de dupla face com o dobro da capacidade, mas naturalmente mais caros. Agora começam a surgir os HD-DVD e Bluray, com suporte para 30-50GB num só disco, mas dada a &#8220;novidade&#8221; ainda são um meio muito caro para o utilizador doméstico. Este tipo de disco é óptimo pela facilidade de arrumação &#8211; guarda-se numa caixa ou empilhada numa cakebox. Apesar de haver deles re-graváveis, a grande maioria é de apenas uma gravação, o que significa que uma vez gravado os dados, não mais poderão ser alterados &#8211; uma protecção óptima para um backup. 
        
        Devido à sua natureza óptica, é necessário ter cuidados extra com o seu manuseamento para preservar a face de leitura dos dados de riscos que poderão comprometer os dados armazenados. É necessário ter também cuidado com a preservação &#8211; alguns materiais usados no fabrico não garantem a durabilidade a longo prazo da informação. Corrosão e humidades podem provocar danos irreversíveis. Até as tintas usadas podem comprometer o disco (já tive alguns &#8211; poucos &#8211; em que a tinta da superfície superior descolou, tornando o disco inútil). Mas existem com diversos, materiais &#8211; uns mais caros que outros, e portanto deve ser tido em conta a importância dos dados na altura da compra. 
        
        Um aspecto óptimo dos DVD é que, apesar de ser mais pequeno em capacidade que um disco, se tiveres um conjunto de DVDs e um se estragar, apenas perdes uma parte dos dados da colecção; já se um disco rígido vai à vida, lá se vão os dados todos&#8230;
        
        <span style="font-weight:bold;">Distribuição e replicação</span>  
        Já foi mencionado que uma das formas importantes de salvaguardar os dados é a criação de redundância. Uma forma possível é a distribuição de dados por vários computadores. é uma solução que faz sentido com redes de computadores, e com conjuntos de informação que convém estar constantemente disponíveis. Os dados de uma máquina são replicados em várias máquinas da rede. Se uma falha , há cópias da informação noutros locais, e não deixam de estar acessíveis.
        
        Para soluções localizadas, um método de redundância existente para discos duros é o sistema RAID. RAID significa Redundant Array os Independent(Inexpensive) Discs &#8211; vários discos a funcionar como um único. Existem vários modos de RAID, identificados por números &#8211; 0, 1, &#8230; em que os mais comuns são o 0, 1, e 5. 
        
        Em RAID 0, dois discos são usados como se apenas um se tratasse, com a soma da capacidade de âmbos. Assim dois discos de 250Gb funcionam como 1 de 500Gb. Parece uma boa solução, mas acredita que, se queres guardar dados a longo prazo, é de esquecer. Os dados são divididos por ambos os discos e se um falha, lá se vão os dados dos dois (já o vi a acontecer :S)! É óptimo para armazenar dados pouco importantes ou com curto prazo de necessidade e é rápido (porque os dados são divididos pelos dois discos e armazenados em paralelo). Mas para salvaguardar dados por um período longo é um risco demasiado grande. 
        
        RAID 1 utiliza pelo menos dois discos e espelha-os. Ou seja dois discos de 250Gb funcionam como um de 250Gb e um é espelho directo do outro. É um óptimo sistema, pois mantém sempre duas cópias da informação, bastando estar um em funcionamento para garantir continuidade de dados. Se um falha, o outro tem os dados intactos. No entanto utiliza apenas 50% da capacidade total dos discos. Se tiveres 3 discos de igual capacidade disponíveis, este pode implementar um sistema de rotatividade interessante &#8211; em qualquer momento há dois discos em uso no RAID1, e um off-line com os dados de até determinado período. Periodicamente a troca entre o que está off-line e um em funcionamento pode ser feito para garantir uma grande quantidade de dados existentes até determinada altura. No entanto é necessário ter cuidado com dados entretanto eliminados&#8230;
        
        RAID5 utiliza 3 discos no mínimo e mantém um esquema de paridade de dados entre os discos. Se um falha, o sistema funciona normalmente com os outros dois. Basta substituir o que falhou por um novo e o arranjo é reconstruído sem perda de dados. Também é aproveitado melhor a capacidade dos discos. Mas é claro que não é perfeito.. se dois discos falharem, capute&#8230;
        
        O RAID é geralmente efectuado através de controladores de hardware específicos mas também é possível por software. Algumas motherboards já a incluem, se não me engano, mas é possível adicionar os componentes necessários para o efectuar. Também existem equipamentos de armazenamento externo com RAID implementado, mas que é naturalmente uma solução mais cara.
        
          * <span style="font-weight:bold;">DAS e NAS</span>  
            DAS significa &#8220;Directly Attached Storage&#8221; e NAS significa &#8220;Network Attached Storage&#8221;. No primeiro, o equipamento de armazenamento está ligado directamente ao computador &#8211; o perfeito exemplo é o disco externo. No segundo, o equipamento está à distância, ligado através de uma rede, seja interna (LAN) ou externa (Internet). Ambas são boas opções, dependendo das quantidades de equipamentos disponíveis. Para apenas um computador  
            com dados a salvaguardar, o DAS é a solução típica; para um conjunto de computadores em rede, o NAS poderá ser a melhor solução, se bem que sempre mais cara..</p> 
            As NAS também vêm com muitas variedades e preços. A força principal dos NAS são servidores dedicados ao armazenamento. Geralmente tem grande poder de processamento e grande capacidade de armazenamento&#8230; e são caros! Óptimos para empresas com grandes fluxos de dados, mas &#8220;overkill&#8221; para o utilizador doméstico. Mas os NAS não estão limitados a servidores poderosos. É possível construir um NAS de baixo custo / pouco investimento. Se tiveres algum equipamento mais antigo disponível como um PC com alguns anos mas ainda funciona, basta arranjar os discos para armazenamento e a placa de rede. Falarei melhor desta solução nos próximos posts. A outra opção são discos NAS dedicados. São basicamente discos externos, mas com controladores de rede integrados. E sem duvida uma forma simples de partilhar dados entre vários computadores. Por exemplo quer a <a href="http://www.iomega.com" target="_blank">Iomega </a> quer a [Lacie][1] produz uns com o RAID implementado, como também os discos externos simples e com o controlador de rede. Estes últimos são pouco mais caros que os discos externos USB e uma solução interessante para partilhar informação.
            
            <span style="font-weight:bold;">Conclusão #2</span>  
            Enfim, o post vai longo, mas ficam alguns conceitos dedicados ao tema e que serviram para encontrar soluções. Esse aspecto fica para o próximo post!
            
            <span style="font-weight:bold;">A Serie TechWeek#1 &#8211; Backups</span>  
            <a href="http://mytymyky.blogspot.com/2007/06/tech-week-1-backups-i.html" target="_blank">Backups I &#8211; O porquê;</a>  
            <a href="http://mytymyky.blogspot.com/2007/06/tech-week-1-backups-ii_05.html" target="_blank">Backups II &#8211; Os conceitos e tecnologias;</a>  
            <a href="http://mytymyky.blogspot.com/2007/06/tech-week-1-backups-iii.html" target="_blank">Backups III &#8211; Soluções, Tipos, e problemas</a>

 [1]: http://www.lacie.com/intl/products/range.htm?id=10007