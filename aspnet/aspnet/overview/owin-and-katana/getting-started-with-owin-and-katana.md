---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introduzione a OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: fb3ff1d061fb89b3236a05326c1c08b0240d5a1e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377051"
---
<a name="getting-started-with-owin-and-katana"></a>Introduzione a OWIN e Katana
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface for .NET (OWIN)](http://owin.org/) definisce un'astrazione tra i server web .NET e applicazioni web. Separando il server web dell'applicazione, OWIN rende più semplice creare middleware per lo sviluppo web .NET. Inoltre, OWIN rende più semplice per trasportare le applicazioni web in altri host&#8212;, ad esempio, self-hosting in un servizio di Windows o un altro processo.

OWIN è una specifica proprietà della community, non è un'implementazione. Il progetto Katana è un set di componenti OWIN open source sviluppato da Microsoft. Per una panoramica generale di OWIN e Katana, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). In questo articolo, passerà a destra nel codice per iniziare.

Questa esercitazione viene usato [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ma è anche possibile usare Visual Studio 2012. Alcuni dei passaggi sono diversi in Visual Studio 2012, nota di seguito.

## <a name="host-owin-in-iis"></a>Hosting di OWIN in IIS

In questa sezione verranno ospitati in IIS OWIN. Questa opzione offre la flessibilità e la possibilità di composizione di una pipeline OWIN insieme ai set di funzionalità avanzata di IIS. Con questa opzione, viene eseguita l'applicazione OWIN nella pipeline delle richieste ASP.NET.

In primo luogo, creare un nuovo progetto di applicazione Web ASP.NET. (In Visual Studio 2012, utilizzare il tipo di progetto applicazione Web ASP.NET vuota).

![](getting-started-with-owin-and-katana/_static/image1.png)

Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Aggiungere i pacchetti NuGet

Successivamente, aggiungere i pacchetti NuGet necessari. Dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Aggiungere una classe di avvio

Successivamente, aggiungere una classe di avvio OWIN. In Esplora soluzioni fare clic sul progetto e selezionare **Add**, quindi selezionare **nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona **Owin Startup class**. Per altre informazioni su come configurare la classe startup, vedi [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Aggiungere al metodo `Startup1.Configuration` il codice seguente:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Questo codice aggiunge un semplice componente middleware alla pipeline OWIN, implementata come funzione che riceve un **Microsoft.Owin.IOwinContext** istanza. Quando il server riceve una richiesta HTTP, la pipeline OWIN richiama il middleware. Il middleware imposta il tipo di contenuto per la risposta e scrive il corpo della risposta.

> [!NOTE]
> Il modello di classe OWIN Startup è disponibile in Visual Studio 2013. Se si usa Visual Studio 2012, è sufficiente aggiungere una nuova classe vuota denominata `Startup1`e incollare il codice seguente:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Premere F5 per avviare il debug. Visual Studio aprirà una finestra del browser a `http://localhost:*port*/`. La pagina dovrebbe essere simile al seguente:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Self-hosting OWIN in un'applicazione Console

È facile convertire questa applicazione da IIS che ospita self-hosting in un processo personalizzato. Con l'hosting di IIS, IIS agisce come sia il server HTTP e del processo che ospita il servizio. Con self-hosting, l'applicazione crea il processo e Usa il **HttpListener** classe come il server HTTP.

In Visual Studio, creare una nuova applicazione console. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Aggiungere un `Startup1` classe della parte 1 di questa esercitazione per il progetto. Non devi modificare questa classe.

Implementare l'applicazione `Main` metodo come indicato di seguito.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Quando si esegue l'applicazione console, il server inizia ad ascoltare `http://localhost:9000`. Se si passa a questo indirizzo in un web browser, si verrà visualizzata la pagina "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Aggiungere diagnostica OWIN

Il pacchetto Microsoft.Owin.Diagnostics contiene middleware che intercetta le eccezioni non gestite e visualizza una pagina HTML con i dettagli dell'errore. Questo funziona pagina come pagina di errore ASP.NET che viene talvolta definita di "[giallo schermata](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Come YSOD, la pagina di errore Katana è utile durante lo sviluppo, ma è consigliabile disabilitarlo in modalità di produzione.

Per installare il pacchetto di diagnostica nel progetto, digitare il comando seguente nella finestra della Console di gestione pacchetti:

`install-package Microsoft.Owin.Diagnostics –Pre`

Modificare il codice di `Startup1.Configuration` metodo come indicato di seguito:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

A questo punto usare CTRL + F5 per eseguire l'applicazione senza debug, in modo che Visual Studio non si interrompe l'eccezione. L'applicazione si comporta esattamente come in precedenza, fino a quando non si passa a `http://localhost/fail`, a quel punto l'applicazione genera l'eccezione. Il middleware di pagina di errore verrà intercettare l'eccezione e visualizzare una pagina HTML con informazioni sull'errore. È possibile fare clic sulle schede per vedere dello stack, stringa di query, cookie, intestazione di richiesta e le variabili di ambiente OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Passaggi successivi

- [Rilevamento della classe di avvio OWIN](owin-startup-class-detection.md)
- [Usare OWIN per l'hosting indipendente di API Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Usare OWIN per l'hosting indipendente di SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
