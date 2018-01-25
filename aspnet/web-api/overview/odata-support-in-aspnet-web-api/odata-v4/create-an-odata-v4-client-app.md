---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creare un'App Client di OData v4 (c#) | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>Creare un'App Client di OData v4 (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

Nell'esercitazione precedente, creato un servizio OData di base che supporta operazioni CRUD. Ora è possibile crearne un client per il servizio.

Avviare una nuova istanza di Visual Studio e creare un nuovo progetto applicazione console. Nel **nuovo progetto** finestra di dialogo Seleziona **installato** &gt; **modelli** &gt; **Visual c#** &gt; **Windows Desktop**e selezionare il **applicazione Console** modello. Denominare il progetto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> È anche possibile aggiungere l'applicazione console alla stessa soluzione di Visual Studio che contiene il servizio OData.


## <a name="install-the-odata-client-code-generator"></a>Installare il generatore di codice Client OData

Dal **strumenti** dal menu **estensioni e aggiornamenti**. Selezionare **Online** &gt; **Visual Studio Gallery**. Nella casella di ricerca, cercare &quot;generatore di codice Client OData&quot;. Fare clic su **scaricare** per installare l'estensione VSIX. Potrebbe essere necessario riavviare Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Esegue il servizio OData in locale

Eseguire il progetto ProductService da Visual Studio. Per impostazione predefinita, Visual Studio avvia un browser per la radice dell'applicazione. Prendere nota dell'URI; è necessario nel passaggio successivo. Lasciare l'applicazione in esecuzione.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Se si inseriscono entrambi i progetti nella stessa soluzione, assicurarsi di eseguire il progetto ProductService senza debug. Nel passaggio successivo, sarà necessario mantenere il servizio in esecuzione mentre si modifica il progetto di applicazione console.


## <a name="generate-the-service-proxy"></a>Generare il Proxy del servizio

Il proxy del servizio è una classe .NET che definisce i metodi per l'accesso al servizio OData. Il proxy converte le chiamate di metodo in richieste HTTP. Si creerà la classe proxy eseguendo un [modello T4](https://msdn.microsoft.com/library/bb126445.aspx).

Fare clic sul progetto. Selezionare **aggiungere** &gt; **nuovo elemento**.

![](create-an-odata-v4-client-app/_static/image5.png)

Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **elementi di Visual c#** &gt; **codice** &gt; **OData Client**. Nome del modello &quot;ProductClient.tt&quot;. Fare clic su **Aggiungi** e fare clic su tramite l'avviso di sicurezza.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

A questo punto, si otterrà un errore, che è possibile ignorare. Visual Studio esegue automaticamente il modello, ma il modello debba essere alcune impostazioni di configurazione prima.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Aprire il file ProductClient.odata.config. Nel `Parameter` elemento Incolla dell'URI dal progetto ProductService (passaggio precedente). Ad esempio:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Eseguire nuovamente il modello. In Esplora soluzioni, fare clic con il pulsante destro del file ProductClient.tt e selezionare **Esegui strumento personalizzato**.

Il modello crea un file di codice denominato ProductClient.cs che definisce il proxy. Quando si sviluppa l'app, se si modifica l'endpoint OData, eseguire il modello di nuovo per aggiornare il proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Utilizzare il Proxy del servizio per chiamare il servizio OData

Aprire il file Program.cs e sostituire il codice boilerplate con le operazioni seguenti.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Sostituire il valore di *serviceUri* con l'URI del servizio da versioni precedenti.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Quando si esegue l'app, deve restituire le operazioni seguenti:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
