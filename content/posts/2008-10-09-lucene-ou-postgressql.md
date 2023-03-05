---
title: Lucene ou PostgresSQL
author: Miguel Alho
type: post
date: 2008-10-09T11:08:06+00:00
url: /lucene-ou-postgressql/
categories:
  - 'Code &amp; IT'
tags:
  - 'C#'
  - comparação
  - IR
  - Lucene
  - PostresSQL
  - testes

---
Estou num certo impasse num projecto. Necessito de indexar um repositório de documentos, como também armazenar dados sobre esses documentos, numa aplicação. A aplicação terá um conjunto extra de funcionalidades, para além da gestão documental, e que utilizarei uma base de dados como suporte. mas para os documentos, dada a modelariedade do sistema, quer um indexador com base em sistema de ficheiros, quer com base em pesquisa de texto livre numa base de dados, são opções válidas. Simplesmente a manipulação dos dados (organização) e o cruzamento de dados teria que ser planeada consoante a forma de indexação.

Percori uma série de artigos e experiências na tentativa de chegar a uma conclusão. Uma hipótese era o Index Server da Microsoft que tinha sido usado num projecto universitário sobre o qual laborei. Pedi opinião a um colega que mantém o projecto e recomendou evitar&#8230; Sobrou o Lucene, que armazena os indices no sistema de ficheiros <a href="http://incubator.apache.org/projects/lucene.net.html" target="_blank">e tem uma versão .NET</a> e o <a href="http://www.postgresql.org/about/" target="_blank">PostgresSQL</a>, que sendo aberto e funcionando sobre o Windows, pareceu-me uma óptima escolha parta o suporte de dados. Ambos indexam o texto do documento, se bem que esse texto deve ser extraído, por exemplo, no caso dos PDFs. Decidi testar ambos para de alguma forma comparar e verificar se um é claramente melhor que outro.

Primeiro passo, criar um _dataset_. Usei cerca de 10000 mensagens de email, da qual tive que extrair os documentos em _attachment_ (que é efectivamente o conjunto de dados que vou indexar na aplicação). Sobre esse processo falerei noutro post, que por si só é bastante interessante. Dos emails tenho cerca de 10000 emails, 2500 dos quais PDF.

Próximo passo, indexar ambos e comparar pesquisas. Para o caso do Lucene, utilizei um exemplo do <a href="http://www.keylimetie.com/ShowItem10.aspx" target="_blank">KeyLimeTie</a>. O exemplo inclui a biblioteca do Lucene.Net e um projecto de interface de utilizador, para indexar e pesquisar. Foi necessário efectuar uma pequena alteração para incluir a indexação de PDFs (os .doc funcionou nativamente no exemplo). Para o efeito, criei um novo tipo de documento PDFDocument, similar ao Document que já estava definido, e alterei apenas o pedaço de código que define o campo de texto a armazenar no Lucene:

<pre lang="csharp">FileInputStream fi = new FileInputStream(new java.io.File(f.FullName));

PDFParser parser = new PDFParser(fi);
parser.parse();
COSDocument cd = parser.getDocument();
PDFTextStripper stripper = new PDFTextStripper();
String text = stripper.getText(new PDDocument(cd));
cd.close();
            
doc.Add(Field.Text("contents", text));
</pre>

Este processo utiliza a biblioteca do <a href="http://sourceforge.net/projects/pdfbox/" target="_blank">PDFBox</a>, que apesar de ser Java, é utilizável através de outra referência, o <a href="http://www.ikvm.net/" target="_blank">IKVM </a>.

para proceder á indexação, separei o formato PDF dos restantes num switch/case e adicionei ao IndexWriter um PDFDocument em vez de um Document

<pre lang="csharp">case "pdf":
     objIndexWriter.AddDocument(FileDocument.PDFDocument(objFile as FileInfo));
     mlngNumDocsIndexed += 1;
     break;
</pre>

Indexar os documentos todos demorou aproximadamente 65 minutos (9830 documentos, 580 não indexados, 181 com erro). O índice (a pasta) ocupava cerca de 360MB). Posteriormente re-indexei apenas os PDF com o resultados de 23minutos para 2551 documentos, 580 não indexados, 181 com erro).

Para o Postgres, criei uma base através do pgAdmin III, com uma tabela: 

<pre lang="sql">CREATE TABLE "DocTest"
(
  "docID" integer NOT NULL DEFAULT 0,
  docfile character varying(300),
  "docText" text,
  "docVector" tsvector,
  id serial NOT NULL,
  CONSTRAINT "DocTest_pkey" PRIMARY KEY (id)
)
</pre>

e com um trigger para criar o tsvector (vector com os termos e localização do módulo tsearch2).

<pre lang="sql">CREATE TRIGGER tsvectorupdate2
  BEFORE INSERT OR UPDATE
  ON "DocTest"
  FOR EACH ROW
  EXECUTE PROCEDURE tsvector_update_trigger('docVector', 'pg_catalog.english', 'docText');
</pre>

também importante foi a criação do indice GIN sobre o vector:

<pre lang="sql">CREATE INDEX textsearch_idx
  ON "DocTest"
  USING gin
  ("docVector");
</pre>

Em termos de aplicação, utilizei a base do exemplo do Lucene, mas modifiqeui o processo de indexação, claro. Extraí o texto e inseri os dados na BD, o método de extração dos dados do PDF é identico:

<pre lang="csharp">public class DocumentoIndexavel
{
    public int DocID { get; set; }
    public string DocFile { get; set; }
    public string DocText { get; set; }
}

public class DocDAL
{
    public static int Save(DocumentoIndexavel doc)
    {
        NpgsqlConnection conn = new NpgsqlConnection("Server=127.0.0.1;Port=5432;User Id=postgres;Password=postgresDB;Database=IndexerTest;");
        conn.Open();
        
        NpgsqlCommand command = new NpgsqlCommand("insert into \"DocTest\" (\"docID\", \"docfile\", \"docText\") values(" + doc.DocID +",'"+doc.DocFile+"','"+doc.DocText.Replace("'","")+"')", conn);
        Int32 rowsaffected;

        try
        {
            return command.ExecuteNonQuery();
        }
        catch (Exception e)
        {
            throw e;
        }
        finally
        {
          conn.Close();
        }
    }

}
</pre>

Consegui em termos de tempos de indexação, este foi feito em dois tempos (devido a um bug q tinah no meu código) mas a app identificou 4093 ficheiros, indexando 3549 e os restantes com erros em cerca de 37 minutos. 

Em ambos os casos houve um largo conjunto de campos não indexados, devido, especialmente, a problemas de abertura de ficheiros ou de codificação do mesmo. Mas para o efeito do teste essa situação não era grave.

À partida sabia que o caso da base de dados seria naturalmente mais lenta. Entre conectar à BD, e processar o insert, certamente que o tempo de execução seria mais longa que o da escrita para ficheiro no Lucene. De qualquer forma, a diferença encontrada não foi tão elevada quanto a que estava à espera.

Em temros de pesquisa, há algumas diferenças quer no conjunto de resultados, quer nos tempos:

<table>
  <tr>
    <td>
    </td>
    
    <td>
      Lucene
    </td>
    
    <td>
      Postgres
    </td>
  </tr>
  
  <tr>
    <td colspan="3">
      Indexação
    </td>
  </tr>
  
  <tr>
    <td>
      ficheiros indexados
    </td>
    
    <td>
      2551
    </td>
    
    <td>
      3549
    </td>
  </tr>
  
  <tr>
    <td>
      tempo indexação
    </td>
    
    <td>
      23min.
    </td>
    
    <td>
      37minutos
    </td>
  </tr>
  
  <tr>
    <td colspan="3">
      Pesquisa
    </td>
  </tr>
  
  <tr>
    <td>
      &#8220;europass&#8221;
    </td>
    
    <td>
      465 hits / 184ms
    </td>
    
    <td>
      718 hits / 287ms
    </td>
  </tr>
  
  <tr>
    <td>
      &#8220;sql&#8221;
    </td>
    
    <td>
      10 hits / 60ms
    </td>
    
    <td>
      132 hits / 17ms
    </td>
  </tr>
  
  <tr>
    <td>
      &#8220;postgres&#8221;
    </td>
    
    <td>
      6 hits / 80ms
    </td>
    
    <td>
      9 hits / 20ms
    </td>
  </tr>
  
  <tr>
    <td>
      &#8220;psicologia&#8221;
    </td>
    
    <td>
      208 hits / 80ms
    </td>
    
    <td>
      0 hits / 200ms*
    </td>
  </tr>
  
  <tr>
    <td>
      &#8220;psicologia | stress&#8221;
    </td>
    
    <td>
      244 hits / 72ms
    </td>
    
    <td>
      120 hits / 20ms
    </td>
  </tr>
</table>

Há resultados aceitaveis e outros muito estranhos. Por exemplo &#8220;psicologia&#8221; retornou resultados correctamente no Lucene e não no Postgres, apesar de &#8220;psicolog&#8221; ser um &#8220;stem&#8221; presente na coluna de vector. A generalidade das pesquisas parecem ser suficientemente rápidas, quer num sistema, quer noutro. Ainda vou ter de investigar melhor o porquê da falha na palavra &#8220;psicologia&#8221; (se alguem souber, agradeço comentário).

Apesar de o Postgres ser mais lento na indexação (em massa) penso que o vou manter. Tornará a gestão da aplicação mais simples (menos um repositório) e permitirá efectuar cruzamento de dados directamente na base, através das relações entre tabelas.