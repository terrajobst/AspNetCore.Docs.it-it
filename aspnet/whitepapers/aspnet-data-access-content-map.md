---
uid: whitepapers/aspnet-data-access-content-map
title: Accesso ai dati ASP.NET - consigliato risorse | Documenti Microsoft
author: rick-anderson
description: In questo argomento vengono forniti collegamenti alle risorse di documentazione su come accedere ai dati nelle applicazioni web ASP.NET, principalmente tramite Entity Framework sia SQL Server...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-data-access---recommended-resources"></a>Accesso ai dati ASP.NET - risorse
====================
> In questo argomento vengono forniti collegamenti alle risorse di documentazione su come accedere ai dati nelle applicazioni web ASP.NET, principalmente tramite Entity Framework sia SQL Server.
> 
> Se si conosce un grande blog post, [stackoverflow](http://stackoverflow.com) thread o qualsiasi altro tipo di collegamento che può essere utile, [inviare un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con il collegamento.
> 
> Aggiornamento 4/3/2014 ultimo


Di seguito sono elencate le diverse sezioni di questo argomento:

- [Introduzione all'accesso ai dati in ASP.NET](#gettingstarted)
- [Tramite Entity Framework](#ef)

    - [Con codice di Entity Framework](#cf)
    - [Utilizzo di Entity Framework migrazioni Code First](#efcfmigrations)
    - [Con Entity Framework Database o del modello prima (Entity Framework Designer)](#efdbf)
    - [Caricamento dei dati correlati in Entity Framework (caricamento Lazy, il caricamento Eager e il caricamento esplicito)](#efrelateddata)
    - [Ottimizzazione delle prestazioni di Entity Framework](#optimizingef)
    - [La gestione della concorrenza in un'applicazione Entity Framework](#efconcurrency)
    - [Documentazione su Entity Framework](#efbooks)
    - [Risorse aggiuntive di Entity Framework](#otherefresources)
- [Applicazioni di Data Binding in ASP.NET Web Forms](#wfdatabinding)

    - [Con Web Form di associazione del modello](#wfmodelbinding)
    - [Utilizzo di Web Form controlli origine dati](#wfdsc)
    - [Utilizzo di Web Form controlli con associazione a dati e le espressioni di associazione dati](#wfdbc)
- [Utilizzo di database di SQL Server](#sqlserver)

    - [Utilizzo di database SQL Server Express LocalDB](#sslocaldb)
    - [Utilizzo di database SQL Server Express](#sse)
    - [Utilizzo di Database SQL di Azure](#ssdb)
    - [Scelta tra SQL Server e Database SQL di Azure](#ssdbchoosing)
- [Utilizzo dei sistemi di gestione di Database NoSQL](#nosql)
- [Utilizzo di query LINQ nelle applicazioni ASP.NET](#linq)
- [Utilizzare lo scaffolding di Dynamic Data](#dd)
- [Protezione dell'accesso ai dati](#securing)
- [L'ottimizzazione delle prestazioni di accesso ai dati](#optimizingdataaccess)
- [Distribuzione di un Database](#deploying)
- [L'accesso ai dati tramite un servizio Web](#webservice)
- [Risorse aggiuntive](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introduzione all'accesso ai dati in ASP.NET

- [Opzioni di archiviazione di dati (creazione di applicazioni Cloud del mondo reale con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capitolo di un e-book sullo sviluppo per il cloud. Introduce il database NoSQL come un'alternativa che molti sviluppatori ha familiari con i database relazionali tendono a sottovalutare. Vengono fornite indicazioni su cosa considerare quando si sceglie relazionale o NoSQL o scegliendo una determinata piattaforma.
- [Opzioni di accesso ASP.NET Data](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Un'introduzione alle opzioni di accesso ai dati per i database relazionali per ASP.NET e informazioni aggiuntive su come selezionare le piattaforme e accedere ai metodi appropriati per lo scenario.
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Se si è mai lavorato con i database relazionali, vedere questa pagina per un'introduzione ai concetti e terminologia dei database relazionali. Per un'introduzione a SQL Server, in particolare vedere [si lavora con database di SQL Server](#sqlserver) più avanti in questo argomento.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Tramite Entity Framework

- [Approcci allo sviluppo con Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Informazioni aggiuntive su come scegliere un Entity Framework approccio di sviluppo il primo Database Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Con codice di Entity Framework
  

Le esercitazioni seguenti offrono le applicazioni di esempio scaricabile:

- [Introduzione a Entity Framework 6 con MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Include un'ampia gamma di scenari di Code First di Entity Framework, incluse le migrazioni ed Entity Framework 6 funzionalità quali resilienza delle connessioni, intercettazione di comando e asincrono. Si tratta di una versione aggiornata del [EF 5 / MVC 4 serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie precedente include un'esercitazione sul repository e modelli di unità di lavoro che non è incluso nella nuova serie.
- [Introduction to ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Illustra una più stretta scenari di primo intervallo di codice di Entity Framework, ma non un processo più completo dell'introduzione di funzionalità MVC.
- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Utilizza Code First in un'applicazione Web Form.
- [Introduzione a ASP.NET 4.5 Web Form](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introduzione a Web Form con alcuni copertura di Code First. Associazione del modello viene utilizzato.
- [MVC negozio](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilizza Code First in un'applicazione di e-commerce MVC 3 che implementa anche l'appartenenza e l'autorizzazione. La versione MVC e un sistema appartenenze ASP.NET (autenticazione e autorizzazione) utilizzato in questo argomento non sono aggiornati; Per informazioni più aggiornate sulle appartenenze di ASP.NET, vedere [https://asp.net/identity](https://asp.net/identity).

Altre risorse:

- [Entity Framework - codice per un Database esistente](https://msdn.microsoft.com/data/jj200620). MSDN. Procedura dettagliata in cui viene illustrato come utilizzare Code First con un database esistente e video.
- [Centro per sviluppatori dati - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Per una Guida alla documentazione di Entity Framework in cui è stata creata e gestita dal team di Entity Framework, vedere il [iniziare](https://msdn.microsoft.com/data/ee712907) collegamento.

Vedere anche [documentazione su Entity Framework](#efbooks) e [ulteriori risorse di Entity Framework](#otherefresources) più avanti in questo argomento.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Utilizzo di Entity Framework migrazioni Code First
  

Maggior parte delle esercitazioni di Code First elencate in precedenza le migrazioni di copertura. Vedere anche le risorse seguenti.

- [Distribuzione Web ASP.NET utilizzando Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). parte 2 serie di esercitazioni in cui viene illustrato come utilizzare migrazioni Code First per distribuire un database.
- [Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Descrive come usare le migrazioni per distribuire i dati di appartenenza e l'applicazione in Azure.
- [Cenni preliminari sulla distribuzione di Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Vedere il **configurazione Database di distribuzione in Visual Studio** sezione per una spiegazione delle modalità migrazioni Code First è integrata con funzionalità di distribuzione web di Visual Studio.
- [Centro per sviluppatori dati - migrazioni Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentazione di migrazioni del team di Entity Framework.
- [Le migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog di Entity Framework). Tre video su argomenti avanzati nelle migrazioni Code First.
- [Migrazioni Code First con ASP.NET Web Pages siti](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog di Mikesdotnetting). Viene illustrato come utilizzare migrazioni Code First con un sito ASP.NET Web Pages inserendo il contesto dei dati in un progetto di libreria di classi di Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Con Entity Framework Database o del modello prima (Entity Framework Designer)

- [Introduzione a Entity Framework 6 Database First MVC 5 con](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Eseguire uno script in Esplora Server per creare un database e quindi utilizzare la finestra di progettazione di Entity Framework per creare il modello di dati. Viene spiegato come creare semplici CRUD web pages e per altri dati, è possibile seguire una delle esercitazioni di Code First poiché tutti i flussi di lavoro di Entity Framework utilizzano la stessa API DbContext funzioni di gestione.

Le seguenti risorse risultano meno recenti. Sono utili se si desidera utilizzare la versione 4.0 di Entity Framework e si desidera utilizzare un controllo origine dati per il data binding in un'applicazione Web Form.

- [Introduzione a Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Viene illustrato come utilizzare il **EntityDataSource** controllo.
- [Continuare con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(di seguito viene illustrato come utilizzare il **ObjectDataSource** controllo. Include un'esercitazione sulla gestione di concorrenza, un'esercitazione sulle prestazioni di Entity Framework e un'esercitazione sulle novità in Entity Framework 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Gestione dati correlati in Entity Framework (caricamento Lazy, il caricamento Eager e il caricamento esplicito)

- [Lettura dei dati correlati con Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). In primo luogo, il codice dell'applicazione di esempio MVC. I metodi illustrati si applicano anche per l'associazione di modelli Web Form e il Database prima del flusso di lavoro.
- [Centro per sviluppatori di dati, il caricamento delle entità correlate](https://msdn.microsoft.com/data/jj574232) (MSDN). Dati relativi alla documentazione del team di Entity Framework sul caricamento.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Ottimizzazione delle prestazioni di Entity Framework

- [Entity Framework scenari avanzati di per un'applicazione ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Viene illustrato come eseguire istruzioni SQL o chiamare le stored procedure personalizzate, come disabilitare il rilevamento delle modifiche e come disabilitare la convalida durante il salvataggio delle modifiche.
- [Considerazioni sulle prestazioni per Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Ottimizzare le prestazioni con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0.
- Vedere anche [accesso ai dati ASP.NET ottimizzazione](#optimizingdataaccess) più avanti in questo argomento.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>La gestione della concorrenza in un'applicazione Entity Framework

- [La gestione della concorrenza con Entity Framework in un'applicazione MVC ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). In primo luogo, il codice API DbContext, utilizzando un'applicazione di esempio MVC.
- [Centro per sviluppatori di dati: i modelli di concorrenza ottimistica](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentazione di concorrenza del team di Entity Framework.
- [La gestione della concorrenza con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0. API di ObjectContext, usando un'applicazione di esempio di Web Form in primo luogo, del database.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Documentazione su Entity Framework

- [Entity Framework di programmazione: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman e Miller Rowan.
- [Entity Framework di programmazione: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman e Miller Rowan.

I manuali vengano aggiornati con le tecniche consigliate corrente. Forniscono un'introduzione più completa ancora da seguire a Entity Framework di qualsiasi elemento disponibili su Internet. Un altro libro, [Entity Framework di programmazione](http://shop.oreilly.com/product/9780596807252.do) per Julie Lerman, è più grande e più completa, ma è meno recente e molte delle tecniche copre non sono più il metodo consigliato per l'utilizzo di Entity Framework. Vedere anche l'elenco di libri consigliati dal team di Entity Framework in [Centro per sviluppatori dati - documentazione](https://msdn.microsoft.com/data/aa937716) nel sito MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Altre risorse di Entity Framework

- [Blog del team di Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Una delle migliori risorse per informazioni più recenti e gli annunci di nuove funzionalità migliorate. Per altri blog relativi a Entity Framework, vedere il Blogroll in [Introduzione a Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Vedere il **punti dati** colonna, è spesso sugli argomenti relativi a Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Applicazioni di Data Binding in ASP.NET Web Forms

- [Web Form ASP.NET le opzioni di accesso ai dati](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Con Web Form di associazione del modello

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni tramite EF Code First.
- [Web Form modello associazione parte 1: Selezione dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog di Scott Guthrie). In questi post di blog precedenti, la proprietà attualmente denominato ItemType è denominata ModelType, ma in caso contrario le informazioni che contengono sono valide.
- [Web Form modello associazione parte 2: Filtrare i dati](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog di Scott Guthrie).
- [Web Form modello associazione parte 3: Aggiornamento e la convalida](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog di Scott Guthrie).
- [Associazione del modello ASP.NET 4.5 Web Form](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Associazione parte 1: selezione di dati del modello](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Modello di associazione parte 2 - filtro](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Guida introduttiva a ASP.NET 4.5 Web Form - visualizzare i dati elementi e dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Utilizzo di Web Form controlli origine dati

- [Controlli Server Web di origine dati](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annuncio del rilascio di provider di dati dinamici e controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog di sviluppo Web Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Utilizzo di Web Form controlli con associazione a dati e le espressioni di associazione dati

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni che usa Entity Framework Code First.
- [Guida introduttiva a ASP.NET 4.5 Web Form - visualizzare i dati elementi e dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controlli dati fortemente tipizzati](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog di Scott Guthrie).
- [Controlli dati fortemente tipizzati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 controlli per dati tipizzati di Web Form sicuro](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [I controlli Server Web con associazione a dati](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Cenni preliminari sulle espressioni di associazione a dati](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Questa pagina include informazioni soli **Eval** e **associare**; non è stato aggiornato per includere **elemento** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilizzo di database di SQL Server

- [Le funzionalità di Database SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Per un'introduzione generale a una vasta gamma di argomenti di SQL Server, vedere le voci sotto al nodo del sommario.
- [Edizioni di SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un riepilogo delle edizioni di SQL Server disponibili, con collegamenti a ulteriori informazioni su ciascuno di essi.)
- [Stringhe di connessione di SQL Server per applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Utilizzo di SQL Server Compact per applicazioni Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Database Product Samples](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Database di esempio AdventureWorks.
- [L'installazione dei database di esempio](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oltre ai metodi illustrati di seguito, è anche possibile scaricare un file di esempio con estensione mdf all'App\_cartella dati di un progetto web, convertire il database in LocalDB e creare una stringa di connessione del database locale. Per informazioni su come eseguire questa operazione, vedere [procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Vedere anche le sezioni seguenti sull'uso di SQL Server Express e del database locale e scegliendo tra SQL Server e Database SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilizzo di database SQL Server Express LocalDB

- [SQL Server Express LocalDB 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). L'introduzione di MSDN ufficiale di LocalDB.
- [Stringhe di connessione di SQL Server per applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Come eseguire la migrazione di un file con estensione mdf da una versione precedente di SQL Server Express a LocalDB. È inoltre necessario eseguire questa procedura se si scarica un il [database di esempio di SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introduzione a LocalDB, una migliore SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (blog di SQL Server Express). Dispone di ulteriori informazioni sui motivi per cui è stato creato LocalDB rispetto a quella inclusa in MSDN.
- [LocalDB: Dove è My Database?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog di SQL Server Express). Informazioni su dove vengono creati i file di database LocalDB.
- [Utilizzo di LocalDB con IIS completo, parte 1: profilo utente](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog di SQL Server Express). Database locale non è progettato per funzionare con IIS. Questa serie di post di blog vengono illustrati i problemi e alcune soluzioni alternative.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilizzo di database SQL Server Express

- [Stringhe di connessione di SQL Server per applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se si utilizza l'impostazione della stringa di connessione AttachDBFileName con SQL Server Express, vedere in particolare la sezione istanza utente di questa pagina.
- [Come acquisire la proprietà del locale di SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog di SQL Server Express). Non è un problema comune è in grado di lavorare con i database di SQL Server Express perché non si è un amministratore nell'istanza di SQL Server Express. Per impostazione predefinita, solo la persona che ha installato SQL Server Express è un amministratore. In questo blog viene illustrato come creare manualmente un amministratore di SQL Server Express se sei un amministratore nel computer.
- [Se l'applicazione web ASP.NET può usare un database di SQL Server Express nell'ambiente di produzione?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilizzo di Database SQL di Azure

- [Distribuire un'app di ASP.NET MVC Secure con appartenenza, OAuth e il Database SQL a un sito Web di Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sito Web di Microsoft Azure).
- [Database SQL](https://docs.microsoft.com/azure/sql-database/) (sito Web di Microsoft Azure). Recupero delle esercitazioni introduttive e procedure dettagliate.
- [Database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nodo di livello superiore della tabella del contenuto per il Database SQL in MSDN.
- [Indice di articoli di Windows Azure SQL Database TechNet Wiki](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sito Microsoft TechNet).
- [Blocco applicazione di gestione degli errori temporanei](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un framework che consente di gestire gli errori di rete temporanei e gli errori di connessione risultanti dalla limitazione delle richieste. Disponibile in un pacchetto NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Guida introduttiva a Database SQL e di Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (area Download Microsoft). Include esercitazioni per Database SQL.
- [Forum della Community di Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Lo spostamento di Database SQL di Azure](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capitolo di uno scenario end-to-end completo dal team Microsoft Patterns and Practices. Viene illustrata perché si potrebbe voler eseguire la migrazione e come eseguire la migrazione da SQL Server al Database SQL.
- [La migrazione di database di SQL Server per Database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Migrazione guidata Database SQL](http://sqlazuremw.codeplex.com/). Uno strumento open source per la migrazione dei database da e verso il Database SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Scelta tra SQL Server e Database SQL di Azure

- [Confronto di SQL Server con Database SQL di Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sito Microsoft TechNet).
- [Migrazione dei dati in Database SQL di Azure: strumenti e tecniche](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Include le sezioni di confronto di SQL Server al Database SQL e vengono fornite indicazioni su quando eseguire la migrazione da SQL Server al Database SQL.
- [Guida di Windows Azure SQL Database recapito](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sito Microsoft TechNet).
- [Limitazioni delle funzionalità SQL Server (Database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Archiviazione tabelle di Azure di Windows e Database SQL di Azure - confronto e Contrapposizioni](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Per un'applicazione che si distribuisce in Windows Azure, archiviazione tabelle di Azure può essere un'alternativa al Database SQL di Windows Azure. In questo argomento consente di scegliere tra queste alternative.
- [Database SQL di Azure](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Linee guida e limitazioni (Database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilizzo dei sistemi di gestione di Database NoSQL

- [Servizi dati di Windows Azure](https://www.windowsazure.com/develop/net/data/) (sito Web di Microsoft Azure). Vedere il [Guida alle funzionalità del servizio tabelle](https://docs.microsoft.com/azure/) e **Big Data** sezione della pagina.
- [Applicazione multilivello ASP.NET utilizzando archiviazione tabelle, code e blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito Web di Microsoft Azure). Esercitazione end-to-end con l'applicazione di esempio scaricabile che Usa tabelle di database NoSQL di archiviazione Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Utilizzo di query LINQ nelle applicazioni ASP.NET

- [Opzioni di accesso ASP.NET Data](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Include un'introduzione a LINQ.
- [Video di formazione LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog di Joe Stagner).
- [Thread del Forum di ASP.NET con collegamenti alle risorse dinamiche di LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Utilizzare lo Scaffolding di Dynamic Data

- [Modelli di progetto Dynamic Data](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Informazioni aggiuntive sull'utilizzo di progetti Dynamic Data.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protezione dell'accesso ai dati

- [Protezione dell'accesso ai dati in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerazioni sulla sicurezza (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Procedura: Proteggere le stringhe di connessione quando si utilizzano controlli origine dati](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>L'ottimizzazione delle prestazioni di accesso ai dati

- [Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [La memorizzazione nella cache di ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). È un avviso "Contenuto ritirato" nella parte superiore della pagina, ma la maggior parte delle informazioni sono ancora rilevanti e nessuna risorsa aggiornata confrontabili.
- [Miglioramento delle prestazioni di SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Commento stesso come il collegamento precedente.

Vedere anche [delle prestazioni di Entity Framework ottimizzazione](#optimizingef) più indietro in questo argomento.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Distribuzione di un Database

- [Distribuzione Web ASP.NET - consigliato risorse](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>L'accesso ai dati tramite un servizio Web

- [L'accesso ai dati tramite un servizio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Informazioni aggiuntive sull'utilizzo di API Web e WCF.
- [Introduzione a ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Accesso ai dati ASP.NET domande frequenti su](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Web Form ASP.NET esercitazioni - dati](../web-forms/overview/data-access/index.md). La maggior parte di queste esercitazioni sono relativamente precedente; Assicurarsi di leggere [opzioni di accesso ai dati ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) e [le opzioni di archiviazione di dati (creazione reali Cloud di applicazioni con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) primo in modo che non si ottengono troppo lontano in un metodo di accesso ai dati che non è corretto per lo scenario.
- [Mappa del contenuto ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages esercitazioni - dati](../web-pages/overview/data/index.md).
- [L'accesso ai dati in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornisce un elenco di collegamenti simile a questa mappa del contenuto, ma con l'obiettivo di Visual Studio anziché di ASP.NET.
