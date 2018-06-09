---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale SignalR con il Bus di servizio di Azure (SignalR 1. x) | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036440"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Scalabilità orizzontale SignalR con il Bus di servizio di Azure (SignalR 1. x)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In questa esercitazione, si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, con il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Prerequisiti:

- Un account di Windows Azure.
- Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Backplane di bus di servizio è inoltre compatibile con [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1. Non è tuttavia compatibile con la versione 1.0 di Service Bus per Windows Server.

## <a name="pricing"></a>Pricing

Bus di servizio backplane utilizza argomenti per inviare messaggi. Per informazioni più recenti sui prezzi, vedere [Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/). Al momento della redazione del presente documento, è possibile inviare 1.000.000 messaggi al mese per meno di $1. Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR. Esistono inoltre alcuni messaggi di controllo per le connessioni, disconnessioni, unione o uscire da gruppi e così via. Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamate del metodo dell'hub.

## <a name="overview"></a>Panoramica

Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.

1. Utilizzare il portale Windows Azure per creare un nuovo spazio dei nomi Service Bus.
2. Aggiungere questi pacchetti NuGet per l'applicazione: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Creare un'applicazione di SignalR.
4. Aggiungere il codice seguente per Global. asax per configurare il backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Per ogni applicazione, selezionare un valore diverso per "NomeApp". Non utilizzare lo stesso valore in più applicazioni.

## <a name="create-the-azure-services"></a>Creare i servizi di Azure

Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Seguire i passaggi nella sezione "procedura: creare un servizio cloud utilizzando Creazione rapida". Per questa esercitazione, non è necessario caricare un certificato.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Creare un nuovo spazio dei nomi Service Bus, come descritto in [come al Bus di servizio utilizzare argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Seguire i passaggi nella sezione "Creare un servizio Namespace".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.


## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

Avviare Visual Studio. Dal **File** menu, fare clic su **nuovo progetto**.

Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**. In **modelli installati**selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**. Mantenere il valore predefinito di .NET Framework 4.5. L'applicazione ChatService e fare clic su **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare Web ruoli di ASP.NET MVC 4. Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.

Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile. Fare clic su questa icona per rinominare il ruolo. Il ruolo "SignalRChat" e fare clic su **OK**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

Nel **nuovo progetto ASP.NET MVC 4** procedura guidata, selezionare **applicazione Internet**. Fare clic su **OK**. La creazione guidata progetto crea due progetti:

- ChatService: Questo progetto è l'applicazione di Windows Azure. Definisce i ruoli Azure e altre opzioni di configurazione.
- SignalRChat: Questo progetto è il progetto ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Creare l'applicazione di Chat di SignalR

Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Guida introduttiva a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Utilizzare NuGet per installare le librerie necessarie. Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nel **Package Manager Console** finestra, immettere i comandi seguenti:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Utilizzare il `-ProjectName` opzione per installare i pacchetti del progetto ASP.NET MVC, piuttosto che il progetto di Windows Azure.

## <a name="configure-the-backplane"></a>Configurare il Backplane

Nel file Global. asax dell'applicazione, aggiungere il codice seguente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

È necessario ottenere una stringa di connessione del bus di servizio. Nel portale di Azure, selezionare i nomi del bus di servizio creato e fare clic sull'icona di tasto di scelta rapida.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Distribuire in Azure

In Esplora soluzioni, espandere il **ruoli** cartella all'interno del progetto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Il ruolo SignalRChat di mouse e scegliere **proprietà**. Selezionare il **configurazione** scheda. In **istanze** selezionare 2. È inoltre possibile impostare le dimensioni della VM su **molto piccolo**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Salvare le modifiche.

In Esplora soluzioni, fare clic sul progetto ChatService. Selezionare **Pubblica**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Se questa è la prima ora la pubblicazione in Windows Azure, è necessario scaricare le credenziali. Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali". Verrà chiesto di accedere al portale di Windows Azure e scaricare un file di impostazioni di pubblicazione.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione che è stato scaricato.

Scegliere **Avanti**. Nel **impostazioni di pubblicazione** finestra di dialogo, in **servizio Cloud**, selezionare il servizio cloud creato in precedenza.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Fare clic su **Pubblica**. Può richiedere alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.

A questo punto quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite Bus di servizio di Azure, mediante un argomento del Bus di servizio. Un argomento è una coda di messaggi che consente a più sottoscrittori.

Backplane crea automaticamente gli argomenti e le sottoscrizioni. Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Può richiedere alcuni minuti per l'attività di messaggio da visualizzare nel dashboard.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR gestisce la durata di argomento. Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.
