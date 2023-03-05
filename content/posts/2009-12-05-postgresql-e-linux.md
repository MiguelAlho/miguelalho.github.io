---
title: PostgreSQL e Linux
author: Miguel Alho
type: post
date: 2009-12-05T02:58:52+00:00
url: /postgresql-e-linux/
wordbooker_options:
  - 'a:16:{s:23:"wordbook_default_author";s:1:"0";s:23:"wordbook_extract_length";s:3:"256";s:25:"wordbooker_like_share_too";s:2:"on";s:25:"wordbook_fbshare_location";s:3:"top";s:24:"wordbook_fblike_location";s:3:"top";s:22:"wordbook_fblike_action";s:9:"recommend";s:27:"wordbook_fblike_colorscheme";s:4:"dark";s:20:"wordbook_fblike_font";s:5:"arial";s:22:"wordbook_fblike_button";s:12:"button_count";s:21:"wordbook_fblike_faces";s:5:"false";s:18:"wordbook_attribute";s:31:"Posted a new post on their blog";s:29:"wordbook_republish_time_frame";s:2:"10";s:29:"wordbooker_status_update_text";s:35:": New blog post :  %title% - %link%";s:19:"wordbook_actionlink";s:3:"300";s:18:"wordbook_page_post";s:4:"-100";s:18:"wordbook_orandpage";s:1:"2";}'
categories:
  - 'Code &amp; IT'
tags:
  - base de dados
  - configurar
  - Linux
  - PostgreSQL
  - Ubuntu

---
Cada vez mais aprecio o PostgreSQL. A base de dados é, efectivamente, muito capaz e poderosa, e felizmente não transporta a barreira das licenças que alguns outros sistemas de base de dados portam. Não é que não os justificam, e empresas que compram licenças desses sistemas reconhecem a sua importância e valor. Mas o PostgreSQL é efectivamente uma base de dados bastante simpático e funcional com um custo reduzido.

Algo que considero muito útil no Postgre é ser multi plataforma. Windows, Linux e MAC.. é escolher, que o PostgreSQL corre. O PgAdminIII, aplicação de gestão da base de dados com GUI também corre em Windows e Linux. Ainda não testei comunicação entre base de dados e servers aplicacionais suportando SOs diferentes, mas penso que é evidente o correcto funcionamento e comunicação.

Hoje estou a instalar um server em Linux, para suportar o JIRA.Tive dificuldades com o Confluence (coisa estranha de má tradução entre IIS e Tomcat dos endereços das páginas), e decidi mover as aplicações de gestão para um server dedicado. O Ubuntu é a minha &#8220;flavor&#8221; preferida (é todo o conceito&#8230;). As aplicações da Atlassian utilizam Tomcat, e tem versões que portam o serviço com elas. Pensei em o server como Tomcat server (o Ubuntu tem essa opção), mas decidi seguir a recomendação do fabricante e deixar as aplicações correr em instâncias dedicadas. O que instalei por defeito foi o clássico LAMP e a base de dados PostgreSQL.

No entanto, o PG n funciona por si só sem uma ligeira configuração. No Windows, é realizado na instalação com o Wizard, mas no Linux é mais simples / imediato com umas linhas de comando. Naturalmente [o processo está mais que documento na web][1], mas nunca é demais reescrever.:

Para começar, duas instruções para instalar o PostgreSQL (caso n tenha sido usado a opção de instalação no processo de instalação do SO):

 `sudo apt-get install postgresql<br />
 sudo apt-get install pgadmin3<br />
` 

O primeiro pode ser ignorado caso a instalação tenha sido efectuada na instalação do SO. A segunda é um GUI de administração muito útil.

Segue então a configuração, sendo necessário inicializar o utilizador base e a password para o mesmo:  
`sudo -u postgres psql postgres<br />
\password postgres`  
e introduza a password desejada para o utilizador postgres.

Outras coisas que podem ser feitas:  
criar uma bd:> `sudo -u postgres createdb [nome da base]`  
iniciar o serviço:> `sudo /etc/init.d/postgresql-8.4 [start | stop | restart]`

Finalmente, há mais dois detalhes importantes a resolver. O Postgre é, por defeito, bastante restriivo no acesso, permitindo acesso apenas por conexões vindas da própria máquina. Para aceder remotamente, primeiro deve permitir que os utilizadores da base possam autenticar-se na base na rede (caso queira esta funcionalidade). É necessário acrescentar o seguinte ao ficheiro /etc/postgresql/8.4/main/pg_hba.conf

`<br />
# TYPE  DATABASE    USER        IP-ADDRESS        IP-MASK           METHOD<br />
host    all         all         x.x.x.0       255.255.255.0    md5<br />
`  
onde x.x.x.0 é a definição da rede (p.e. 10.0.0.0 ou 192.168.0.0).

Para que seja possível acesso externo ao servidor de base de dados, e necessário editar /etc/postgresql/8.4/main/postgresql.conf, retirando o comentário à linha #listen_addresses = &#8216;localhost&#8217;  
e sustituír ou acrescentar ao &#8216;localhost&#8217; o &#8216;*&#8217; para todas as conecções, ou uma gama de IPs para limitar. P.e.:  
listen_addresses = &#8216;*,localhost&#8217;

E já vai bem encaminhado 😀

 [1]: https://help.ubuntu.com/community/PostgreSQL