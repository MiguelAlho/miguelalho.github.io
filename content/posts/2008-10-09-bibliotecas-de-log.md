---
title: Bibliotecas de Log
author: Miguel Alho
type: post
date: 2008-10-09T18:33:16+00:00
url: /bibliotecas-de-log/
categories:
  - 'Code &amp; IT'
tags:
  - .net
  - Elmah
  - log
  - log4net
  - Nlog
  - regsito
  - spacebin

---
Logging &#8211; guardar registos de actividades das aplicações &#8211; é uma tarefa extremamente importante no desenvolvimento de uma aplicação, e ajuda muito a resolver as dores de cabeça que vão surgindo. Eu tenho usado, nas minhas aplicações, algumas classes de log construídas de raiz &#8211; num caso escrevendo os registos para XML, noutro, para um ficheiro texto simples (CSV).

Mas na verdade, existem já alguns módulos pela web dedicados a esta tarefa. Algumas efectuam o registo apenas de erros das aplicações (.NET) como é o caso do <a href="http://miguelalho.com/?p=588#comment-417" target="_blank">Elmah </a>que falei anteriormente, ou do <a href="http://www.spacebin.net/" target="_blank">Spacebin</a> que é um serviço externo de armazenamento dos registos. A vantagem destes é que captam automaticamente os erros, sem necessitar de inserir código específico À tarefa &#8211; apenas uma entrada no ficheiro de configuração.

Mas num comentário que surgiu no post do Elmah (e que não tomei a devida atenção), estava referido o <a href="http://www.nlog-project.org/tutorial.html" target="_blank">Nlog</a>, um projecto aberto com suporte para diversos típos de registos (trace, debug, info, erro&#8230;) e múltiplos repositórios (dbs, ficheiros, serviços, etc&#8230;). Este parece muito completo e quero experimentar! A <a href="http://www.nlog-project.org/documentation.html" target="_blank">documentação</a> parece bastante completa e intuitiva.

Outro similar ao NLog, mas que utiliza uma abordagem diferente em termos do esquema dos repositórios, é o <a href="http://logging.apache.org/log4net/index.html" target="_blank">log4net</a> que faz parte dos serviços de log da <a href="http://www.apache.org/" target="_blank">Apache</a>. Novamente, múltiplas tipos e repositórios de registo, e OpenSource.

Não havendo razão para construir algo especifico a uma aplicação, estas são, certamente óptimas soluções!