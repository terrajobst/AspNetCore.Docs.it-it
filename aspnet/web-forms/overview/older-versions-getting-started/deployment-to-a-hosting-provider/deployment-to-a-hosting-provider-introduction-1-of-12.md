---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio: Introduzione - 1 di 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3f1572bb890ee136cdd746040a5efae2ce537116
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio: Introduzione - 1 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010.
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire l'App Web di servizio App di Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Queste esercitazioni consentono di eseguire la distribuzione prima di IIS nel computer di sviluppo locale per il test, quindi su un provider di hosting di terze parti. L'applicazione che verrà distribuito utilizza un database dell'applicazione e un database delle appartenenze ASP.NET. È iniziare utilizzando SQL Server Compact e la distribuzione di SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche del database e come eseguire la migrazione a SQL Server.
> 
> Le esercitazioni presuppongono che una conoscenza di come utilizzare ASP.NET in Visual Studio. In caso contrario, un buon punto di partenza è un [esercitazione di base ASP.NET Web Forms](../tailspin-spyworks/tailspin-spyworks-part-1.md) o [esercitazione di base ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum sulla distribuzione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Panoramica

Queste esercitazioni consentono di eseguire la distribuzione prima di IIS nel computer di sviluppo locale per il test, quindi su un provider di hosting di terze parti. L'applicazione che verrà distribuito utilizza un database dell'applicazione e un database delle appartenenze ASP.NET. È iniziare utilizzando SQL Server Compact e la distribuzione di SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche del database e come eseguire la migrazione a SQL Server.

Il numero delle esercitazioni – 11 in tutte e una pagina sulla risoluzione dei problemi, potrebbe rendere il processo di distribuzione risultare molto complessa. In effetti, le procedure di base per la distribuzione di un sito costituiscono una parte del set di esercitazione relativamente piccola. Tuttavia, in situazioni reali, è spesso necessario informazioni su alcuni aspetti aggiuntivi piccola ma importante della distribuzione, ad esempio, impostando le autorizzazioni di cartella nel server di destinazione. Sono incluse molte di queste tecniche aggiuntive nelle esercitazioni, senza la possibilità che le esercitazioni, è necessario compilare le informazioni che potrebbero impedire correttamente la distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per l'esecuzione in sequenza e ogni parte genera nella parte precedente. Tuttavia, è possibile ignorare parti che non sono pertinenti alla propria situazione. (Ignorare parti potrebbe essere necessario regolare le procedure descritte nelle esercitazioni successive.)

## <a name="intended-audience"></a>Destinatari

Le esercitazioni sono rivolte agli sviluppatori ASP.NET che lavorano in organizzazioni di piccole dimensioni o in altri ambienti in cui:

- Non viene utilizzato un processo di integrazione continua (compilazioni automatiche e distribuzione).
- L'ambiente di produzione è un provider di hosting di terze parti.
- Una persona assume in genere più ruoli (la stessa persona sviluppa, verifica e distribuisce).

In ambienti aziendali, di solito per implementare i processi di integrazione continua e ambiente di produzione è ospitato in genere dai server dell'azienda. Utenti diversi anche in genere eseguono ruoli diversi. Per informazioni sulla distribuzione aziendale, vedere [distribuzione di applicazioni Web in scenari aziendali](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Le organizzazioni di tutte le dimensioni possono anche distribuire applicazioni web in Azure e la maggior parte delle procedure illustrate in queste esercitazioni si applicano anche nelle App Web di servizi di App di Azure. Per un'introduzione a Azure, vedere [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Il Provider di Hosting illustrato nelle esercitazioni

Le esercitazioni consentono di configurare un account con una società di hosting e distribuire l'applicazione a tale provider di hosting. Una società di hosting specifica è stato scelto in modo che le esercitazioni possano illustrare un'esperienza completa di distribuzione in un sito Web in tempo reale. Ogni società di hosting fornisce diverse funzionalità e l'esperienza di distribuzione per i server varia leggermente a; Tuttavia, il processo descritto in questa esercitazione è tipico per l'intero processo.

Il provider di hosting utilizzato per questa esercitazione, Cytanium.com, è uno dei numerosi bug che sono disponibili e il relativo utilizzo in questa esercitazione non costituisce una raccomandazione o verifica dell'autenticità.

## <a name="deploying-web-site-projects"></a>Distribuzione di progetti di sito Web

University Contoso è un progetto di applicazione web di Visual Studio. La maggior parte dei metodi di distribuzione e gli strumenti illustrati in questa esercitazione non si applicano a [progetti di sito Web](https://msdn.microsoft.com/library/dd547590.aspx). Per informazioni su come distribuire i progetti di siti web, vedere [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti ASP.NET MVC

Per questa esercitazione si distribuisce un progetto di Web Form ASP.NET, ma tutti gli elementi che informazioni su come eseguire è applicabili a ASP.NET MVC anche. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione web. L'unica differenza è che se si distribuisce in un provider di hosting che supportano ASP.NET MVC o la versione di destinazione, è necessario assicurarsi di avere installato appropriata ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) Il pacchetto NuGet nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio viene utilizzato c# ma le esercitazioni non richiedono conoscenze di c# e le tecniche di distribuzione illustrate per le esercitazioni non sono specifiche della lingua.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante l'esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non offrono una soluzione. Per aiutare con alcuni scenari comuni di problemi, un [risoluzione dei problemi di pagina di riferimento](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) è disponibile. Se viene visualizzato un messaggio di errore o non funzioni come eseguire le esercitazioni, assicurarsi di selezionare la pagina sulla risoluzione dei problemi.

## <a name="comments-welcome"></a>Pagina iniziale di commenti

I commenti sulle esercitazioni sono iniziale e quando viene aggiornata l'esercitazione verrà resa ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi che si dispone di Windows 7 o versioni successive e uno dei seguenti prodotti installati nel computer in uso:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Se si dispone di Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, installare anche i prodotti seguenti:

- [Azure SDK per .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (include l'aggiornamento di pubblicazione Web)
- [Microsoft Visual Studio 2010 SP1 Tools per SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Altro software è necessario per completare l'esercitazione, ma non è necessario disporre che ancora caricata. L'esercitazione verrà illustrati i passaggi per l'installazione quando necessario.

## <a name="downloading-the-sample-application"></a>Download dell'applicazione di esempio

L'applicazione che verrà distribuita è denominato Contoso University ed è già stato creato automaticamente. È una versione semplificata di un sito web university, in base ad accoppiamento l'applicazione Contoso University descritta nel [esercitazioni di Entity Framework sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare il [applicazione web di Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Il *zip* file contiene più versioni del progetto e un file PDF che contiene tutte le 12 esercitazioni. Per eseguire i passaggi dell'esercitazione, iniziare con ContosoUniversity Begin. Per visualizzare l'aspetto del progetto alla fine delle esercitazioni, aprire ContosoUniversity-End. Per visualizzare l'aspetto del progetto prima della migrazione di Server SQL completo nell'esercitazione 10, aprire ContosoUniversity AfterTutorial09.

Per preparare i passaggi dell'esercitazione, salvare ContosoUniversity Begin in qualsiasi cartella utilizzata per l'utilizzo di progetti di Visual Studio. Per impostazione predefinita è la seguente cartella:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Per le schermate in questa esercitazione, la cartella di progetto si trova nella directory radice nel `C`: unità.)

Avviare Visual Studio, aprire il progetto e premere CTRL + F5 per eseguirla.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Pagine dei siti Web sono accessibili dalla barra dei menu e consentono di eseguire le funzioni seguenti:

- Visualizzare le statistiche di student (la pagina di informazioni su).
- Visualizzare, modificare, eliminare e aggiungere gli studenti.
- Visualizzare e modificare i corsi.
- Visualizzare e modificare i docenti.
- Visualizzare e modificare i reparti.

Di seguito sono catture di schermata di poche pagine rappresentative.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Esaminare le funzionalità dell'applicazione che influiscono sulla distribuzione

Le seguenti funzionalità dell'applicazione influisce su come distribuire o ciò che è necessario eseguire per distribuirla. Ognuno di questi è illustrato più dettagliatamente nelle seguenti esercitazioni della serie.

- University Contoso Usa un database di SQL Server Compact per archiviare dati applicativi quali nomi student e instructor. Il database contiene una combinazione di dati di test e dati di produzione, e quando si distribuisce in produzione è necessario escludere i dati di test. Più avanti in una serie di esercitazioni sarà eseguire la migrazione da SQL Server Compact a SQL Server.
- L'applicazione utilizza il sistema di appartenenze ASP.NET, che archivia informazioni sull'account utente in un database di SQL Server Compact. L'applicazione definisce un utente amministratore che dispone dell'accesso a informazioni riservate. È necessario distribuire il database delle appartenenze senza account di prova con un account amministratore.
- Poiché il database dell'applicazione e il database delle appartenenze utilizza SQL Server Compact come il motore di database, è necessario distribuire il motore di database per il provider di hosting, nonché i database.
- L'applicazione utilizza il provider di appartenenze universale di ASP.NET in modo che il sistema di appartenenze può archiviare i dati in un database di SQL Server Compact. L'assembly che contiene i provider di appartenenze universale deve essere distribuito con l'applicazione.
- L'applicazione utilizza Entity Framework 5.0 per accedere ai dati nel database dell'applicazione. L'assembly che contiene Entity Framework 5.0 deve essere distribuito con l'applicazione.
- L'applicazione utilizza un errore di terze parti, registrazione e report utilità. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione errore scrive informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account che viene eseguito ASP.NET nel sito distribuito disponga dell'autorizzazione di scrittura in questa cartella e che è necessario escludere la cartella di distribuzione. (In caso contrario, potrebbero essere distribuiti errore dati di log dall'ambiente di testing nell'ambiente di produzione e/o file di log degli errori di produzione potrebbero essere eliminati.)
- L'applicazione include alcune impostazioni che devono essere modificate in distribuito *Web. config* file in base all'ambiente di destinazione (test o produzione) e altre impostazioni che devono essere modificate a seconda della compilazione configurazione (Debug o Release).
- La soluzione di Visual Studio include un progetto libreria di classi. Deve essere distribuito solo l'assembly che genera il progetto, non il progetto stesso.

In questa esercitazione prima della serie, aver scaricato il progetto di Visual Studio e rivedere le funzionalità del sito che influiscono sulla modalità di distribuzione dell'applicazione. Nelle esercitazioni seguenti, ci si prepara per la distribuzione mediante l'impostazione di alcuni di questi elementi devono essere gestiti automaticamente. Altri occuperà di manualmente.

> [!div class="step-by-step"]
> [avanti](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
