---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata di API Web da un Windows Phone 8 applicazione (c#) | Microsoft Docs
author: rmcmurray
description: Crea uno scenario end-to-end completato costituita da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 40be2935c52e7dcab9e682d4d15e9e75c0260223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388422"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Chiamata di API Web da un'applicazione di 8 Windows Phone (c#)
====================
da [Robert McMurray](https://github.com/rmcmurray)

In questa esercitazione si apprenderà come creare uno scenario end-to-end completato costituita da un'applicazione API Web ASP.NET che fornisce un catalogo di libri a un'applicazione Windows Phone 8.

### <a name="overview"></a>Panoramica

Servizi rESTful, ad esempio API Web ASP.NET semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori tramite l'astrazione l'architettura per le applicazioni lato client e lato server. Anziché creare un protocollo basato sui socket proprietario per la comunicazione, gli sviluppatori di API Web devono semplicemente pubblicare i metodi HTTP necessari per la propria applicazione (ad esempio: GET, POST, PUT, DELETE), e gli sviluppatori di applicazioni client solo bisogno di utilizzarlo i metodi HTTP che sono necessari per la propria applicazione.

In questa esercitazione end-to-end, si apprenderà come usare l'API Web per creare i progetti seguenti:

- Nel [prima parte di questa esercitazione](#STEP1), si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per gestire un catalogo di libro. L'applicazione utilizzerà il [esempio di File XML (books. XML)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) da MSDN.
- Nel [seconda parte di questa esercitazione](#STEP2), si creerà un'applicazione Windows Phone 8 interattiva che consente di recuperare i dati dall'applicazione API Web.

#### <a name="prerequisites"></a>Prerequisiti

- Visual Studio 2013 con Windows Phone 8 SDK installato
- Windows 8 o versioni successive in un sistema a 64 bit con installato Hyper-V
- Per un elenco di requisiti aggiuntivi, vedere la *requisiti di sistema* sezione il [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) pagina di download.

> [!NOTE]
> Se si desidera testare la connettività tra API Web e i progetti Windows Phone 8 nel sistema locale, è necessario seguire le istruzioni di *[ci si connette all'emulatore di Windows Phone 8 alle applicazioni API Web in un locale Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Passaggio 1: Creazione del progetto della libreria di API Web

Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto API Web che supporta tutte le operazioni CRUD. Si noti che si aggiunge il progetto di applicazione Windows Phone alla soluzione in [passaggio 2](#STEP2) di questa esercitazione.

1. Aprire **Visual Studio 2013**.
2. Fare clic su **File**, quindi **nuove**e quindi **progetto**.
3. Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, espandere **installati**, quindi **modelli**, quindi **Visual c#** e quindi **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Fare clic sull'immagine per espandere                                                                |


4. Evidenziare **applicazione Web ASP.NET**, immettere **BookStore** per il nome del progetto e quindi fare clic su **OK**.
5. Quando la **nuovo progetto ASP.NET** verrà visualizzata la finestra di dialogo, selezionare la **API Web** modello e quindi fare clic su **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Fare clic sull'immagine per espandere                                                                |


6. Quando si apre il progetto API Web, rimuovere il controller di esempio dal progetto:

    1. Espandere la **controller** cartella in Esplora soluzioni.
    2. Fare doppio clic il **ValuesController.cs** del file e quindi fare clic su **eliminare**.
    3. Fare clic su **OK** quando viene richiesto di confermare l'eliminazione.
7. Aggiungere un file di dati XML al progetto API Web. Questo file contiene il contenuto del catalogo della libreria:

   1. Fare doppio clic sui **App\_dati** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **nuovo elemento**.
   2. Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, evidenziare il **File XML** modello.
   3. Denominare il file **books. XML**, quindi fare clic su **Add**.
   4. Quando la **books. XML** apertura file, sostituire il codice nel file con il codice XML di esempio **books. XML** file su MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Salvare e chiudere il file XML.

8. Aggiungere il modello della libreria al progetto API Web. Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione della libreria:

   1. Fare doppio clic sui **modelli** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **classe**.
   2. Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Add**.
   3. Quando la **BookDetails.cs** file viene aperto, sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Salvare e chiudere il **BookDetails.cs** file.

9. Aggiungere il controller della libreria al progetto API Web:

   1. Fare doppio clic sul **controller** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **Controller**.
   2. Quando la **Add Scaffold** verrà visualizzata la finestra di dialogo, evidenziare **Web API 2 Controller - Empty**, quindi fare clic su **Add**.
   3. Quando la **Aggiungi Controller** verrà visualizzata la finestra di dialogo, denominare il controller **BooksController**, quindi fare clic su **Add**.
   4. Quando la **BooksController.cs** file viene aperto, sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Salvare e chiudere il **BooksController.cs** file.

10. Compilare l'applicazione API Web per verificare la presenza di errori.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Passaggio 2: Aggiungere il progetto di catalogo della libreria di Windows Phone 8

Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8. L'applicazione utilizzerà il *Windows Phone Databound App* modello per l'interfaccia utente predefinita che utilizzerà l'applicazione API Web creato nel [passaggio 1](#STEP1) di questa esercitazione come origine dati.

1. Fare doppio clic sul **BookStore** soluzione nel in Esplora soluzioni, quindi fare clic su **Add**e quindi **nuovo progetto**.
2. Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, espandere **installati**, quindi **Visual c#** e quindi **Windows Phone**.
3. Evidenziare **Windows Phone Databound App**, immettere **BookCatalog** per il nome e quindi fare clic su **OK**.
4. Quando si preme F5, Visual Studio avvierà sia il Windows Phone emulatore, che verrà visualizzato un **attendere** del messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:

    1. Se tutto ciò che ha esito positivo, verrà visualizzato il catalogo visualizzato:
    2. Se si tocca su qualsiasi titolo del libro, l'applicazione visualizzerà la descrizione del libro:
    3. Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:
    4. Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:
    5. Al termine dell'installazione, fare clic su **Chiudi**.
5. Aggiungere il **BookDetails** del modello per il **BookCatalog** progetto; contiene un modello generico della classe della libreria:

   1. Fare doppio clic sui **BookCatalog** del progetto in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **nuova cartella**.
   2. Denominare la nuova cartella **modelli**.
   3. Fare doppio clic sui **modelli** cartella in Esplora soluzioni, quindi fare clic su **Add**, quindi fare clic su **classe**.
   4. Quando la **Aggiungi nuovo elemento** verrà visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Add**.
   5. Quando la **BookDetails.cs** file viene aperto, sostituire il codice nel file con il codice seguente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Salvare e chiudere il **BookDetails.cs** file.

6. Aggiorna il **MainViewModel.cs** classe per includere la funzionalità per comunicare con l'applicazione API Web della libreria:

   1. Espandere la **ViewModel** cartella in Esplora soluzioni e quindi fare doppio clic il **MainViewModel.cs** file.
   2. Quando la **MainViewModel.cs** file viene aperto, sostituire il codice nel file con il codice seguente; si noti che sarà necessario aggiornare il valore della `apiUrl` costanti con l'URL effettivo dell'API Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Salvare e chiudere il **MainViewModel.cs** file.

7. Aggiorna il **MainPage. XAML** . ini per personalizzare il nome dell'applicazione:

   1. Fare doppio clic il **MainPage. XAML** file in Esplora soluzioni.
   2. Quando la **MainPage. XAML** file viene aperto, individuare le righe di codice seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Sostituire le righe con gli elementi seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Salvare e chiudere il **MainPage. XAML** file.

8. Aggiorna il **DetailsPage.xaml** . ini per personalizzare gli elementi visualizzati:

   1. Fare doppio clic il **DetailsPage.xaml** file in Esplora soluzioni.
   2. Quando la **DetailsPage.xaml** file viene aperto, individuare le righe di codice seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Sostituire le righe con gli elementi seguenti: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Salvare e chiudere il **DetailsPage.xaml** file.

9. Compilare l'applicazione Windows Phone per individuare eventuali errori.

### <a name="step-3-testing-the-end-to-end-solution"></a>Passaggio 3: Test della soluzione End-to-End

Come accennato nel *prerequisiti* sezione di questa esercitazione, quando si testa la connettività tra l'API Web e Windows Phone 8 i progetti nel sistema locale, sarà necessario seguire le istruzioni riportate nel *[ Ci si connette all'emulatore di Windows Phone 8 alle applicazioni API Web in un Computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)* articolo per configurare l'ambiente di test.

Dopo aver configurato l'ambiente di testing, è necessario impostare l'applicazione Windows Phone come progetto di avvio. A tale scopo, evidenziare il **BookCatalog** dell'applicazione in Esplora soluzioni e quindi fare clic su **imposta come progetto di avvio**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Fare clic sull'immagine per espandere |

Quando si preme F5, Visual Studio avvierà sia il Windows Phone emulatore, che verrà visualizzato un &quot;attendere&quot; del messaggio mentre i dati dell'applicazione vengono recuperati dall'API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Fare clic sull'immagine per espandere |

Se tutto ciò che ha esito positivo, verrà visualizzato il catalogo visualizzato:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Fare clic sull'immagine per espandere |

Se si tocca su qualsiasi titolo del libro, l'applicazione visualizzerà la descrizione del libro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Fare clic sull'immagine per espandere |

Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Fare clic sull'immagine per espandere |

Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Fare clic sull'immagine per espandere                                                                 |

