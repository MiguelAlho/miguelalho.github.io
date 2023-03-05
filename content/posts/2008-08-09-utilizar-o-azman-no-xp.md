---
title: Utilizar o AzMan no XP
author: Miguel Alho
type: post
date: 2008-08-09T18:45:02+00:00
url: /utilizar-o-azman-no-xp/
categories:
  - 'Code &amp; IT'
tags:
  - ASP.NET
  - Autenticação
  - AzMan
  - Windows
  - XP

---
O Authorization Manager (ou AzMan) é uma implementação de uma architectura baseado em perfis (RBAC &#8211; Role Based Access Control). Com o Azman, podemos definir um conjunto de perfis para uma aplicação, definir os utilizadores que herdam cada perfil, e depois na aplicação gerir os acessos a operações (e não apenas à aplicação em si) pelos perfis.

Os utilizadores que associamos pode vir de variadíssimas fontes como um ficheiro XML, uma AD ou ainda num ADAM. O caso da AD é óptimo para implementar métodos de _single sign-on,_ especialmente em aplicações que correm numa intranet, permitindo conceder autorização a operações em aplicações por utilizadores já autenticados no domínio. E integra-se com relativa facilidade no DotNet, utilizando as classes de Membership e Roles existentes.

O AzMan é parte integral do Windows 2003, mas é possível utiliza-lo no 2000 ou XP. Aliás surgiu uma aplicação em que tive de o implementar numa web app a correr num &#8220;servidor&#8221; baseado numa máquina com o XP. Infelizmente a instalação no XP não é imediato. É necessário introduzir dois elementos, principalmente &#8211; o Snap-in de administração (para ter um UI de administração dos acessos e criar o armazem de associações) e as classes próprias para o .NET.

O processo:

  1. Instale o <a target="_blank" href="http://www.microsoft.com/downloads/details.aspx?FamilyID=e487f885-f0c7-436a-a392-25793a25bad7&DisplayLang=en">Windows Server 2003 Administration Tools Pack</a>. Este passo instala uma serie de ferramnetas e snap-ins típicos do Server 2003, incluindo o AzMan que nos interessa. 
  2. A instalação inclui o snap-in (azman.msc) mas não inclui a dll (primary interop assembly). Para isso deve instalar o <a target="_blank" href="http://www.microsoft.com/downloads/details.aspx?FamilyID=7edde11f-bcea-4773-a292-84525f23baf7&DisplayLang=en">Windows 2000 Authorization Manager Runtime</a>. Depois de extrair, é necessário registar o Microsoft.Interop.Security.AzRoles.dll no GAC do .NET. Abre a ferramenta de configuração do .NET (_Painel de controlo -> Ferramentas administrativas -> Microsoft .Net 2.0 Configuration Tool_; Caso não esteja disponivel pode ser necessário <a target="_blank" href="http://www.microsoft.com/downloads/details.aspx?FamilyID=fe6f2099-b7b4-4f47-a244-c96d69c35dec&DisplayLang=en">instalar o SDK</a>). Na ferramenta de configuração, segue para o _Assemblies Cache_ e adiciona a dll presente em _Windows(R) 2000 Authorization Manager Runtime\PIA\1.2_. 

Deste modo, o servidor da aplicação, em XP, fica preparado para utilizar o AzMan (Felizmente no server 2003 é bem mais facil&#8230;). Resta agora criar as políticas e configurar a aplicação. Para configurar as políticas:

  1. Excute o azman.msc, para abrir o snap-in e criar as políticas e crie uma nova _authorization store_. Eu, por exemplo, e para o que necessitava, armazenei as poíticas num ficheiro XML, numa pasta não acessivel por web. A criação das politicas deve ser efectuado em modo developer (_Action -> Options -> Developer Mode_). 
  2. Crie uma nova aplicação. 
  3. Em _Definitions_ -> _Role Definitions_ adiciona os nomes dos perfis que necessitas. No meu caso, só queria incluir uma forma simplificada de autenticação e portanto apenas criei um grupo chamado _AppUsers_. O AzMan permite incluir também operações se necessitares. 
  4. Em _Role Assignments_ adiciona o(s) perfil(s) adicionado(s) e a cada perfil adiciona os utilizadores (da máquina e/ou AD) ao perfil. O Windows, durante o processo de autenticação ira atribuir o perfil ao utilizador.
  5. Atribui permissões ao utilizador ASPNET (ou equivalente) para aceder ao ficheiro XML criado, no caso de estar a usar um ficheiro XML. 

Para configurar a aplicação para utilizar o AzMan, 

  1. Adiciona a connection string, no caso de usar o ficheiro XML, à lista de connection strings: <pre lang="xml">&lt;configuration> 
  &lt;connectionstrings> 
    &lt;add name="LocalPolicyStore">
         connectionString="msxml://c:/Aplicação/azman.xml" />;
  &lt;/add> 
&lt;/connectionstrings>

&lt;/configuration></pre>

  2. Define no <system.web> o RoleProvider: <pre lang="xml">&lt;system.web>
    &lt;roleManager 
        enabled="true" 
        cacheRolesInCookie="true" 
        defaultProvider="RoleManagerAzManProvider"
        cookieName=".ASPXROLES" 
        cookiePath="/" 
        cookieTimeout="30" 
        cookieRequireSSL="true" 
        cookieSlidingExpiration="true"
        createPersistentCookie="false" 
        cookieProtection="All">
        &lt;providers>
           &lt;add name="RoleManagerAzManProvider"
                type="System.Web.Security.AuthorizationStoreRoleProvider, System.Web, Version=2.0.0.0, 
                    Culture=neutral, publicKeyToken=b03f5f7f11d50a3a"
                connectionStringName="LocalPolicyStore" 
                applicationName="Nome da Aplicação"/>
        &lt;/providers>
    &lt;/roleManager>
&lt;/system.web>

</pre>
    
    onde _applicationName_ é o nome da aplicação atribuída no AzMan </li> 
    
      * A partir daí posso defenir no web.config as regras de autoriação para uma pasta, com base no perfil:
        
        <pre lang="xml">&lt;authorization>
   &lt;allow roles="AppUsers" />
   &lt;deny users="*" />
&lt;/authorization>

</pre>
        
        ou utilizar a API do Azman para definir e testar autorizações (até ao nível da operação). </li> </ol> 
        
        Mais informação em <a target="_blank" href="http://msdn.microsoft.com/en-us/library/ms998336.aspx">http://msdn.microsoft.com/en-us/library/ms998336.aspx</a>