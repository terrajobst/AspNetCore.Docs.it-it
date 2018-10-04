---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creazione di App per Cloud funzionanti con Azure | Microsoft Docs
author: MikeWasson
description: Questo e-book descrive un approccio basato su modelli per la creazione di soluzioni cloud reali. I modelli si applicano al processo di sviluppo nonché come a un...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: c496e93d7517bc187514d5fa2dfa90d29c5f47f9
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576313"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Creazione di App per Cloud funzionanti con Azure
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Questo e-book descrive un approccio basato su modelli per la creazione di soluzioni cloud reali. I modelli si applicano al processo di distribuzione nonché all'architettura e alle procedure di codifica.
> 
> Il contenuto è basato su una presentazione sviluppato da Scott Guthrie e recapitati da quest'ultimo della Norwegian Developers Conference (NDC) nel giugno del 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e al Microsoft Tech Ed Australia Settembre 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Molti altri](more-patterns-and-guidance.md#acknowledgments) aggiornare ed ampliare i contenuti di passaggio da video a testo forma scritta.


## <a name="intended-audience"></a>Destinatari

Gli sviluppatori che sono curioso di scoprire sullo sviluppo per il cloud, valutando se passare al cloud o siano lo sviluppo troveranno qui una breve panoramica dei concetti più importanti e procedure consigliate che è necessario conoscere. I concetti sono accompagnati da esempi concreti e ogni capitolo sono collegate ad altre risorse per informazioni più dettagliate. Gli esempi e i collegamenti a risorse aggiuntive sono per Framework e servizi Microsoft, ma i concetti illustrati si applicano agli altri framework di sviluppo web e ambienti cloud.

Gli sviluppatori che stanno sviluppando già per il cloud possono risultare idee qui che consentono di rendono più successo. Ogni capitolo della serie può essere letto in modo indipendente, pertanto è possibile selezionare e scegliere argomenti in cui si sono interessati.

Tutti gli utenti che hanno guardato Guthrie *creazione Real World di App Cloud con Azure* presentazione e vuole ulteriori dettagli e informazioni aggiornate relative scopriranno che qui.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Modelli di sviluppo cloud

Questo e-book spiega che tredici consigliato modelli per lo sviluppo cloud. "Modello" viene usato qui in senso ampio per indicare un approccio consigliato per eseguire operazioni: ottimale per lo sviluppo, progettazione e codifica di App cloud. Questi sono i modelli principali che ti consentiranno "rientrano nel pit di operazione riuscita" Se si seguono tali.

- [Automatizzare tutto](automate-everything.md).

    - Usare gli script per massimizzare l'efficienza e ridurre al minimo gli errori nei processi ricorrenti.
    - Demo: Script gestione di Azure.
- [Controllo del codice sorgente](source-control.md). 

    - Configurare la struttura con rami nel controllo del codice sorgente per facilitare flusso di lavoro DevOps.
    - Demo: aggiungere gli script di controllo del codice sorgente.
    - Demo: mantenere i dati sensibili all'esterno di controllo del codice sorgente.
    - Demo: uso di Git in Visual Studio.
- [Integrazione continua e recapito](continuous-integration-and-continuous-delivery.md). 

    - Automatizzare la compilazione e distribuzione con ogni check-in controllo di origine.
- [Procedure guidate di sviluppo Web](web-development-best-practices.md). 

    - Mantieni livello web senza stato.
    - Demo: la scalabilità e la scalabilità automatica nell'App Web nel servizio App di Azure.
    - Evitare lo stato della sessione.
    - Usare una rete CDN con fallback quando non è disponibile la rete CDN.
    - Usare il modello di programmazione asincrono.
    - Demo: programmazione asincrona in ASP.NET MVC ed Entity Framework.
- [L'accesso Single sign-on](single-sign-on.md). 

    - Introduzione ad Azure Active Directory.
    - Demo: creare un'app ASP.NET che usa Azure Active Directory.
- [Opzioni di archiviazione dati](data-storage-options.md). 

    - Tipi di archivi dati.
    - Come scegliere l'archivio dati corretto.
    - Demo: Database SQL di Azure.
- [Strategie di partizionamento dei dati](data-partitioning-strategies.md). 

    - Partizionare i dati in verticale, orizzontale o entrambi per semplificare il ridimensionamento di un database relazionale.
- [Archivio blob non strutturati](unstructured-blob-storage.md). 

    - Store file nel cloud usando il servizio blob.
    - Demo: utilizzo dell'archiviazione blob nell'app Fix It.
- [Progettazione per sopravvivere a errori](design-to-survive-failures.md). 

    - Tipi di errori.
    - Ambito di un errore.
    - Informazioni sui contratti di servizio.
- [Monitoraggio e telemetria](monitoring-and-telemetry.md). 

    - Il motivo per cui occorre sia acquista un'app per i dati di telemetria e scrivere codice personalizzato per instrumentare l'app.
    - Demo: New Relic per Azure
    - Demo: la registrazione del codice nell'app Fix It.
    - Demo: inserimento di dipendenze nell'app Fix It.
    - Demo: supporto per la registrazione predefinita in Azure.
- [Gestione degli errori temporanei](transient-fault-handling.md). 

    - Utilizzare logica di ripetizione dei tentativi/Backoff intelligente per ridurre l'effetto di errori temporanei.
    - Demo: nuovo tentativo/Backoff in Entity Framework 6.
- [Memorizzazione nella cache distribuita](distributed-caching.md). 

    - Migliorare la scalabilità e ridurre i costi di transazione di database utilizzando la cache distribuita.
- [Modello di lavoro incentrato sulle code](queue-centric-work-pattern.md). 

    - Abilitare la disponibilità elevata e per migliorare la scalabilità di accoppiamento regime di controllo libero tra i livelli web e ruolo di lavoro.
    - Demo: Le code di archiviazione di Azure nell'app Fix It.
- [I modelli dell'app e informazioni aggiuntive più cloud](more-patterns-and-guidance.md).
- [Appendice: applicazione di esempio Fix It](the-fix-it-sample-application.md)

    - Problemi noti
    - Suggerimenti
    - Come scaricare, compilare, eseguire e distribuire.

Questi modelli si applicano a tutti gli ambienti cloud, ma verrà illustrata la loro usando esempi basati su tecnologie Microsoft e servizi, ad esempio Visual Studio, Team Foundation Service, ASP.NET e Azure.

Parte rimanente di questo capitolo introduce l'applicazione di esempio Fix It e le app Web nell'ambiente cloud di servizio App di Azure che esegue l'app Fix It in.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>La correzione applicazione di esempio

La maggior parte delle schermate e gli esempi di codice illustrati in questo e-book sono basata sull'originariamente sviluppato da app Fix It [Scott Guthrie](https://weblogs.asp.net/scottgu/) illustrare le procedure consigliate e modelli di sviluppo di app cloud consigliato.

![Correggere la home page dell'app](introduction/_static/image1.png)

L'app di esempio è un elemento di lavoro semplice sistema di gestione ticket. Quando è necessaria una correzione, creare un ticket e assegnare a un utente e così via può accedere e visualizzare i ticket assegnati ad essi e contrassegnare i ticket come completata quando il lavoro viene completato.

È un progetto web Visual Studio standard. È creato in ASP.NET MVC e Usa un database di SQL Server. È possibile eseguire in locale in IIS Express e può essere distribuito in un sito Web di Azure per l'esecuzione nel cloud. È possibile accedere usando l'autenticazione basata su form e un database locale o tramite un provider di social networking, ad esempio Google. (In un secondo momento si verrà anche illustrato come accedere con un account aziendale di Active Directory.)

![Pagina di accesso](introduction/_static/image2.png)

Una volta connessi in è possibile creare un ticket, assegnarla a un utente e caricare un'immagine di ciò che si desidera essere risolto.

![Creare un'attività Fix It](introduction/_static/image3.png)

![Risolvere il problema di creazione attività](introduction/_static/image4.png)

È possibile tenere traccia dell'avanzamento di elementi di lavoro che è stato creato, vedere i ticket assegnati all'utente, visualizzare i dettagli di ticket e contrassegnare elementi come completati.

Si tratta di un'app molto semplice dal punto di vista della funzionalità, ma si noterà come compilarlo in modo che è possibile applicare la scalabilità a milioni di utenti e sia resiliente a elementi quali gli errori di database e le interruzioni di connessione. Verrà anche illustrato come creare un flusso di lavoro di sviluppo agile e automatizzati, che consente di avviare semplice e rendere l'app migliori e migliori eseguendo l'iterazione del ciclo di sviluppo in modo rapido e in modo efficiente.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>App Web nel servizio App di Azure

L'ambiente cloud usato per l'applicazione Fix It è un servizio di Azure che abbiamo chiamato siti Web. Questo servizio è un modo che è possibile ospitare app web in Azure senza la necessità di creare macchine virtuali e Mantieniti aggiornato, installare e configurare IIS e così via. Si ospita il sito nelle VM e fornire automaticamente il backup e ripristino e altri servizi per l'utente. Il servizio siti Web funziona con ASP.NET, Node. js, PHP e Python. Permette di eseguire la distribuzione molto rapidamente usando Visual Studio, Web Deploy, FTP, Git o TFS. È in genere solo pochi secondi tra l'avvio di una distribuzione e l'ora che di aggiornamento è disponibile tramite Internet. È tutto gratuito iniziare a usare ed è possibile aumentare le prestazioni man mano che aumenta il traffico.

Dietro le quinte, App Web nel servizio App di Azure offre numerose funzionalità che sarebbe necessario per compilare manualmente se si prevede di ospitare un sito web mediante IIS nelle proprie macchine virtuali e i componenti dell'architettura. Un componente è un endpoint di distribuzione che consente di configurare IIS e installa l'applicazione su tutte le macchine virtuali che si desidera eseguire il tuo sito automaticamente.

![Servizi di distribuzione](introduction/_static/image5.png)

Quando un utente visita il sito web, non accedono direttamente le macchine virtuali di IIS, impegnate [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) bilanciamenti del carico. È possibile usare questi valori con i propri server, ma il vantaggio è che si sta automaticamente impostate automaticamente. Usano un'euristica intelligente che prenda in considerazione fattori quali l'affinità di sessione, profondità della coda in IIS, e utilizzo della CPU in ogni computer per indirizzare il traffico alle macchine virtuali che ospitano il sito web.

![Servizio di bilanciamento del carico ARR](introduction/_static/image6.png)

Se una macchina si arresta, Azure automaticamente esegue il pull dalla rotazione, attiva una nuova istanza di macchina virtuale e inizia a indirizzare il traffico alla nuova istanza, tutto senza alcun tempo di inattività per l'applicazione.

![Recupero automatico da errore della macchina](introduction/_static/image7.png)

Tutto ciò avviene automaticamente. È sufficiente eseguire è creare un sito web e distribuire l'applicazione, usando Windows PowerShell, Visual Studio o il portale di gestione di Azure.

Per una rapida e semplice esercitazione dettagliata che illustra come creare un'applicazione web in Visual Studio e distribuirla in un sito Web di Azure, vedere [Introduzione a Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Riepilogo

In questa introduzione ha fornito un elenco di argomenti che illustra il libro, le schermate dell'applicazione di esempio e una breve panoramica delle App Web nell'ambiente di servizio App di Azure basato su cloud. Uno dei principali vantaggi di sviluppo di App in e per il cloud è che risulti semplice automatizzare le attività di sviluppo ricorrenti come la creazione di un ambiente di test e distribuzione del codice a esso. Come eseguire questa operazione che rappresenta l'oggetto del [capitolo successivo](automate-everything.md).

## <a name="resources"></a>Risorse

Per altre informazioni sugli argomenti trattati in questo capitolo, vedere le risorse seguenti.

Documentazione:

- [App Web nel servizio App di Azure](https://azure.microsoft.com/services/app-service/web/). Pagina del portale per la documentazione sulle App Web di Azure.
- [Web App, servizi Cloud e macchine virtuali: quando usarli?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS come illustrato in questo capitolo è solo uno dei tre modi, è possibile eseguire le app web in Azure. Questo articolo illustra le differenze tra i tre modi e fornisce istruzioni su come scegliere quello più adatto per lo scenario. Ad esempio siti Web, servizi Cloud è una funzionalità PaaS di Azure. Le macchine virtuali sono una funzionalità di IaaS. Per una spiegazione dei servizi PaaS o IaaS, vedere la [opzioni di dati](data-storage-options.md#paasiaas) capitolo.

Video

- [Scott Guthrie inizia in corrispondenza di passaggio 0 - che cos'è il sistema operativo Cloud di Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architettura dei siti Web - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Elementi interni di siti Web di Azure con Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [avanti](automate-everything.md)
