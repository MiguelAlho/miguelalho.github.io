---
title: Adicionar novas línguas ao Windows (edições Home)
author: Miguel Alho
type: post
date: 2010-07-02T16:53:10+00:00
url: /adicionar-novas-linguas-ao-windows-edicoes-home/
categories:
  - 'Code &amp; IT'
tags:
  - globalização
  - local
  - localização
  - MUI
  - PostgreSQL
  - Windows 7
  - Windows Server 2008

---
Eu desenvolvo sobre Windows, sobre um Windows 7 Home Premium, para ser mais exacto. E ao longo do tempo tenho sofrido de alguns obstáculos oferecido pelas limitações da versão. A versão é a que veio com o PC, e pretendo puxá-lo até onde possível, antes de fazer um upgrade para um Business ou Ultimate (apesar das restantes máquinas &#8211; desktop &#8211; aqui do escritório estarem com o Ultimate).

Mas infelizmente ontem, deparei-me com um problema que demorou a tarde toda a perceber, e quando assim é, é geralmente algo do OS e nada óbvio. Contextualizando, estou numa fase de internacionalizar uma aplicação web de gestão de currículos e processos de recrutamento e selecção, que desenvolvi no ano passado (e ainda este ano). Foi grande sucesso junto da equipa de trabalho, pois permitiu acelerar em muito os processos de recrutamento, que dado o volume de candidaturas que lhes chegavam &#8211; dezenas de milhares em meio ano &#8211; era tediante e constrangedor. Felizmente, a app foi criada à medida da equipa e adaptado ao processo de trabalho, e resultou em pleno.

<!--more-->

De qualquer forma, a app está a ser internacionalizado para suportar as acções em Espanha, e o processo de internacionalização tem os seus próprios desafios associados &#8211; a globalização da aplicação para não ter dependências directas da língua nem numeração, e a localização para corresponder a cada realidade, quer de linguagem, quer de informação (por exemplo as diferenças de moradas e códigos postais e informação regional).

Sei que esta fase trará alguns problemas de estrutura e configuração &#8211; umas mais complicadas que outras &#8211; e a primeira apareceu logo na base de dados. Para a aplicação, optei pelo suporte de dados em base de dados relacional, e a base escolhida foi o PostgreSQL. Tem óptimas capacidades de armazenamento, suporte a pesquisa em XML, indexação e pesquisa de texto livre com o tSearch2, e é OpenSource que também foi muito atractivo na escolha.

Quando se cria uma base de dados no Postgre, o único parâmetro obrigatório é o nome da base de dados. As restantes definições são preenchidas com os valores pré-definidos da instalação. As que condicionam a base e que só podem ser definidas na criação são o LC\_COLLATE e o LC\_CTYPE. A primeira &#8211; LC\_COLLATE, determina a forma como são ordenadas as palavras e que pode variar de cultura para cultura. O mesmo para o LC\_CTYPE que caracteriza os caracteres em termos de maiúsculas, minúsculas, e pontuação

O problema destes parâmetros são a dependência do OS para esta informação. Seria muito interessante se dependesse apenas da base de dados e os executáveis associados, mas na verdade esta informação vem do sistema operativo. Por defeito, o valor para as variáveis são o da definições do OS. O Windows Server 2008 permite que o utilizador possa configurar a sua conta para definições regionais e linguísticas diferentes e que podem ser acrescentados através de MUIs (Multilingual User Interface). O mesmo é possível nas versões do Business e Ultimate do Windows. Infelizmente, no Home Premium não é possível, sem alguns workarounds.

Como afecta a criação da base? Simples. Se não tiver as definições linguísticas instaladas na máquina, não posso criar a base de dados usando essas definições, apenas com as existentes. Para portuguese, e portanto com base na definição do OS, tenho quer para LC\_CTYPE, quer para LC\_COLLATE o valor:

Portuguese_Portugal.1252

Para a base espanhola queria

spanish_Spain.1252

**Acrescentar definição no Windows Server 2008**

Trabalhamos aqui geralmente com duas instâncias de base de dados, uma local e uma remota, centralizada. Procurei resolver primeiro o caso no server, para verificar se era mesmo este o problema. Não tinha o pack espanhol instalado, e portanto puxei-o do site da Microsoft:

<http://www.microsoft.com/Downloads/details.aspx?familyid=03831393-EEF7-48A5-A69F-0CE72B883DF2&displaylang=en>

ou para o Server 2008 c/ SP2

<http://www.microsoft.com/Downloads/details.aspx?familyid=3A7FB7A2-3519-495B-9BC5-2007082CA9A6&displaylang=en>

ou para Server 2008 R2

<http://www.microsoft.com/Downloads/details.aspx?familyid=03831393-EEF7-48A5-A69F-0CE72B883DF2&displaylang=en>

são ficheiros .img pelo que devem ser carregados numa drive virtual como o Virtual Clone Drive.

Depois em Control Panel > Regional and Language Options > Keyboards and Languages > Display Language aparece a lista de linguagens disponíveis e em uso pelo Windows. Há tb um botão &#8220;Install/uninstall Languages&#8221; que permite acrescentar. Deve no Wizard que aparece escolher a pasta da drive para a linguagem especifica e deixar correr a instalação.

No caso do espanhol, puxei o ficheiro do grupo 1, e no wizard escolhi a pasta &#8220;es-ES&#8221;. &#8220;Et voilá&#8221;, spanish_Spain.1252 fica disponível no PostgreSQL. Simples e resolvido.

O processo para as versões do 7 Ultimate e Business devem ser semelhantes. O problema surge para o Home. Não é suportado, mas é possível introduzir as definições de localização. Para tal guiei-me pelas notas em <http://xaueious.wordpress.com/2009/08/22/changing-installed-language-of-windows-7-home-premium-pro-from-en-us/> e segui o caminho da linha de comandos.

**Acrescentar definição no Windows 7 Home Premium**

  1. Descarregue o MUI associado à língua específica daqui <http://www.froggie.sk/7lp64rtm.html>
  2. Agora um dos truques: Se executares o ficheiro descarregado, é extraído um ficheiro &#8220;lp.cab&#8221; para a mesma pasta e que é fundamental preservar. No entanto, pouco depois de executar, o ficheiro desaparece. Portanto considera-o um pouco como um jogo de reacção, e prepara o ambiente para mover o ficheiro para outro local ou renomea-lo ASAP. Copy-paste não resulta.
  3. Na linha de comandos, executado com privilégios de administrador, executa o seguinte comando: 
    <pre>DISM /Online /Add-Package /PackagePath:D:\Caminho\lp.cab</pre>
    
    onde D:\Caminho\lp.cab é o caminho completo para o ficheiro extraído.</li> </ol> </ol> 
    
    O processo demora uns minutos, mas uma vez findado, no Postgre, é possível criar a base com as chaves de locale correctas e desejadas.