---
title: Passwords “insonsas”
author: Miguel Alho
type: post
date: 2012-07-14T12:15:15+00:00
url: /passwords-insonsas/
categories:
  - 'Code &amp; IT'
tags:
  - password
  - segurança

---
Um dos pontos fulcrais na segurança de um site é a gestão de passwords dos utilizadores.Sempre que nós como programadores implementamos um sistema de autenticação que recorre a nome de utilizador e password, temos de ter cuidados especiais. Vulnerabilidades que desconhecemos nos nossos sistemas podem comprometer a base de dados e tornar o seu conteúdo acessível.

Se considerar-mos os hábitos de utilizadores, nomeadamente a reutilização frequente de palavras passe, a descoberta de uma palavra passe num local poderá facilitar o acesso a dados do utilizador noutros locais e sistemas. Agentes maliciosos utilizam ataques para obter este tipo de informação devido ao seu valor. Ainda recentemente houve o ataque ao [Linkedin, e a publicação da informação de passwords dos utilizadores][1]. É portanto necessário tomar medidas, não só para preservar a segurança dos dados, mas garantir que o acesso a elas não crie novas vulnerabilidades.

<!--more-->

Em circunstância alguma devemos armazenar a palavra passe do utilizador na base de dados num formato legível! Se considerar-mos o funcionamento de websites e aplicações, as únicas situações que necessitaria-mos de ter a password original do utilizador numa aplicação seria para reutiliza-la na autenticação a um serviço externo, caso utilizássemos o serviço em representação do utilizador. Mas o risco associado não justifica a prática &#8211; a troca de outro tipo de chaves de autenticação é mais seguro para o utilizador. Um exemplo são as autorizações em aplicações de sites como o Facebook: a passe do utilizador não é transmitida, apenas uma chave complexa que autoriza o acesso pela aplicação.

### **Técnicas para ofuscar a password**

Mas e a autenticação no próprio site?! A autenticação no site é efectuado através da comparação da password do utilizador, inserido no formulário de autenticação, e a password armazenada. Recorrendo a um exemplo simples, para ver o funcionamento, imagina que o nosso user José Silva utiliza uma password simples e comum &#8220;qwerty&#8221;. No formulário ele introduz o username e password, e quando clica para fazer o login, procuramos o registo dele na base e comparamos as passes. Se coincidirem:

 `if(formData["password"] == dbData["password"]){ ...` 

o utilizador passa a ter acesso ao site/aplicação. No (semi-pseudo)código, o formData representa os dados vindos do formulário e o dbData os dados provenientes da base de dados.

O erro que alguns programadores inconscientes destes problemas têm é não compreenderem que esta comparação não tem de ser efectuada com as versões originais da passe. Na verdade, podemos e devemos usar a versão codificada da passe, conhecida por &#8220;**hash**&#8221; (ou escrutínio, mas a palavra em inglês é bem mais agradável). Ou seja, a partir do momento que o utilizador regista a sua conta, o que deve ser armazenado é uma versão codificada da passe, e utilizando o um algoritmo que permita apenas a codificação num sentido (impedido recuperar o original por processamento inverso). Se recorrermos a um algoritmo como o [MD5][2] ou o [SHA-1][3] a password fica armazenado num formato críptico e bem mais difícil de descodificar. (Nota: o SHA-1 e MD5 hoje em dia não são suficientemente seguros, já que com a capacidade de processamento disponível, a descodificação por ataques de dicionário ou força bruta são viáveis).

Vejam a mesma passe em ambos os formatos:

 `MD5: "qwerty" => d8578edf8458ce06fbc5bb76a58c5ca4<br />
SHA-1: "qwerty" => b1b3773a05c0ed0176787a4f1574ff0075f7521e<br />
` 

A comparação no método de autenticação passa então a ser entre versões codificadas da mesma palavra, em vez da versão original da palavra:

 `if(MD5.codificar(formData["password"]) == dbData["password"]){ ...` 

Apesar de ser muito melhor, e mais seguro que o armazenamento em texto livre da password, não é suficientemente seguro. Existem tabelas com as palavras codificadas pré-calculadas (lookup tables e rainbow tables) que permitem comparação para obter os valores originais, e mesmo a capacidade de processamento actual do hardware disponível permite efectuar o cálculo por força bruta (este a todas as combinações possíveis) em tempo útil.

### Um pouco de sal

Uma técnica importante para contrariar o uso de tabelas de lookup consiste em adicionar um pouco de texto adicional à palavra passe, conhecido por um &#8220;salt&#8221; (daí o trocadilho do sal insonso&#8230;enfim&#8230;). O facto de acrescentar mais caracteres no inicio e/ou no fim tornará a palavra a codificar mais complexa e comprida, tornando a sua descodificação também mais complexa.

Voltando ao exemplo, imagina que acrescentamos o texto &#8220;p@$$wordComSal&#8221; no início da password do utilizador, antes de a codificar. Teriamos então:

`<br />
MD5: "p@$$wordComSalqwerty" => cd17413e25e513690f7c53fbab394e25<br />
` 

O facto de acrescentarmos um termo incomum no inicio da password do utilizador faz com que o uso de dicionários fique inutilizado, uma vez que o dicionário (e as hashes associadas) não terão o salt em consideração. A base de dados terá armazenado a passe codificada com o salt, e a comparação a efectuar será do tipo:

 `if(MD5.codificar(salt + formData["password"]) == dbData["password"]){ ...` 

Ainda estou a considerar um salt fixo e geral para o sistema. E mais uma vez, apesar de ser mais seguro que as anteriores, ainda não é o suficiente. Se o salt é descoberto, ataques de dicionário e força bruta passam a ser novamente possíveis, necessitando apenas de recalcular as hashes com o salt.

### Aleatoriedade

A melhoria neste processo é possível garantindo que o salt para cada user seja distinto e, preferencialmente, aleatório. Se cada passe tem um salt diferente, a descoberta de um (por força bruta) impossibilita a criação de tabelas de lookup gerais &#8211; apenas serviriam para o registo em que foi descoberto. Além disso, as hashes devem ser geradas usando algoritmos mais seguros (SHA256, SHA512, RipeMD, WHIRLPOOL, SHA3) e a criação deverá ser lenta, recorrendo a estratégias de _key-streching_ ( PBKDF2, bcrypt, and scrypt).

A [crackstation][4] tem um excelente artigo sobre o tema.

Recomendo a escolha do bloco adequado de código e a integração em frameworks pessoais _core_ para que possa ser usado facilmente.A validação da passe continua com o mesmo processo (apenas escrito de forma adequada às classes fornecidas) :

 `if(PasswordHash.ValidatePassword(formData["password], dbData["password"])){ ...` 

O campo de password do utilizador na base de dados passa a ter uma concatenação do número de iterações PBKDF2, o salt, e a hash gerada, separados pelo caracter &#8220;:&#8221;. O método _validate_ separa as componentes armazenado, aplica a salt à passe submetida (o salt é especifico do utilizador) e compara a hash gerada com a armazenada. O cálculo é demorado (centenas de milissegundos) tornando ataques de força bruta pouco úteis.

É naturalmente importante não esquecer que esta não é a única técnica, e outros métodos devem ser empregues na construção do site / aplicação para anular todos os &#8220;buracos&#8221; existentes, reduzindo ao máximo e sempre que possível as falhas de segurança.

 [1]: http://press.linkedin.com/node/1212 "Linked in password atack"
 [2]: http://pt.wikipedia.org/wiki/MD5 "Algoritmo MD5"
 [3]: http://pt.wikipedia.org/wiki/SHA-1 "Algoritmo SHA-1"
 [4]: http://crackstation.net/hashing-security.htm