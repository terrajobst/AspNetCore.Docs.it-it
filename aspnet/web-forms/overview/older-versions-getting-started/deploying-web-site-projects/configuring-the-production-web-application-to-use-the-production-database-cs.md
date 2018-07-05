---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configurazione dell'applicazione Web di produzione per usare il Database di produzione (c#) | Microsoft Docs
author: rick-anderson
description: Come illustrato nelle esercitazioni precedenti, non è insolito per le informazioni di configurazione in modo diverso tra gli ambienti di sviluppo e produzione. Si tratta di es...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 559697a08200e43e955697a7ad8613f1a495c073
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803016"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configurazione dell'applicazione Web di produzione per usare il Database di produzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Come illustrato nelle esercitazioni precedenti, non è insolito per le informazioni di configurazione in modo diverso tra gli ambienti di sviluppo e produzione. Ciò vale soprattutto per le applicazioni web basate sui dati, come le stringhe di connessione del database differiscono tra ambienti di sviluppo e produzione. Questa esercitazione illustra alcuni modi per configurare l'ambiente di produzione in modo da includere la stringa di connessione appropriate in modo più dettagliato.


## <a name="introduction"></a>Introduzione

Le applicazioni web basate sui dati, in genere, utilizzare un database diverso quando in fase di sviluppo rispetto a quando nell'ambiente di produzione. Per le applicazioni ospitate da un provider di hosting web e sviluppato in locale, lo sviluppo in genere si trova il database nel computer s dello sviluppatore mentre il database di produzione è ospitato in un server di database alla struttura s società di hosting web. Distribuzione di un'applicazione web basata sui dati vengono copiati nel database di sviluppo al server di database di produzione. Nell'esercitazione precedente abbiamo esaminato modi per eseguire questo passaggio.

L'applicazione web Usa le informazioni contenute in un *stringa di connessione* per stabilire una connessione con il database. La stringa di connessione, che in genere viene archiviata in `Web.config`, specifica il nome del server di database, il nome del database, il contesto di sicurezza e altre informazioni. Poiché il database utilizzato dall'applicazione web dipende dal fatto che l'applicazione web è in esecuzione negli ambienti di sviluppo o produzione, le stringhe di connessione devono essere diversa tra i due ambienti.

Non è insolito per le informazioni di configurazione in modo diverso tra gli ambienti di sviluppo e produzione. Il *differenze di configurazione comuni tra sviluppo e produzione* esercitazione illustrato le tecniche per la gestione delle informazioni di configurazione separato tra questi due ambienti, nonché una breve discussione su stringhe di connessione di database. Questa esercitazione illustra alcuni modi per configurare l'ambiente di produzione in modo da includere la stringa di connessione appropriate in modo più dettagliato.

## <a name="examining-the-connection-string-information"></a>Esaminare le informazioni sulla stringa di connessione

La stringa di connessione utilizzata dall'applicazione web le recensioni dei libri è archiviata nel file di configurazione dell'applicazione s, `Web.config`. `Web.config` include una sezione speciale per l'archiviazione delle stringhe di connessione, il nome appropriato [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Il `Web.config` file per il sito Web le recensioni dei libri è una stringa di connessione definita in questa sezione denominata `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

La stringa di connessione - origine dati =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True; User Instance = True: è costituito da un numero di opzioni e i valori, con coppie di opzione/valore delimitate da punto e virgola e ogni opzione e il valore delimitate da un segno di uguale. Le quattro opzioni usate nella stringa di connessione sono:

- `Data Source` -Specifica il percorso del server di database e il nome dell'istanza server database (se presente). Il valore, `.\SQLEXPRESS`, è riportato un esempio in cui è presente un server di database e un nome di istanza. Il periodo di specifica che il server di database è nello stesso computer come applicazione. il nome dell'istanza è `SQLEXPRESS`.
- `AttachDbFilename` -Specifica il percorso del file di database. Il valore contiene il segnaposto `|DataDirectory|`, che viene risolto il percorso completo di s applicazione `App_Data` cartella in fase di esecuzione.
- `Integrated Security` -un valore booleano che indica se utilizzare un nome utente/password specificata durante la connessione al database (false) o di Windows corrente le credenziali dell'account (true).
- `User Instance` -un'opzione di configurazione specifica per edizioni di SQL Server Express che indica se consentire agli utenti senza privilegi di amministratore nel computer locale collegarsi e connettersi a un database di SQL Server Express Edition. Visualizzare [istanze di SQL Server Express utente](https://msdn.microsoft.com/library/ms254504.aspx) per altre informazioni su questa impostazione.
  

Le opzioni di stringa di connessione consentite variano a seconda del database a cui a che ci si connette e il provider di database ADO.NET in uso. Ad esempio, la stringa di connessione per la connessione a Microsoft SQL Server database è diverso da usare per connettersi a un database Oracle. Allo stesso modo, ci si connette a un database di Microsoft SQL Server tramite il provider SqlClient utilizza una stringa di connessione diversa rispetto all'uso di provider OLE DB.

È possibile compilare la stringa di connessione di database manualmente utilizzando un sito, ad esempio [ConnectionStrings.com](http://www.connectionstrings.com/) come una risorsa per i quali sono le opzioni sono disponibili. Tuttavia, un approccio più semplice consiste nell'aggiungere il database in Esplora Server in Visual Studio e quindi recuperare la stringa di connessione dalla finestra Proprietà. Consentire s usare questa tecnica quest'ultima per costruire la stringa di connessione al server di database di produzione.

Aprire Visual Studio e quindi passare alla finestra di Esplora Server (in Visual Web Developer, questa finestra è denominata Database Explorer). Fare doppio clic sull'opzione connessioni dati e scegliere l'opzione Aggiungi connessione dal menu di scelta rapida. Verrà visualizzata la procedura guidata illustrata nella figura 1. Scegliere l'origine dati appropriata e fare clic su Continua.


[![Scegliere di aggiungere un nuovo Database in Esplora Server](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figura 1**: scegliere di aggiungere un nuovo Database in Esplora Server ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Successivamente, specificare i vari database le informazioni di connessione (vedere la figura 2). Quando si è effettuata l'iscrizione con la società di hosting web si devono fornite informazioni su come connettersi al database - il nome del server di database, il nome del database, il nome utente e password da usare per connettersi al database e così via. Dopo aver immesso queste informazioni fare clic su OK per completare questa procedura guidata e aggiungere il database in Esplora Server.


[![Specificare le informazioni di connessione di Database](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figura 2**: specificare le informazioni di connessione di Database ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Il database ambiente di produzione dovrebbe ora essere elencato in Esplora Server. Selezionare il database da Esplora Server e passare alla finestra Proprietà. Qui troverai una proprietà denominata stringa di connessione con la stringa di connessione database s. Supponendo che si usa un database Microsoft SQL Server in ambiente di produzione e il provider SqlClient la stringa di connessione dovrebbe essere simile al seguente:

<strong>Origine dati =<em>serverName</em>; Catalogo iniziale =<em>databaseName</em>; Persist Security Info = True; ID utente =<em>username</em>; Password =*password</strong>*

In cui *serverName*, *databaseName*, *username*, e *password* sono con i valori per il nome del server di database, il database nome e il nome utente e la password è fornito dall'azienda host web.

## <a name="deploying-the-book-reviews-web-application"></a>Distribuzione dell'applicazione Web di revisioni Rubrica

L'esercitazione precedente si sono esaminati copia del database di sviluppo all'ambiente di produzione, ma non è stato esplorare la distribuzione dell'applicazione basate sui dati. A questo punto l'ambiente di produzione contiene il database, ma usa la versione dell'applicazione libro esamina con le verifiche statiche. È necessario distribuire l'applicazione di nuovo, guidate dai dati al server di produzione insieme alle informazioni di configurazione aggiornate.

Si consiglia di distribuire l'applicazione basate sui dati dall'ambiente di sviluppo nell'ambiente di produzione. Questo processo è stato analizzato in dettaglio nelle esercitazioni precedenti. Se è necessario un aggiornamento, vedere la *la distribuzione del sito Web usando un FTP Client* o nella *distribuzione del sito Web con Visual Studio* esercitazioni. È necessario assicurarsi che la stringa di connessione di database di produzione è quello utilizzato nell'ambiente di produzione, il che significa che un'alternativa `Web.config` file deve essere distribuito. In particolare, questa impostazione modificata `Web.config` file s `<connectionStrings>` elemento deve contenere la stringa di connessione di database di produzione e dovrebbe essere simile al seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Si noti che la stringa di connessione nel `<connectionStrings>` viene assegnato lo stesso elemento (`ReviewsConnectionString`), ma che adesso contiene la stringa di connessione del database di produzione anziché la stringa di connessione di database di sviluppo.

A meno che non si dispone di un flusso di lavoro di distribuzione più formale, modificare manualmente il `Web.config` file da usare la stringa di connessione di database di produzione prima della distribuzione (ricordando di ripristinarla tornare a usare la stringa di connessione di database di sviluppo in un secondo momento) o mantenere separato `Web.config` file con le informazioni di configurazione Ambiente di produzione che viene caricate nell'ambiente di produzione come parte del processo di distribuzione.

> [!NOTE]
> Se si distribuisce accidentalmente un `Web.config` file che contiene la stringa di connessione di database di sviluppo, vi sarà un errore quando l'applicazione in produzione prova a connettersi al database. Questo errore manifesta come un `SqlException` con un messaggio di creazione di report che il server non è stato trovato o non è accessibile.


Dopo aver distribuito il sito alla produzione, visitare il sito di produzione tramite il browser. È consigliabile vedere e sfruttare la stessa esperienza utente durante l'esecuzione locale dell'applicazione basate sui dati. Naturalmente quando si visita il sito Web in produzione al sito si basa su server di database di produzione, mentre visitando il sito Web nell'ambiente di sviluppo viene utilizzato il database in fase di sviluppo. Figura 3 mostra le *insegnare manualmente ASP.NET 3.5 in 24 ore* esaminare pagina dal sito Web nell'ambiente di produzione (si noti l'URL nella barra degli indirizzi del browser s).


[![Le applicazioni guidate dai dati è ora disponibile in produzione.](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figura 3**: The Data-Driven applicazione è ora disponibile in produzione. ([Fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>L'archiviazione delle stringhe di connessione in un File di configurazione separato

Una tecnica comune per la gestione delle informazioni di configurazione separato per gli ambienti di sviluppo e di produzione è avere due versioni del `Web.config`: uno per l'ambiente di sviluppo e uno per la produzione. In fase di distribuzione appropriato `Web.config` versione può essere copiata nell'ambiente di produzione. In teoria, questo processo potrebbe essere automatizzato nell'ambito del flusso di lavoro di distribuzione.

Anziché mantenere due `Web.config` file, facoltativamente, è possibile forniscono le differenze più granulare. Gli elementi che costituiscono il `Web.config` file può essere definite in un file di configurazione esterni che vengono usati nel `Web.config` file. In pratica è possibile avere un `Web.config` file per entrambi gli ambienti che fa riferimento a un file databaseConnectionStrings.config, che contiene le stringhe di connessione utilizzata dall'applicazione e sono univoci per ogni ambiente. È possibile individuare che fornisce una più ordinata che separa le informazioni di configurazione diverse in file separati e più semplice `Web.config` file e altro ancora in modo chiaro vengono descritte le differenze di configurazione tra gli ambienti di sviluppo e produzione.

Per usare questa tecnica, iniziare creando una nuova cartella nell'applicazione web denominata `ConfigSections`. Successivamente, aggiungere due file a questa nuova cartella denominata databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config. Successivamente, copiare il `<connectionStrings>` elemento da `Web.config` nei file databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config e quindi modificare la stringa di connessione nel databaseConnectionStrings.production.config del file in modo che specifichi la stringa di connessione di database di produzione. Ad esempio, il file di databaseConnectionStrings.dev.config deve contenere solo il `<connectionStrings>` elemento con una stringa di connessione che fa riferimento al database di sviluppo:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Analogamente, il file databaseConnectionStrings.production.config deve contenere solo un `<connectionStrings>` elemento, ma quello contenente la stringa di connessione di database di produzione.

Creare una copia del file databaseConnectionStrings.dev.config e denominarlo databaseConnectionStrings.config.

> [!NOTE]
> È possibile assegnare un nome file di configurazione diverso databaseConnectionStrings.config, se d desiderato, ad esempio `connectionStrings.config` o `dbInfo.config`. Tuttavia, assicurarsi di assegnare un nome del file con un `.config` estensione come `.config` file, per impostazione predefinita, non vengono serviti dal motore di ASP.NET. Se si assegnano nomi file di un altro elemento, ad esempio `connectionStrings.txt`, un utente può puntare il browser al [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e visualizzare il contenuto del file.


A questo punto il `ConfigSections` cartella deve contenere tre file (vedere la figura 4). I file databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config contengono le stringhe di connessione per gli ambienti di sviluppo e produzione, rispettivamente. Il file databaseConnectionStrings.config contiene le informazioni sulla stringa di connessione che verrà usati dall'applicazione web in fase di esecuzione. Di conseguenza, il file databaseConnectionStrings.config deve essere identico al file databaseConnectionStrings.dev.config nell'ambiente di sviluppo, mentre in produzione, il file databaseConnectionStrings.config deve essere identico a databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figura 4**: ConfigSections ([fare clic per visualizzare l'immagine con dimensioni normali](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


È ora necessario indicare a `Web.config` per utilizzare il file databaseConnectionStrings.config per il proprio archivio di stringhe di connessione. Aprire `Web.config` e sostituire l'elemento `<connectionStrings>` esistente con il codice seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

Il `configSource` attributo specifica un percorso fisico relativo al `Web.config` file. Se esterno `.config` file si trova nella stessa directory `Web.config` quindi impostare questo attributo al nome del file del `.config` file. Se è in una sottodirectory, come avviene con databaseConnectionStrings.config, specificano la sottocartella con una barra rovesciata per delimitare i nomi di file e cartelle, ad esempio ConfigSections\databaseConnectionStrings.config.

Grazie a questa modifica, gli ambienti di sviluppo e produzione contengono gli stessi `Web.config` file. A questo punto l'unica differenza è il file databaseConnectionStrings.config. Copiare il file databaseConnectionStrings.production.config nell'ambiente di produzione e rinominarlo in databaseConnectionStrings.config. Se, in futuro, sono state apportate modifiche alla stringa di connessione di database di produzione è necessario renderli al file databaseConnectionStrings.production.config e quindi caricare il file nell'ambiente di produzione, rinominarlo databaseConnectionStrings.config.

> [!NOTE]
> È possibile specificare le informazioni di presenza `Web.config` elemento in un file separato e usare il `configSource` attributo fare riferimento a tale file dall'interno `Web.config`.


## <a name="summary"></a>Riepilogo

Le applicazioni basate sui dati in genere usano database diversi in ambienti di sviluppo e produzione. Di conseguenza, le stringhe di connessione di database archiviate nella configurazione di s dell'applicazione web devono essere univoche per ogni ambiente. In questa esercitazione è stato illustrato come determinare la stringa di connessione di database di produzione e i modi per mantenere le informazioni sulla stringa di connessione univoco nei due ambienti.

Buona programmazione!

#### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Stringhe di connessione e file di configurazione](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informazioni @ ConnectionStrings.com le stringhe di configurazione del database](http://www.connectionstrings.com/)
- [Spostare le impostazioni dal File Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentazione tecnica per il &lt;connectionStrings&gt; elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-a-database-cs.md)
> [Successivo](configuring-a-website-that-uses-application-services-cs.md)
