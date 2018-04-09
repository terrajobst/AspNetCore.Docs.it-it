---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creazione di applicazioni Cloud del mondo reale con Azure | Documenti Microsoft
author: MikeWasson
description: Questo manuale e viene illustrato un approccio basato su modelli per la creazione di soluzioni cloud del mondo reale. I modelli si applicano al processo di sviluppo anche come un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>Creazione di applicazioni Cloud del mondo reale con Azure
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [scaricare E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Questo manuale e viene illustrato un approccio basato su modelli per la creazione di soluzioni cloud del mondo reale. I modelli si applicano al processo di sviluppo, nonché all'architettura e procedure di codifica.
> 
> Il contenuto è basato su una presentazione sviluppato da Scott Guthrie e recapitati da quest'ultimo in norvegese sviluppatori conferenza (NDC) nel mese di giugno 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) e in Microsoft Tech Ed Australia Settembre 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Molti altri](more-patterns-and-guidance.md#acknowledgments) aggiornato e aumentati in modo il contenuto durante la transizione da video al modulo è stato scritto.


## <a name="intended-audience"></a>Destinatari

Gli sviluppatori che sono conoscere lo sviluppo per il cloud, prendere in considerazione uno spostamento nel cloud o nuova nel cloud di sviluppo troverà qui una breve introduzione dei concetti più importanti e delle procedure che devono conoscere. I concetti sono illustrati con esempi concreti e ogni capitolo sono collegate ad altre risorse per informazioni più dettagliate. Gli esempi e i collegamenti a risorse aggiuntive sono per Framework Microsoft e i servizi, ma i principi illustrati si applicano a altri framework di sviluppo web e anche l'ambiente cloud.

Gli sviluppatori che stanno sviluppando già per il cloud è possono trovare qui idee che consentiranno di rendono più efficace. Ogni capitolo della serie può essere letto in modo indipendente, pertanto è possibile selezionare e scegliere argomenti in cui si sono interessati.

Tutti gli utenti che hanno guardato di Scott Guthrie *predefiniti reale World Cloud App con Azure* presentazione e si desiderano ulteriori dettagli e informazioni aggiornate troverà che qui.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Modelli di sviluppo cloud

Questo e-book indica che tredici consigliate modelli per lo sviluppo cloud. "Modello" viene usato in una prospettiva ampliata per indicare una modalità consigliata per eseguire operazioni: ottimale per lo sviluppo, la progettazione e codifica di applicazioni basate su cloud. Questi sono i principali pattern che consentono di "rientrano in pit di successo" Se si seguono tali.

- [Automatizzare tutti gli elementi](automate-everything.md).

    - Utilizzare script per ottimizzare l'efficienza e ridurre al minimo gli errori nei processi ricorrenti.
    - Demo: Script di gestione di Azure.
- [Controllo del codice sorgente](source-control.md). 

    - Impostare la struttura con rami nel controllo del codice sorgente per facilitare DevOps del flusso di lavoro.
    - Demo: aggiungere script al controllo del codice sorgente.
    - Demo: mantenere i dati sensibili fuori controllo del codice sorgente.
    - Demo: usare Git in Visual Studio.
- [Integrazione continua e il recapito](continuous-integration-and-continuous-delivery.md). 

    - Automatizzare la compilazione e distribuzione con ogni check-in controllo di origine.
- [Procedure guidate di sviluppo Web](web-development-best-practices.md). 

    - Mantieni livello web senza stato.
    - Demo: ridimensionamento e il ridimensionamento automatico in App Web nel servizio App di Azure.
    - Evitare lo stato della sessione.
    - Utilizzare una rete CDN con fallback quando la rete CDN non è disponibile.
    - Utilizzare il modello di programmazione asincrono.
    - Demo: asincrono in ASP.NET MVC ed Entity Framework.
- [L'accesso Single sign-on](single-sign-on.md). 

    - Introduzione ad Azure Active Directory.
    - Demo: creare un'applicazione ASP.NET che usa Azure Active Directory.
- [Opzioni di archiviazione dati](data-storage-options.md). 

    - Tipi di archivi dati.
    - Come scegliere l'archivio dati di destra.
    - Demo: Database SQL di Azure.
- [Strategie di partizionamento dei dati](data-partitioning-strategies.md). 

    - Partizionare i dati in verticale, orizzontale o entrambi per semplificare il ridimensionamento di un database relazionale.
- [Archiviazione blob non strutturati](unstructured-blob-storage.md). 

    - Archiviare i file nel cloud tramite il servizio blob.
    - Demo: nell'app Correggi utilizzo dell'archiviazione blob.
- [Progettazione di restare attiva quando errori](design-to-survive-failures.md). 

    - Tipi di errori.
    - Ambito di un errore.
    - Informazioni sui contratti di servizio.
- [Monitoraggio e telemetria](monitoring-and-telemetry.md). 

    - Perché è necessario acquistare un'app di telemetria sia scrivere codice personalizzato per instrumentare l'app.
    - Demo: New Relic per Azure
    - Demo: la registrazione del codice nell'app Correggi.
    - Demo: inserimento di dipendenze nell'app Correggi.
    - Demo: supporto di registrazione incorporata in Azure.
- [Gestione degli errori temporanei](transient-fault-handling.md). 

    - Utilizzare logica di tentativi/Backoff intelligente per ridurre l'effetto di errori temporanei.
    - Demo: tentativi/Backoff in Entity Framework 6.
- [Memorizzazione nella cache distribuita](distributed-caching.md). 

    - Migliorare la scalabilità e ridurre i costi di transazione di database tramite la memorizzazione nella cache distribuita.
- [Modello basato su coda lavoro](queue-centric-work-pattern.md). 

    - Abilitare la disponibilità elevata e migliorare la scalabilità accoppiando regime di controllo libero livelli web e di lavoro.
    - Demo: Code di archiviazione di Azure nell'app Correggi.
- [Più cloud app modelli e le indicazioni](more-patterns-and-guidance.md).
- [Appendice: applicazione di esempio Fix It](the-fix-it-sample-application.md)

    - Problemi noti
    - Suggerimenti
    - Come scaricare, compilare, eseguire e distribuire.

Questi modelli si applicano a tutti gli ambienti di cloud, ma li illustrare con esempi in base alle tecnologie e servizi, ad esempio Visual Studio Team Foundation Service, ASP.NET e Azure.

La parte restante di questo capitolo viene Correggi l'applicazione di esempio e le app Web nell'ambiente cloud di Azure App Service che viene eseguita l'app Correggi in.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Per risolvere il problema applicazione di esempio

La maggior parte degli esempi di codice illustrati in questa Guida e le catture di schermata si basano originariamente sviluppati da app Correggi [Scott Guthrie](https://weblogs.asp.net/scottgu/) per illustrare i modelli di sviluppo di app cloud consigliato e procedure consigliate.

![Correggerlo home page app](introduction/_static/image1.png)

L'applicazione di esempio è un elemento di lavoro semplice sistema di registrazione. Quando è necessario un valore fisso, creare un ticket e assegnare un utente e altri tipi può accedere e visualizzare il ticket assegnato a essi e contrassegnare il ticket come completato al completamento.

Si tratta di un progetto web di Visual Studio standard. È basato su ASP.NET MVC e utilizza un database di SQL Server. È possibile eseguire in locale in IIS Express e può essere distribuito a un sito Web di Azure per l'esecuzione nel cloud. È possibile accedere utilizzando l'autenticazione basata su form e un database locale o tramite un provider di social networking, ad esempio Google. (In un secondo momento si verrà inoltre illustrato come accedere con un account aziendale di Active Directory.)

![Pagina di accesso](introduction/_static/image2.png)

Dopo aver eseguito in è possibile creare un ticket, assegnarla a un utente e una foto di ciò che si desidera risolvere il problema.

![Creare un'attività di correzione](introduction/_static/image3.png)

![Risolvere il problema di attività creata](introduction/_static/image4.png)

È possibile tenere traccia dell'avanzamento di elementi di lavoro che è stato creato, vedere il ticket assegnato a, visualizzare i dettagli di ticket e contrassegnare elementi come completata.

Si tratta di un'applicazione molto semplice da una prospettiva di funzionalità, ma si noterà come compilarlo in modo che è possibile applicare la scalabilità a milioni di utenti e sarà resistente agli elementi quali errori di database e interruzioni di connessione. Si noterà inoltre come creare un flusso di lavoro di sviluppo agile e automatizzati, che consente di avviare semplice e rendere l'app sempre migliori eseguendo l'iterazione del ciclo di sviluppo in modo efficace e rapido.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>App Web nel servizio App di Azure

L'ambiente cloud utilizzato per l'applicazione Correggi è un servizio di Azure che abbiamo chiamato siti Web. Questo servizio è un modo che è possibile ospitare l'app web in Azure senza dover creare macchine virtuali e mantenerli aggiornati, installare e configurare IIS e così via. È host del sito nel nostro macchine virtuali e fornire automaticamente il backup e ripristino e altri servizi per l'utente. Il servizio siti Web funziona con ASP.NET, Node.js, PHP e Python. Consente di distribuire rapidamente usando Visual Studio, distribuzione Web, FTP, Git o TFS. È in genere solo pochi secondi tra l'avvio di una distribuzione e l'ora in cui che l'aggiornamento è disponibile su Internet. È tutto gratuito iniziare, ed è possibile scalare in verticale man mano che aumenta il traffico.

Dietro le quinte, App Web nel servizio App di Azure offre numerose funzionalità che è necessario compilare manualmente se si prevede di ospitare un sito web utilizzando IIS nelle proprie macchine virtuali e i componenti dell'architettura. Un componente è un punto di distribuzione finale che consente di configurare IIS e installa l'applicazione in tanti macchine virtuali che si desidera eseguire il sito in automaticamente.

![Servizio di distribuzione](introduction/_static/image5.png)

Quando un utente raggiunge il sito web, non accedono direttamente le macchine virtuali di IIS, passano [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) bilanciamenti del carico. È possibile utilizzarle con i propri server, ma il vantaggio è che si è configurati automaticamente. Usano un approccio euristico intelligente che prende in considerazione fattori quali l'affinità di sessione, profondità della coda in IIS, e l'utilizzo della CPU in ogni computer per il traffico diretto verso le macchine virtuali che ospitano il sito web.

![Servizio di bilanciamento del carico ARR](introduction/_static/image6.png)

Se si arresta una macchina, Azure automaticamente effettua il pull dalla rotazione, creata una nuova istanza della macchina virtuale e inizia a indirizzare il traffico alla nuova istanza, tutti con nessun tempi di inattività per l'applicazione.

![Recupero automatico da errori di computer](introduction/_static/image7.png)

Tutto ciò avviene automaticamente. È necessario eseguire è creare un sito web e distribuire l'applicazione, mediante Windows PowerShell, Visual Studio o il portale di gestione di Azure.

Per una semplice e rapida esercitazione dettagliata che illustra come creare un'applicazione web in Visual Studio e distribuirlo a un sito Web di Azure, vedere [Introduzione a Azure e ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Riepilogo

In questa introduzione ha fornito un elenco di argomenti che di book coprirà, le schermate dell'applicazione di esempio e una breve panoramica delle App Web in un ambiente cloud di Azure App Service. Uno dei principali vantaggi di sviluppo di App e per il cloud è che risulti semplice automatizzare le attività di sviluppo ripetitive, ad esempio la creazione di un ambiente di test e distribuzione del codice a esso. Come eseguire questa operazione l'oggetto del [capitolo successivo](automate-everything.md).

## <a name="resources"></a>Risorse

Per ulteriori informazioni sugli argomenti trattati in questo capitolo, vedere le risorse seguenti.

Documentazione:

- [Le app nel servizio App di Azure Web](https://azure.microsoft.com/services/app-service/web/). Pagina del portale per la documentazione sulle App Web di Azure.
- [App Web, servizi Cloud e le macchine virtuali: situazioni in cui utilizzare?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, come illustrato in questo capitolo è solo uno dei tre modi diversi, è possibile eseguire le applicazioni web in Azure. In questo articolo vengono illustrate le differenze tra i tre modi e vengono fornite istruzioni su come scegliere quello più adatto per lo scenario. Ad esempio siti Web, servizi Cloud è una funzionalità PaaS di Azure. Le macchine virtuali sono una funzionalità di IaaS. Per una spiegazione di PaaS e IaaS, vedere il [opzioni dati](data-storage-options.md#paasiaas) capitolo.

Video

- [Scott Guthrie inizia in corrispondenza di passaggio 0 - che cos'è il sistema operativo Cloud di Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architettura dei siti Web - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Elementi interni di siti Web di Azure con Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [avanti](automate-everything.md)
