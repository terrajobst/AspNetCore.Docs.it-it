---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio: Introduzione - 1 di 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9dacafaacdab12b8005cb6073647ae526cefcfb4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831582"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio: Introduzione - 1 di 12
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web. Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010.
> 
> Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e Mostra come distribuire in App Web di servizio App di Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Queste esercitazioni illustrano in dettaglio la distribuzione prima di tutto IIS nei computer di sviluppo locale per il test, quindi su un provider di hosting di terze parti. L'applicazione che verrà distribuita Usa un database dell'applicazione e un database di appartenenze ASP.NET. Iniziare usando SQL Server Compact e distribuzione in SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche del database e come eseguire la migrazione a SQL Server.
> 
> Le esercitazioni presuppongono che una conoscenza di come usare ASP.NET core in Visual Studio. In caso contrario, un buon punto di partenza è una [esercitazione di base ASP.NET Web Forms](../tailspin-spyworks/tailspin-spyworks-part-1.md) o una [esercitazione di base ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum sulla distribuzione di ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Panoramica

Queste esercitazioni illustrano in dettaglio la distribuzione prima di tutto IIS nei computer di sviluppo locale per il test, quindi su un provider di hosting di terze parti. L'applicazione che verrà distribuita Usa un database dell'applicazione e un database di appartenenze ASP.NET. Iniziare usando SQL Server Compact e distribuzione in SQL Server Compact e le esercitazioni successive illustrano come distribuire le modifiche del database e come eseguire la migrazione a SQL Server.

Il numero di esercitazioni – 11 in tutte le oltre a una pagina sulla risoluzione dei problemi, potrebbe rendere il processo di distribuzione potrebbe sembrare complicata. In effetti, le procedure di base per la distribuzione di un sito costituiscono una parte relativamente piccola del set di esercitazioni. Tuttavia, in situazioni reali, è spesso necessario informazioni su alcuni aspetti aggiuntivi piccola ma importante della distribuzione, ad esempio, impostando le autorizzazioni della cartella nel server di destinazione. Sono incluse molte di queste tecniche aggiuntive nelle esercitazioni, con la speranza che non lasciano le esercitazioni di informazioni che potrebbero impedire il correttamente la distribuzione di un'applicazione reale.

Le esercitazioni sono progettate per eseguire in sequenza e ogni parte è basata sulla parte precedente. Tuttavia, è possibile ignorare parti che non sono pertinenti allo specifico scenario. (Ignorare parti potrebbe essere necessario regolare le procedure descritte nelle esercitazioni successive.)

## <a name="intended-audience"></a>Destinatari

Le esercitazioni sono destinate agli sviluppatori ASP.NET che lavorano in organizzazioni di piccole dimensioni o in altri ambienti in cui:

- Non viene utilizzato un processo di integrazione continua (compilazioni automatizzate e distribuzione).
- L'ambiente di produzione è un provider di hosting di terze parti.
- Una persona assume in genere più ruoli (la stessa persona sviluppa, testa e distribuisce).

Negli ambienti aziendali, è più comune per implementare i processi di integrazione continua e l'ambiente di produzione è in genere ospitata dai server dell'azienda. Persone diverse in genere anche svolgono ruoli diversi. Per informazioni sulla distribuzione aziendale, vedere [distribuzione di applicazioni Web in scenari aziendali](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Le organizzazioni di tutte le dimensioni possono inoltre distribuire applicazioni web in Azure e la maggior parte delle procedure illustrate in queste esercitazioni si applicano anche alle App Web di servizi App di Azure. Per un'introduzione ad Azure, vedere [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Il Provider di Hosting illustrato nelle esercitazioni

Le esercitazioni descrivono il processo di configurazione di un account con una società di hosting e distribuzione dell'applicazione per tale provider di hosting. Una società di hosting specifica è stato scelto in modo che le esercitazioni è stato possibile illustrare un'esperienza completa di distribuzione in un sito Web in tempo reale. Ogni società di hosting fornisce diverse funzionalità e l'esperienza di distribuzione dei server varia alquanto; Tuttavia, la procedura descritta in questa esercitazione è tipica per il processo complessivo.

Il provider di hosting utilizzato per questa esercitazione, Cytanium.com, è uno dei numerosi disponibili e l'utilizzo in questa esercitazione non costituisce un vuole raccomandare né consigliare.

## <a name="deploying-web-site-projects"></a>Distribuzione di progetti di siti Web

Contoso University è un progetto di applicazione web di Visual Studio. La maggior parte dei metodi di distribuzione e gli strumenti descritti in questa esercitazione non si applicano ai [progetti di siti Web](https://msdn.microsoft.com/library/dd547590.aspx). Per informazioni su come distribuire progetti di siti web, vedere [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Distribuzione di progetti ASP.NET MVC

Per questa esercitazione si distribuisce un progetto di Web Form ASP.NET, ma tutto ciò che descrive come eseguire questa operazione è anche applicabile a ASP.NET MVC. Un progetto MVC di Visual Studio è semplicemente un altro tipo di progetto di applicazione web. L'unica differenza è che se si distribuisce in un provider di hosting che supportano ASP.NET MVC o della versione di destinazione di esso, è necessario assicurarsi di aver installato appropriato ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) Il pacchetto NuGet nel progetto.

## <a name="programming-language"></a>Linguaggio di programmazione

L'applicazione di esempio Usa c#, ma le esercitazioni non richiedono la conoscenza del linguaggio c# e le tecniche di distribuzione illustrate per le esercitazioni non sono specifiche della lingua.

## <a name="troubleshooting-during-this-tutorial"></a>Risoluzione dei problemi durante l'esercitazione

Quando si verifica un errore durante la distribuzione o se il sito distribuito non viene eseguito correttamente, i messaggi di errore non offrono una soluzione. Per semplificare alcuni scenari comuni di problemi, un [risoluzione dei problemi di pagina di riferimento](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) è disponibile. Se viene visualizzato un messaggio di errore o qualcosa non funziona come eseguono le esercitazioni, assicurarsi di controllare la pagina sulla risoluzione dei problemi.

## <a name="comments-welcome"></a>Benvenuti i commenti

Sono Benvenuti i commenti sulle esercitazioni e quando viene aggiornata l'esercitazione è viene compiuto ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di avere Windows 7 o versione successiva e uno dei seguenti prodotti installati nel computer in uso:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Se si dispone di Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, installare anche i prodotti seguenti:

- [Azure SDK per .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (include l'aggiornamento della pubblicazione sul Web)
- [Microsoft Visual Studio 2010 SP1 Tools per SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Altro software è necessario per completare l'esercitazione, ma non è necessario avere che ancora caricata. L'esercitazione illustrerà i passaggi per l'installazione quando ti serve.

## <a name="downloading-the-sample-application"></a>Scaricare l'applicazione di esempio

L'applicazione che verrà distribuita è denominato Contoso University ed è già stato creato automaticamente. Si tratta di una versione semplificata di un sito web university, in base ad accoppiamento l'applicazione Contoso University descritta nel [esercitazioni di Entity Framework sul sito ASP.NET](https://asp.net/entity-framework/tutorials).

Dopo aver installato i prerequisiti, scaricare il [applicazione web di Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Il *zip* file contiene più versioni del progetto e un file PDF che contiene tutte le esercitazioni 12 su. Per eseguire i passaggi dell'esercitazione, iniziare con ContosoUniversity Begin. Per visualizzare il progetto come appare alla fine delle esercitazioni, aprire ContosoUniversity-End. Per visualizzare il progetto come appare prima della migrazione a SQL Server completo nell'esercitazione 10, aprire ContosoUniversity AfterTutorial09.

Per preparare eseguire i passaggi dell'esercitazione, salvare ContosoUniversity Begin in qualsiasi cartella utilizzata per l'utilizzo di progetti di Visual Studio. Per impostazione predefinita, questa è la cartella seguente:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Per gli screenshot in questa esercitazione, la cartella di progetto si trova nella directory radice nel `C`: unità.)

Avviare Visual Studio, aprire il progetto e premere CTRL+F5 per eseguirla.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Pagine del sito Web sono accessibili dalla barra dei menu e consentono di eseguire le attività seguenti:

- Visualizzare le statistiche degli studenti (la pagina About).
- Visualizzare, modificare, eliminare e aggiungere gli studenti.
- Visualizzare e modificare i corsi.
- Visualizzare e modificare instructors (insegnanti).
- Visualizzare e modificare i reparti.

Di seguito sono catture di schermata di poche pagine rappresentative.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Esaminare le funzionalità dell'applicazione che influiscono sulla distribuzione

Le seguenti funzionalità dell'applicazione influiscono sul modo in cui si distribuisce o ciò che è necessario effettuare per eseguire la distribuzione. Ognuno di questi è illustrata in dettaglio le esercitazioni seguenti mostrano la serie.

- Contoso University Usa un database di SQL Server Compact per archiviare i dati dell'applicazione, ad esempio i nomi di student e instructor. Il database contiene una combinazione di dati di test e i dati di produzione e quando si distribuisce in produzione è necessario escludere i dati di test. In un secondo momento nella serie di esercitazioni sarà eseguire la migrazione da SQL Server Compact a SQL Server.
- L'applicazione usa il sistema di appartenenze ASP.NET, che archivia informazioni sull'account utente in un database di SQL Server Compact. L'applicazione definisce un utente amministratore chi può accedere a informazioni riservate. È necessario distribuire il database di appartenenza senza account di test, ma con un account amministratore.
- Poiché il database dell'applicazione e il database di appartenenza usare SQL Server Compact come il motore di database, è necessario distribuire il motore di database per il provider di hosting, nonché database stessi.
- L'applicazione Usa provider di appartenenze universal ASP.NET in modo che il sistema di appartenenze può archiviare i dati in un database SQL Server Compact. L'assembly che contiene i provider di appartenenze universale deve essere distribuito con l'applicazione.
- L'applicazione usa Entity Framework 5.0 per accedere ai dati nel database dell'applicazione. L'assembly contenente Entity Framework 5.0 deve essere distribuito con l'applicazione.
- L'applicazione usa un errore di terze parti, la registrazione e l'utilità di creazione report. Questa utilità viene fornita in un assembly che deve essere distribuito con l'applicazione.
- L'utilità di registrazione errore scrive le informazioni sugli errori nei file XML in una cartella di file. È necessario assicurarsi che l'account che viene eseguito ASP.NET nel sito distribuito disponga dell'autorizzazione di scrittura a questa cartella è necessario escludere tale cartella dalla distribuzione. (In caso contrario, è possibile distribuire i dati di log di errore dall'ambiente di testing nell'ambiente di produzione e/o file di log degli errori di produzione potrebbero essere eliminati.)
- L'applicazione include alcune impostazioni che devono essere modificate in distribuito *Web. config* file in base all'ambiente di destinazione (test o produzione) e altre impostazioni che devono essere modificate in base alla compilazione configurazione (Debug o rilascio).
- La soluzione di Visual Studio include un progetto di libreria di classi. Solo l'assembly che genera il progetto deve essere distribuita, non il progetto stesso.

In questa prima esercitazione della serie, è stato scaricato il progetto di Visual Studio di esempio ed esaminato le funzionalità del sito che influiscono sul modo in cui si distribuisce l'applicazione. Nelle esercitazioni seguenti, Prepara per la distribuzione impostando alcune di queste azioni devono essere gestiti automaticamente. Gli altri occuperà di manualmente.

> [!div class="step-by-step"]
> [avanti](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
