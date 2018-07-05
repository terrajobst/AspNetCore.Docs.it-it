---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET (c#) opzioni di Hosting | Microsoft Docs
author: rick-anderson
description: Le applicazioni web ASP.NET sono progettate in genere, creano e testati in un ambiente di sviluppo locale e devono essere distribuite a una o ambiente di produzione...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 12d4e0e4332cf611e304799155b45f5d668da8f6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804187"
---
<a name="aspnet-hosting-options-c"></a>Opzioni di Hosting ASP.NET (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Le applicazioni web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale e debbano essere distribuite in un ambiente di produzione dopo che è pronta per il rilascio. Questa esercitazione offre una panoramica generale del processo di distribuzione e costituisce un'introduzione a questa serie di esercitazioni.


## <a name="introduction"></a>Introduzione

Le applicazioni Web sono in genere progettate, create e testate in un ambiente di sviluppo che è accessibile solo per i programmatori che lavorano nel sito. Dopo l'applicazione è pronta per essere rilasciato, viene spostato in un ambiente di produzione in cui il sito è accessibile da qualsiasi utente su Internet. Questo processo di distribuzione introduce una serie di difficoltà:

- Un ambiente di produzione deve esistere ed essere correttamente il programma di installazione prima di poter distribuire un'applicazione ASP.NET; Inoltre, l'ambiente di produzione deve rimanere aggiornato con le ultime patch di sicurezza.
- Il set corretto di file di markup, i file di codice e i file di supporto deve essere copiato dall'ambiente di sviluppo all'ambiente di produzione. Per le applicazioni guidate dai dati, potrebbe essere necessario copiare lo schema del database e/o dati, nonché.
- Possono essere presenti differenze di configurazione tra i due ambienti. Server di database connection string o messaggio di posta elettronica usato nell'ambiente di sviluppo saranno probabilmente diverso dall'ambiente di produzione. Inoltre, il comportamento dell'applicazione può dipendere l'ambiente. Ad esempio, quando si verifica un errore in fase di sviluppo è possono visualizzare i dettagli dell'errore sullo schermo, ma quando si verifica un errore nell'ambiente di produzione, deve venire invece visualizzata una pagina di errore descrittivo e i dettagli dell'errore tramite posta elettronica per gli sviluppatori.

Per prevenire il primo problema - impostazione e la gestione di un ambiente di produzione - molti privati e aziende relativi ambienti di produzione a outsourcing *provider di hosting web*. Un provider di hosting web è una società che gestisce l'ambiente di produzione per tuo conto. Esistono innumerevoli web provider host, ognuno con prezzi diversi e livelli di servizio. vedere la sezione "Individuazione di un Provider di hosting Web" per suggerimenti su come trovare un provider di servizi.

Questo è il primo di una serie di esercitazioni in cui è Esaminiamo i passaggi necessari per distribuire un'applicazione web ASP.NET in un ambiente di produzione gestita da un provider di hosting web. Nel corso delle esercitazioni seguenti verranno esaminate:

- I file che dovranno essere distribuiti per il provider di hosting web.
- Strumenti per semplificare il processo di distribuzione.
- Come distribuire un database.
- Suggerimenti per la distribuzione di un database che utilizza [il provider di ruoli e appartenenza basata su SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), unitamente a modi per simulare lo strumento Amministrazione sito Web in un ambiente di produzione.
- Strategie per senza problemi l'aggiornamento del database nell'ambiente di produzione con le modifiche apportate durante lo sviluppo.
- Tecniche per la registrazione degli errori che si verifica in produzione, nonché di notificare agli sviluppatori quando si verifica un errore.

Queste esercitazioni sono pensate per essere concise e vengono fornite istruzioni dettagliate con numerose catture di schermata che illustra il processo in modo visivo. Questa esercitazione inaugurale offre una panoramica del processo di distribuzione di ASP.NET e consigli su come trovare un provider di hosting web. Iniziamo!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Una panoramica del processo di distribuzione di ASP.NET

In breve, la distribuzione di un'applicazione ASP.NET richiede i tre passaggi seguenti:

1. Configurare l'applicazione web, server web e database nell'ambiente di produzione.
2. Sincronizzare le pagine ASP.NET, i file di codice, gli assembly di `Bin` cartella e file di supporto HTML correlati, ad esempio file CSS e JavaScript.
3. Sincronizzare lo schema del database e/o dei dati.

Le informazioni di configurazione per un'applicazione web si trova in genere nel `Web.config` file e include le stringhe di connessione del database, i criteri di gestione degli errori, URL di riscrittura di regole e informazioni sul server di posta elettronica. Spesso queste informazioni sono diverse per un'applicazione in fase di sviluppo e la stessa applicazione nell'ambiente di produzione. Ad esempio, quando si sviluppa un'applicazione è consigliabile usare un database di sviluppo in modo che non si sta testando nel database di produzione. Di conseguenza, le stringhe di connessione del database è in genere differiscono tra le applicazioni di sviluppo e produzione. A causa di queste differenze, parte della distribuzione comporta modifiche alle informazioni di configurazione dell'applicazione web.

Oltre alle modifiche alla configurazione dell'applicazione web, passaggio 1 anche può comportare una configurazione per il database e server web. Ad esempio, se una pagina ASP.NET crea o Elimina i file da una directory nel server web del server web deve quindi essere configurato per consentire le modifiche al sistema questi file. Analogamente, potrebbe esserci delle impostazioni di autorizzazione o autenticazione che devono essere apportate al database.


Passaggio 2 richiede la sincronizzazione il set di pagine ASP.NET essenziali e file di supporto tra gli ambienti di sviluppo e produzione. Il set specifico di ASP. I file correlati alla rete che devono essere sincronizzati tra i due ambienti varia a seconda del tipo di progetto creato in Visual Studio e la discussione nella prossima esercitazione, viene [ *determinare quali file devono essere distribuiti*](determining-what-files-need-to-be-deployed-cs.md). Le esercitazioni terza e quarta - [ *distribuzione del sito tramite FTP* ](deploying-your-site-using-an-ftp-client-cs.md) e [ *distribuzione del sito tramite Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -esaminare diversi strumenti e tecniche per la sincronizzazione di questi file.

Quando si compilano applicazioni basate sui dati sono in genere due database in uso: uno per lo sviluppo e uno in produzione. Durante lo sviluppo, lo sviluppo schema del database possa essere modificato per includere le nuove tabelle, colonne, le stored procedure e trigger, o può essere modificato per rimuovere o rinominare gli oggetti di database esistenti. Tra l'ora in cui vengono apportate queste modifiche e l'ora che l'applicazione viene distribuita nell'ambiente di produzione, i database di sviluppo e produzione non sono sincronizzati. Questa asincronia deve essere corretto durante il processo di distribuzione. Queste sfide verranno esaminate in esercitazioni future.

## <a name="finding-a-web-host-provider"></a>Ricerca di un Provider di hosting Web

Le applicazioni ASP.NET possono essere distribuite in qualsiasi server web con .NET Framework e Internet Information Services (IIS). È possibile ospitare un sito dal computer in uso, presupponendo che si era presente una connessione a banda larga a Internet e di sapere come configurare il router per consentire le richieste web in ingresso. È inoltre possibile ospitare un sito da un computer in una rete intranet, come molte aziende. L'obiettivo di queste esercitazioni, tuttavia, ospita il sito Web con un provider di hosting web.

> [!NOTE]
> [IIS](https://www.iis.net/) è server web di livello aziendale di Microsoft. Fornito con le edizioni Home non di Windows, ad esempio Windows Server 2008 e in alcune edizioni di Windows Vista. Non occorre installare IIS per la gestione di applicazioni ASP.NET in un ambiente di sviluppo come Visual Studio include Server Web di sviluppo ASP.NET. Tuttavia, il Server Web di sviluppo ASP.NET accetta solo connessioni locali e pertanto non può essere utilizzato in un ambiente di produzione.


Prima di poter distribuire il sito per un provider di hosting web, è prima necessario decidere quali aziendali per svolgere attività commerciali con. Esistono innumerevoli web che ospita le aziende nel marketplace; cercare "società di hosting web" restituisce i risultati di più di cinque milioni. Come individuare quella più adatta a te? Motore di ricerca preferito è un buon punto di partenza, come siti Web, ad esempio [TopHosts](http://www.tophosts.com/) e [HostCritique](http://www.hostcritique.net/), quale analizzare e confrontare vari servizi di hosting. Consiglio inoltre porre colleghi e i colleghi per eventuali indicazioni; è anche possibile porre per le indicazioni nel [Hosting Forum Open](https://forums.asp.net/158.aspx) sentiamo i [forum ASP.NET](https://forums.asp.net/).

Le società di hosting Web in genere offrono servizi di hosting condivisi e dedicati i piani di hosting. Con host condivisi un host di server web single decine se non centinaia di siti Web diversi. Con l'hosting dedicato lease un computer della società che gestisce il sito e il sito. Un piano di hosting condiviso potrebbe includere il supporto per le pagine ASP.NET, la possibilità di lavorare con database Microsoft Access, 5 GB di spazio su disco e 100 GB di traffico mensile in larghezza di banda per 9,95 dollari al mese. Un altro piano di hosting condiviso potrebbe includere il supporto per le pagine ASP.NET, l'accesso al server di database Microsoft SQL Server 2008, 10 GB di spazio su disco e 250 GB di traffico mensile in larghezza di banda per 19,95 dollari con acquisto al mese. Dedicato i piani di hosting sono in genere molto più costosi e determinazione costi centinaia dollari diversi per ogni mese, ma offrono prestazioni migliorate e maggiore controllo rispetto a condiviso opzioni di hosting. Il piano scelto dipende il tuo budget, la quantità di traffico del sito Web riceve e le funzionalità si prevede si saranno necessario.

Due considerazioni importanti quando si sceglie un provider di hosting web sono servizio clienti e la qualità del servizio. Se hai una domanda o un problema di configurazione, quanto tempo occorre dall'invio del problema al supporto tecnico dell'host web fino a quando non si ottiene una risposta? Affidabili sono i servizi dell'azienda? Spesso sono interruzioni database? Quanto spesso venga portato offline il server di posta elettronica? È sempre possibile porre un'azienda per fornire informazioni dettagliate sul tempo di attività e sapere se i criteri di servizio del cliente, ma è un modo più surefire sollecitazione feedback dei clienti correnti e precedenti, ed è possibile farlo tramite i forum, newsgroup e Consiglio di posta elettronica .

> [!NOTE]
> Alcune società di hosting web loro attività aziendali di concentrarsi su uno stack particolare tecnologia, ad esempio .NET o [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **oggetto** pache, **M** ySQL, e **P** HP), quindi assicurarsi che la società si seleziona ospita le applicazioni ASP.NET. Verificare anche che supportano la versione di ASP.NET si usa per compilare l'applicazione. E se si compila un'applicazione basata su dati, assicurarsi che l'host web offre il server di database e la versione che si sta utilizzando stesso.


## <a name="summary"></a>Riepilogo

Le applicazioni web ASP.NET sono in genere progettate, create e testate in un ambiente di sviluppo locale. Una volta che una versione è pronta per il rilascio, viene spostato in un ambiente di produzione. Sebbene sia possibile per ospitare siti Web ASP.NET nel proprio computer o nei server all'interno della società, molte aziende e individui scegliere per l'outsourcing relativi a un provider di hosting web di hosting.

Questa serie di esercitazioni vengono esaminati i passaggi per la distribuzione di un'applicazione ASP.NET in un provider di hosting web, esplorare le problematiche più comuni. Questa esercitazione offerta una panoramica generale del processo di distribuzione di ASP.NET e ha offerto suggerimenti per la ricerca di un provider di hosting web appropriato. L'esercitazione successiva illustra ciò che i file correlati al ASP.NET devono essere copiati nell'ambiente di produzione quando si distribuisce il sito Web.

Buona programmazione!

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [avanti](determining-what-files-need-to-be-deployed-cs.md)
