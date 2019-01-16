---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introduzione a 4,7 Web Form ASP.NET e Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni dettagliata insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con Microsoft Visual Studio e ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341553"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2017
====================

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Questa serie di esercitazioni illustra come compilare un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introduzione

Questa serie di esercitazioni illustra come creare un'applicazione Web Form ASP.NET mediante ASP.NET 4.5 e Visual Studio 2017. Si creerà un'applicazione denominata **Wingtip Toys** : un sito web StoreFront-semplificata la vendita in linea degli oggetti. Durante la serie, vengono evidenziate nuove funzionalità di ASP.NET 4.5.

### <a name="target-audience"></a>Destinatari

Gli sviluppatori al Web Form ASP.NET sono i destinatari di questa serie di esercitazioni.

È necessario avere una certa conoscenza nelle aree seguenti:

- Programmazione orientata agli oggetti (OOP) e lingue
- Sviluppo di applicazioni Web (HTML, CSS, JavaScript)
- database relazionali
- Architettura a più livelli

Per esaminare queste aree, è consigliabile esaminare il contenuto seguente:

- [Introduzione a Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
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
- Controlli dati fortemente tipizzati
- Associazione di modelli
- Annotazioni dei dati
- Provider di valori
- SSL e OAuth
- ASP.NET Identity, la configurazione e autorizzazione
- Convalida discreta
- Routing
- Gestione degli errori di ASP.NET

### <a name="application-scenarios-and-tasks"></a>Attività e gli scenari di applicazione

Serie di esercitazioni attività includono:

- Creazione, la revisione e l'esecuzione di un nuovo progetto
- Creazione di una struttura di database
- L'inizializzazione e il seeding di un database
- Personalizzazione dell'interfaccia utente con gli stili, grafica e una pagina master
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

Questa serie di esercitazioni è destinato a chiunque abbia familiarità con i concetti di programmazione, ma per Web Form ASP.NET. Se si ha già familiarità con Web Form ASP.NET, questa serie comunque utili sulle nuove funzionalità di ASP.NET 4.5. Per i lettori familiarità con la programmazione di concetti e Web Form ASP.NET, vedere le altre esercitazioni di Web Form fornite nel [introduttiva](../../../index.md) sezione sul sito Web ASP.NET.

Di ASP.NET 4.5 forniti in questa serie di esercitazioni include le funzionalità seguenti:

- Un'interfaccia utente semplice per la creazione di progetti che offre [supporto per molti framework di ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un layout, temi e framework progettazione reattiva.
- [ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona in tutti i framework di ASP.NET e funziona con software diversi da IIS di hosting web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Un aggiornamento a Entity Framework che consente di:
  - Recuperare e modificare i dati come oggetti fortemente tipizzati
  - Accedere ai dati in modo asincrono
  - Gestire gli errori di connessione temporanei
  - Istruzioni di log SQL

Per un elenco completo delle funzionalità ASP.NET 4.5, vedere [ASP.NET and Web Tools per Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>L'applicazione di esempio Wingtip Toys

Gli screenshot seguenti sono compresi tra l'applicazione Web Form ASP.NET creato in questa serie di esercitazioni. Quando si esegue l'applicazione in Visual Studio, viene visualizzata la Home page di web seguenti.

![Wingtip Toys - pagina predefinita](introduction-and-overview/_static/image1.png)

È possibile registrare come un nuovo utente, o accedere come un utente esistente. Barra di spostamento superiore è disponibili collegamenti a categorie di prodotti e dei loro prodotti dal database.

Se si seleziona **prodotti**, vengono visualizzati tutti i prodotti disponibili. 

![Wingtip Toys - prodotti](introduction-and-overview/_static/image2.png)

Se si seleziona un prodotto specifico, vengono visualizzati i dettagli del prodotto.


![Wingtip Toys - dettagli sul prodotto](introduction-and-overview/_static/image3.png)

Come un utente, è possibile registrare e accedere con la funzionalità predefinita di modello Web Form. Questa esercitazione illustra anche come eseguire l'accesso con un account Gmail esistente. Inoltre, è possibile accedere come amministratore di aggiungere e rimuovere i prodotti dal database.

![Wingtip Toys - Accedi](introduction-and-overview/_static/image4.png)

Una volta che si è connessi come utente, è possibile aggiungere prodotti al carrello acquisti e completamento della transazione con PayPal. L'applicazione di esempio è progettata per funzionare nell'ambiente sandbox per gli sviluppatori di PayPal. Nessuna transazione denaro effettivamente viene eseguita.

![Wingtip Toys - carrello della spesa](introduction-and-overview/_static/image5.png)

Conferma le informazioni di account, l'ordine e il pagamento.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Dopo la restituzione da PayPal, è possibile esaminare e completare il tuo ordine.

![Wingtip Toys - verifica ordine](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare che il software seguente sia installato nel computer:

- [Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

.NET Framework viene installato automaticamente.

Questa serie di esercitazioni viene utilizzato Microsoft Visual Studio Community 2017. È possibile usare entrambi che o Microsoft Visual Studio 2017 per il completamento di questa serie di esercitazioni.

Tenere presente quanto segue su Visual Studio:

* Microsoft Visual Studio 2017 e Microsoft Visual Studio Community 2017 sono dette *Visual Studio* in tutta questa serie di esercitazioni.

* Visual Studio 2017 viene installata accanto a tutte le versioni precedenti già installate. I siti creati nelle versioni precedenti possono essere aperti in Visual Studio 2017 e continuano ad aprire nelle versioni precedenti.

* La prima volta che è stato avviato Visual Studio, si presuppone che è stata selezionata la *lo sviluppo Web* impostazioni. Per altre informazioni, vedere [Procedura: Selezionare le impostazioni di ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il progetto Web presentato in questa serie di esercitazioni.

## <a name="download-the-sample-application"></a>Scaricare l'applicazione di esempio

 È possibile scaricare l'esempio completato applicatiion in qualsiasi momento dal sito di esempi MSDN:

[Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

 Questo download include gli elementi seguenti:

- L'applicazione di esempio nel *WingtipToys* cartella.
- Le risorse usate per creare l'applicazione di esempio nel *WingtipToys-asset* cartella le *WingtipToys* cartella.

Il download è un *zip* file. Per visualizzare il progetto completato creato questa serie di esercitazioni, trovare e selezionare il *C#* cartella nel file con estensione zip. Salvare il C# cartella per la cartella utilizzata per lavorare con progetti di Visual Studio. Per impostazione predefinita, la cartella di progetti di Visual Studio 2017 è:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

Rinominare il ***c#*** cartella in cui ***WingtipToys***.

> [!NOTE]
> Se si dispone già di una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente la cartella esistente prima di rinominare il *c#* cartella in cui *WingtipToys*.

Per eseguire il progetto completato, aprire il *WingtipToys* cartella e fare doppio clic il *WingtipToys.sln* file. Visual Studio 2017 apre il progetto. Successivamente, fare doppio clic il *default. aspx* del file in **Esplora soluzioni** e selezionare **Visualizza nel Browser**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Questionario per esaminare il contenuto Web Form ASP.NET

Dopo aver completato la serie di esercitazioni, questionario per testare le proprie conoscenze e rafforzano i concetti chiave. Ogni domanda fornisce una spiegazione e collegamenti a indicazioni aggiuntive.

 * [Web Form ASP.NET Quiz](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>I commenti e supporto dell'esercitazione

Per domande e commenti, usare la sezione di domande e risposte inclusa nella [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) pagina di esempio.

Sono Benvenuti i commenti su questa serie di esercitazioni. Quando questa serie di esercitazioni viene aggiornata, viene compiuto ogni sforzo per prendere in considerazione le correzioni o suggerimenti per possibili miglioramenti.

Se si verifica un errore, messaggi di errore corrispondente potrebbero generare confusione, con nessuna spiegazioni su come risolverlo. Per altre informazioni, è possibile controllare la [forum ASP.NET](https://forums.asp.net/). Un'altra valida fonte è la sezione di domande e risposte nel [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) pagina di esempio. 

> [!div class="step-by-step"]
> [avanti](create-the-project.md)
