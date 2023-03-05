---
title: Mover Ficheiros De Um Repositório Para Outro com Subversion.
author: Miguel Alho
type: post
date: 2010-06-30T13:56:11+00:00
url: /mover-ficheiros-de-um-repositorio-para-outro-com-subversion/
categories:
  - 'Code &amp; IT'
tags:
  - svn
  - svnadmin dump
  - svnadmin load

---
Ontem, num curta sessão de manutenção dos meus repositórios de Subversion, senti necessidade de reorganizar algumas, como creio ser &#8220;normal&#8221; ao fim de algum tempo. Tendo a organizar o meu SVN como múltiplos repositórios, um por projecto. Dentro de cada repositório tenho os diversos projectos de classes, sites ou aplicações associados. Tenho assim vários repositórios contextualizados e mais pequenos, em vez de um repositório geral com diversos projectos. A única dificuldade que apresenta é o backup / dump que obriga a manter um script com a lista de comandos de &#8220;_svnadmin dump_&#8221; para cada repositorio individual.

<!--more-->

Também, apenas agora começo a usar a estrutura de pastas típicas do SVN &#8211; /trunk, /brenches e /tags, porque só agora tive necessidade de usar tags para marcar algumas situações. Mover as pastas dentre do repositório para incluir esta alteração é bastante simples, e com o ToirtoiseSVN no server, é drag n&#8217; drop.

Durante a reorganização, senti necessidade de mover alguns repositórios &#8211; essencialmente de projectos de testes &#8211; e uni-los num só repositório contextualizado. Mover pastas _dentro_ de um repositório é muito simples, usando o commando _svn move_ ou usando o RepoBrowser do ToirtoiseSVN Repo Browser. Mas entre repositórios é um pouco mais elaborado. é preciso efectuar o _dump_ do repositórios, e recarrega-lo no novo repositório, preferencialmente indicando a nova pasta para a qual deve ser carregada. A vantagem é que o histórico de alterações é conservada (sofre apenas uma renumeração).

Para fazer o dump, o comando é:

<pre lang="svn">svnadmin dump d:\repoPathToMove &gt; d:\backupFolder\repoName.dump</pre>

Para carregar o _dump_ criado:

<pre lang="svn">svnadmin load d:\pathToNewRepo --parent-dir InternalFolder &lt; d:\backupFolder\repoName.dump</pre>

De notar a opção _&#8211;parent-dir_ onde indico a pasta do repositório para onde os ficheiros devem ser carregados. É necessário criar a pasta no repositório de destino antes de efectuar o _load_. Caso contrário é levantado um erro e os _dump_ não é carregado. Uma forma mais simpels de o escrever (e sem criar o ficheiro _dump_) é:

<pre lang="svn">svnadmin dump d:\repoPathToMove | svnadmin load d:\pathToNewRepo --parent-dir InternalFolde</pre>