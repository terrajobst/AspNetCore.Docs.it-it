---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Host OWIN in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come a Self-hosting OWIN in un ruolo di lavoro di Microsoft Azure. Open Web Interface for .NET (OWIN) definisce un'astrazione tra i server web di .NET...
ms.author: aspnetcontent
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: f62b9299a4e369ae3a938c85e60dd6a79108548d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826481"
---
<a name="host-owin-in-an-azure-worker-role"></a>Hosting di OWIN in un ruolo di lavoro di Azure
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Questa esercitazione illustra come a Self-hosting OWIN in un ruolo di lavoro di Microsoft Azure.
> 
> [Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web. OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN – ad esempio, all'interno di un ruolo di lavoro di Azure.
> 
> In questa esercitazione si apprenderà come indipendente su delle applicazioni OWIN all'interno di un ruolo di lavoro di Microsoft Azure. Per altre informazioni sui ruoli di lavoro, vedere [modelli di esecuzione di Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK per .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Creare un progetto di Microsoft Azure

Avviare Visual Studio con privilegi di amministratore. Sono necessari privilegi di amministratore per eseguire il debug in locale, l'applicazione usa l'emulatore di calcolo di Azure.

Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**. Dal **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**. Denominare il progetto "AzureApp" e fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

Nel **nuovo servizio Cloud di Windows Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**. Lasciare il nome predefinito ("WorkerRole1"). Questo passaggio aggiunge un ruolo di lavoro alla soluzione. Fare clic su **OK**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La soluzione di Visual Studio creato contiene due progetti:

- &quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione di Azure.
- &quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.

In generale, un'applicazione Azure può contenere più ruoli, sebbene questa esercitazione Usa un singolo ruolo.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Aggiungere i pacchetti di self-hosting OWIN

Dal **degli strumenti** menu, fare clic su **Library Package Manager**, quindi fare clic su **Package Manager Console**.

Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Aggiungere un HTTP Endpoint

In Esplora soluzioni espandere il progetto AzureApp. Espandere il nodo ruoli, selezionare e fare doppio clic su WorkerRole1 **proprietà**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Fare clic su **endpoint**, quindi fare clic su **Add Endpoint**.

Nel **protocollo** dall'elenco a discesa selezionare "http". Nelle **porta pubblica** e **porta privata**, digitare 80. Questi numeri di porta possono essere diversi. La porta pubblica è ciò che i client usano quando si invia una richiesta al ruolo.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Creare la classe di avvio OWIN

In Esplora soluzioni fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Add** / **classe** per aggiungere una nuova classe. Assegnare alla classe il nome `Startup`.

Sostituire tutto il codice boilerplate con gli elementi seguenti:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

Il `UseWelcomePage` metodo di estensione aggiunge una pagina HTML semplice all'applicazione, per verificare il sito sia funzionante.

## <a name="start-the-owin-host"></a>Avviare l'Host OWIN

Aprire il file WorkerRole.cs. Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.

Aggiungere la seguente istruzione using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Aggiungere un **IDisposable** membro per il `WorkerRole` classe:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

Il **WebApp.Start** metodo avvia l'host OWIN. Il nome del `Startup` classe è un parametro di tipo al metodo. Per convenzione, l'host chiamerà il `Configure` overload di questa classe.

Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Ecco il codice completo per WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compilare la soluzione e premere F5 per eseguire l'applicazione in locale nell'emulatore di calcolo di Azure. A seconda delle impostazioni di firewall, potrebbe essere necessario consentire all'emulatore attraverso il firewall.

L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint. È possibile trovare l'indirizzo IP, visualizzare l'interfaccia utente emulatore di calcolo. Fare doppio clic sull'icona dell'emulatore nella barra area di notifica delle attività e selezionare **Mostra interfaccia emulatore di calcolo**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Trovare l'indirizzo IP sotto le distribuzioni del servizio, la distribuzione [id], i dettagli del servizio. Aprire un web browser e passare a http://<em>indirizzi</em>, dove <em>indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80`. Si dovrebbe vedere la pagina di benvenuto OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

Per questo passaggio, è necessario disporre di un account Azure. Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

In Esplora soluzioni fare clic sul progetto AzureApp. Selezionare **Pubblica**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Se non è stato eseguito l'accesso al proprio account Azure, fare clic su **Accedi**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Dopo che è stato effettuato, scegliere una sottoscrizione e fare clic su **successivo**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Immettere un nome per il servizio cloud e scegliere un'area. Scegliere **Crea**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Fare clic su **Pubblica**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione. Quando l'app viene distribuita, passa a `http://appname.cloudapp.net/`, dove *appname* è il nome del servizio cloud.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Panoramica del progetto Katana](an-overview-of-project-katana.md)
- [Katana Project su GitHub](https://github.com/aspnet/AspNetKatana/)
