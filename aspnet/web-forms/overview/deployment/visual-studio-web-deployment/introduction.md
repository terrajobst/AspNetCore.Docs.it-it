---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Distribuzione Web ASP.NET tramite Visual Studio: introduzione | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET, pubblicare, applicazione web ad App Web di servizio App di Azure o un provider di hosting di terze parti, per utilizzo di V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: a227f6564607ed9e909fc4d6297d7370f8fb62b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827889"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Distribuzione Web ASP.NET tramite Visual Studio: introduzione
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET, pubblicare, applicazione web ad App Web di servizio App di Azure o un provider di hosting di terze parti, da usando Visual Studio 2012 con Azure SDK per .NET. La maggior parte delle procedure sono simile per Visual Studio 2013.
> 
> Si sviluppa un'applicazione web per renderla disponibile agli utenti tramite Internet. Tuttavia, le esercitazioni di programmazione web arresta genere subito dopo che è stato illustrato come ottenere elementi che funzionano su computer di sviluppo. Inizia a questa serie di esercitazioni in cui gli altri lasciare disattivato: è stata compilata un'app web, testati, ed è pronto per iniziare. Argomenti successivi Queste esercitazioni illustrano come distribuire prima di tutto IIS nei computer di sviluppo locale per il test, quindi in Azure o in un provider di hosting di terze parti per lo staging e produzione. L'applicazione di esempio che verrà distribuita è un progetto di applicazione web che usa Entity Framework, SQL Server e il sistema di appartenenze ASP.NET. L'applicazione di esempio Usa Web Form ASP.NET, ma le procedure riportate si applicano anche a ASP.NET MVC e API Web.
> 
> Queste esercitazioni presuppongono che una conoscenza di come usare ASP.NET core in Visual Studio. In caso contrario, un buon punto di partenza è una [esercitazione di base ASP.NET Web Forms](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o una [esercitazione di base ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum sulla distribuzione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) oppure [StackOverflow](http://stackoverflow.com).
> 
> Questo contenuto è disponibile anche come un e-book gratuito nel [raccolta di E-Book di TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Panoramica

Queste esercitazioni illustrano in dettaglio la distribuzione di un'applicazione web ASP.NET che include i database di SQL Server. Verrà distribuita prima di tutto IIS nei computer di sviluppo locale per il test, quindi alle App Web in servizio App di Azure e Database SQL di Azure per lo staging e produzione. Si noterà come distribuire con Visual Studio pubblicazione con un clic, e verrà illustrato come eseguire la distribuzione tramite la riga di comando.

Il numero di esercitazioni potrebbe rendere il processo di distribuzione potrebbe sembrare complicata. In effetti, le procedure di base sono semplici. Tuttavia, in situazioni reali, è spesso necessario eseguire le attività di distribuzione aggiuntivi, ad esempio, impostando le autorizzazioni della cartella nel server di destinazione. Abbiamo illustrato alcune di queste attività aggiuntive, nella speranza che non lasciano le esercitazioni di informazioni che potrebbero impedire il correttamente la distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per eseguire in sequenza e ogni parte è basata sulla parte precedente. È possibile ignorare parti che non sono pertinenti alla propria situazione, ma quindi potrebbe essere necessario regolare le procedure descritte nelle esercitazioni successive.

## <a name="intended-audience"></a>Destinatari

Le esercitazioni sono destinate agli sviluppatori ASP.NET che funzionano in ambienti in cui:

- L'ambiente di produzione è App Web di servizio App di Azure o un provider di hosting di terze parti.
- Distribuzione non è limitata a un processo di integrazione continua, ma può essere eseguita direttamente da Visual Studio.

Distribuzione dal [controllo del codice sorgente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) usando un [recapito continuo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) non viene descritto in queste esercitazioni, ad eccezione di un'esercitazione che illustra come distribuire dalla riga di comando. Per informazioni sul recapito continuo, vedere le risorse seguenti:

- [Integrazione continua e recapito continuo (creazione di App Cloud reali con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Distribuire un'app web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un set meno recente delle esercitazioni scritta per Visual Studio 2010, che contiene informazioni utili per gli ambienti aziendali.)

## <a name="using-a-third-party-hosting-provider"></a>Usando un provider di hosting di terze parti

Le esercitazioni descrivono il processo di configurazione di un account Azure e distribuzione dell'applicazione per le app Web nel servizio App di Azure per lo staging e produzione. Tuttavia, è possibile utilizzare le stesse procedure di base per la distribuzione in un provider di hosting di terze parti di propria scelta. In cui le esercitazioni passano per processi specifici in Azure, sono illustrate di seguito e indicare quali differenze è possibile prevedere in un provider di terze parti.

## <a name="deploying-web-app-projects"></a>Distribuzione di progetti di app web

L'applicazione di esempio che si scarica e si distribuisce per queste esercitazioni è un progetto di applicazione web di Visual Studio. Tuttavia, se si installa la versione più recente [aggiornamento della pubblicazione sul Web per Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), è possibile utilizzare le stessi metodi di distribuzione e gli strumenti per i progetti di app web.

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti ASP.NET MVC

L'applicazione di esempio è un progetto di Web Form ASP.NET, ma tutto ciò che descrive come eseguire questa operazione è anche applicabile a ASP.NET MVC. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione web. L'unica differenza è che se si distribuisce in un provider di hosting che supportano ASP.NET MVC o della versione di destinazione di esso, è necessario assicurarsi di aver installato appropriato ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) oppure [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) il pacchetto NuGet nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio Usa c#, ma le esercitazioni non richiedono la conoscenza del linguaggio c# e le tecniche di distribuzione illustrate per le esercitazioni non sono specifiche della lingua.

## <a name="database-deployment-methods"></a>Metodi di distribuzione di database

Esistono tre modi che è possibile distribuire un database di SQL Server insieme a distribuzione web in Visual Studio:

- Migrazioni di Entity Framework Code First
- Il provider di Web Deploy dbDacFx
- Il provider di distribuzione Web dbFullSql

In questa esercitazione si userà i primi due di questi metodi. Il provider di distribuzione Web dbFullSql è un metodo legacy che non è più consigliato, ad eccezione di alcuni scenari specifici, ad esempio la migrazione da SQL Server Compact a SQL Server.

I metodi illustrati in questa esercitazione sono per i database di SQL Server, non SQL Server Compact. Per informazioni su come distribuire un database di SQL Server Compact, vedere [Visual Studio Web distribuzione con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

I metodi illustrati in questa esercitazione è necessario utilizzare la distribuzione Web metodo di pubblicazione. Se si preferisce pubblicare un altro metodo, ad esempio FTP, File System o FPSE, vedere [distribuzione di un database separatamente dalla distribuzione di applicazioni web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) nella mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Migrazioni di Entity Framework Code First

Nella versione 4.3 di Entity Framework, Microsoft ha introdotto le migrazioni Code First. Migrazioni Code First consente di automatizzare il processo di apportare modifiche incrementali al modello di dati e propagare tali modifiche al database. Nelle versioni precedenti di Code First, in genere consentire a Entity Framework Elimina e ricrea il database ogni volta che si modifica il modello di dati. Ciò non è un problema in fase di sviluppo perché i dati di test viene facilmente ricreati, ma nell'ambiente di produzione in genere si desidera aggiornare lo schema del database senza eliminare il database. Abilita Code First aggiornare il database senza eliminare e ricreare la funzionalità delle migrazioni. È possibile consentire a Code First automaticamente decidere come rendere le modifiche necessarie allo schema oppure è possibile scrivere codice che consente di personalizzare le modifiche. Per un'introduzione alle migrazioni Code First, vedere [migrazioni Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Quando si distribuisce un progetto web, Visual Studio possono automatizzare il processo di distribuzione di un database gestito da migrazioni Code First. Quando si crea il profilo di pubblicazione, si seleziona una casella di controllo etichettato Esegui migrazioni Code First (inizia all'avvio dell'applicazione). Questa impostazione fa in modo che il processo di distribuzione configurare automaticamente il file Web. config dell'applicazione nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non ha alcun effetto con il database durante il processo di distribuzione. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First automaticamente crea il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un metodo di inizializzazione delle migrazioni, il metodo viene eseguito dopo la creazione del database o lo schema viene aggiornato.

In questa esercitazione si userà migrazioni Code First per distribuire il database dell'applicazione.

### <a name="the-dbdacfx-web-deploy-provider"></a>Il provider di Web Deploy dbDacFx

Per un database di SQL Server che non è gestito da Code First di Entity Framework, è possibile selezionare una casella di controllo etichettato Aggiorna database quando si configura il profilo di pubblicazione. Durante la distribuzione iniziale, il provider dbDacFx crea tabelle e altri oggetti di database nel database di destinazione in modo che corrisponda il database di origine. Nelle successive distribuzioni, il provider determina ciò che è diverso tra i database di origine e destinazione e aggiorna lo schema del database di destinazione in modo che corrisponda il database di origine. Per impostazione predefinita, il provider non apportare modifiche che causano la perdita di dati, ad esempio quando viene eliminata una tabella o colonna.

Questo metodo non automatizza la distribuzione dei dati nelle tabelle di database, ma è possibile creare script per eseguire questa operazione e configurare Visual Studio per eseguirle durante la distribuzione. Un altro motivo per eseguire script durante la distribuzione è apportare modifiche allo schema che non possono essere eseguite automaticamente, perché potrebbe causare la perdita di dati.

In questa esercitazione si userà il provider dbDacFx per distribuire il database di appartenenze ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante l'esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non sempre forniscono una soluzione ovvia. Per semplificare alcuni scenari comuni di problemi, un [risoluzione dei problemi di pagina di riferimento](troubleshooting.md) è disponibile. Se viene visualizzato un messaggio di errore o qualcosa non funziona come eseguono le esercitazioni, assicurarsi di controllare la pagina sulla risoluzione dei problemi.

## <a name="comments-welcome"></a>Benvenuti i commenti

Sono Benvenuti i commenti sulle esercitazioni e quando viene aggiornata l'esercitazione è viene compiuto ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti

Questa esercitazione è stata scritta per i prodotti seguenti:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio Express 2012 per Web con [l'aggiornamento più recente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

È possibile seguire l'esercitazione usando Visual Studio 2010 SP1 o Visual Studio 2013, ma alcune schermate saranno diverse e alcune funzionalità saranno diversi.

Se si usa Visual Studio 2013, installare [Azure SDK per Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Se si usa Visual Studio 2010 SP1, installare il software seguente:

- [Azure SDK per Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

A seconda del numero di dipendenze da SDK è già nel computer, installare Azure SDK può richiedere tempi lunghi, da alcuni minuti ad almeno mezz'ora. Anche se si prevede di pubblicare in un provider di hosting di terze parti anziché in Azure, le funzionalità di pubblicazione perché il SDK include gli aggiornamenti più recenti sul web di Visual Studio, è necessario Azure SDK.

> [!NOTE]
> Questa esercitazione è stata scritta con la versione 1.8.1 del SDK di Azure. Da allora sono state rilasciate le versioni più recenti con funzionalità aggiuntive. Le esercitazioni sono state aggiornate per citare queste funzionalità e i collegamenti alle risorse con le altre relative informazioni.


Le istruzioni e schermate sono basate su Windows 8, ma le esercitazioni illustrano le differenze per Windows 7.

Altro software è necessario per completare l'esercitazione, ma non è necessario disporre di che è ancora stato installato. L'esercitazione illustrerà i passaggi per l'installazione quando ti serve.

## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

L'applicazione che verrà distribuita è denominato Contoso University ed è già stato creato automaticamente. Si tratta di una versione semplificata di un sito web university, in base ad accoppiamento l'applicazione Contoso University descritta nel [esercitazioni di Entity Framework sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare il [applicazione web di Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Il *zip* file contiene più versioni del progetto. Per eseguire i passaggi dell'esercitazione, iniziare con il progetto che si trova nella cartella di c#. Per visualizzare il progetto come appare alla fine delle esercitazioni, aprire il progetto nella cartella ContosoUniversity-End.

Per preparare il progetto per eseguire i passaggi dell'esercitazione, eseguire i passaggi seguenti:

1. Salvare i file della soluzione ContosoUniversity dalla cartella c# in una cartella denominata ContosoUniversity in qualsiasi cartella utilizzata per l'utilizzo di progetti di Visual Studio.

    Per impostazione predefinita, questa è la cartella seguente per Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Per gli screenshot in questa esercitazione, la cartella di progetto si trova nella directory radice nel `C`: unità.)
2. Avviare Visual Studio e aprire il progetto.
3. Nelle **Esplora soluzioni**, fare doppio clic la soluzione e fare clic su **ripristino dei pacchetti EnableNuGet**.
4. Compilare la soluzione.
5. Se si verificano errori di compilazione, è necessario ripristinare manualmente i pacchetti NuGet:

    1. Nelle **Esplora soluzioni**, fare doppio clic la soluzione e quindi fare clic su **Gestisci pacchetti NuGet per la soluzione**.
    2. Nella parte superiore del **Gestisci pacchetti NuGet** finestra di dialogo si vedrà **pacchetti NuGet alcune non sono presenti da questa soluzione. Fare clic per ripristinare.** Scegliere il **ripristinare** pulsante.
    3. Ricompilare la soluzione.
6. Premere CTRL+F5 per eseguire l'applicazione.

    L'applicazione viene aperto alla home page di Contoso University.

    ![Home Page Dev](introduction/_static/image1.png)

    (Potrebbe esserci un tempo di attesa mentre Visual Studio avvia l'istanza di SQL Server Express LocalDB e si verifichi un errore di timeout se che processo impiega troppo tempo. In questo caso solo avviare il progetto nuovamente.)

Pagine del sito Web sono accessibili dalla barra dei menu e consentono di eseguire le attività seguenti:

- Visualizzare le statistiche degli studenti (la pagina About).
- Visualizzare, modificare, eliminare e aggiungere gli studenti.
- Visualizzare e modificare i corsi.
- Visualizzare e modificare instructors (insegnanti).
- Visualizzare e modificare i reparti.

Di seguito sono catture di schermata di poche pagine rappresentative.

![Pagina di studenti sviluppatori](introduction/_static/image2.png)

![Aggiungere gli studenti sviluppatori di pagina](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Esaminare le funzionalità dell'applicazione che influiscono sulla distribuzione

Le seguenti funzionalità dell'applicazione influiscono sul modo in cui si distribuisce o ciò che è necessario effettuare per eseguire la distribuzione. Ognuno di questi è illustrata in dettaglio le esercitazioni seguenti mostrano la serie.

- Contoso University Usa un database di SQL Server per archiviare i dati dell'applicazione, ad esempio i nomi di student e instructor. Il database contiene una combinazione di dati di test e i dati di produzione e quando si distribuisce in produzione è necessario escludere i dati di test.
- L'applicazione usa il sistema di appartenenze ASP.NET, che archivia informazioni sull'account utente in un database di SQL Server. L'applicazione definisce un utente amministratore chi può accedere a informazioni riservate. È necessario distribuire il database di appartenenza senza account di test, ma con un account amministratore.
- L'applicazione usa un errore di terze parti, la registrazione e l'utilità di creazione report. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione errore scrive le informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account che viene eseguito ASP.NET nel sito distribuito disponga dell'autorizzazione di scrittura a questa cartella è necessario escludere tale cartella dalla distribuzione. (In caso contrario, è possibile distribuire i dati di log di errore dall'ambiente di testing nell'ambiente di produzione e/o file di log degli errori di produzione potrebbero essere eliminati.)
- L'applicazione include alcune impostazioni che devono essere modificate in distribuito *Web. config* file in base all'ambiente di destinazione (test, gestione temporanea o produzione) e altre impostazioni che devono essere modificate in base alla compilazione configurazione (Debug o rilascio).
- La soluzione di Visual Studio include un progetto di libreria di classi. Solo l'assembly che genera il progetto deve essere distribuita, non il progetto stesso.

## <a name="summary"></a>Riepilogo

In questa prima esercitazione della serie, è stato scaricato il progetto di Visual Studio di esempio ed esaminato le funzionalità del sito che influiscono sul modo in cui si distribuisce l'applicazione. Nelle esercitazioni seguenti, Prepara per la distribuzione impostando alcune di queste azioni devono essere gestiti automaticamente. Gli altri occuperà di manualmente.

> [!div class="step-by-step"]
> [avanti](preparing-databases.md)
