---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Guida introduttiva a OWIN e Katana | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a>Guida introduttiva a OWIN e Katana
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Aprire l'interfaccia Web per .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server web .NET e le applicazioni web. Separando il server web dall'applicazione, OWIN rende più semplice creare il middleware per lo sviluppo web .NET. Inoltre, OWIN semplifica porting di applicazioni web in altri host&#8212;, ad esempio, self-hosting in un servizio Windows o un altro processo.

OWIN è una specifica community di proprietà, non è un'implementazione. Il progetto Katana è un set di componenti OWIN open-source sviluppato da Microsoft. Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). In questo articolo, verrà passare direttamente al codice per iniziare.

Questa esercitazione viene utilizzato [Visual Studio 2013 versione finale candidata](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012. Alcuni dei passaggi sono diversi in Visual Studio 2012, nota di seguito.

## <a name="host-owin-in-iis"></a>Host OWIN in IIS

In questa sezione, ospitare OWIN in IIS. Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN con il set di funzionalità avanzata di IIS. Con questa opzione, l'applicazione OWIN viene eseguito nella pipeline delle richieste ASP.NET.

Innanzitutto, creare un nuovo progetto applicazione Web ASP.NET. (In Visual Studio 2012, utilizzare il tipo di progetto applicazione Web ASP.NET vuota).

![](getting-started-with-owin-and-katana/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Aggiungere pacchetti NuGet

Successivamente, aggiungere i pacchetti NuGet richiesti. Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Aggiungere una classe di avvio

Successivamente, aggiungere una classe di avvio OWIN. In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **classe di avvio di Owin**. Per ulteriori informazioni su come configurare la classe di avvio, vedere [OWIN avvio classe rilevamento](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Aggiungere al metodo `Startup1.Configuration` il codice seguente:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Questo codice aggiunge un semplice componente middleware alla pipeline OWIN, implementata come una funzione che riceve un **Microsoft.Owin.IOwinContext** istanza. Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware. Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.

> [!NOTE]
> Il modello di classe di avvio di OWIN è disponibile in Visual Studio 2013. Se si utilizza Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug. Verrà aperta una finestra del browser per `http://localhost:*port*/`. La pagina dovrebbe essere simile al seguente:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN indipendente in un'applicazione Console

È facile convertire l'applicazione da IIS che ospita in modalità self-hosting in un processo personalizzato. Con l'hosting IIS, IIS utilizzata sia come server HTTP del processo che ospita il servizio. Con self-hosting, l'applicazione viene creato il processo e utilizza il **HttpListener** classe come il server HTTP.

In Visual Studio, creare una nuova applicazione console. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Aggiungere un `Startup1` classe della parte 1 di questa esercitazione al progetto. Non è necessario modificare questa classe.

Implementare l'applicazione `Main` metodo come indicato di seguito.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando si esegue l'applicazione console, il server inizia ad ascoltare `http://localhost:9000`. Se si passa a questo indirizzo in un web browser, si verrà visualizzata la pagina "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Aggiungere OWIN Diagnostics

Il pacchetto italiano contiene middleware che intercetta le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore. Questo funziona pagina come pagina di errore ASP.NET che talvolta viene denominata il "[schermata giallo](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Ad esempio YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarla in modalità di produzione.

Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della Console di gestione pacchetti:

`install-package Microsoft.Owin.Diagnostics –Pre`

Modificare il codice del `Startup1.Configuration` metodo come indicato di seguito:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Utilizzare CTRL + F5 per eseguire l'applicazione senza eseguire il debug, in modo che Visual Studio non si interrompe l'eccezione. L'applicazione si comporta esattamente come in precedenza, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione. Il middleware di pagina di errore verrà intercettare l'eccezione e visualizzare una pagina HTML con informazioni sull'errore. È possibile scegliere le schede per visualizzare lo stack, stringa di query, i cookie, intestazione della richiesta e le variabili di ambiente OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Passaggi successivi

- [Rilevamento della classe di avvio OWIN](owin-startup-class-detection.md)
- [Consente di self-hosting ASP.NET Web API OWIN](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Utilizzare OWIN per l'hosting indipendente SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
