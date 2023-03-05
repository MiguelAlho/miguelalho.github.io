---
title: 'Tech Week #1 – Backups – I'
author: Miguel Alho
type: post
date: 2007-06-04T15:04:00+00:00
url: /tech-week-1-backups-i/
categories:
  - 'Code &amp; IT'
tags:
  - Aplicações
  - equipamento
  - Tech Week
  - técnica

---
Vou iniciar uma rubrica não regular no blog. Vou dedicar uma semana a um tema principalmente ligadas a temas tecnológicos (ligadas directa ou indirectamente à fotografia). E porque nestas ultimas semanas tenho andado às volta com backups e sistemas de backups, decidi falar no blog sobre isso, 

<span style="font-weight:bold;">Porquê precisas de um backup?</span>

Porque é uma chatice do caraças perder todo aquele trabalho que passaste tempos a realizar, os documentos que perdeste imenso tempo a escrever e a compilar, e os dados que foste recolhendo e organizando!

Algo que geralmente não percebemos é o risco a que estamos sujeitos com o uso de sistemas computacionais &#8211; é que os sistemas computacionais não são à prova de falhas. E quando descubrimos, é pelas piores razões. O hardware pode falhar por desgaste de peças móveis, defeito de fabrico, condições atmosféricas, sobrecargas energéticas, descuído&#8230; A própria forma de utilização do equipamento (PCs principalmente) representa um risco, principalmente por descuido ou desconhecimento relativo aos vírus e às formas como se propaguem, mas também pela desatenção na utilização do equipamento &#8211; o típico apagar um ficheiro sem crer, pensando que era outro ou que estava noutro sitio. 

Se bem que não podemos garantir a salvaguarda de dados a 100%, é possível reduzir em muito a possibilidade de perda. O principal método de diminuir o risco é o uso de boas práticas na utilização e gestão de equipamento. Não tenho números para suportar isto, mas diria que grande parte das perdas de dados se devem ao mau uso do PC. Especialmente por aqueles que não tem muito conhecimento técnico sobre como funciona a &#8220;maquina&#8221;. Digo isto porque quem sabe como funciona internamente o equipamento terá boa ideia das possíveis falhas. Veremos algumas ideias a ter em conta neste aspecto.

O segundo método de diminuir o risco, e que suporta as falhas do primeiro, é a criação de redundância dos dados. Copias da informação. Se uma falhar, existe a copia. E se esta redundância for criada de uma forma automatizada, tanto melhor! Existem muitas opções no mercado para esse efeito. Alguns são sistemas carrissimos que saem fora do âmbito deste conjunto de posts, mas naturalmente existem muitas mais opções para sistemas simples. Tendo em conta o baixo custo actual dos equipamentos de armazenamento de dados, é possível fazer muito com pouco. 

<span style="font-weight:bold;">Boas Práticas</span>

Deve haver uma lista enorme de boas práticas para salvaguardar dados. Eu posso mencionar alguns pontos, que ajudam a preservar dados. As dicas que seguem são principalmente ligadas ao Windows por ser a que mais utilizo. Tenho algum conhecimento dos sistemas em Linux (mas ainda não são muito profundos), e nenhuns do OS da MAC (espero que deixem comments nesse sentido), portanto é dificil dar muita informação nesse sentido. Aqui vai:

  * <span style="font-weight:bold;">Utilize Partições</span> &#8211; Da próxima vez que formatar a o disco para instalar o SO de novo (como por exemplo depois de um ataque de um virus, ou uma falha geral do SO), crie partições. Partições não são mais que divisões lógicas da informação no disco. A maioria dos computadores vem novos com um disco de grande capacidade, mas apenas com uma partição (conhecido tipicamente como &#8220;disco C&#8221;). Nesta situação, quando guardas informação, geralmente guardas algures na pasta &#8220;Meus documentos&#8221; do Windows ou no &#8220;/home&#8221; do Linux. E desta forma os teus dados (docs, videos, musica, fotos, etc) ficam misturadas no mesmo disco que os ficheiros de sistema. Se houver uma falha no sistema, dificilmente terás acesso aos teus ficheiros. Quando acontece, normalmente o disco é formatado de novo e perdes tudo &#8211; sistema E dados. 
    Com partições, crias divisões lógicas no mesmo disco. É como ter vários discos num só. Há software que permite fazê-lo sem perda dos dados já existentes (como o Partition Magic ou o LVM no caso do Linux), mas nunca usei. Geralmente faço na altura da formatação do disco. A ideia é deixar espaço suficiente para o sistema e os programas q iremos instalar (20-30GB é mais que suficiente para a grande generalidade das pessoas), e com os espaço que sobra manter uma ou duas partições. No caso do Windows, o que fica das partições são novos discos no SO &#8211; tipicamente indexados nas letras seguintes e de forma sequencial &#8211; D:, E:, &#8230; No caso do Linux pode ser montado como qualquer pasta que se deseja, como por exemplo montar a partição na pasta &#8220;/home&#8221;. A partir do momento de instalação, os dados devem ser armazenados nessas partições novas. 
    
    Qual a vantagem? Assim, o sistema e os dados ficam separados de uma forma lógica. E se tiveres necessidade de repor o sistema, apenas necessitas de formatar a partição com o sistema (C:) que os restantes mantém-se intactos. Preferes usar &#8220;Os Meus Documentos&#8221; como destino dos ficheiros? Uma solução para integrar com as partições é criar atalhos das pastas onde armazenas os dados nas partições dentro de &#8220;Os Meus Documentos&#8221;. Assim, quando guardas um ficheiro dentro duma pasta de &#8220;Os Meus Documentos&#8221;, na realidade estás a guarda-la dentro da pasta presente na partição dos dados.
    
      * <span style="font-weight:bold;">Utilize o caixote do lixo!</span>  
        Não é só na rua. É também no PC. A combo Shift+Del é perigosa e pode-se apagar o ficheiro errado&#8230; para sempre! Se for para o caixote do lixo, é sempre possível ir lá buscar. 
          * <span style="font-weight:bold;">Use protecção</span>  
            Antivirus, anti-spyware e firewalls são essenciais para proteger o PC de elementos maléficos que entram pela rede. Convém estar sempre configurados e actualizados. Mas acima de tudo, é preciso usar a cabeça. Cuidado com os links do MSN (especialmente se aparecerem do nada e com endereços russos ou estranhos), dos spywares e outro tipo de wares que vem nas instalações de algumas aplicações, e dos links em mails. São os típicos pontos de entrada dos vírus no computador. 
              * <span style="font-weight:bold;">Guarda os dados periodicamente</span>  
                Convém fazer backups periodicamente. Como costumam dizer e com toda a razão, cria uma agenda e mantenha-o de forma rigorosa! Mais sobre isto nos próximos posts. 
                <span style="font-weight:bold;">A minha maneira</span>  
                Backups sempre estiveram presentes no meu workflow. A razão principal era a constante falta de espaço nos discos. Como em qualquer tipo de esquema de armazenamento, por mais espaço que tenhamos, precisamos sempre de mais!
                
                Geralmente faço backups para DVD. É uma solução barata tendo em conta o espaço de armazenamento. E não requer grande investimento pontual. Posso ir adquirindo packs à medida das necessidades. Especialmente para fotografias, faço sempre duas cópias. Depois guardo cada um em sítios diferentes (um está numa capa, e que tem mais acessos; a outra cópia fica numa cake-box). Em muitos casos, como são as únicas duas cópias que tenho (tinha que eliminar as imagens para libertar espaço em disco) se uma falhar, a outra está preservada e poderei fazer novas copias.
                
                Há pouco tempo atrás, adquiri um disco externo de 250Gb o que é óptimo e estava destinado a manter a colecção fotográfica (apenas) mas infelizmente a falta de espaço noutros discos obrigou-me a começar a misturar outros tipos de dados. No entanto, continuo a realizar periodicamente a dupla cópia de backups das imagens, sempre que tenha o espaço de um DVD por preencher. Há quem faça backups imediatos de imagens (mal saem para o PC são armazenados em DVD), mas geralmente não tenho necessidade de levar o processo a esse ponto. Se o trabalho ou conjunto de imagens for muito importante, de certeza que o farei.
                
                Actualmente estou no processo de organizar dois sistemas de backups. Um é pessoal. Agora que tenho um portátil a meu dispor para al