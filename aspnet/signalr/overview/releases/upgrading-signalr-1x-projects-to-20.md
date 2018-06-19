---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: L'aggiornamento di progetti di 1. x SignalR versione 2 | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come aggiornare un progetto esistente di 1. x SignalR per SignalR 2. x e come risolvere i problemi che possono verificarsi durante il processo di aggiornamento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505740"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>L'aggiornamento di progetti SignalR 1. x alla versione 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> In questo argomento viene descritto come aggiornare un progetto esistente di 1. x SignalR per SignalR 2. x e come risolvere i problemi che possono verificarsi durante il processo di aggiornamento.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versioni 1 e 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Utilizzo di Visual Studio 2012 con questa esercitazione
> 
> 
> Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:
> 
> - Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.
> - Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**. Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.
> - Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.
> 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 offre un'esperienza di sviluppo coerente tra le piattaforme server utilizzando [OWIN](http://owin.org). Questo articolo descrive alcuni passaggi necessari per aggiornare un'applicazione di SignalR 1. x alla versione 2.

È consigliabile aggiornare le applicazioni a 2 SignalR, SignalR 1. x verrà comunque essere supportato.

In questa esercitazione viene descritto come aggiornare un'applicazione ospitata sul web a 2 SignalR. Le applicazioni indipendenti (quelli che ospitano un server in un'applicazione console, il servizio di Windows o altri processi) sono ora supportate in 2 SignalR. Per informazioni su come iniziare a creare un'applicazione indipendente con 2 SignalR, vedere [esercitazione: SignalR indipendente](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Contenuto

Nelle sezioni seguenti vengono descrivono le attività necessarie per l'aggiornamento di progetti SignalR e come risolvere i problemi che possono verificarsi.

- [Esempio: Aggiornamento di esercitazione introduttiva su SignalR 2](#example)
- [Risoluzione dei problemi rilevati errori durante l'aggiornamento](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Esempio: L'aggiornamento dell'applicazione di esercitazione Introduzione a SignalR 2

In questa sezione si aggiornerà l'applicazione creata nel [versione 1. x SignalR dell'esercitazione introduttiva](../older-versions/index.md) per usare 2 SignalR.

1. Dopo aver completato l'esercitazione introduttiva, pulsante destro del mouse sul progetto e selezionare **proprietà**. Verificare che il **framework di destinazione** è impostato su **.NET Framework 4.5.**
2. Aprire la Console di gestione pacchetti. Rimuovere SignalR 1. x dal progetto utilizzando il comando seguente:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Installare SignalR 2 utilizzando il comando seguente:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Nella pagina HTML, aggiornare il riferimento allo script per SignalR in base alla versione dello script ora inclusa nel progetto.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Nella classe di applicazione globale, rimuovere la chiamata a MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. La soluzione e scegliere **Aggiungi**, **nuovo elemento...** . Nella finestra di dialogo selezionare **classe di avvio di Owin**. Denominare la nuova classe **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Sostituire il contenuto di Startup.cs con il codice seguente:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    L'attributo di assembly consente di aggiungere la classe al processo di avvio di Owin, che esegue il `Configuration` metodo all'avvio di Owin. A sua volta chiama il `MapSignalR` metodo, che crea le route per tutti gli hub SignalR nell'applicazione.
8. Eseguire il progetto e copiare l'URL della pagina principale in un altro browser o il riquadro del browser, come prima. Ogni pagina richiederà l'immissione di un nome utente e i messaggi inviati da ogni pagina devono essere visibili in entrambi i riquadri del browser.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Risoluzione dei problemi rilevati errori durante l'aggiornamento

In questa sezione vengono descritti i problemi che possono verificarsi durante l'aggiornamento. Per un elenco più completo di errori e problemi che possono verificarsi con un'applicazione di SignalR, vedere [la risoluzione dei problemi di SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'La chiamata è ambigua tra i seguenti metodi o proprietà'

Questo errore si verifica se un riferimento a `Microsoft.AspNet.SignalR.Owin` non viene rimosso. Questo pacchetto è stato deprecato. il riferimento deve essere rimossa e la versione 1. x del pacchetto host deve essere disinstallata.

### <a name="hub-methods-fail-silently"></a>Metodi dell'hub non venga segnalato

Verificare che i riferimenti a script nel client e che il `OwinStartup` attributo per la classe di avvio è la classe corretta e nomi di assembly per il progetto. Inoltre, provare ad aprire l'indirizzo di hub (signalr/hub) nel browser; qualsiasi errore che compare offrono ulteriori informazioni su qual è il problema.
