---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni dettagliata verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni dettagliata verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. [Web Form ASP.NET Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Testare la conoscenza e rafforzare i concetti chiave eseguendo il Quiz di ASP.NET Web Form. Questo quiz è stato progettato in modo specifico dal contenuto di questa serie di esercitazioni. Ogni domanda il quiz viene fornita una spiegazione vengono forniti collegamenti a informazioni aggiuntive.


## <a name="introduction"></a>Introduzione

Questa serie di esercitazioni in modo semplificato i passaggi necessari per creare un'applicazione Web Form ASP.NET utilizzando Visual Studio Express 2013 per Web e ASP.NET 4.5.

L'applicazione che creerai denominato **Wingtip Toys**. È un esempio semplificato di un sito web di front-archivio che vende articoli online. Questa serie di esercitazioni evidenzia nuove caratteristiche disponibili in ASP.NET 4.5.

I commenti sono iniziale e ci accerteremo ogni sforzo per aggiornare questa serie di esercitazioni in base a suggerimenti.

### <a name="download-completed-project"></a>Download completato progetto

È possibile scaricare un progetto c# che contiene l'esercitazione completata.

- [Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Esaminare il contenuto effettuando il quiz di Web Form ASP.NET correlato

Dopo aver completato questa esercitazione, testare la conoscenza e rafforzare i concetti chiave eseguendo il [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Questo quiz è stato progettato in modo specifico dal contenuto di questa serie di esercitazioni. Ogni domanda il quiz viene fornita una spiegazione vengono forniti collegamenti a informazioni aggiuntive.

- [Web Form ASP.NET Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Destinatari

Il gruppo di destinatari di questa serie di esercitazioni è sviluppatori esperti nuovi utenti di Web Form ASP.NET. Gli sviluppatori interessati a questa serie di esercitazioni devono avere le seguenti aree:

- Che hanno familiarità con un oggetto orientata ai servizi di linguaggio di programmazione (OOP)
- Familiarità con concetti di sviluppo Web (HTML, CSS, JavaScript)
- Familiarità con i concetti di database relazionale
- Familiarità con concetti dell'architettura a più livelli

Se si desidera esaminare le aree elencate in precedenza, è consigliabile rivedere il contenuto seguente:

- [Introduzione a Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Sviluppo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Database relazionale](http://en.wikipedia.org/wiki/Relational_database)
- [Architettura a più livelli](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funzionalità dell'applicazione

Le funzionalità di Web Form ASP.NET presentate in questa serie includono:

- Il progetto di applicazione Web (non un progetto di sito Web)
- Web Form
- Pagine master, configurazione
- Bootstrap
- Entity Framework prima di tutto, codice del database locale
- Convalida della richiesta
- Fortemente tipizzato, i controlli di dati del modello di associazione, le annotazioni dei dati e il valore di provider
- SSL e OAuth
- ASP.NET Identity, configurazione e l'autorizzazione
- Convalida non intrusiva
- Routing
- Gestione degli errori ASP.NET

### <a name="application-scenarios-and-tasks"></a>Attività e gli scenari di applicazione

Le attività illustrate in questa serie includono:

- Creazione, la revisione ed esecuzione del nuovo progetto
- Creazione della struttura di database
- L'inizializzazione e il seeding del database
- Personalizzare l'interfaccia utente utilizzando gli stili, grafica e una pagina master
- Aggiunta di pagine e navigazione
- Visualizzazione dei dettagli di menu e i dati di prodotto
- Creazione di un carrello acquisti
- Supporto SSL aggiunta e OAuth
- Aggiunta di un metodo di pagamento
- Tra cui un ruolo di amministratore e un utente per l'applicazione
- Limitare l'accesso a pagine specifiche e cartella
- Caricamento di un file all'applicazione web
- Implementazione di convalida dell'input
- La registrazione delle route per l'applicazione web
- Implementazione della gestione degli errori e registrazione degli errori

## <a name="overview"></a>Panoramica

Se si ha familiarità con i Web Form ASP.NET ma hanno una certa familiarità con i concetti di programmazione, è necessario l'esercitazione di destra. Se si ha già familiarità con i Web Form ASP.NET, è possibile trarre vantaggio da questa serie di esercitazioni dalle nuove funzionalità disponibili in ASP.NET 4.5. Se non si ha familiarità con concetti e Web Form ASP.NET di programmazione, vedere le esercitazioni aggiuntive fornite in Web Form [Introduzione](../../../index.md) sezione sul sito Web ASP.NET.

La specifica **più recente** ASP.NET 4.5 funzionalità fornite in questo Web Form serie di esercitazioni includono quanto segue:

- Una semplice interfaccia utente per la creazione di progetti dell'offerta [il supporto per più framework ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e Web API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un framework di layout e i temi che fornisce funzionalità di progettazione e i temi reattiva.
- [ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona allo stesso in tutti i framework ASP.NET e funziona con software diversi da IIS di hosting web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), gli oggetti di un aggiornamento a Entity Framework che consente di recuperare e manipolare i dati fortemente tipizzati, accedere ai dati in modo asincrono, gestire errori di connessione temporanei e registrare le istruzioni SQL.

Per un elenco completo delle funzionalità ASP.NET 4.5, vedere [ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>L'applicazione di esempio Wingtip Toys

Le schermate seguenti forniscono una visualizzazione rapida dell'applicazione Web ASP.NET form che si creerà in questa serie di esercitazioni. Quando si esegue l'applicazione da Visual Studio Express 2013 per Web, si visualizzeranno la Home page web seguenti.

![Wingtip Toys - pagina predefinita](introduction-and-overview/_static/image1.png)

È possibile registrare un nuovo account utente o accedere come un utente esistente. Navigazione viene fornita nella parte superiore per ogni categoria di prodotto tramite il recupero dei prodotti disponibili dal database.

Se si seleziona il collegamento di prodotti, sarà in grado di visualizzare un elenco di tutti i prodotti disponibili.

![Wingtip Toys - prodotti](introduction-and-overview/_static/image2.png)

È inoltre possibile visualizzare i dettagli di singoli prodotti selezionando uno dei prodotti elencati.

![Wingtip Toys - dettagli sul prodotto](introduction-and-overview/_static/image3.png)

Come un utente, è possibile registrare e accedere con la funzionalità predefinita del modello Web Form. In questa esercitazione viene inoltre illustrato come eseguire l'accesso utilizzando un account Gmail esistente. Inoltre, può eseguire l'accesso come amministratore di aggiungere e rimuovere i prodotti dal database.

![Wingtip Toys - Log in](introduction-and-overview/_static/image4.png)

Una volta che è connessi come un utente, è possibile aggiungere i prodotti per il carrello acquisti e l'estrazione con PayPal. Si noti che questa applicazione di esempio è progettata per funzionare con sandbox per sviluppatori di PayPal. Nessuna transazione money effettivo verrà eseguito.

![Wingtip Toys - carrello degli acquisti](introduction-and-overview/_static/image5.png)

L'account, l'ordine e le informazioni di pagamento PayPal confermare.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Dopo la restituzione da PayPal, è possibile esaminare e completare l'ordine.

![Wingtip Toys - ordine revisione](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, assicurarsi di aver installato nel computer il software seguente:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente.

Questa serie di esercitazioni utilizza Microsoft Visual Studio Express 2013 per Web. Per completare questa serie di esercitazioni, è possibile utilizzare Microsoft Visual Studio Express 2013 per il Web o Microsoft Visual Studio 2013.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.


Se è già installata una versione di Visual Studio, il processo di installazione installerà Visual Studio 2013 o Microsoft Visual Studio Express 2013 per Web accanto alla versione esistente. I siti creati nelle versioni precedenti possono essere aperto in Visual Studio 2013 e continuano a aprire nelle versioni precedenti.

> [!NOTE] 
> 
> Questa procedura dettagliata si presuppone che sia selezionato il *lo sviluppo Web* raccolta di impostazioni la prima volta che viene avviato Visual Studio. Per ulteriori informazioni, vedere [come: Seleziona impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il nuovo progetto Web che viene presentato in questa serie di esercitazioni. Se si desidera **facoltativamente** eseguire l'applicazione di esempio che crea questa serie di esercitazioni, è possibile scaricarlo dal sito di esempi MSDN. Questo download contiene quanto segue:

- L'applicazione di esempio di *WingtipToys* cartella.
- Le risorse utilizzate per creare l'applicazione di esempio nella *WingtipToys asset* cartella la *WingtipToys* cartella.

#### <a name="download-the-file-from-msdn-samples-site"></a>Scaricare il file dal sito di esempi MSDN:

[Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

Il download è un *zip* file. Per visualizzare il progetto completato crea questa serie di esercitazioni, trovare e selezionare il *c#*cartella la *zip* file. Salvare il *c#* la cartella in cartella consentono di lavorare con progetti di Visual Studio 2013. Per impostazione predefinita, la cartella di progetti di Visual Studio 2013 è la seguente:

**C:\Users\*****&lt;username&gt;* * * \Documents\Visual 2013\Projects Studio**

Rinominare il ***c#*** cartella ***WingtipToys***.

> [!NOTE]
> Se si dispone già di una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente tale cartella esistente prima di rinominare il *c#* cartella *WingtipToys*.


Per eseguire il progetto completato, aprire il *WingtipToys* cartella e fare doppio clic il *WingtipToys.sln* file. Visual Studio 2013 verrà aperto il progetto. Successivamente, fare doppio clic su di *Default.aspx* file nella finestra Esplora soluzioni e fare clic su Visualizza nel Browser dal menu di scelta rapida.

### <a name="tutorial-support-and-comments"></a>I commenti e supporto dell'esercitazione

Utilizzare la sezione Domande e risposte, inclusa il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) esempio (c#) per eventuali domande o commenti.

I commenti in questa serie di esercitazioni sono, e quando viene aggiornata la serie di esercitazioni sarà ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.

Quando si verifica un errore durante lo sviluppo oppure se il sito Web non viene eseguito correttamente, i messaggi di errore possono fornire indicazioni complesse per l'origine del problema o non potrebbero spiegare come correggerlo. Per alcuni scenari comuni di problema, è inoltre possibile utilizzare il [forum ASP.NET](https://forums.asp.net/) o nella sezione delle domande e risposte incluso con il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) esempio. Se viene visualizzato un messaggio di errore o non funzioni come eseguire le esercitazioni, assicurarsi di controllare nei percorsi indicati.

>[!div class="step-by-step"]
[avanti](create-the-project.md)
