---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN a un ruolo di lavoro di Azure | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come self-hosting OWIN in un ruolo di lavoro di Microsoft Azure. Interfaccia Web aperta per .NET (OWIN) definisce un'astrazione tra il server web .NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 13bccc4b2d6f1b22c94446deaf6795dab766275b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868425"
---
<a name="host-owin-in-an-azure-worker-role"></a>Host OWIN a un ruolo di lavoro di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

> In questa esercitazione viene illustrato come self-hosting OWIN in un ruolo di lavoro di Microsoft Azure.
> 
> [Aprire l'interfaccia Web per .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e le applicazioni web. OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN: ad esempio, all'interno di un ruolo di lavoro di Azure.
> 
> In questa esercitazione si apprenderà come host indipendente di un'applicazione OWIN all'interno di un ruolo di lavoro di Microsoft Azure. Per ulteriori informazioni sui ruoli di lavoro, vedere [modelli di esecuzione di Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK per .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. Sono necessari privilegi di amministratore per eseguire il debug dell'applicazione localmente, utilizzando l'emulatore di calcolo di Azure.

Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**. Da **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Nel **nuovo servizio Cloud Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio viene aggiunto un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creato contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e configurazione per l'applicazione Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, anche se in questa esercitazione viene utilizzato un solo ruolo.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Aggiungere i pacchetti di Host indipendente OWIN

Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**.

Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un HTTP Endpoint

In Esplora soluzioni, espandere il progetto AzureApp. Espandere il nodo ruoli, fare doppio clic su WorkerRole1 e selezionare **proprietà**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Fare clic su **endpoint**, quindi fare clic su **Aggiungi Endpoint**.

Nel **protocollo** elenco a discesa, selezionare "http". In **porta pubblica** e **porta privata**, digitare 80. I numeri di porta possono essere diversi. La porta pubblica è ciò che usano i client che inviano una richiesta per il ruolo.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Creare la classe di avvio OWIN

In Esplora soluzioni, fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Aggiungi** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

Sostituire tutto il codice boilerplate con gli elementi seguenti:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Il `UseWelcomePage` metodo di estensione aggiunge una semplice pagina HTML per l'applicazione, per verificare il sito sia funzionante.

## <a name="start-the-owin-host"></a>Avviare l'Host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere la seguente istruzione using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Aggiungere un **IDisposable** membro per la `WorkerRole` classe:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Il **WebApp** metodo avvia l'host OWIN. Il nome della `Startup` classe è un parametro di tipo al metodo. Per convenzione, l'host chiamerà il `Configure` metodo di questa classe.

Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione localmente nell'emulatore di calcolo di Azure. A seconda delle impostazioni del firewall, potrebbe essere necessario consentire l'emulatore attraverso il firewall.

L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint. È possibile trovare l'indirizzo IP visualizzando l'interfaccia utente emulatore di calcolo. Fare doppio clic sull'icona dell'emulatore nell'area di notifica della barra attività e selezionare **Mostra interfaccia utente di emulatore di calcolo**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Individuare l'indirizzo IP in distribuzioni del servizio, la distribuzione [id], i dettagli del servizio. Aprire un web browser e passare a http://<em>indirizzo</em>, dove <em>indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80`. Si dovrebbe vedere la pagina di benvenuto OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account di Azure. Se non hai già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni, fare clic sul progetto AzureApp. Selezionare **Pubblica**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se non è stato effettuato l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Dopo che si è connessi, scegliere una sottoscrizione e fare clic su **Avanti**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Fare clic su **Pubblica**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando l'applicazione viene distribuita, passare a `http://appname.cloudapp.net/`, dove *appname* è il nome del servizio cloud.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](an-overview-of-project-katana.md)
- [Progetto Katana su GitHub](https://github.com/aspnet/AspNetKatana/)
