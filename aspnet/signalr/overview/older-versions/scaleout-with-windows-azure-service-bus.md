---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il Bus di servizio di Azure (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 687d3d7787baa69410ee35d651a029c69d28c70b
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287000"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Scalabilità orizzontale di SignalR con il Bus di servizio di Azure (SignalR 1.x)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In questa esercitazione si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, utilizzando il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Prerequisiti:

- Un account di Windows Azure.
- Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

È anche compatibile con il backplane del bus di servizio [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1. Tuttavia, non compatibile con la versione 1.0 di Service Bus per Windows Server.

## <a name="pricing"></a>Pricing

Backplane del Bus di servizio Usa gli argomenti per inviare messaggi. Per informazioni più aggiornate sui prezzi, vedere [del Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/). Al momento della stesura di questo articolo, è possibile inviare 1.000.000 di messaggi al mese per meno di $1. Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR. Esistono anche alcuni messaggi di controllo per le connessioni, disconnessioni, unita tramite join o uscire da gruppi e così via. Nella maggior parte delle applicazioni, la maggior parte del traffico di messaggi sarà chiamate del metodo dell'hub.

## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Usare il portale di Azure per creare un nuovo spazio dei nomi del Bus di servizio.
2. Aggiungere i pacchetti NuGet per l'applicazione: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente al Global. asax per configurare il backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Per ogni applicazione, selezionare un valore diverso per "YourAppName". Non utilizzare lo stesso valore tra più applicazioni.

## <a name="create-the-azure-services"></a>Creare i servizi di Azure

Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Seguire i passaggi nella sezione "procedura: Creare un servizio cloud con creazione rapida". Per questa esercitazione, non occorre caricare un certificato.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Creare un nuovo spazio dei nomi del Bus di servizio, come descritto in [procedura per usare Service Bus argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Seguire i passaggi nella sezione "Creare un servizio Namespace".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.


## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Avviare Visual Studio. Dal **File** menu, fare clic su **nuovo progetto**.

Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**. Sotto **modelli installati**, selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**. Mantenere il valore predefinito di .NET Framework 4.5. Denominare l'applicazione ChatService e fare clic su **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare Web ruoli di ASP.NET MVC 4. Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.

Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile. Fare clic su questa icona per rinominare il ruolo. Nome del ruolo "SignalRChat", quindi scegliere **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 4** procedura guidata, selezionare **applicazione Internet**. Fare clic su **OK**. La creazione guidata progetto crea due progetti:

- ChatService: Questo progetto è l'applicazione di Windows Azure. Definisce i ruoli di Azure e altre opzioni di configurazione.
- SignalRChat: Questo progetto è il progetto ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Creare l'applicazione di Chat di SignalR

Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Introduzione a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Usare NuGet per installare le librerie necessarie. Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nel **Console di gestione pacchetti** finestra, immettere i comandi seguenti:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Usare il `-ProjectName` opzione per installare i pacchetti per il progetto ASP.NET MVC, piuttosto che il progetto Azure.

## <a name="configure-the-backplane"></a>Configurare il Backplane

Nel file Global. asax dell'applicazione, aggiungere il codice seguente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

A questo punto è necessario ottenere la stringa di connessione del bus di servizio. Nel portale di Azure, selezionare lo spazio dei nomi del bus di servizio è stato creato e fare clic sull'icona di tasto di scelta rapida.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Distribuire in Azure

In Esplora soluzioni espandere la **ruoli** cartella all'interno del progetto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Il ruolo SignalRChat e scegliere **proprietà**. Selezionare il **configurazione** scheda. Sotto **istanze** selezionare 2. È anche possibile impostare le dimensioni VM su **molto piccola**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Salvare le modifiche.

In Esplora soluzioni fare clic sul progetto le ChatService. Selezionare **Pubblica**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Se questa è la prima pubblicazione ora Windows Azure, è necessario scaricare le credenziali. Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali". Verrà chiesto di accedere al portale di Azure e scaricare un file di impostazioni di pubblicazione.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione scaricato.

Scegliere **Avanti**. Nel **impostazioni di pubblicazione** finestra di dialogo, sotto **servizio Cloud**, selezionare il servizio cloud creato in precedenza.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Fare clic su **Pubblica**. Possono volerci alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.

A questo punto quando si esegue l'applicazione di chat, istanze del ruolo di comunicano tramite il Bus di servizio di Azure, usando un argomento del Bus di servizio. Un argomento è una coda di messaggi che consente a più sottoscrittori.

Backplane crea automaticamente gli argomenti e sottoscrizioni. Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Rende necessari alcuni minuti per l'attività del messaggio da visualizzare nel dashboard.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR gestisce la durata di argomento. Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.
