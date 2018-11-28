---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni dettagliata insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET mediante ASP.NET 4.5 e Microsoft Visual Studio Express...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450684"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Questa serie di esercitazioni dettagliata insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. 

## <a name="introduction"></a>Introduzione

Questa serie di esercitazioni consente di eseguire in modo semplificato i passaggi necessari per creare un'applicazione Web Form ASP.NET con Visual Studio Express 2013 per Web e ASP.NET 4.5.

L'applicazione verrà creata è denominata **Wingtip Toys**. È un esempio semplificato di un sito web di front-store che vende elementi online. Questa serie di esercitazioni vengono evidenziate nuove funzionalità disponibili in ASP.NET 4.5.

Sono Benvenuti i commenti e ci assicureremo che ogni sforzo per aggiornare i suggerimenti in base a questa serie di esercitazioni.

### <a name="download-completed-project"></a>Download completato progetto

È possibile scaricare un progetto c# che contiene l'esercitazione completata.

- [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Esaminare il contenuto eseguendo il quiz correlato di Web Form ASP.NET

Dopo aver completato questa esercitazione, verificare le proprie conoscenze e rafforzano i concetti chiave eseguendo il [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Questo test è stato progettato specificatamente dal contenuto di questa serie di esercitazioni. Ogni domanda il quiz fornisce una spiegazione oltre a collegamenti a indicazioni aggiuntive.

- [Web Form ASP.NET Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Destinatari

I destinatari di questa serie di esercitazioni sono gli sviluppatori esperti che hanno familiarità con Web Form ASP.NET. Un sviluppatore di questa serie di esercitazioni deve avere le capacità seguenti:

- Che hanno familiarità con un oggetto orientata ai servizi di linguaggio di programmazione (OOP)
- Familiarità con concetti di sviluppo Web (HTML, CSS, JavaScript)
- Familiarità con i concetti di database relazionale
- Familiarità con i concetti di architettura a più livelli

Se si è interessati a esaminare le aree elencate in precedenza, è consigliabile rivedere il contenuto seguente:

- [Introduzione a Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Sviluppo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database)
- [Architettura a più livelli](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funzionalità dell'applicazione

Le funzionalità di Web Form ASP.NET, presentate in questa serie includono:

- Il progetto di applicazione Web (non il progetto sito Web)
- Web Form
- Pagine master, configurazione
- Bootstrap
- Entity Framework Code First, Local DB
- Convalida delle richieste
- Controlli dati, fortemente tipizzati del modello di associazione, le annotazioni dei dati e il valore di provider
- SSL e OAuth
- ASP.NET Identity, la configurazione e autorizzazione
- Convalida discreta
- Routing
- Gestione degli errori di ASP.NET

### <a name="application-scenarios-and-tasks"></a>Attività e gli scenari di applicazione

Le attività illustrate in questa serie di includono:

- Creazione, la revisione ed esecuzione del nuovo progetto
- Creazione della struttura del database
- L'inizializzazione e il seeding del database
- Personalizzazione dell'interfaccia utente utilizzando gli stili, grafica e una pagina master
- Aggiunta di pagine e navigazione
- Visualizzazione dettagli di menu e i dati del prodotto
- Creazione di un carrello acquisti
- Supporto SSL aggiunta e OAuth
- Aggiunta di un metodo di pagamento
- Tra cui un ruolo di amministratore e un utente all'applicazione
- Limitare l'accesso a pagine specifiche e cartella
- Caricare un file all'applicazione web
- Implementazione della convalida dell'input
- La registrazione delle route per l'applicazione web
- Implementazione della gestione degli errori e registrazione degli errori

## <a name="overview"></a>Panoramica

Se si ha familiarità con ASP.NET Web Forms ma hanno familiarità con i concetti di programmazione, è necessario l'esercitazione a destra. Se si ha già familiarità con Web Form ASP.NET, è possibile trarre vantaggio da questa serie di esercitazioni per le nuove funzionalità disponibili in ASP.NET 4.5. Se non si ha familiarità con la programmazione di concetti e Web Form ASP.NET, vedere le esercitazioni aggiuntive fornite in Web Form [introduttiva](../../../index.md) sezione sul sito Web ASP.NET.

Le specifiche **più recente** ASP.NET 4.5 le funzionalità fornite in questo Web Form serie di esercitazioni includono quanto segue:

- Una semplice interfaccia utente per la creazione di progetti offerta [supporto per più framework ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un framework di layout e temi che fornisce funzionalità reattive di progettazione e dei temi.
- [ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software diversi da IIS di hosting web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), gli oggetti di un aggiornamento a Entity Framework che consente di recuperare e manipolare i dati fortemente tipizzati, accedere ai dati in modo asincrono, gestire errori di connessione temporanei e registrare le istruzioni SQL.

Per un elenco completo delle funzionalità ASP.NET 4.5, vedere [ASP.NET and Web Tools per Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>L'applicazione di esempio Wingtip Toys

Le schermate seguenti forniscono una visualizzazione rapida dell'applicazione Web Form ASP.NET che verrà creato in questa serie di esercitazioni. Quando si esegue l'applicazione da Visual Studio Express 2013 per Web, si verrà visualizzata la pagina di iniziale web seguenti.

![Wingtip Toys - pagina predefinita](introduction-and-overview/_static/image1.png)

È possibile registrare come un nuovo utente, o accedere come un utente esistente. Navigazione viene fornita nella parte superiore per ogni categoria di prodotti mediante il recupero di prodotti disponibili dal database.

Se si seleziona il collegamento di prodotti, sarà in grado di visualizzare un elenco di tutti i prodotti disponibili.

![Wingtip Toys - prodotti](introduction-and-overview/_static/image2.png)

È anche possibile visualizzare i dettagli di singoli prodotti selezionando tutti i prodotti elencati.

![Wingtip Toys - dettagli sul prodotto](introduction-and-overview/_static/image3.png)

Come un utente, è possibile registrare e accedere con le funzionalità predefinite del modello Web Form. Questa esercitazione illustra anche come eseguire l'accesso con un account Gmail esistente. Inoltre, è possibile accedere come amministratore di aggiungere e rimuovere i prodotti dal database.

![Wingtip Toys - Log in](introduction-and-overview/_static/image4.png)

Dopo aver eseguito l'accesso con un account utente, è possibile aggiungere prodotti al carrello acquisti e completamento della transazione con PayPal. Si noti che questa applicazione di esempio è progettata per funzionare con sandbox di sviluppo di PayPal. Nessuna transazione denaro effettivamente avrà luogo.

![Wingtip Toys - carrello della spesa](introduction-and-overview/_static/image5.png)

PayPal confermerà l'account, l'ordine e le informazioni di pagamento.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Dopo la restituzione da PayPal, è possibile esaminare e completare il tuo ordine.

![Wingtip Toys - verifica ordine](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di aver installato nel computer il software seguente:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oppure [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente.

Questa serie di esercitazioni viene utilizzato Microsoft Visual Studio Express 2013 per Web. È possibile utilizzare Microsoft Visual Studio Express 2013 per il Web o Microsoft Visual Studio 2013 per il completamento di questa serie di esercitazioni.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.


Se è già installata una versione di Visual Studio, il processo di installazione verrà installato Visual Studio 2013 o Microsoft Visual Studio Express 2013 per Web accanto alla versione esistente. Siti creati nelle versioni precedenti possono essere aperti in Visual Studio 2013 e continuano ad aprire nelle versioni precedenti.

> [!NOTE] 
> 
> Questa procedura dettagliata si presume che il *lo sviluppo Web* raccolta di impostazioni la prima volta che è stato avviato Visual Studio. Per altre informazioni, vedere [procedura: selezionare impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il nuovo progetto Web che viene presentato in questa serie di esercitazioni. Se si vuole **facoltativamente** eseguire l'applicazione di esempio che crea questa serie di esercitazioni, è possibile scaricarlo dal sito di esempi MSDN. Questo download contiene quanto segue:

- L'applicazione di esempio nel *WingtipToys* cartella.
- Le risorse usate per creare l'applicazione di esempio nel *WingtipToys-asset* cartella le *WingtipToys* cartella.

#### <a name="download-the-file-from-msdn-samples-site"></a>Scaricare il file dal sito degli esempi di MSDN:

[Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

Il download è un <em>zip</em> file. Per visualizzare il progetto completato creato questa serie di esercitazioni, trovare e selezionare il <em>c#</em>cartella le <em>zip</em> file. Salvare il <em>c#</em> la cartella nella cartella utilizzata per lavorare con i progetti di Visual Studio 2013. Per impostazione predefinita, la cartella di progetti di Visual Studio 2013 è la seguente:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Rinominare il ***c#*** cartella in cui ***WingtipToys***.

> [!NOTE]
> Se si dispone già di una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente la cartella esistente prima di rinominare il *c#* cartella in cui *WingtipToys*.


Per eseguire il progetto completato, aprire il *WingtipToys* cartella e fare doppio clic il *WingtipToys.sln* file. Visual Studio 2013 verrà aperto il progetto. Successivamente, fare doppio clic il *default. aspx* file nella finestra Esplora soluzioni e fare clic su Visualizza nel Browser dal menu di scelta rapida.

### <a name="tutorial-support-and-comments"></a>I commenti e supporto dell'esercitazione

Usare la sezione di domande e risposte inclusa con il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) esempio (c#) per eventuali domande o commenti.

Sono Benvenuti i commenti su questa serie di esercitazioni e quando questa serie di esercitazioni viene aggiornata verrà resa ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

Quando si verifica un errore durante lo sviluppo, se il sito Web non viene eseguito correttamente, i messaggi di errore possono dare indicazioni complesse per l'origine del problema oppure potrebbero non illustra come risolverlo. Per aiutarti con alcuni scenari comuni di problema, è anche possibile usare la [forum ASP.NET](https://forums.asp.net/) o nella sezione di domande e risposte incluso con il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) esempio. Se viene visualizzato un messaggio di errore o qualcosa non funziona come eseguono le esercitazioni, assicurarsi di controllare i percorsi precedenti.

> [!div class="step-by-step"]
> [avanti](create-the-project.md)
