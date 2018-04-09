---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.NET (VB) di opzioni di Hosting | Documenti Microsoft
author: rick-anderson
description: Le applicazioni web ASP.NET sono progettate in genere, creato e testati in un ambiente di sviluppo locale e devono essere distribuiti a una o ambiente di produzione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 0e99423ec803927d0f621c88f3d814578fec11f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-hosting-options-vb"></a>Opzioni di Hosting ASP.NET (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Le applicazioni web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale e sarà necessario distribuire un ambiente di produzione quando è pronta per il rilascio. In questa esercitazione viene fornita una panoramica del processo di distribuzione e funge da introduzione per questa serie di esercitazioni.


## <a name="introduction"></a>Introduzione

Applicazioni Web sono in genere progettate, create e testate in un ambiente di sviluppo che è accessibile solo per i programmatori che lavorano nel sito. Dopo l'applicazione è pronta per essere rilasciata, viene spostato in un ambiente di produzione in cui il sito è possibile accedere da qualsiasi utente su Internet. Questo processo di distribuzione introduce una serie di difficoltà:

- Un ambiente di produzione deve esistere ed essere correttamente il programma di installazione prima di poter distribuire un'applicazione ASP.NET; Inoltre, l'ambiente di produzione deve essere mantenuto aggiornato con le ultime patch di sicurezza.
- Il set corretto di file di markup, i file di codice e i file di supporto deve essere copiato dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni guidate dai dati, potrebbe essere necessario copiare lo schema del database e/o, i dati.
- Possono essere presenti differenze di configurazione tra i due ambienti. Server di database connessione stringa o di posta elettronica utilizzato nell'ambiente di sviluppo saranno probabilmente diversi rispetto all'ambiente di produzione. Inoltre, il comportamento dell'applicazione può variare a seconda dell'ambiente. Ad esempio, quando si verifica un errore in fase di sviluppo è possono visualizzare i dettagli dell'errore sullo schermo, ma quando si verifica un errore nell'ambiente di produzione, deve essere visualizzata una pagina di errore descrittivi e i dettagli dell'errore inviato tramite posta elettronica per gli sviluppatori.

Per evitare il primo problema - impostazione e gestione di un ambiente di produzione - outsourcing relativi ambienti di produzione per molti utenti singoli e aziende *provider di hosting web*. Un provider di hosting web è una società che gestisce l'ambiente di produzione per conto dell'utente. Esistono innumerevoli web provider host, ognuna con prezzi variabili e i livelli di servizio. vedere la sezione "Ricerca di un Provider di hosting Web" per suggerimenti su come individuare un provider di servizi.

Questo è il primo in una serie di esercitazioni che esamina i passaggi necessari per distribuire un'applicazione web ASP.NET in un ambiente di produzione gestito da un provider di hosting web. Nel corso di queste esercitazioni verranno presi in esame:

- I file che devono essere distribuite per il provider di hosting web.
- Strumenti per semplificare il processo di distribuzione.
- Come distribuire un database.
- Suggerimenti per la distribuzione di un database che utilizza [il provider di ruoli e appartenenza basata su SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), unitamente a modi per simulare lo strumento Amministrazione sito Web in un ambiente di produzione.
- Strategie per correttamente l'aggiornamento del database nell'ambiente di produzione con le modifiche apportate durante lo sviluppo.
- Tecniche per la registrazione degli errori che si verifica in produzione, nonché di notificare agli sviluppatori quando si verifica un errore.

Queste esercitazioni sono pensate per essere conciso e vengono fornite istruzioni dettagliate con una notevole quantità di schermate guidare il processo in modo visivo. In questa esercitazione primo viene fornita una panoramica del processo di distribuzione di ASP.NET e consigli sull'individuazione di un provider di hosting web. Iniziamo!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Una panoramica del processo di distribuzione di ASP.NET

In breve, la distribuzione di un'applicazione ASP.NET implica i tre passaggi seguenti:

1. Configurare l'applicazione web, un server web e un database nell'ambiente di produzione.
2. Sincronizzare le pagine ASP.NET, i file di codice, gli assembly di `Bin` cartelle e file di supporto HTML correlati come file CSS e JavaScript.
3. Eseguire la sincronizzazione dello schema del database e/o dati.

Le informazioni di configurazione per un'applicazione web in genere si trovano nella `Web.config` file e include le stringhe di connessione di database, i criteri di gestione degli errori, URL riscrittura regole e informazioni sul server di posta elettronica. Spesso queste informazioni sono diverse per un'applicazione in fase di sviluppo e la stessa applicazione nell'ambiente di produzione. Ad esempio, quando si sviluppa un'applicazione è consigliabile utilizzare un database di sviluppo in modo che non si sta testando nel database di produzione. Di conseguenza, le stringhe di connessione di database è in genere variano tra le applicazioni di sviluppo e produzione. A causa di tali differenze, parte della distribuzione prevede di apportare modifiche alle informazioni di configurazione dell'applicazione web.

Oltre alle modifiche di configurazione dell'applicazione web, passaggio 1 inoltre può comportare una configurazione per il server web e il database. Ad esempio, se una pagina ASP.NET crea o Elimina i file da una directory nel server web quindi il server web deve essere configurato per consentire le modifiche al sistema questi file. Analogamente, potrebbero essere presenti impostazioni di autenticazione o autorizzazione che devono essere apportate al database.


Passaggio 2 richiede la sincronizzazione di set di pagine ASP.NET essenziali e file di supporto tra gli ambienti di sviluppo e produzione. Il set specifico di ASP. I file correlati alla rete che devono essere sincronizzati tra i due ambienti dipende dal tipo di progetto creato in Visual Studio e la discussione nella prossima esercitazione, è  <em>[determinare quali file devono essere distribuiti](determining-what-files-need-to-be-deployed-vb.md)</em>. Il terza e quarta esercitazioni -  <em>[la distribuzione del sito tramite FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>e <em>[la distribuzione del sito tramite Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> -esaminare strumenti e tecniche diverse per la sincronizzazione di questi file.

Durante la compilazione di applicazioni guidate dai dati sono in genere due database in uso: uno per lo sviluppo e uno in produzione. Durante lo sviluppo, lo sviluppo schema del database può essere modificato per includere nuove tabelle, colonne, stored procedure e trigger, o può essere modificato per rimuovere o rinominare gli oggetti di database esistenti. Tra l'ora che vengono apportate queste modifiche e l'ora in cui che l'applicazione viene distribuita nell'ambiente di produzione, i database di sviluppo e produzione non sono sincronizzati. Questa modalità asincrona deve essere corretto durante il processo di distribuzione. Questi problemi verranno esaminati in esercitazioni future.

## <a name="finding-a-web-host-provider"></a>Ricerca di un Provider di hosting Web

Applicazioni ASP.NET possono essere distribuite a qualsiasi server web che dispone di .NET Framework e Internet Information Services (IIS). È possibile ospitare un sito dal computer in uso, presupponendo che era una connessione a banda larga a Internet e in grado di configurare il router per consentire le richieste web in ingresso. È inoltre possibile ospitare un sito da un computer in una rete intranet, come molte aziende. L'obiettivo di queste esercitazioni, tuttavia, ospita il sito Web con un provider di hosting web.

> [!NOTE]
> [IIS](https://www.iis.net/) è il server web aziendale di Microsoft. È disponibile nelle edizioni di Windows, ad esempio Windows Server 2008 e in alcune edizioni di Windows Vista non Home. Non è necessario installare IIS per la gestione di applicazioni ASP.NET in un ambiente di sviluppo come Visual Studio include il Server Web di sviluppo ASP.NET. Tuttavia, il Server Web di sviluppo ASP.NET accetta solo connessioni locali e pertanto non può essere utilizzato in un ambiente di produzione.


Prima di poter distribuire il sito per un provider di hosting web, è innanzitutto necessario decidere quali aziendale ai rapporti commerciali con. Esistono innumerevoli web nel marketplace; società di hosting cercare "società di hosting web" restituisce i risultati di più di 5 milioni. Come si individuare quello più adatta alle proprie esigenze? Motore di ricerca preferito è un ottimo punto di partenza, perché sono siti Web come [TopHosts](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), confrontare e contrapporre vari servizi di hosting. Si consiglia inoltre di porre i colleghi e collaboratori per eventuali indicazioni; è anche possibile richiedere al consigli di [Hosting Forum aperto](https://forums.asp.net/158.aspx) qui nel [forum ASP.NET](https://forums.asp.net/).

Società di hosting Web in genere offrono servizi di hosting condivisi e dedicati piani di hosting. Con host condivisi un singolo server host decine se non centinaia di siti Web diversi. Con hosting dedicato lease di un computer dell'azienda che serve il sito e il sito. Un piano di hosting condiviso potrebbe includere il supporto per le pagine ASP.NET, la possibilità di lavorare con i database di Microsoft Access, 5 GB di spazio su disco e 100 GB di traffico mensile in larghezza di banda per $9.95 al mese. Un altro piano di hosting condiviso potrebbe includono il supporto per le pagine ASP.NET, l'accesso al server di database Microsoft SQL Server 2008, 10 GB di spazio su disco e 250 GB del traffico mensile in larghezza di banda per $19,95 al mese. I piani di hosting dedicato vengono in genere molto più costosi, determinazione costi centinaia dollari diversi per ogni mese, ma offrono prestazioni migliori e maggiore controllo rispetto a condiviso opzioni di hosting. Il piano scelto dipende il budget, la quantità di traffico del sito Web riceve e le funzionalità che si prevede che è necessario.

Due considerazioni importanti quando si sceglie un provider di hosting web sono servizio clienti e qualità del servizio. Se si dispone di una domanda o un problema di configurazione, come tempo di inoltrare il problema al supporto tecnico dell'host web fino a ottenere una risposta? L'affidabilità sono servizi della società? Spesso sono interruzioni database? La frequenza con cui venga portato offline il server di posta elettronica? È sempre possibile porre una società per fornire informazioni dettagliate sul tempo di attività e richiedere informazioni sui criteri di servizio clienti, ma un modo più surefire consiste nel richiedere commenti dei clienti correnti e precedenti, è possibile eseguire tramite il Consiglio di posta elettronica, newsgroup e forum in linea .

> [!NOTE]
> Alcune società di hosting web loro attività aziendali di concentrarsi in uno stack di tecnologie particolari, ad esempio .NET o [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, e **P** HP), quindi assicurarsi che la società si seleziona ospita applicazioni ASP.NET. Verificare inoltre che supportano la versione di ASP.NET in uso per compilare l'applicazione. E, se si compila un'applicazione basata sui dati, assicurarsi che l'host web offre nello stesso server di database e la stessa versione in uso.


## <a name="summary"></a>Riepilogo

Le applicazioni web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale. Una volta che una versione è pronta per il rilascio, viene spostata in un ambiente di produzione. Sebbene sia possibile ospitare siti Web ASP.NET nel proprio computer o nei server all'interno della società, in molte aziende e utenti singoli scegliere per l'outsourcing relativi a un provider di hosting web di hosting.

Questa serie di esercitazioni vengono esaminati i passaggi per la distribuzione di un'applicazione ASP.NET per un provider di hosting web, esplorazione problematiche più comuni. In questa esercitazione offerta una panoramica del processo di distribuzione di ASP.NET e l'assegnazione di suggerimenti per la ricerca di un provider di hosting web appropriato. L'esercitazione successiva esamina quali file relativi ad ASP.NET che devono essere copiati nell'ambiente di produzione quando si distribuisce il sito Web.

Buona programmazione!

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](users-and-roles-on-the-production-website-cs.md)
> [Successivo](determining-what-files-need-to-be-deployed-vb.md)
