---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Gli unit test di applicazioni SignalR | Microsoft Docs
author: pfletcher
description: Questo articolo descrive come usare le funzionalità di Testing unità di SignalR 2.0.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910824"
---
<a name="unit-testing-signalr-applications"></a>Unit test di applicazioni SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher)

> Questo articolo descrive l'utilizzo delle funzionalità Testing unità di SignalR 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Unit test di applicazioni SignalR

È possibile usare la funzionalità di unit test in SignalR 2 creare unit test per l'applicazione di SignalR. SignalR 2 include i [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaccia, che può essere utilizzato per creare un oggetto fittizio per simulare i metodi dell'hub di test.

In questa sezione si aggiungerà gli unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando [XUnit.net](https://github.com/xunit/xunit) e [Moq](https://github.com/Moq/moq4).

XUnit.net verrà usato per controllare il test. Moq consentirà di creare un [simulare](http://en.wikipedia.org/wiki/Mock_object) oggetto per il test. Se lo si desidera; è possibile utilizzare altri framework di simulazione [NSubstitute](http://nsubstitute.github.io/) è anche una buona scelta. Questa esercitazione illustra come configurare l'oggetto fittizio in due modi: innanzitutto, usando un `dynamic` oggetto (introdotta in .NET Framework 4) e i secondi, utilizzando un'interfaccia.

### <a name="contents"></a>Sommario

Questa esercitazione contiene le sezioni seguenti.

- [Unit test con Dynamic](#dynamic)
- [Unit test per tipo](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Unit test con Dynamic

In questa sezione si aggiungerà uno unit test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) usando un oggetto dinamico.

1. Installare il [estensione Runner di XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) per Visual Studio 2013.
2. Completare la [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md), o scaricare l'applicazione completata [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Se si usa la versione di download dell'applicazione introduttiva, aprire **Console di gestione pacchetti** e fare clic su **ripristinare** per aggiungere il pacchetto di SignalR per il progetto.

    ![Il ripristino dei pacchetti](unit-testing-signalr-applications/_static/image1.png)
4. Aggiungere un progetto alla soluzione per lo unit test. Fare doppio clic su della soluzione nel **Esplora soluzioni** e selezionare **Add**, **nuovo progetto...** . Sotto il **c#** nodo, seleziona la **Windows** nodo. Selezionare **libreria di classi**. Denominare il nuovo progetto **TestLibrary** e fare clic su **OK**.

    ![Creare una libreria di Test](unit-testing-signalr-applications/_static/image2.png)
5. Aggiungere un riferimento nel progetto libreria di test al progetto SignalRChat. Fare doppio clic il **TestLibrary** del progetto e selezionare **Add**, **riferimento...** . Selezionare il **progetti** nodo sotto il **soluzione** nodo e controllare **SignalRChat**. Scegliere **OK**.

    ![Aggiungi riferimento al progetto](unit-testing-signalr-applications/_static/image3.png)
6. Aggiungere i pacchetti di SignalR, Moq e XUnit per il **TestLibrary** progetto. Nel **Console di gestione pacchetti**, impostare il **progetto predefinito** elenco a discesa per **TestLibrary**. Nella finestra della console, eseguire i comandi seguenti:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installare i pacchetti](unit-testing-signalr-applications/_static/image4.png)
7. Creare il file di test. Fare doppio clic il **TestLibrary** del progetto e fare clic su **Aggiungi...** , **Classe**. Denominare la nuova classe **Tests.cs**.
8. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Nel codice precedente, viene creato un client di test usando il `Mock` dall'oggetto di [Moq](https://github.com/Moq/moq4) libreria, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1, SignalR assegnare `dynamic` per il tipo parametro). Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con il quale richiamare metodi sul client. Il `broadcastMessage` funzione viene quindi definita per il client fittizio in modo che può essere chiamato dal `ChatHub` classe. Il motore di test chiama quindi il `Send` metodo del `ChatHub` (classe), che a sua volta chiama il fittizia `broadcastMessage` (funzione).
9. Compilare la soluzione premendo **F6**.
10. Eseguire lo unit test. In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**. Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image5.png)
11. Verificare che il test superato selezionando il riquadro inferiore nella finestra Esplora Test. Nella finestra verrà visualizzato che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Unit test per tipo

In questa sezione si aggiungerà un test per l'applicazione creata nel [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md) utilizzando un'interfaccia che contiene il metodo da sottoporre a test.

1. Completare i passaggi da 1 a 7 nella [Unit test con Dynamic](#dynamic) esercitazione precedente.
2. Sostituire il contenuto di Tests.cs con il codice seguente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Nel codice precedente, viene creata un'interfaccia che definisce la firma del `broadcastMessage` metodo per il quale il motore dei test verrà creato un client fittizio. Un client fittizio viene quindi creato utilizzando il `Mock` oggetto, di tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (2.1, SignalR assegnare `dynamic` per il parametro type.) Il `IHubCallerConnectionContext` interfaccia è l'oggetto proxy con il quale richiamare metodi sul client.

    Il test quindi crea un'istanza di `ChatHub`e quindi crea una versione fittizia del `broadcastMessage` metodo, che a sua volta viene richiamata chiamando il `Send` metodo dell'hub.
3. Compilare la soluzione premendo **F6**.
4. Eseguire lo unit test. In Visual Studio, selezionare **Test**, **Windows**, **Esplora Test**. Nella finestra Esplora Test, fare doppio clic su **HubsAreMockableViaDynamic** e selezionare **Esegui test selezionati**.

    ![Esplora test](unit-testing-signalr-applications/_static/image7.png)
5. Verificare che il test superato selezionando il riquadro inferiore nella finestra Esplora Test. Nella finestra verrà visualizzato che il test è stato superato.

    ![Test superato](unit-testing-signalr-applications/_static/image8.png)
