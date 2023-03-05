---
title: 'Backup de PostgresSQL em C#'
author: Miguel Alho
type: post
date: 2009-03-25T23:00:05+00:00
url: /backup-de-postgressql-em-c/
categories:
  - 'Code &amp; IT'
tags:
  - Backup
  - base de dados
  - 'C#'
  - PostgreSQL

---
Para uma aplicação que estou a desenvolver com base numa BD Postgre, precisei de criar um script de backup da base de dados. A ideia é clicar num botão da interface web, e fazer o dump da BD, de forma simples, e permitir que o utilizador (que neste caso não tem nada a haver com IT) possa descarregar e archivar a base facilmente.

O método:

<pre lang="csharp">/// &lt;summary&gt;
        /// Backup Database to file (dump)
        /// &lt;/summary&gt;
        /// &lt;param name="server"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupserver"]
        /// &lt;param name="port"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupport"]
        /// &lt;param name="user"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupuser"]
        /// &lt;param name="password"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbpbackuppassword"]
        /// &lt;param name="dbname"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupdbname"]
        /// &lt;param name="backupdir"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupdir"]
        /// &lt;param name="backupFileName"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupfilename"]
        /// &lt;param name="backupCommandDir"&gt; &lt;/param&gt;db -&gt; ConfigKey ["dbbackupworkdir"]
        /// &lt;returns&gt;The file name with the db dump&lt;/returns&gt;
        public string BackupDatabase(
            string server, 
            string port, 
            string user, 
            string password,
            string dbname, 
            string backupdir, 
            string backupFileName,
            string backupCommandDir)
        {
            //string password = ConfigurationManager.AppSettings["dbpbackuppassword"];
            //string server = ConfigurationManager.AppSettings["dbbackupserver"];
            //string port = ConfigurationManager.AppSettings["dbbackupport"];
            //string user = ConfigurationManager.AppSettings["dbbackupuser"];
            //string dbname = ConfigurationManager.AppSettings["dbbackupdbname"];
            //string backupdir = ConfigurationManager.AppSettings["dbbackupdir"];
            //string backupFileName = ConfigurationManager.AppSettings["dbbackupfilename"];
            //string backupCommandDir = ConfigurationManager.AppSettings["dbbackupworkdir"];
            
            try
            {

                Environment.SetEnvironmentVariable("PGPASSWORD", password);

                string backupFile = backupdir + backupFileName +
                    DateTime.Now.ToString("yyyyMMdd_HHmmss") + ".backup";
                string BackupString = "-ibv -Z3 -f \"" + backupFile + "\" " +
                    "-Fc -h " + server + " -U " + user + " -p " + port + " " + dbname;

                Process proc = new System.Diagnostics.Process();
                proc.StartInfo.FileName = backupCommandDir + "\\pg_dump.exe";
                proc.StartInfo.Arguments = BackupString;
                                
                proc.Start();

                proc.WaitForExit();
                proc.Close();

                return backupFile; 

            }
            catch (Exception ex)
            {
                throw new Exception("An unknown error occured while trying to perform the database backup/restore operation.\n\nException: " + ex.Message);
            }
        }
</pre>

Basicamente, o método inicia o processo / commando da shell &#8220;pgdump&#8221; que vem com a instalação do PostgreSQL e faz um dump das tabelas e dados. Eu passo os parâmetros de nome de ficheiro e BD e directório de armazenamento etc, como parâmetro do método, porque tenho integrado numa framework e deverá servir para outras aplicações, mas podia perfeitamente referenciar as chaves da configuração directamente ou mesmo escrever o código do commando (hardcoded). No fim, ele devolve o caminho do ficheiro para enviar para a interface.

<div class="zemanta-pixie">
  <img class="zemanta-pixie-img" src="http://img.zemanta.com/pixy.gif?x-id=e4b0f8fc-087a-45e7-8673-06c543526722" />
</div>