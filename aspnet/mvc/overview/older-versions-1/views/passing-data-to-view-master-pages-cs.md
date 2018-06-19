---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: Passaggio di dati per le pagine Master di visualizzazione (c#) | Documenti Microsoft
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Verranno esaminati due strategie per passare dati a una vista m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: bfb58cbe0c415c092f3a41e518281a7461d2803c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869842"
---
<a name="passing-data-to-view-master-pages-c"></a>Passaggio di dati a pagine Master di visualizzazione (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Verranno esaminati due strategie per passare dati a una pagina master di visualizzazione. In primo luogo, vengono illustrati una soluzione semplice che comporta un'applicazione che è difficile da gestire. Successivamente, esamineremo una soluzione migliore che richiede un po' più lavoro iniziale ma i risultati in un'applicazione molto più facili da gestire.


## <a name="passing-data-to-view-master-pages"></a>Passaggio di dati a pagine Master di visualizzazione

L'obiettivo di questa esercitazione è illustrare come è possibile passare i dati da un controller a una pagina master di visualizzazione. Verranno esaminati due strategie per passare dati a una pagina master di visualizzazione. In primo luogo, vengono illustrati una soluzione semplice che comporta un'applicazione che è difficile da gestire. Successivamente, esamineremo una soluzione migliore che richiede un po' più lavoro iniziale ma i risultati in un'applicazione molto più facili da gestire.

### <a name="the-problem"></a>Il problema

Si supponga che si compila un'applicazione di database di film e si desidera visualizzare l'elenco delle categorie di film in ogni pagina dell'applicazione (vedere la figura 1). Si supponga inoltre che l'elenco delle categorie di film viene archiviato in una tabella di database. In tal caso, sarebbe più utile recuperare le categorie dal database ed eseguire il rendering l'elenco delle categorie del film all'interno di una pagina master di visualizzazione.


[![Visualizzazione categorie film in una pagina master visualizzazione](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**Figura 01**: visualizzare le categorie di film in una pagina master visualizzazione ([fare clic per visualizzare l'immagine ingrandita](passing-data-to-view-master-pages-cs/_static/image3.png))


Di seguito è riportato il problema. Come è recuperare l'elenco delle categorie di film nella pagina master? Può essere tentati di chiamare direttamente i metodi delle classi modello nella pagina master. In altre parole, è tentato di includere il codice per recuperare i dati da destra nella pagina master del database. Tuttavia, ignorando i controller MVC per accedere al database possa costituire una violazione netta separazione delle problematiche che è uno dei vantaggi principali della compilazione di un'applicazione MVC.

In un'applicazione MVC, si desidera che tutte le interazioni tra le visualizzazioni MVC e il modello MVC per essere gestita dal controller MVC. Questa separazione delle problematiche risultati in un'applicazione più gestibile, flessibile e testabile.

In un'applicazione MVC, tutti i dati passati a una visualizzazione, inclusi quelli di una pagina master di visualizzazione: devono essere passati a una visualizzazione per un'azione del controller. Inoltre, i dati devono essere passati per sfruttando la possibilità di visualizzare i dati. Nella parte restante di questa esercitazione, esamina due metodi del passaggio di visualizzare i dati a una pagina master di visualizzazione.

### <a name="the-simple-solution"></a>La soluzione semplice

Iniziamo con la soluzione più semplice per il passaggio di visualizzare i dati da un controller a una pagina master visualizzazione. La soluzione più semplice consiste nel passare i dati della visualizzazione per la pagina master in ogni azione del controller.

Prendere in considerazione il controller nel listato 1. Espone due azioni denominate `Index()` e `Details()`. Il `Index()` metodo di azione restituisce ogni film nella tabella di database film. Il `Details()` metodo di azione restituisce ogni film in una categoria filmato specifico.

**Elenco 1: `Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Si noti che l'indice sia le azioni Details() aggiungano due elementi per visualizzare i dati. L'azione Index () aggiunge due chiavi: categorie e i film. La chiave di categorie rappresenta l'elenco delle categorie di film visualizzato nella pagina di visualizzazione master. La chiave di filmati rappresenta l'elenco di film visualizzato nella pagina di visualizzazione dell'indice.

L'azione Details() aggiunge inoltre due chiavi denominato categorie e i film. La chiave di categorie, ancora una volta, rappresenta l'elenco delle categorie di film visualizzato nella pagina di visualizzazione master. La chiave di filmati rappresenta l'elenco di film in una particolare categoria visualizzato nella pagina di visualizzazione dei dettagli (vedere la figura 2).


[![La visualizzazione dei dettagli](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**Figura 02**: consente di visualizzare i dettagli di ([fare clic per visualizzare l'immagine ingrandita](passing-data-to-view-master-pages-cs/_static/image6.png))


La visualizzazione dell'indice è contenuta nel listato 2. Semplicemente scorre l'elenco di film rappresentata dall'elemento di filmati nei dati di visualizzazione.

**Elenco 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

La visualizzazione pagina master è contenuta nel listato 3. La pagina master visualizzazione esegue l'iterazione e viene eseguito il rendering di tutte le categorie di film rappresentate dall'elemento di categorie da visualizzare i dati.

**Elenco di 3: `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Tutti i dati viene passato alla visualizzazione e la pagina master visualizzazione tramite i dati della visualizzazione. Che è il modo corretto per passare i dati della pagina master.

In tal caso, qual è la soluzione? Il problema è che questa soluzione viola il principio di sorgente (non ripetere manualmente). Ogni azione del controller è necessario aggiungere lo molto stesso elenco di categorie di film per visualizzare i dati. La presenza di codice duplicato all'applicazione rende molto più difficile da gestire, adattare e modificare l'applicazione.

### <a name="the-good-solution"></a>La soluzione ottima

In questa sezione verranno esaminati una soluzione alternativa e migliore, per passare dati da un'azione del controller a una pagina master di visualizzazione. Anziché aggiungere le categorie di film per la pagina master in ogni azione del controller, è aggiungere le categorie di film ai dati della visualizzazione una sola volta. Tutti i dati di visualizzazione utilizzati dalla pagina master visualizzazione viene aggiunto in un controller dell'applicazione.

La classe ApplicationController contenuta nel listato 4.

**Elenco di 4: `Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Ci sono tre aspetti che è opportuno notare sul controller applicazione listato 4. In primo luogo, si noti che la classe eredita dalla classe di base System. Il controller dell'applicazione è una classe controller.

In secondo luogo, si noti che la classe controller dell'applicazione è una classe astratta. Una classe astratta è una classe che deve essere implementata da una classe concreta. Perché il controller dell'applicazione è una classe astratta, non è possibile non richiamare qualsiasi metodo definito nella classe direttamente. Se si tenta di richiamare direttamente la classe dell'applicazione si otterrà un messaggio di errore di risorsa non trovata.

In terzo luogo, si noti che il controller dell'applicazione contiene un costruttore che aggiunge l'elenco delle categorie di film per visualizzare i dati. Ogni classe controller da cui eredita il controller dell'applicazione chiama il costruttore del controller applicazione automaticamente. Quando si chiama qualsiasi azione in alcun controller da cui eredita il controller dell'applicazione, le categorie di film viene inclusa automaticamente nei dati di visualizzazione.

Il controller di filmati listato 5 eredita dal controller di applicazione.

**Elenco 5: `Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Il controller di filmati, come illustrato nella sezione precedente, controller Home espone due metodi di azione denominati `Index()` e `Details()`. Si noti che l'elenco delle categorie di film visualizzato dalla pagina master visualizzazione non è aggiunto per visualizzare i dati in entrambi i `Index()` o `Details()` metodo. Poiché il controller di filmati eredita dal controller di applicazione, l'elenco delle categorie di film viene aggiunto per visualizzare automaticamente i dati.

Si noti che questa soluzione per l'aggiunta di dati di visualizzazione per una pagina master di visualizzazione non violino il principio sorgente (non ripetere manualmente). Il codice per aggiungere l'elenco delle categorie di film per visualizzare i dati è contenuto in un solo percorso: il costruttore per il controller dell'applicazione.

### <a name="summary"></a>Riepilogo

In questa esercitazione è descritti due approcci per il passaggio di visualizzare i dati da un controller a una pagina master di visualizzazione. In primo luogo, esaminata una semplice, ma difficile da gestire approccio. Nella prima sezione, è descritto come aggiungere dati di visualizzazione per una pagina master in ogni azione di ogni controller nell'applicazione. Siamo giunti alla conclusione che ciò non è un approccio non valido perché viola il principio sorgente (non ripetere manualmente).

Successivamente, viene esaminato una strategia migliore per l'aggiunta di dati necessari per una pagina master di visualizzazione per visualizzare i dati. Anziché aggiungere i dati della visualizzazione in ogni azione del controller, abbiamo aggiunto i dati della visualizzazione una sola volta all'interno di un controller dell'applicazione. In questo modo, quando si passano dati a una pagina master di visualizzazione in un'applicazione ASP.NET MVC, è possibile evitare il codice duplicato.

> [!div class="step-by-step"]
> [Precedente](creating-page-layouts-with-view-master-pages-cs.md)
> [Successivo](asp-net-mvc-views-overview-vb.md)
