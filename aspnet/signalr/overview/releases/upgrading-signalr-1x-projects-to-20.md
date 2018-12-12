---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: Aggiornamento dei progetti di SignalR 1.x alla versione 2 | Microsoft Docs
author: pfletcher
description: In questo argomento descrive come aggiornare un progetto 1.x SignalR esistente a SignalR 2.x e come risolvere i problemi che potrebbero verificarsi durante il processo di aggiornamento...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 23ea23585b15395cf86bdad13885af32d1b64e79
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286844"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>L'aggiornamento di progetti SignalR 1.x alla versione 2
====================
da [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento descrive come aggiornare un progetto 1.x SignalR esistente a SignalR 2.x e come risolvere i problemi che potrebbero verificarsi durante il processo di aggiornamento.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Versioni di SignalR 1 e 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso di Visual Studio 2012 con questa esercitazione
>
>
> Per usare Visual Studio 2012 con questa esercitazione, eseguire le operazioni seguenti:
>
> - Aggiornamento di [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.
> - Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e Web Tools 2013.1 per Visual Studio 2012**. Verrà installato, ad esempio modelli di Visual Studio per le classi di SignalR **Hub**.
> - Alcuni modelli (ad esempio **classe di avvio OWIN**) non è disponibile; per questo motivo, usare invece un file di classe.
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


SignalR 2 offre un'esperienza di sviluppo coerente nelle diverse piattaforme server usando [OWIN](http://owin.org). Questo articolo descrive i passaggi necessari per aggiornare un'applicazione di SignalR 1.x alla versione 2.

Anche se è consigliabile aggiornare le applicazioni a SignalR 2, SignalR 1.x verrà comunque essere supportato.

Questa esercitazione descrive come aggiornare un'applicazione ospitata sul web a SignalR 2. Le applicazioni self-hosted (quelli che ospitano un server in un'applicazione console, il servizio di Windows o altri processi) sono ora supportate in SignalR 2. Per informazioni su come iniziare a creare un'applicazione indipendente con SignalR 2, vedere [esercitazione: Hosting indipendente di SignalR](../deployment/tutorial-signalr-self-host.md).

## <a name="contents"></a>Sommario

Le sezioni seguenti descrivono le attività coinvolte relativi all'aggiornamento di progetti SignalR e come risolvere i problemi che potrebbero verificarsi.

- [Esempio: L'aggiornamento dell'esercitazione Introduzione a SignalR 2](#example)
- [Risoluzione degli errori rilevati durante l'aggiornamento](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>Esempio: L'aggiornamento dell'applicazione di esercitazione Introduzione a SignalR 2

In questa sezione, si aggiornerà l'applicazione creata nel [versione di SignalR 1.x dell'esercitazione introduttiva](../older-versions/index.md) usare SignalR 2.

1. Dopo aver completato l'esercitazione introduttiva, pulsante destro del mouse sul progetto e selezionare **proprietà**. Verificare che il **framework di destinazione** è impostata su **.NET Framework 4.5.**
2. Aprire la Console di gestione pacchetti. Rimuovere SignalR 1.x dal progetto usando il comando seguente:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. Installare SignalR 2 usando il comando seguente:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. Nella pagina HTML, aggiornare il riferimento allo script per SignalR in base alla versione dello script ora inclusa nel progetto.

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. Nella classe di applicazione globale, rimuovere la chiamata a MapHubs.

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. La soluzione e scegliere **Add**, **nuovo elemento...** . Nella finestra di dialogo, selezionare **classe di avvio Owin**. Denominare la nuova classe **Startup.cs**.

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Sostituire il contenuto di Startup.cs con il codice seguente:

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    L'attributo assembly aggiunge la classe al processo di avvio di Owin, che viene eseguito il `Configuration` metodo all'avvio di Owin. Ciò a sua volta chiama il `MapSignalR` metodo, che consente di creare le route per tutti gli hub SignalR nell'applicazione.
8. Eseguire il progetto e copiare l'URL della pagina principale in un altro browser o nel riquadro del browser, come in precedenza. Ogni pagina chiederà un nome utente e i messaggi inviati da ogni pagina devono essere visibili in entrambi i riquadri del browser.

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>Risoluzione degli errori rilevati durante l'aggiornamento

In questa sezione vengono descritti i problemi che potrebbero verificarsi durante l'aggiornamento. Per un elenco più completo di errori e problemi che possono verificarsi con un'applicazione di SignalR, vedere [risoluzione dei problemi di SignalR](../testing-and-debugging/troubleshooting.md).

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>'È ambigua tra i seguenti metodi o proprietà chiamata'

Questo errore si verifica se un riferimento a `Microsoft.AspNet.SignalR.Owin` non viene rimosso. Questo pacchetto è deprecato. il riferimento deve essere rimossa e la versione 1.x del pacchetto SelfHost deve essere disinstallata.

### <a name="hub-methods-fail-silently"></a>I metodi dell'hub interrompersi senza avvisi

Verificare che i riferimenti a script nel client vengono aggiornate e che il `OwinStartup` attributo per la classe Startup ha la classe corretto e nomi di assembly per il progetto. Inoltre, provare ad aprire l'indirizzo di hub (hub signalr /) nel browser; qualsiasi errore che compare offrono altre informazioni su cosa sta succedendo errato.
