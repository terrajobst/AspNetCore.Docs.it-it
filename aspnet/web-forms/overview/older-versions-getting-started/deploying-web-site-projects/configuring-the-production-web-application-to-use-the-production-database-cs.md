---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configurazione dell'applicazione Web di produzione per usare il Database di produzione (c#) | Documenti Microsoft
author: rick-anderson
description: Come illustrato nelle esercitazioni precedenti, non è insolito che le informazioni di configurazione in modo diverso tra gli ambienti di sviluppo e produzione. Si tratta es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fef7ecab0e51790ff7737b16500f6c2bb5eecdf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887694"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configurazione dell'applicazione Web di produzione per usare il Database di produzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Come illustrato nelle esercitazioni precedenti, non è insolito che le informazioni di configurazione in modo diverso tra gli ambienti di sviluppo e produzione. Ciò vale soprattutto per le applicazioni web basate sui dati, come le stringhe di connessione del database differiscono tra gli ambienti di sviluppo e produzione. In questa esercitazione verranno illustrati i modi per configurare l'ambiente di produzione per includere la stringa di connessione appropriate in modo più dettagliato.


## <a name="introduction"></a>Introduzione

Applicazioni web basate sui dati, in genere, utilizzare un database diverso quando è in fase di sviluppo rispetto a quando nell'ambiente di produzione. Per le applicazioni ospitate da un provider di hosting web e sviluppata in locale, il database di sviluppo in genere si trova nel computer s dello sviluppatore mentre il database di produzione è ospitato in un server di database in una struttura s società di hosting web. Distribuzione di un'applicazione web basate sui dati comporta la copia del database di sviluppo al server di database di produzione. Nell'esercitazione precedente abbiamo esaminato modi per eseguire questo passaggio.

L'applicazione web utilizza le informazioni contenute in un *stringa di connessione* per stabilire una connessione con il database. La stringa di connessione, viene in genere archiviata in `Web.config`, specifica il nome del server di database, il nome del database, il contesto di sicurezza e altre informazioni. Poiché il database utilizzato dall'applicazione web varia a seconda che l'applicazione web sia in esecuzione in ambienti di sviluppo o di produzione, le stringhe di connessione devono presentare differenze tra i due ambienti.

Non è insolito per informazioni sulla configurazione di differenze tra gli ambienti di sviluppo e produzione. Il *comuni configurazione differenze tra lo sviluppo e produzione* esercitazione illustrate le tecniche per mantenere le informazioni di configurazione separati tra questi due ambienti, nonché una breve descrizione in stringhe di connessione di database. In questa esercitazione verranno illustrati i modi per configurare l'ambiente di produzione per includere la stringa di connessione appropriate in modo più dettagliato.

## <a name="examining-the-connection-string-information"></a>Esaminare le informazioni sulla stringa di connessione

La stringa di connessione utilizzata dall'applicazione web recensioni viene archiviata nel file di configurazione dell'applicazione s, `Web.config`. `Web.config` include una sezione speciale per l'archiviazione delle stringhe di connessione, il nome appropriato [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Il `Web.config` file per il sito Web recensioni dispone di una stringa di connessione definita in questa sezione denominata `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

La stringa di connessione - origine dati =. \SQLEXPRESS; AttachDbFilename = | Sicurezza DataDirectory|\Reviews.mdf;Integrated = True; User Instance = True - è costituito da un numero di opzioni e i valori, con coppie opzione/valore delimitate da un punto e virgola e ogni opzione e il valore delimitate da un segno di uguale. Le quattro opzioni utilizzate nella stringa di connessione sono:

- `Data Source` -Specifica il percorso del server di database e il nome istanza server di database (se presente). Il valore, `.\SQLEXPRESS`, è riportato un esempio in cui è presente un server di database e un nome di istanza. Il periodo di specifica che il server di database è nello stesso computer come applicazione. il nome dell'istanza è `SQLEXPRESS`.
- `AttachDbFilename` -Specifica il percorso del file di database. Il valore contiene il segnaposto `|DataDirectory|`, che viene risolto il percorso completo di s applicazione `App_Data` cartella in fase di esecuzione.
- `Integrated Security` -un valore booleano che indica se utilizzare un nome utente/password specificata durante la connessione al database (false) o di Windows corrente, le credenziali dell'account (true).
- `User Instance` -un'opzione di configurazione specifica per SQL Server Express Edition che indica se consentire agli utenti senza privilegi di amministratore nel computer locale, collegarsi e connettono a un database di SQL Server Express Edition. Vedere [le istanze di SQL Server Express utente](https://msdn.microsoft.com/library/ms254504.aspx) per ulteriori informazioni su questa impostazione.
  

Le opzioni di stringa di connessione consentite dipendono dal database a cui a che ci si connette e il provider di database ADO.NET in uso. Ad esempio, la stringa di connessione per la connessione a Microsoft SQL Server database è diverso da usare per connettersi a un database Oracle. Analogamente, la connessione a un database di Microsoft SQL Server tramite il provider SqlClient utilizza una stringa di connessione diversa rispetto all'utilizzo del provider OLE DB.

È possibile compilare la stringa di connessione del database manualmente utilizzando un sito come [ConnectionStrings.com](http://www.connectionstrings.com/) come risorsa per quali opzioni sono disponibili. Tuttavia, è di un approccio più semplice per aggiungere il database in Esplora Server in Visual Studio e quindi di ottenere la stringa di connessione dalla finestra delle proprietà. Consente di utilizzare questa tecnica di quest'ultima per la costruzione di stringa di connessione al server di database di produzione s.

Aprire Visual Studio e quindi passare alla finestra di Esplora Server (in Visual Web Developer, questa finestra è detta Esplora Database). Fare doppio clic sull'opzione di connessioni dati e scegliere l'opzione Aggiungi connessione dal menu di scelta rapida. Verrà visualizzata la procedura guidata illustrata nella figura 1. Scegliere l'origine dati appropriata e fare clic su Continua.


[![Scegliere di aggiungere un nuovo Database in Esplora Server](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figura 1**: scegliere di aggiungere un nuovo Database in Esplora Server ([fare clic per visualizzare l'immagine ingrandita](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Successivamente, specificare le varie informazioni di connessione del database (vedere la figura 2). Al momento dell'iscrizione con la società di hosting web sono deve fornite informazioni su come connettersi al database - il nome del server di database, il nome del database, il nome utente e password da utilizzare per connettersi al database e così via. Dopo avere immesso queste informazioni fare clic su OK per completare questa procedura guidata e aggiungere il database in Esplora Server.


[![Specificare le informazioni di connessione di Database](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figura 2**: specificare le informazioni di connessione di Database ([fare clic per visualizzare l'immagine ingrandita](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Il database di ambiente di produzione dovrebbe ora essere elencato in Esplora Server. Selezionare il database da Esplora Server e passare alla finestra Proprietà. Non esiste, si noterà una proprietà denominata stringa di connessione con la stringa di connessione di database s. Presupponendo che si utilizza un database di Microsoft SQL Server di produzione e il provider SqlClient la stringa di connessione dovrebbe essere simile al seguente:

<strong>Origine dati =<em>serverName</em>; Catalogo iniziale =<em>databaseName</em>; Persist Security Info = True; ID utente =<em>username</em>; Password =*password</strong>*

Dove *serverName*, *databaseName*, *username*, e *password* sono con i valori per il nome del server di database, il database nome e il nome utente e la password è fornito dall'azienda host web.

## <a name="deploying-the-book-reviews-web-application"></a>Distribuzione dell'applicazione Web di revisioni Rubrica

L'esercitazione precedente esaminati copia del database di sviluppo all'ambiente di produzione, ma non esplorare la distribuzione dell'applicazione basati sui dati. A questo punto l'ambiente di produzione contiene il database ma viene usata la versione dell'applicazione libro esamina con revisioni statiche. È necessario distribuire l'applicazione di nuovo, basati sui dati al server di produzione con le informazioni di configurazione aggiornata.

Richiedere qualche istante per distribuire l'applicazione basati sui dati dall'ambiente di sviluppo alla produzione. Questo processo è stato illustrato in dettaglio nelle esercitazioni precedenti. Se è necessario un aggiornamento, vedere il *la distribuzione del sito Web utilizzando un FTP Client* o *la distribuzione del sito Web tramite Visual Studio* esercitazioni. È necessario assicurarsi che la stringa di connessione di database di produzione è quello utilizzato nell'ambiente di produzione, il che significa che un'alternativa `Web.config` file deve essere distribuito. In particolare, questa impostazione modificata `Web.config` file s `<connectionStrings>` elemento deve contenere la stringa di connessione di database di produzione e dovrebbe essere simile al seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Si noti che la stringa di connessione nel `<connectionStrings>` elemento è lo stesso nome (`ReviewsConnectionString`), ma che adesso contiene la stringa di connessione di database di produzione anziché la stringa di connessione di database di sviluppo.

A meno che non si dispone di un flusso di lavoro di distribuzione più formale, modificare manualmente il `Web.config` file da utilizzare la stringa di connessione di database di produzione prima della distribuzione (ricordando di ripristinarne nuovamente utilizzando la stringa di connessione di database di sviluppo in un secondo momento) o mantenere una separata `Web.config` file con le informazioni di configurazione Ambiente di produzione che viene caricate nell'ambiente di produzione come parte del processo di distribuzione.

> [!NOTE]
> Se si distribuisce accidentalmente un `Web.config` file che contiene la stringa di connessione di database di sviluppo, vi sarà un errore quando l'applicazione in produzione tenta di connettersi al database. Questo errore si manifesta come una `SqlException` con un messaggio di reporting che il server non è stato trovato o non accessibile.


Il sito è già stato distribuito nell'ambiente di produzione, visitare il sito di produzione tramite il browser. Si dovrebbe vedere e sfruttare la stessa esperienza utente durante l'esecuzione dell'applicazione basati sui dati in locale. Naturalmente quando si visita il sito Web in produzione il sito è attivato dal server di database di produzione, mentre il sito Web nell'ambiente di sviluppo viene utilizzato il database in fase di sviluppo. La figura 3 mostra il *insegna manualmente ASP.NET 3.5 nelle 24 ore* esaminare pagina dal sito Web nell'ambiente di produzione (si noti l'URL nella barra degli indirizzi del browser s).


[![L'applicazione guidata dai dati è ora disponibile in produzione.](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figura 3**: The Data-Driven applicazione è ora disponibile in produzione. ([Fare clic per visualizzare l'immagine ingrandita](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>L'archiviazione delle stringhe di connessione in un File di configurazione separato

Una tecnica comune per la gestione delle informazioni di configurazione separato per gli ambienti di sviluppo e produzione è avere due versioni di `Web.config`: uno per l'ambiente di sviluppo e uno per la produzione. Fase di distribuzione appropriata `Web.config` versione può essere copiata nell'ambiente di produzione. In teoria, questo processo potrebbe essere automatizzato nell'ambito del flusso di lavoro di distribuzione.

Anziché mantenere due `Web.config` file, facoltativamente, è possibile forniscono le differenze più granulari. Gli elementi che costituiscono il `Web.config` file può essere definite in un file di configurazione esterni che vengono quindi fatto riferimento nel `Web.config` file. In pratica è possibile avere una `Web.config` file per entrambi gli ambienti che fa riferimento a un file databaseConnectionStrings.config, che contiene le stringhe di connessione utilizzata dall'applicazione e sono univoci per ogni ambiente. È possibile individuare che fornisce un più chiaro che separa le informazioni di configurazione diverse in file separati e più semplice `Web.config` file e più chiaramente vengono descritte le differenze di configurazione tra gli ambienti di sviluppo e produzione.

Per utilizzare questa tecnica, iniziare creando una nuova cartella nell'applicazione web denominata `ConfigSections`. Successivamente, aggiungere due file a questa nuova cartella denominata databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config. Successivamente, copiare il `<connectionStrings>` elemento `Web.config` nei file di databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config e quindi modificare la stringa di connessione nel databaseConnectionStrings.production.config file in modo che specifichi la stringa di connessione di database di produzione. Ad esempio, il file di databaseConnectionStrings.dev.config deve contenere solo il `<connectionStrings>` elemento con una stringa di connessione che fa riferimento al database di sviluppo:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Analogamente, il file databaseConnectionStrings.production.config deve contenere solo un `<connectionStrings>` elemento, ma che la stringa di connessione di database di produzione.

Creare una copia del file databaseConnectionStrings.dev.config e denominarlo databaseConnectionStrings.config.

> [!NOTE]
> È possibile denominare il file di configurazione diverso databaseConnectionStrings.config, se d desiderata, ad esempio `connectionStrings.config` o `dbInfo.config`. Tuttavia, assicurarsi di assegnare un nome file con un `.config` estensione come `.config` file, per impostazione predefinita, non vengono utilizzati dal motore di ASP.NET. Il nome file di un altro elemento, ad esempio `connectionStrings.txt`, un utente può scegliere browser per [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) e visualizzare il contenuto del file.


A questo punto il `ConfigSections` cartella deve contenere tre file (vedere la figura 4). I file databaseConnectionStrings.dev.config e databaseConnectionStrings.production.config contengono le stringhe di connessione per gli ambienti di sviluppo e produzione, rispettivamente. Il file databaseConnectionStrings.config contiene le informazioni sulla stringa di connessione che verrà utilizzati dall'applicazione web in fase di esecuzione. Di conseguenza, il file databaseConnectionStrings.config deve essere identico al file databaseConnectionStrings.dev.config nell'ambiente di sviluppo, mentre in produzione, il file databaseConnectionStrings.config deve essere identico a databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figura 4**: ConfigSections ([fare clic per visualizzare l'immagine ingrandita](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


È ora necessario indicare `Web.config` per utilizzare il file databaseConnectionStrings.config per il proprio archivio di stringa di connessione. Aprire `Web.config` e sostituire l'elemento `<connectionStrings>` esistente con il codice seguente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

Il `configSource` attributo specifica un percorso fisico relativo al `Web.config` file. Se esterno `.config` trova nella stessa directory `Web.config` quindi impostare questo attributo per il nome del file di `.config` file. Se si s in una sottodirectory, come accade con databaseConnectionStrings.config, specificare la sottocartella utilizzando una barra rovesciata per delimitare i nomi di file e cartelle, ad esempio ConfigSections\databaseConnectionStrings.config.

Grazie a questa modifica, gli ambienti di sviluppo e produzione contengono gli stessi `Web.config` file. A questo punto l'unica differenza è il file databaseConnectionStrings.config. Copiare il file databaseConnectionStrings.production.config nell'ambiente di produzione e rinominarlo databaseConnectionStrings.config. Se, in futuro, sono state apportate modifiche alla stringa di connessione di database di produzione è necessario renderli al file databaseConnectionStrings.production.config e quindi caricare il file nell'ambiente di produzione, rinominarlo databaseConnectionStrings.config.

> [!NOTE]
> È possibile specificare le informazioni per qualsiasi `Web.config` elemento in un file separato e usare il `configSource` attributo fare riferimento a tale file dall'interno `Web.config`.


## <a name="summary"></a>Riepilogo

Applicazioni guidate dai dati utilizzano in genere database diversi in ambienti di sviluppo e produzione. Di conseguenza, le stringhe di connessione di database archiviate nella configurazione di s dell'applicazione web devono essere univoche per ogni ambiente. In questa esercitazione illustra in che modo determinare la stringa di connessione di database di produzione e i metodi per la gestione delle informazioni della stringa di connessione univoche nei due ambienti.

Buona programmazione!

#### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Stringhe di connessione e file di configurazione](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informazioni @ ConnectionStrings.com le stringhe di configurazione del database](http://www.connectionstrings.com/)
- [Spostare le impostazioni dal File Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentazione tecnica per il &lt;connectionStrings&gt; elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-a-database-cs.md)
> [Successivo](configuring-a-website-that-uses-application-services-cs.md)
