---
title: Javascript Debugger para IE
author: Miguel Alho
type: post
date: 2008-08-10T16:24:26+00:00
url: /javascript-debugger-para-ie/
categories:
  - 'Code &amp; IT'
tags:
  - debug
  - Firebug
  - IE
  - Javascript

---
Uma das tarefas mais dificis que encontro é efectuar debug, especialmente ao javascript. Se bem que no Firefox o Firebug é uma ferramenta espectacular, as diferenças de interpretação de javascript entre browsers não resolver todos os problemas.

No caso da Microsoft, para o IE6/7 não há nada que se integre directamente no browser como o <a href="http://getfirebug.com/" target="_blank">Firebug</a> no Firefox. No entanto a Micrsoft tem efectivamente um debugger que geralmente vem o Microsoft Office que é o &#8220;Microsoft Script Debugger. É pequeno (um installer de 650K) e pode ser obtido em <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=2f465be0-94fd-4569-b3c4-dffdf19ccd99&displaylang=en" target="_blank">http://www.microsoft.com/downloads/details.aspx?FamilyID=2f465be0-94fd-4569-b3c4-dffdf19ccd99&displaylang=en</a>. A ferramneta pode ser utilizado em conjunto com o Visual Studio (para debug de script cliente e de servidor) ou solitáriamnete.

Eu por exemplo necessitei para fazer um debug de um script numa máquina XP sem o Office. Além de ter instalado o Debugger, necessitei de habilitar o debugger no IE. As instalações são descritas em <a href="http://www.jonathanboutelle.com/mt/archives/2006/01/howto_debug_jav.html" target="_blank">http://www.jonathanboutelle.com/mt/archives/2006/01/howto_debug_jav.html</a> e <a href="http://www.my-debugbar.com/wiki/CompanionJS/HomePage" target="_blank">http://www.my-debugbar.com/wiki/CompanionJS/HomePage</a>.

E já agora, ao ver este ultimo link, aparece o <a href="http://getfirebug.com/lite.html" target="_blank">Firebug Lite</a>, a espelhar o Firebug do FF mas para IE, Opera e Safari. É baseado num ficheiro javascript a incluir na página a depurar e q sobrepõe um género de consola sobre a página! A ver, sem dúvida.