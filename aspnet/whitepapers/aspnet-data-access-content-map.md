---
uid: whitepapers/aspnet-data-access-content-map
title: Accesso ai dati ASP.NET - risorse consigliate | Microsoft Docs
author: rick-anderson
description: In questo argomento vengono forniti collegamenti a risorse della documentazione sull'accesso ai dati nelle applicazioni web ASP.NET, principalmente tramite Entity Framework e SA SQL...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832500"
---
<a name="aspnet-data-access---recommended-resources"></a>Accesso ai dati ASP.NET - risorse consigliata
====================
> In questo argomento vengono forniti collegamenti a risorse della documentazione sull'accesso ai dati nelle applicazioni web ASP.NET, principalmente tramite Entity Framework e SQL Server.
> 
> Se si conosce un grande blog post [stackoverflow](http://stackoverflow.com) thread o qualsiasi altro tipo di collegamento che può essere utile, [inviarci un messaggio di posta elettronica](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con il collegamento.
> 
> Ultimo aggiornamento 4 fino al 3 maggio 2014


Di seguito sono elencate le diverse sezioni di questo argomento:

- [Guida introduttiva con l'accesso ai dati in ASP.NET](#gettingstarted)
- [Utilizzo di Entity Framework](#ef)

    - [Con codice di Entity Framework](#cf)
    - [Tramite migrazioni di Entity Framework Code First](#efcfmigrations)
    - [Utilizzando Entity Framework Database prima di tutto o Model First (la finestra di progettazione di Entity Framework)](#efdbf)
    - [Caricamento dei dati correlati in Entity Framework (il caricamento Lazy, il caricamento Eager e il caricamento esplicito)](#efrelateddata)
    - [Ottimizzazione delle prestazioni di Entity Framework](#optimizingef)
    - [Gestione della concorrenza in un'applicazione Entity Framework](#efconcurrency)
    - [Documentazione su Entity Framework](#efbooks)
    - [Risorse aggiuntive di Entity Framework](#otherefresources)
- [Applicazioni di Data Binding in ASP.NET Web Form](#wfdatabinding)

    - [Tramite Web Form di associazione di modelli](#wfmodelbinding)
    - [Uso di Web Form controlli origine dati](#wfdsc)
    - [Uso di Web Form controlli con associazione a dati e le espressioni di associazione dati](#wfdbc)
- [Utilizzo di database di SQL Server](#sqlserver)

    - [Utilizzo di database SQL Server Express LocalDB](#sslocaldb)
    - [Utilizzo dei database SQL Server Express](#sse)
    - [Utilizzo di Database SQL di Azure](#ssdb)
    - [Scelta tra SQL Server e Database SQL di Azure](#ssdbchoosing)
- [Utilizzo dei sistemi di gestione di Database NoSQL](#nosql)
- [Usando le query LINQ nelle applicazioni ASP.NET](#linq)
- [Usare lo scaffolding di Dynamic Data](#dd)
- [Protezione dell'accesso ai dati](#securing)
- [Ottimizzazione delle prestazioni di accesso dati](#optimizingdataaccess)
- [Distribuzione di un Database](#deploying)
- [L'accesso ai dati tramite un servizio Web](#webservice)
- [Risorse aggiuntive](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Guida introduttiva con l'accesso ai dati in ASP.NET

- [Opzioni di archiviazione di dati (creazione di App Cloud reali con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capitolo di un e-book sullo sviluppo per il cloud. Introduce i database NoSQL come un'alternativa che molti sviluppatori ha familiari con i database relazionali tendono a sottovalutare. Vengono fornite linee guida sugli elementi da considerare quando si sceglie relazionale o NoSQL o scelta di una determinata piattaforma.
- [Opzioni di accesso ai dati ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Un'introduzione alle opzioni di accesso di dati per i database relazionali per ASP.NET e informazioni aggiuntive su come scegliere le piattaforme e accedere ai metodi appropriati per il proprio scenario.
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Se si è mai lavorato con i database relazionali, vedere questa pagina per un'introduzione ai concetti e terminologia dei database relazionali. Per un'introduzione a SQL Server vedere in particolare [funziona con i database di SQL Server](#sqlserver) più avanti in questo argomento.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Utilizzo di Entity Framework

- [Approcci di sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Materiale sussidiario su come scegliere un Entity Framework sviluppo approccio Database First, Model-First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Con codice di Entity Framework
  

Le esercitazioni seguenti offrono applicazioni di esempio scaricabile:

- [Introduzione a EF 6 con MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Include un'ampia gamma di scenari Code First di Entity Framework, inclusi le migrazioni ed Entity Framework 6 funzionalità quali la resilienza di connessione e intercettazione dei comandi asincroni. Si tratta di una versione aggiornata del [5 EF / MVC 4 serie](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie precedente include un'esercitazione sul repository e i modelli di unità di lavoro che non è incluso nella serie nuova.
- [Introduzione ad ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Viene illustrato un scenari primo intervallo di codice di Entity Framework più stretta ma esegue un processo più completo di introdurre funzionalità MVC.
- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Utilizza Code First in un'applicazione Web Form.
- [Introduzione a Web Form ASP.NET 4.5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introduzione a Web Form con alcune code coverage di Code First. Associazione di modelli Usa.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utilizza Code First in un'applicazione MVC 3 e-commerce che implementa anche l'appartenenza e l'autorizzazione. La versione MVC e ASP.NET (autenticazione e autorizzazione) sistema di appartenenze utilizzato qui non sono aggiornati; Per ulteriori informazioni aggiornate sulle appartenenze ASP.NET, vedere [ https://asp.net/identity ](https://asp.net/identity).

Altre risorse:

- [Entity Framework - Code First per un Database esistente](https://msdn.microsoft.com/data/jj200620). MSDN. Video e procedura dettagliata in cui viene illustrato come utilizzare Code First con un database esistente.
- [Centro per sviluppatori di dati - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Per una Guida alla documentazione di Entity Framework che è stata creata e gestita dal team di Entity Framework, vedere la [iniziare a usare](https://msdn.microsoft.com/data/ee712907) collegamento.

Vedere anche [libri su Entity Framework](#efbooks) e [ulteriori risorse di Entity Framework](#otherefresources) più avanti in questo argomento.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Tramite migrazioni di Entity Framework Code First
  

Maggior parte delle esercitazioni elencate in precedenza le migrazioni di copertina Code First. Vedere anche le risorse seguenti.

- [Distribuzione Web ASP.NET tramite Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). parte 2 serie di esercitazioni che illustra come usare migrazioni Code First per distribuire un database.
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in un sito Web di Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Come usare le migrazioni per distribuire i dati di appartenenza e l'applicazione in Azure.
- [Panoramica di distribuzione Web per Visual Studio e ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Vedere le **configurazione Database di distribuzione in Visual Studio** sezione per una spiegazione del modo in cui le migrazioni Code First è integrata in funzionalità di distribuzione web di Visual Studio.
- [Centro per sviluppatori di dati - migrazioni Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). Documentazione di migrazioni del team Entity Framework.
- [Le migrazioni Screencast serie](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog di Entity Framework). Video di tre in argomenti avanzati illustrati nella migrazioni Code First.
- [Codice di migrazioni First con ASP.NET Web Pages siti](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog di Mikesdotnetting). Viene illustrato come usare migrazioni Code First con un sito di ASP.NET Web Pages inserendo il contesto dei dati in un progetto di libreria di classi di Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Utilizzando Entity Framework Database prima di tutto o Model First (la finestra di progettazione di Entity Framework)

- [Guida introduttiva a 6 Database First di Entity Framework con MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Eseguire uno script in Esplora Server per creare un database e quindi usare la finestra di progettazione di Entity Framework per creare il modello di dati. Viene spiegato come creare web CRUD semplice pagine e per altri dati di gestione delle funzioni è possibile seguire una delle esercitazioni Code First poiché tutti i flussi di lavoro di Entity Framework usano la stessa API DbContext.

Le risorse seguenti sono meno recenti. Sono utili se si vuole usare la versione 4.0 di Entity Framework e si vuole usare un controllo origine dati per il data binding in un'applicazione Web Form.

- [Introduzione a Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Viene illustrato come utilizzare il **EntityDataSource** controllo.
- [Continuando con Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(Mostra come usare il **ObjectDataSource** controllo. Include un'esercitazione nella gestione della concorrenza, un'esercitazione sulle prestazioni di Entity Framework e un'esercitazione sulle novità in EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>La gestione dati correlati in Entity Framework (il caricamento Lazy, il caricamento Eager e il caricamento esplicito)

- [Lettura dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, applicazione di esempio MVC. I metodi illustrati si applicano anche ai Database primo flusso di lavoro e l'associazione di modelli Web Form.
- [Centro per sviluppatori di dati - caricamento delle entità correlate](https://msdn.microsoft.com/data/jj574232) (MSDN). Documentazione del team Entity Framework sul caricamento dei dati correlati.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Ottimizzazione delle prestazioni di Entity Framework

- [Advanced scenari di Entity Framework per un'applicazione ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Di seguito viene illustrato come eseguire istruzioni SQL o chiamare le stored procedure personalizzate, come disabilitare il rilevamento delle modifiche e come disabilitare la convalida durante il salvataggio delle modifiche.
- [Considerazioni sulle prestazioni per Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Considerazioni sulle prestazioni (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Massimizzazione delle prestazioni con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0.
- Vedere anche [l'accesso ai dati ASP.NET ottimizzazione](#optimizingdataaccess) più avanti in questo argomento.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Gestione della concorrenza in un'applicazione Entity Framework

- [Gestione della concorrenza con Entity Framework in un'applicazione ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Prima di tutto il codice API DbContext, usando un'applicazione di esempio MVC.
- [Centro per sviluppatori di dati: i modelli di concorrenza ottimistica](https://msdn.microsoft.cus/data/jj592904) (MSDN). Documentazione di concorrenza del team Entity Framework.
- [Gestione della concorrenza con Entity Framework in un'applicazione Web ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Si applica a Entity Framework 4.0. API di ObjectContext, usando un'applicazione di esempio Web Form in primo luogo, del database.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Documentazione su Entity Framework

- [Entity Framework di programmazione: DbContext](http://shop.oreilly.com/product/0636920022237.do) da Julie Lerman e Rowan Miller.
- [Programmazione di Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) da Julie Lerman e Rowan Miller.

Entrambi questi libri sono aggiornati con le tecniche consigliate corrente. Forniscono un'introduzione più completa è ancora facile da seguire a Entity Framework rispetto a qualsiasi elemento disponibile in Internet. Un altro libro intitolato [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) di Julie Lerman, è più grande e più completa, ma è meno recente e molte delle tecniche viene descritto come non sono più il metodo consigliato per l'utilizzo di Entity Framework. Vedere anche l'elenco di libri consigliati dal team di Entity Framework presso [Data Developer Center - documentazione](https://msdn.microsoft.com/data/aa937716) nel sito MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Altre risorse di Entity Framework

- [Blog del team di Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Una delle migliori risorse per gli annunci di nuove funzionalità e informazioni più recenti. Per altri blog correlate a Entity Framework, vedere il Blogroll alla [Introduzione a Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Vedere le **punti dati** colonna, ovvero spesso sugli argomenti relativi a Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Applicazioni di Data Binding in ASP.NET Web Form

- [Opzioni di accesso ai dati di form Web ASP.NET](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Tramite Web Form di associazione di modelli

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni usando Entity Framework Code First.
- [Web Form associazione modello-parte 1: Selezione dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog di Guthrie). In questi post di blog precedenti, la proprietà che sia attualmente denominata ItemType è stata denominata ModelType, ma in caso contrario, le informazioni che contengono sono valide.
- [Web Form associazione modello-parte 2: Filtro dei dati](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog di Guthrie).
- [Web Form associazione modello-parte 3: L'aggiornamento e la convalida](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog di Guthrie).
- [Associazione di modelli Web Form ASP.NET 4.5](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Associazione modello-parte 1: selezione dei dati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Associazione modello-parte 2: applicazione di filtri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Introduzione a Web Form ASP.NET 4.5 - visualizzare i dati gli elementi e i dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Uso di Web Form controlli origine dati

- [Controlli Server Web di origine dati](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Annuncio della versione del provider di dati dinamica e controllo EntityDataSource per Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog di sviluppo Web Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Uso di Web Form controlli con associazione a dati e le espressioni di associazione dati

- [Associazione di modelli e Web Form](https://go.microsoft.com/fwlink/?LinkId=286117). Serie di esercitazioni che usa Entity Framework Code First.
- [Introduzione a Web Form ASP.NET 4.5 - visualizzare i dati gli elementi e i dettagli](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controlli dati fortemente tipizzati](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog di Guthrie).
- [Controlli dati fortemente tipizzati](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 controlli per dati tipizzati di Web Form sicuro](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [I controlli Server Web con associazione a dati](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Cenni preliminari sulle espressioni di associazione a dati](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Questa pagina illustra solo **Eval** e **associare**; non è stato aggiornato per includere **elemento** e **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Utilizzo di database di SQL Server

- [Le funzionalità di Database SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Per un'introduzione generale a un'ampia gamma di argomenti relativi a SQL Server, vedere le voci sotto al nodo del sommario.
- [Edizioni di SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Riepilogo delle edizioni di SQL Server disponibili, con collegamenti a ulteriori informazioni su ciascuno di essi.)
- [Stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Uso di SQL Server Compact per applicazioni Web ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Database Product Samples](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Database di esempio AdventureWorks.
- [Installazione dei database di esempio](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Oltre ai metodi illustrati di seguito, è anche possibile scaricare uno dei file con estensione mdf di esempio per l'App\_cartella dati di un progetto web, convertire il database in LocalDB e creare una stringa di connessione LocalDB. Per informazioni su come eseguire questa operazione, vedere [procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Vedere anche le sezioni seguenti per l'utilizzo di SQL Server Express e LocalDB e scelta tra SQL Server e Database SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Utilizzo di database SQL Server Express LocalDB

- [SQL Server Express LocalDB 2012](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). L'introduzione di MSDN ufficiale in LocalDB.
- [Stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Procedura: eseguire l'aggiornamento a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Come eseguire la migrazione di un file con estensione mdf da una versione precedente di SQL Server Express in LocalDB. È inoltre necessario eseguire questa procedura se si scarica uno dei [i database di esempio di SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introduzione a LocalDB, un SQL Express migliore](https://go.microsoft.com/fwlink/?LinkId=234375) (blog di SQL Server Express). Dispone di maggiori informazioni sul motivo per cui è stato creato LocalDB quelli inclusi in MSDN.
- [LocalDB: Dov'è il Database?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog di SQL Server Express). Informazioni su dove vengono creati i file di database LocalDB.
- [Utilizzo di LocalDB con IIS completo, parte 1: profilo utente](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (blog di SQL Server Express). Local DB non è progettato per funzionare con IIS. Questa serie di post di blog illustra i problemi e alcune soluzioni alternative.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Utilizzo dei database SQL Server Express

- [Stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Se si usa l'impostazione della stringa di connessione AttachDBFileName con SQL Server Express, vedere in particolare la sezione dell'istanza utente di questa pagina.
- [Come diventare proprietari di SQL Server Express 2008 locale](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog di SQL Server Express). Un problema comune non è in grado di funzionare con i database di SQL Server Express perché non si è un amministratore nell'istanza di SQL Server Express. Per impostazione predefinita, solo la persona che ha installato SQL Server Express è un amministratore. Questo blog illustra come diventare un amministratore di SQL Server Express se sei un amministratore nel computer.
- [Applicazione web ASP.NET consente un database di SQL Server Express nell'ambiente di produzione?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Utilizzo di Database SQL di Azure

- [Distribuire un'app di App ASP.NET MCV sicura con appartenenza, OAuth e Database SQL a un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sito Web Microsoft Azure).
- [Database SQL](https://docs.microsoft.com/azure/sql-database/) (sito Web Microsoft Azure). Recupero delle esercitazioni introduttive e alcune guide pratiche.
- [Database SQL di Azure Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Nodo di livello superiore della tabella dei contenuti per il Database SQL in MSDN.
- [Indice di articoli di Windows Azure SQL Database TechNet Wiki](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sito Microsoft TechNet).
- [Blocco applicazione per la gestione degli errori temporanei](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Un framework che consente di gestire gli errori di rete temporanei e gli errori di connessione derivanti dalla limitazione delle richieste. Disponibile in un pacchetto NuGet: [Enterprise Library 5.0 - Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introduzione a Database SQL ed Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (area download Microsoft). Include laboratori pratici per il Database SQL.
- [Forum della Community di Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Lo spostamento in Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capitolo di uno scenario end-to-end completa dal team Microsoft Patterns and Practices. Illustra il motivo per cui si potrebbe voler eseguire la migrazione e su come eseguire la migrazione da SQL Server al Database SQL.
- [La migrazione di database di SQL Server al Database SQL di Azure Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Migrazione guidata Database SQL](http://sqlazuremw.codeplex.com/). Uno strumento open source per la migrazione dei database da e verso Database SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Scelta tra SQL Server e Database SQL di Azure

- [Confronto di SQL Server con Database SQL di Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sito Microsoft TechNet).
- [Migrazione dei dati al Database SQL di Azure: strumenti e tecniche](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Include varie sezioni che confrontano SQL Server al Database SQL e forniscono indicazioni su quando eseguire la migrazione da SQL Server al Database SQL.
- [Guida alla distribuzione di Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sito Microsoft TechNet).
- [Limitazioni delle funzionalità SQL Server (Database SQL di Azure di Windows)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Archiviazione tabelle di Azure di Windows e Database SQL di Azure - analogie e differenze](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Per un'applicazione da distribuire in Azure, archiviazione tabelle di Azure può essere un'alternativa al Database SQL di Azure. Questo argomento consente di scegliere tra queste alternative.
- [Database SQL di Azure Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Linee guida e limitazioni (Database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Utilizzo dei sistemi di gestione di Database NoSQL

- [Servizi dati di Windows Azure](https://www.windowsazure.com/develop/net/data/) (sito Web Microsoft Azure). Vedere le [Guida alla funzionalità del servizio tabelle](https://docs.microsoft.com/azure/) e il **Big Data** sezione della pagina.
- [Applicazione multilivello ASP.NET usando archiviazione tabelle, code e blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sito Web Microsoft Azure). Esercitazione end-to-end con l'applicazione di esempio scaricabile che Usa tabelle NoSQL di archiviazione Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Usando le query LINQ nelle applicazioni ASP.NET

- [Opzioni di accesso ai dati ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Include un'introduzione a LINQ.
- [Video di formazione su LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog di Joe Stagner).
- [Thread del Forum di ASP.NET con collegamenti a risorse dinamiche di LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Usare lo Scaffolding dei dati dinamici

- [Modelli di progetto di Dynamic Data](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Indicazioni su quando usare i progetti di Dynamic Data.
- [ASP.NET Dynamic Data](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Protezione dell'accesso ai dati

- [Protezione dell'accesso ai dati in ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Considerazioni sulla sicurezza (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Procedura: Proteggere le stringhe di connessione quando si usano controlli origine dati](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Ottimizzazione delle prestazioni di accesso dati

- [Cenni preliminari sulle prestazioni ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [La memorizzazione nella cache di ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Miglioramento delle prestazioni ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). È un avviso "Contenuto rimosso" nella parte superiore della pagina, ma la maggior parte delle informazioni sono ancora rilevanti e non sono presenti risorse aggiornate confrontabili.
- [Migliorare le prestazioni di SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Commento stesso come il collegamento precedente.

Vedere anche [prestazioni ottimizzando Entity Framework](#optimizingef) più indietro in questo argomento.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Distribuzione di un Database

- [Distribuzione Web ASP.NET - risorse consigliate](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>L'accesso ai dati tramite un servizio Web

- [L'accesso ai dati tramite un servizio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Materiale sussidiario su quando usare l'API Web e WCF.
- [Introduzione a ASP.NET Web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

- [Domande frequenti sull'accesso dati ASP.NET](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Web Form ASP.NET esercitazioni - dati](../web-forms/overview/data-access/index.md). La maggior parte delle esercitazioni seguenti sono relativamente precedente; Assicurarsi di leggere [opzioni di accesso dati ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) e [opzioni di archiviazione di dati (creazione reali di App Cloud con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) prima in modo che non si ottengono troppo lontano in un metodo di accesso di dati che non è corretto per il proprio scenario.
- [Mappa del contenuto per ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Esercitazioni - i dati delle pagine Web ASP.NET](../web-pages/overview/data/index.md).
- [L'accesso ai dati in Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Fornisce un elenco di collegamenti simile a questa mappa del contenuto, ma con particolare attenzione per Visual Studio anziché di ASP.NET.
