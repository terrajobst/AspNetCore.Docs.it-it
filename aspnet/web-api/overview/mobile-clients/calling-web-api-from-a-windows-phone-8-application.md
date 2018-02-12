---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Chiamata di API Web da un Windows Phone 8 applicazione (c#) | Documenti Microsoft
author: rmcmurray
description: Creare uno scenario end-to-end completato, costituita da un'applicazione ASP.NET Web API che fornisce un catalogo di libri da un'applicazione Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 2025f31f369153b93cd293884880c97635fc8ab8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Chiamata di API Web da un'applicazione di Windows Phone 8 (c#)
====================
da [Robert McMurray](https://github.com/rmcmurray)

In questa esercitazione si apprenderà come creare uno scenario end-to-end completato, costituita da un'applicazione ASP.NET Web API che fornisce un catalogo di libri da un'applicazione Windows Phone 8.

### <a name="overview"></a>Panoramica

Servizi rESTful come ASP.NET Web API semplificano la creazione di applicazioni basate su HTTP per gli sviluppatori mediante l'astrazione l'architettura per applicazioni sul lato server e lato client. Anziché creare un protocollo basato sui socket proprietario per la comunicazione, gli sviluppatori di Web API semplicemente necessario pubblicare i metodi HTTP necessari per la propria applicazione, (ad esempio: GET, POST, PUT, DELETE), e solo gli sviluppatori di applicazioni client dovranno utilizzare i metodi HTTP che sono necessari per la propria applicazione.

In questa esercitazione end-to-end, si apprenderà come usare l'API Web per creare i progetti seguenti:

- Nel [prima parte di questa esercitazione](#STEP1), si creerà un'applicazione API Web ASP.NET che supporta tutte le operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) per gestire un catalogo di libri. L'applicazione utilizzerà il [File XML di esempio (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) da MSDN.
- Nel [seconda parte di questa esercitazione](#STEP2), si creerà un'applicazione Windows Phone 8 interattiva che consente di recuperare i dati dall'applicazione API Web.

#### <a name="prerequisites"></a>Prerequisiti

- Visual Studio 2013 con Windows Phone 8 SDK installato
- Windows 8 o in un secondo momento con Hyper-V sia installato un sistema a 64 bit
- Per un elenco di requisiti aggiuntivi, vedere il *requisiti di sistema* sezione la [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) pagina di download.

> [!NOTE]
> Se si desidera testare la connettività tra API Web e i progetti Windows Phone 8 nel sistema locale, è necessario seguire le istruzioni di  *[connessione l'emulatore di Windows Phone 8 per le applicazioni API Web in un locale Computer](https://go.microsoft.com/fwlink/?LinkId=324014)*  articolo per configurare l'ambiente di test.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Passaggio 1: Creazione del progetto di libreria di API Web

Il primo passaggio di questa esercitazione end-to-end consiste nel creare un progetto di API Web che supporta tutte le operazioni CRUD. Si noti che si aggiunge il progetto di applicazione Windows Phone per questa soluzione in [passaggio 2](#STEP2) di questa esercitazione.

1. Aprire **Visual Studio 2013**.
2. Fare clic su **File**, quindi **nuova**e quindi **progetto**.
3. Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **installato**, quindi **modelli**, quindi **Visual c#**e quindi **Web**.

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | Fare clic per espandere immagine |
4. Evidenziare **applicazione Web ASP.NET**, immettere **BookStore** per il nome del progetto e quindi fare clic su **OK**.
5. Quando il **nuovo progetto ASP.NET** è visualizzata la finestra di dialogo, selezionare il **API Web** modello e quindi fare clic su **OK**.

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | Fare clic per espandere immagine |
6. Quando si apre il progetto API Web, è possibile rimuovere il controller di esempio dal progetto:

    1. Espandere il **controller** cartella in Esplora soluzioni.
    2. Fare doppio clic su di **ValuesController.cs** file e quindi fare clic su **eliminare**.
    3. Fare clic su **OK** quando viene richiesto di confermare l'eliminazione.
7. Aggiungere un file di dati XML al progetto API Web. Questo file contiene il contenuto del catalogo della libreria:

    1. Fare doppio clic su di **App\_dati** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.
    2. Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, evidenziare il **File XML** modello.
    3. Nome del file **Books.xml**, quindi fare clic su **Aggiungi**.
    4. Quando il **Books.xml** file è aperto, sostituire il codice nel file con il codice XML dall'esempio **books.xml** file su MSDN: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. Salvare e chiudere il file XML.
8. Aggiungere il modello di libreria al progetto API Web. Questo modello contiene la logica di creazione, lettura, aggiornamento ed eliminazione (CRUD) per l'applicazione della libreria:

    1. Fare doppio clic su di **modelli** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **classe**.
    2. Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Aggiungi**.
    3. Quando il **BookDetails.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. Salvare e chiudere il **BookDetails.cs** file.
9. Aggiungere il controller della libreria per il progetto API Web:

    1. Fare doppio clic sul **controller** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **Controller**.
    2. Quando il **aggiungere lo scaffolding** è visualizzata la finestra di dialogo, evidenziare **Web API 2 Controller - vuoto**, quindi fare clic su **Aggiungi**.
    3. Quando il **Aggiungi Controller** è visualizzata la finestra di dialogo, denominare il controller **BooksController**, quindi fare clic su **Aggiungi**.
    4. Quando il **BooksController.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. Salvare e chiudere il **BooksController.cs** file.
10. Compilare l'applicazione API Web per controllare gli errori.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Passaggio 2: Aggiunta del progetto di catalogo della libreria di Windows Phone 8

Il passaggio successivo di questo scenario end-to-end consiste nel creare l'applicazione di catalogo per Windows Phone 8. L'applicazione utilizzerà il *Windows Phone Databound App* modello per l'interfaccia utente predefinita e si utilizzerà l'applicazione API Web che è stato creato in [passaggio 1](#STEP1) di questa esercitazione come origine dati.

1. Fare doppio clic su di **BookStore** soluzione nel in Esplora soluzioni, quindi fare clic su **Aggiungi**e quindi **nuovo progetto**.
2. Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **installato**, quindi **Visual c#**e quindi **Windows Phone**.
3. Evidenziare **Windows Phone Databound App**, immettere **BookCatalog** per il nome e quindi fare clic su **OK**.
4. Aggiungere il pacchetto NuGet di Json.NET per la **BookCatalog** progetto:

    1. Fare doppio clic su **riferimenti** per il **BookCatalog** progetto in Esplora soluzioni e quindi fare clic su **Gestisci pacchetti NuGet**.
    2. Quando il **Gestisci pacchetti NuGet** è visualizzata la finestra di dialogo, espandere il **Online** sezione ed evidenziare **nuget.org**.
    3. Immettere **Json.NET** nella ricerca e fare clic sull'icona di ricerca.
    4. Evidenziare **Json.NET** nei risultati di ricerca e quindi fare clic su **installare**.
    5. Al termine dell'installazione, fare clic su **Chiudi**.
5. Aggiungere il **BookDetails** modello per il **BookCatalog** progetto; contiene un modello generico della classe della libreria:

    1. Fare doppio clic su di **BookCatalog** progetto in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **nuova cartella**.
    2. Denominare la nuova cartella **modelli**.
    3. Fare doppio clic su di **modelli** cartella in Esplora soluzioni, quindi fare clic su **Aggiungi**, quindi fare clic su **classe**.
    4. Quando il **Aggiungi nuovo elemento** è visualizzata la finestra di dialogo, denominare il file di classe **BookDetails.cs**, quindi fare clic su **Aggiungi**.
    5. Quando il **BookDetails.cs** file è aperto, sostituire il codice nel file con le operazioni seguenti: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. Salvare e chiudere il **BookDetails.cs** file.
6. Aggiornamento di **MainViewModel.cs** classe per includere la funzionalità per comunicare con l'applicazione API Web della libreria:

    1. Espandere il **ViewModel** cartella in Esplora soluzioni e quindi fare doppio clic il **MainViewModel.cs** file.
    2. Quando il **MainViewModel.cs** file è aperto, sostituire il codice nel file con il codice seguente, si noti che è necessario aggiornare il valore del `apiUrl` costante con l'URL effettivo dell'API Web: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. Salvare e chiudere il **MainViewModel.cs** file.
7. Aggiornamento di **MainPage. XAML** file per personalizzare il nome dell'applicazione:

    1. Fare doppio clic su di **MainPage. XAML** file in Esplora soluzioni.
    2. Quando il **MainPage. XAML** file è aperto, individuare le righe di codice seguente: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. Sostituire le righe con le operazioni seguenti: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. Salvare e chiudere il **MainPage. XAML** file.
8. Aggiornamento di **DetailsPage.xaml** file per personalizzare gli elementi visualizzati:

    1. Fare doppio clic su di **DetailsPage.xaml** file in Esplora soluzioni.
    2. Quando il **DetailsPage.xaml** file è aperto, individuare le righe di codice seguente: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. Sostituire le righe con le operazioni seguenti: 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. Salvare e chiudere il **DetailsPage.xaml** file.
9. Compilare l'applicazione Windows Phone per controllare gli errori.

### <a name="step-3-testing-the-end-to-end-solution"></a>Passaggio 3: Test della soluzione End-to-End

Come indicato nella *prerequisiti* sezione di questa esercitazione, quando si testa la connettività tra API Web e Windows Phone 8 progetti nel sistema locale, è necessario seguire le istruzioni di  *[ Connessione dell'emulatore di Windows Phone 8 per le applicazioni API Web in un Computer locale](https://go.microsoft.com/fwlink/?LinkId=324014)*  articolo per configurare l'ambiente di test.

Dopo aver configurato l'ambiente di testing, è necessario impostare l'applicazione Windows Phone come progetto di avvio. A tale scopo, evidenziare il **BookCatalog** applicazione in Esplora soluzioni e quindi fare clic su **imposta come progetto di avvio**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Fare clic per espandere immagine |

Quando si preme F5, Visual Studio verrà avviato sia di Windows Phone emulatore, che verrà visualizzato un &quot;attendere&quot; messaggio mentre i dati dell'applicazione viene recuperati dall'API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Fare clic per espandere immagine |

Se tutti gli elementi ha esito positivo, verrà visualizzato il catalogo visualizzato:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Fare clic per espandere immagine |

Se si tocca su qualsiasi titolo del libro, l'applicazione verrà visualizzata la descrizione del libro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Fare clic per espandere immagine |

Se l'applicazione non può comunicare con l'API Web, verrà visualizzato un messaggio di errore:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Fare clic per espandere immagine |

Se si tocca sul messaggio di errore, verranno visualizzati eventuali dettagli aggiuntivi sull'errore:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| Fare clic per espandere immagine |
