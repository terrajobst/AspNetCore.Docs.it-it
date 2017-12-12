---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: introduzione | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire, pubblicare, ASP.NET web da applicazione per App Web di servizio App di Azure o un provider di hosting di terze parti, con V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: e14f3bed001592c85bdbba868f51141bc52a9470
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: introduzione
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire, pubblicare, ASP.NET web da applicazione per App Web di servizio App di Azure o un provider di hosting di terze parti, usando Visual Studio 2012 con Azure SDK per .NET. La maggior parte delle procedure sono simile per Visual Studio 2013.
> 
> Si sviluppa un'applicazione web per renderla disponibile agli utenti su Internet. Ma esercitazioni di programmazione web in genere subito dopo che hai è illustrato come ottenere un elemento di lavoro nel computer di sviluppo. Questa serie di esercitazioni inizia in cui gli altri lasciare scollegati: è stata compilata un'app web, testato, ed è pronto. Argomenti successivi Queste esercitazioni viene illustrato come distribuire innanzitutto a IIS nel computer di sviluppo locale per il test, quindi in Azure o in un provider di hosting di terze parti per la gestione temporanea e produzione. L'applicazione di esempio che verrà distribuita è un progetto di applicazione web che usa Entity Framework, SQL Server e il sistema di appartenenze ASP.NET. L'applicazione di esempio Usa Web Form ASP.NET, ma si applicano anche alle procedure descritte per ASP.NET MVC e Web API.
> 
> Queste esercitazioni presuppongono che una conoscenza di come utilizzare ASP.NET in Visual Studio. In caso contrario, un buon punto di partenza è un [esercitazione di base ASP.NET Web Forms](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o [esercitazione di base ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum sulla distribuzione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) o [StackOverflow](http://stackoverflow.com).
> 
> Questo contenuto è disponibile anche come un e-book gratuito in [Raccolta TechNet E-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Panoramica

Queste esercitazioni consentono di eseguire la distribuzione di un'applicazione web ASP.NET che include i database di SQL Server. Vedrà come distribuire innanzitutto a IIS nel computer di sviluppo locale per il test, quindi ad App Web nel servizio App di Azure e Database SQL di Azure per la gestione temporanea e produzione. Si noterà come distribuire usando Visual Studio pubblicazione con un clic e si vedrà come distribuire la riga di comando.

Il numero di esercitazioni potrebbe rendere il processo di distribuzione risultare molto complessa. In effetti, le procedure di base sono semplici. Tuttavia, in situazioni reali, è spesso necessario eseguire le attività di distribuzione aggiuntivi, ad esempio, impostando le autorizzazioni di cartella nel server di destinazione. È stato illustrato alcune di queste attività aggiuntive, nella speranza che le esercitazioni, è necessario compilare le informazioni che potrebbero impedire correttamente la distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per l'esecuzione in sequenza e ogni parte genera nella parte precedente. È possibile ignorare parti che non sono pertinenti alla propria situazione e quindi potrebbe essere necessario regolare le procedure descritte nelle esercitazioni successive.

## <a name="intended-audience"></a>Destinatari

Le esercitazioni sono rivolte agli sviluppatori ASP.NET che operano in ambienti in cui:

- L'ambiente di produzione è App Web di servizio App di Azure o un provider di hosting di terze parti.
- Distribuzione non è limitata a un processo di integrazione continua, ma può essere eseguita direttamente da Visual Studio.

Distribuzione da [controllo del codice sorgente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) utilizzando un [il recapito continuo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) non viene descritto in queste esercitazioni, ad eccezione di un'esercitazione che illustra come eseguire la distribuzione da riga di comando. Per informazioni sulla distribuzione continua, vedere le risorse seguenti:

- [Integrazione continua e il recapito continuo (creazione di applicazioni Cloud del mondo reale con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Distribuire un'app web nel servizio App di Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-deploy/)
- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un set più vecchio di esercitazioni scritto per Visual Studio 2010 che dispone ancora di informazioni utili per gli ambienti aziendali.)

## <a name="using-a-third-party-hosting-provider"></a>Utilizzando un provider di hosting di terze parti

Le esercitazioni consentono di configurare un account Azure e distribuzione dell'applicazione per le app Web in Azure App Service per la gestione temporanea e produzione. Tuttavia, è possibile utilizzare le stesse procedure di base per la distribuzione in un provider di hosting di terze parti di propria scelta. Qualora le esercitazioni su processi specifici di Azure, sono spiegare che e indicare quali differenze è possibile prevedere in un provider di terze parti.

## <a name="deploying-web-app-projects"></a>Distribuzione di progetti di app web

L'applicazione di esempio disponibili per download e distribuzione per queste esercitazioni è un progetto di applicazione web di Visual Studio. Tuttavia, se si installa la versione più recente [l'aggiornamento di pubblicazione Web per Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), è possibile utilizzare gli strumenti e i metodi di distribuzione stessa per i progetti di app web.

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti ASP.NET MVC

L'applicazione di esempio è un progetto di Web Form ASP.NET, ma tutti gli elementi che informazioni su come eseguire è applicabili a ASP.NET MVC anche. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione web. L'unica differenza è che se si distribuisce in un provider di hosting che supportano ASP.NET MVC o la versione di destinazione, è necessario assicurarsi di avere installato appropriata ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) o [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) il pacchetto NuGet nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio viene utilizzato c# ma le esercitazioni non richiedono conoscenze di c# e le tecniche di distribuzione illustrate per le esercitazioni non sono specifiche della lingua.

## <a name="database-deployment-methods"></a>Metodi di distribuzione del database

Esistono tre modi che è possibile distribuire un database di SQL Server insieme a distribuzione web in Visual Studio:

- Migrazioni di Entity Framework Code First
- Il provider di Web Deploy dbDacFx
- Il provider di distribuzione Web dbFullSql

In questa esercitazione si utilizzerà le prime due di questi metodi. Il provider di distribuzione Web dbFullSql è un metodo di tipo legacy che non è più consigliato ad eccezione di alcuni scenari specifici, ad esempio la migrazione da SQL Server Compact a SQL Server.

I metodi illustrati in questa esercitazione sono per i database di SQL Server, non SQL Server Compact. Per informazioni su come distribuire un database di SQL Server Compact, vedere [Visual Studio Web distribuzione con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

I metodi illustrati in questa esercitazione è necessario utilizzare la distribuzione di Web metodo di pubblicazione. Se si preferisce un altro metodo, ad esempio FTP, File System o FPSE, di pubblicazione vedere [distribuzione di un database separatamente dalla distribuzione di applicazioni web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrazioni di Entity Framework Code First

In Entity Framework versione 4.3, Microsoft ha introdotto le migrazioni Code First. Migrazioni Code First consente di automatizzare il processo di apportare modifiche incrementali a un modello di dati e propagare le modifiche al database. Nelle versioni precedenti di Code First, è in genere consentire Entity Framework eliminare e ricreare il database ogni volta che si modifica il modello di dati. Questo non è un problema in fase di sviluppo perché i dati di test viene facilmente ricreati, ma nell'ambiente di produzione in genere si desidera aggiornare lo schema del database senza eliminare il database. La funzionalità di migrazioni consente Code First aggiornare il database senza eliminare e crearne uno nuovo. È possibile consentire a Code First stabilire automaticamente come apportare le modifiche necessarie allo schema oppure è possibile scrivere codice che consente di personalizzare le modifiche. Per un'introduzione alle migrazioni Code First, vedere [migrazioni Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Quando si distribuisce un progetto web, Visual Studio può automatizzare il processo di distribuzione di un database gestito da migrazioni Code First. Quando si crea il profilo di pubblicazione, selezionare una casella di controllo etichettato eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione). Questa impostazione, il processo di distribuzione configurare automaticamente i file Web. config dell'applicazione nel server di destinazione in modo che usi il primo codice di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First automaticamente viene creato il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un metodo di inizializzazione di migrazioni, il metodo viene eseguito dopo la creazione del database o lo schema viene aggiornato.

In questa esercitazione si userà migrazioni Code First per distribuire il database dell'applicazione.

### <a name="the-dbdacfx-web-deploy-provider"></a>Il provider di Web Deploy dbDacFx

Per un database di SQL Server che non è gestito da Code First di Entity Framework, è possibile selezionare una casella di controllo è denominata database di aggiornamento quando si configura il profilo di pubblicazione. Durante la distribuzione iniziale, il provider dbDacFx crea tabelle e altri oggetti di database nel database di destinazione in modo che corrisponda il database di origine. Nelle distribuzioni successive, il provider determina cosa è diversa tra i database di origine e di destinazione e aggiorna lo schema del database di destinazione in modo che corrisponda il database di origine. Per impostazione predefinita, il provider non apportare modifiche che causa la perdita di dati, ad esempio quando viene eliminata una tabella o colonna.

Questo metodo non automatizzare la distribuzione dei dati nelle tabelle di database, ma è possibile creare script per eseguire questa operazione e configurare Visual Studio per eseguirle durante la distribuzione. Un altro motivo per l'esecuzione di script durante la distribuzione è apportare modifiche allo schema che non possono essere eseguite automaticamente, in quanto la perdita di dati.

In questa esercitazione si userà il provider dbDacFx per distribuire il database delle appartenenze ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante l'esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non offrono una soluzione. Per aiutare con alcuni scenari comuni di problemi, un [risoluzione dei problemi di pagina di riferimento](troubleshooting.md) è disponibile. Se viene visualizzato un messaggio di errore o non funzioni come eseguire le esercitazioni, assicurarsi di selezionare la pagina sulla risoluzione dei problemi.

## <a name="comments-welcome"></a>Pagina iniziale di commenti

I commenti sulle esercitazioni sono iniziale e quando viene aggiornata l'esercitazione verrà resa ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

In questa esercitazione è stata scritta per i prodotti seguenti:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio Express 2012 per Web con [l'aggiornamento più recente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

È possibile seguire l'esercitazione utilizzando Visual Studio 2010 SP1 o Visual Studio 2013, ma alcune schermate sarà diverse e alcune funzionalità saranno diversi.

Se si utilizza Visual Studio 2013, installare [Azure SDK per Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Se si utilizza Visual Studio 2010 SP1, installare il software seguente:

- [Azure SDK per Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/en-us/library/hh500335.aspx).

A seconda di quanti delle dipendenze SDK già presenti nel computer, installare il SDK di Azure potrebbe richiedere molto tempo, da alcuni minuti a mezz'ora o più. Azure SDK è necessario anche se si prevede di pubblicare un provider di hosting di terze parti anziché in Azure, poiché il SDK include gli aggiornamenti più recenti sul web di Visual Studio le funzionalità di pubblicazione.

> [!NOTE]
> In questa esercitazione è stata scritta con la versione 1.8.1 di Azure SDK. Da allora sono state rilasciate le versioni più recenti con funzionalità aggiuntive. Le esercitazioni sono state aggiornate per sottolineare queste funzionalità e i collegamenti alle risorse con ulteriori informazioni.


Le istruzioni e schermate sono basate su Windows 8, ma le esercitazioni illustrano le differenze per Windows 7.

Altro software è necessario per completare l'esercitazione, ma non è necessario disporre che ancora installati. L'esercitazione verrà illustrati i passaggi per l'installazione quando necessario.

## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

L'applicazione che verrà distribuita è denominato Contoso University ed è già stato creato automaticamente. È una versione semplificata di un sito web university, in base ad accoppiamento l'applicazione Contoso University descritta nel [esercitazioni di Entity Framework sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare il [applicazione web di Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Il *zip* file contiene più versioni del progetto. Per eseguire i passaggi dell'esercitazione, iniziare con il progetto che si trova nella cartella c#. Per visualizzare l'aspetto del progetto alla fine delle esercitazioni, aprire il progetto nella cartella ContosoUniversity-End.

Per preparare il progetto per l'utilizzo tramite i passaggi dell'esercitazione, eseguire la procedura seguente:

1. Salvare i file della soluzione ContosoUniversity dalla cartella c# in una cartella denominata ContosoUniversity in qualsiasi cartella utilizzata per l'utilizzo di progetti di Visual Studio.

    Per impostazione predefinita, questa è la cartella seguente per Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Per le schermate in questa esercitazione, la cartella di progetto si trova nella directory radice nel `C`: unità.)
2. Avviare Visual Studio e aprire il progetto.
3. In **Esplora**, fare doppio clic la soluzione e fare clic su **il ripristino del pacchetto EnableNuGet**.
4. Compilare la soluzione.
5. Se si verificano errori di compilazione, è necessario ripristinare manualmente i pacchetti NuGet:

    1. In **Esplora**destro la soluzione e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.
    2. Nella parte superiore del **Gestisci pacchetti NuGet** la finestra di dialogo verrà visualizzato **pacchetti NuGet alcune non sono presenti questa soluzione. Fare clic per ripristinarli.** Fare clic su di **ripristinare** pulsante.
    3. Ricompilare la soluzione.
6. Premere CTRL + F5 per eseguire l'applicazione.

    L'applicazione verrà visualizzata la home page di Contoso University.

    ![Home Page Dev](introduction/_static/image1.png)

    (Potrebbe essere un tempo di attesa durante l'avvio di Visual Studio l'istanza di SQL Server Express LocalDB e si verifichi un errore di timeout se che processo impiega troppo tempo. In questo caso è sufficiente avviare il progetto nuovamente.)

Pagine dei siti Web sono accessibili dalla barra dei menu e consentono di eseguire le funzioni seguenti:

- Visualizzare le statistiche di student (la pagina di informazioni su).
- Visualizzare, modificare, eliminare e aggiungere gli studenti.
- Visualizzare e modificare i corsi.
- Visualizzare e modificare i docenti.
- Visualizzare e modificare i reparti.

Di seguito sono catture di schermata di poche pagine rappresentative.

![Gli studenti pagina Dev](introduction/_static/image2.png)

![Aggiungere gli studenti pagina Dev](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Esaminare le funzionalità dell'applicazione che influiscono sulla distribuzione

Le seguenti funzionalità dell'applicazione influisce su come distribuire o ciò che è necessario eseguire per distribuirla. Ognuno di questi è illustrato più dettagliatamente nelle seguenti esercitazioni della serie.

- University Contoso utilizza un database di SQL Server per archiviare dati applicativi quali nomi student e instructor. Il database contiene una combinazione di dati di test e dati di produzione, e quando si distribuisce in produzione è necessario escludere i dati di test.
- L'applicazione utilizza il sistema di appartenenze ASP.NET, che archivia informazioni sull'account utente in un database di SQL Server. L'applicazione definisce un utente amministratore che dispone dell'accesso a informazioni riservate. È necessario distribuire il database delle appartenenze senza account di prova con un account amministratore.
- L'applicazione utilizza un errore di terze parti, registrazione e report utilità. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione errore scrive informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account che viene eseguito ASP.NET nel sito distribuito disponga dell'autorizzazione di scrittura in questa cartella e che è necessario escludere la cartella di distribuzione. (In caso contrario, potrebbero essere distribuiti errore dati di log dall'ambiente di testing nell'ambiente di produzione e/o file di log degli errori di produzione potrebbero essere eliminati.)
- L'applicazione include alcune impostazioni che devono essere modificate in distribuito *Web. config* file in base all'ambiente di destinazione (test, gestione temporanea o produzione) e altre impostazioni che devono essere modificate a seconda della compilazione configurazione (Debug o Release).
- La soluzione di Visual Studio include un progetto libreria di classi. Deve essere distribuito solo l'assembly che genera il progetto, non il progetto stesso.

## <a name="summary"></a>Riepilogo

In questa esercitazione prima della serie, aver scaricato il progetto di Visual Studio e rivedere le funzionalità del sito che influiscono sulla modalità di distribuzione dell'applicazione. Nelle esercitazioni seguenti, ci si prepara per la distribuzione mediante l'impostazione di alcuni di questi elementi devono essere gestiti automaticamente. Altri occuperà di manualmente.

>[!div class="step-by-step"]
[Successivo](preparing-databases.md)
