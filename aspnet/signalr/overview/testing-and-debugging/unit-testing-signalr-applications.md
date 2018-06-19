---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Unit test delle applicazioni SignalR | Documenti Microsoft
author: pfletcher
description: In questo articolo viene descritto come utilizzare le funzionalità di Unit test di SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870843"
---
<a name="unit-testing-signalr-applications"></a>Unit test delle applicazioni SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo articolo descrive l'uso delle funzionalità di Unit test di SignalR 2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Unit test delle applicazioni SignalR

È possibile utilizzare le funzionalità di unit test in SignalR 2 per creare unit test per l'applicazione di SignalR. SignalR 2 include i [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaccia, che può essere utilizzato per creare un oggetto fittizio per simulare i metodi dell'hub di test.

In questa sezione aggiungerai unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando [XUnit.net](https://github.com/xunit/xunit) e [Moq](https://github.com/Moq/moq4).

XUnit.net verrà utilizzato per controllare il test. Moq verrà usato per creare un [simulare](http://en.wikipedia.org/wiki/Mock_object) oggetto per il test. Se si desidera; è possibile utilizzare altri framework di simulazione [NSubstitute](http://nsubstitute.github.io/) è anche una buona scelta. Questa esercitazione viene illustrato come impostare l'oggetto fittizio in due modi: in primo luogo, usando un `dynamic` oggetto (introdotto in .NET Framework 4) e i secondi, utilizzando un'interfaccia.

### <a name="contents"></a>Sommario

In questa esercitazione include le sezioni seguenti.

- [Unit test con dinamico](#dynamic)
- [Unit test dal tipo](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Unit test con dinamico

In questa sezione si aggiungerà uno unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un oggetto dinamico.

1. Installare il [estensione XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.
2. Completare il [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare l'applicazione completata dal [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Se si utilizza la versione di download dell'applicazione Guida introduttiva, aprire **Package Manager Console** e fare clic su **ripristinare** per aggiungere il pacchetto di SignalR per il progetto.

    ![Ripristino dei pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. Aggiungere un progetto alla soluzione per lo unit test. Fare clic sulla soluzione in **Esplora** e selezionare **Aggiungi**, **nuovo progetto...** . Sotto il **c#** nodo, seleziona il **Windows** nodo. Selezionare **libreria di classi**. Denominare il nuovo progetto **TestLibrary** e fare clic su **OK**.

    ![Creare una libreria di Test](unit-testing-signalr-applications/_static/image2.png)
5. Aggiungere un riferimento nel progetto libreria di test al progetto SignalRChat. Fare doppio clic su di **TestLibrary** del progetto e selezionare **Aggiungi**, **riferimento...** . Selezionare il **progetti** nodo sotto il **soluzione** nodo e controllare **SignalRChat**. Fare clic su **OK**.

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. Aggiungere i pacchetti SignalR, Moq e XUnit il **TestLibrary** progetto. Nel **Package Manager Console**, impostare il **progetto predefinito** elenco a discesa per **TestLibrary**. Nella finestra della console, eseguire i comandi seguenti:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installare i pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. Creare il file di test. Fare doppio clic su di **TestLibrary** sul progetto e scegliere **Aggiungi...** , **Classe**. Denominare la nuova classe **Tests.cs**.
8. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Nel codice precedente, viene creato un client di prova utilizzando il `Mock` dall'oggetto di [Moq](https://github.com/Moq/moq4) libreria, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, assegnare `dynamic` per il tipo parametro). Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con cui per richiamare metodi sul client. Il `broadcastMessage` funzione viene quindi definita per il client fittizio in modo che può essere chiamato dalla `ChatHub` classe. Chiama quindi il motore dei test di `Send` metodo il `ChatHub` (classe), che a sua volta chiama il simulati `broadcastMessage` (funzione).
9. Compilare la soluzione premendo **F6**.
10. Eseguire lo unit test. In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**. Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. Verificare che il test superato controllando riquadro in basso nella finestra Esplora Test. Nella finestra verrà visualizzato che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Unit test dal tipo

In questa sezione si aggiungerà un test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un'interfaccia che contiene il metodo da testare.

1. Completare i passaggi 1-7 nel [Unit test con Dynamic](#dynamic) esercitazione precedente.
2. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Nel codice precedente, viene creata un'interfaccia definisce la firma del `broadcastMessage` metodo per il quale il motore di test verrà creato un client fittizio. Un client fittizio viene quindi creato utilizzando il `Mock` , oggetto di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, assegnare `dynamic` per il parametro di tipo.) Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con cui per richiamare metodi sul client.

    Il test quindi crea un'istanza di `ChatHub`e quindi crea una versione fittizia del `broadcastMessage` metodo, che a sua volta viene richiamata chiamando il `Send` metodo dell'hub.
3. Compilare la soluzione premendo **F6**.
4. Eseguire lo unit test. In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**. Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. Verificare che il test superato controllando riquadro in basso nella finestra Esplora Test. Nella finestra verrà visualizzato che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
