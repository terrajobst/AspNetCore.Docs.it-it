---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il Bus di servizio di Azure | Microsoft Docs
author: bradygaster
description: Le versioni del software utilizzato in questa versione di Visual Studio 2013 .NET 4.5 SignalR argomento 2 nelle versioni precedenti di questa versione 1.x di SignalR per l'argomento di questo argomento,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 9f6188ff5f716c20d759f73975d6a8ad522834d8
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836818"
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Scalabilità orizzontale di SignalR con il Bus di servizio di Azure
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

In questa esercitazione si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, utilizzando il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo. (È anche possibile usare il backplane del Bus di servizio con [App web nel servizio App di Azure](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Prerequisiti:

- Un account di Windows Azure.
- Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 or 2013.

È anche compatibile con il backplane del bus di servizio [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1. Tuttavia, non compatibile con la versione 1.0 di Service Bus per Windows Server.

## <a name="pricing"></a>Pricing

Backplane del Bus di servizio Usa gli argomenti per inviare messaggi. Per informazioni più aggiornate sui prezzi, vedere [del Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/). Al momento della stesura di questo articolo, è possibile inviare 1.000.000 di messaggi al mese per meno di $1. Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR. Esistono anche alcuni messaggi di controllo per le connessioni, disconnessioni, unita tramite join o uscire da gruppi e così via. Nella maggior parte delle applicazioni, la maggior parte del traffico di messaggi sarà chiamate del metodo dell'hub.

## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Usare il portale di Azure per creare un nuovo spazio dei nomi del Bus di servizio.
2. Aggiungere i pacchetti NuGet per l'applicazione: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente per configurare backplane Startup.cs: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Questo codice consente di configurare con i valori predefiniti per il backplane [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: La metrica di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).

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

Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare un ruolo Web ASP.NET. Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.

Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile. Fare clic su questa icona per rinominare il ruolo. Nome del ruolo "SignalRChat", quindi scegliere **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona **MVC**, fare clic su OK.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

La creazione guidata progetto crea due progetti:

- ChatService: Questo progetto è l'applicazione di Windows Azure. Definisce i ruoli di Azure e altre opzioni di configurazione.
- SignalRChat: Questo progetto è il progetto ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Creare l'applicazione di Chat di SignalR

Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Usare NuGet per installare le librerie necessarie. Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**. Nel **Console di gestione pacchetti** finestra, immettere i comandi seguenti:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Usare il `-ProjectName` opzione per installare i pacchetti per il progetto ASP.NET MVC, piuttosto che il progetto Azure.

## <a name="configure-the-backplane"></a>Configurare il Backplane

Nel file Startup.cs dell'applicazione, aggiungere il codice seguente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

A questo punto è necessario ottenere la stringa di connessione del bus di servizio. Nel portale di Azure, selezionare lo spazio dei nomi del bus di servizio è stato creato e fare clic sull'icona di tasto di scelta rapida.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Distribuire in Azure

In Esplora soluzioni espandere la **ruoli** cartella all'interno del progetto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Il ruolo SignalRChat e scegliere **proprietà**. Selezionare il **configurazione** scheda. Sotto **istanze** selezionare 2. È anche possibile impostare le dimensioni VM su **molto piccola**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Salvare le modifiche.

In Esplora soluzioni fare clic sul progetto le ChatService. Selezionare **Pubblica**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Se questa è la prima pubblicazione ora Windows Azure, è necessario scaricare le credenziali. Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali". Verrà chiesto di accedere al portale di Azure e scaricare un file di impostazioni di pubblicazione.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione scaricato.

Scegliere **Avanti**. Nel **impostazioni di pubblicazione** finestra di dialogo, sotto **servizio Cloud**, selezionare il servizio cloud creato in precedenza.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Fare clic su **Pubblica**. Possono volerci alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.

A questo punto quando si esegue l'applicazione di chat, istanze del ruolo di comunicano tramite il Bus di servizio di Azure, usando un argomento del Bus di servizio. Un argomento è una coda di messaggi che consente a più sottoscrittori.

Backplane crea automaticamente gli argomenti e sottoscrizioni. Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Rende necessari alcuni minuti per l'attività del messaggio da visualizzare nel dashboard.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR gestisce la durata di argomento. Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.

## <a name="troubleshooting"></a>Risoluzione dei problemi

**System. InvalidOperationException "L'unico IsolationLevel supportato è 'IsolationLevel.Serializable'".**

Questo errore può verificarsi se il livello di transazione per un'operazione è impostato su un valore diverso da `Serializable`. Verificare che nessuna operazione sono viene eseguita con altri livelli delle transazioni.
