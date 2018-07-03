---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Passaggio di dati a pagine Master di visualizzazione (VB) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione. Verranno esaminati due strategie per passare dati a una visualizzazione m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e70f15d98101336dbef31b4f9d8b958632e46c01
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388519"
---
<a name="passing-data-to-view-master-pages-vb"></a>Passaggio di dati a pagine Master di visualizzazione (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione. Si esamineranno due strategie per passare dati a una pagina master di visualizzazione. In primo luogo, illustreremo una pratica soluzione risultante in un'applicazione che è difficile da gestire. Successivamente, esamineremo una soluzione migliore che richiede un po' più di lavoro iniziale ma i risultati in un'applicazione molto più facile da gestire.


## <a name="passing-data-to-view-master-pages"></a>Passaggio di dati a pagine Master di visualizzazione

L'obiettivo di questa esercitazione è illustrare come è possibile passare dati da un controller a una pagina master visualizzazione. Si esamineranno due strategie per passare dati a una pagina master di visualizzazione. In primo luogo, illustreremo una pratica soluzione risultante in un'applicazione che è difficile da gestire. Successivamente, esamineremo una soluzione migliore che richiede un po' più di lavoro iniziale ma i risultati in un'applicazione molto più facile da gestire.

### <a name="the-problem"></a>Il problema

Si supponga che si compila un'applicazione di database di film e si desidera visualizzare l'elenco delle categorie di film in ogni pagina dell'applicazione (vedere la figura 1). Si supponga inoltre che l'elenco delle categorie di film viene archiviato in una tabella di database. In tal caso, avrebbe senso per recuperare le categorie dal database e il rendering dell'elenco di categorie di film all'interno di una pagina master di visualizzazione.


[![Visualizzare le categorie di film in una pagina master di visualizzazione](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figura 01**: visualizzare le categorie di film in una pagina master di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni normali](passing-data-to-view-master-pages-vb/_static/image3.png))


Ecco il problema. Modo è possibile recuperare l'elenco delle categorie di film nella pagina master? Si è tentati di chiamare direttamente i metodi delle classi di modello nella pagina master. In altre parole, è tentato di includere il codice per recuperare i dati da destra nella pagina master del database. Tuttavia, ignorando i controller MVC per accedere al database violerebbe netta separazione degli aspetti che corrisponde a uno dei principali vantaggi della creazione di un'applicazione MVC.

In un'applicazione MVC, si desidera che tutte le interazioni tra le visualizzazioni MVC e il modello MVC per essere gestito dal controller MVC. Questa separazione dei compiti risultante in un'applicazione più gestibile, flessibile e testabile.

In un'applicazione MVC, tutti i dati passati a una visualizzazione, incluso una pagina master di visualizzazione, devono essere passati a una visualizzazione per un'azione del controller. Inoltre, i dati devono essere passati, sfruttando i vantaggi di visualizzare i dati. Nella parte restante di questa esercitazione, esamina due metodi per il passaggio di visualizzare i dati a una pagina master di visualizzazione.

### <a name="the-simple-solution"></a>La soluzione più semplice

Iniziamo con la soluzione più semplice per passare i dati di visualizzazione da un controller a una pagina master di visualizzazione. La soluzione più semplice consiste nel passare i dati della visualizzazione della pagina master in ogni azione del controller.

Prendere in considerazione il controller nel listato 1. Espone due azioni denominate `Index()` e `Details()`. Il `Index()` metodo di azione restituisce ogni film della tabella di database di film. Il `Details()` metodo di azione restituisce ogni film in una categoria di filmato specifico.

**Listato 1: `Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Si noti che sia la `Index()` e il `Details()` azioni aggiungere due elementi per visualizzare i dati. Il `Index()` azione aggiunge due chiavi: le categorie e i film. La chiave di categorie rappresenta l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione. La chiave di film rappresenta l'elenco di film visualizzati dalla pagina della visualizzazione dell'indice.

Il `Details()` azione aggiunge anche due chiavi denominate le categorie e filmati. La chiave di categorie, ancora una volta, rappresenta l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione. La chiave di film rappresenta l'elenco di film in una determinata categoria visualizzata nella pagina di visualizzazione dei dettagli (vedere la figura 2).


[![La visualizzazione dei dettagli](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figura 02**: consente di visualizzare i dettagli di ([fare clic per visualizzare l'immagine con dimensioni normali](passing-data-to-view-master-pages-vb/_static/image6.png))


La visualizzazione dell'indice è contenuta nel listato 2. Semplicemente scorre l'elenco di film rappresentata dall'elemento di film nei dati di visualizzazione.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La pagina della visualizzazione master è contenuta nel listato 3. La pagina master visualizzazione esegue l'iterazione e viene eseguito il rendering di tutte le categorie di film rappresentate dall'elemento di categorie da visualizzare i dati.

**Listato 3: `Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Tutti i dati viene passato alla visualizzazione e la pagina master visualizzazione tramite i dati di visualizzazione. Che è il modo corretto per passare i dati della pagina master.

Quindi, qual è il problema con questa soluzione? Il problema è che questa soluzione viola il principio DRY (non ' t Repeat Yourself). Ogni azione del controller è necessario aggiungere lo molto stesso elenco di categorie di film per visualizzare i dati. La presenza di codice duplicato all'applicazione rende molto più difficile da gestire, adattare e modificare l'applicazione.

### <a name="the-good-solution"></a>La soluzione ottima

In questa sezione esamineremo una soluzione alternativa e migliore, per passare i dati da un'azione del controller a una pagina master di visualizzazione. Anziché aggiungere le categorie di film per la pagina master in ogni azione del controller, aggiungiamo le categorie di film per visualizzare i dati una sola volta. Tutti i dati di visualizzazione utilizzati dalla pagina master visualizzazione viene aggiunto in un controller dell'applicazione.

La classe ApplicationController è contenuta nel listato 4.

La classe ApplicationController è contenuta nel listato 4.

**Listato 4: `Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Esistono tre operazioni che è possibile notare sul controller dell'applicazione nel listato 4. In primo luogo, si noti che la classe eredita dalla classe di base MVC. Il controller dell'applicazione è una classe controller.

In secondo luogo, si noti che la classe controller dell'applicazione è una classe come MustInherit. Una classe MustInherit è una classe che deve essere implementata da una classe concreta. Perché il controller dell'applicazione è una MustInherit (classe), è possibile non richiamare qualsiasi metodo definito nella classe direttamente. Se si tenta di richiamare direttamente la classe dell'applicazione si riceverà un messaggio di errore risorsa non è stata trovata.

In terzo luogo, si noti che il controller dell'applicazione contiene un costruttore che consente di aggiungere l'elenco delle categorie di film per visualizzare i dati. Ogni classe di controller che eredita dal controller dell'applicazione chiama automaticamente il costruttore del controller dell'applicazione. Ogni volta che si chiama qualsiasi azione in qualsiasi controller che eredita dal controller dell'applicazione, le categorie di film viene inclusa automaticamente nei dati di visualizzazione.

Il controller di film nel listato 5 eredita dal controller dell'applicazione.

**Listato 5: `Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Il controller di film, esattamente come il controller Home illustrato nella sezione precedente, espone due metodi di azione denominati `Index()` e `Details()`. Si noti che l'elenco delle categorie di film visualizzati dalla pagina master visualizzazione non è aggiunto a visualizzare i dati in entrambi i `Index()` o `Details()` (metodo). Poiché il controller di film eredita dal controller dell'applicazione, viene aggiunto l'elenco delle categorie di film per visualizzare automaticamente i dati.

Si noti che questa soluzione per l'aggiunta di dati di visualizzazione per una pagina master di visualizzazione non viola il principio DRY (non ' t Repeat Yourself). Il codice per l'aggiunta nell'elenco delle categorie di film per visualizzare i dati è contenuto in una sola posizione: il costruttore per il controller dell'applicazione.

### <a name="summary"></a>Riepilogo

In questa esercitazione, abbiamo parlato due approcci per il passaggio di visualizzare i dati da un controller a una pagina master di visualizzazione. Prima di tutto esaminata una semplice, ma difficile da gestire approccio. Nella prima sezione, abbiamo parlato di come è possibile aggiungere dati di visualizzazione per una pagina master di visualizzazione in ogni ogni azione del controller all'interno dell'applicazione. Siamo giunti alla conclusione che si tratta di un approccio non valido perché viola il principio DRY (non ' t Repeat Yourself).

Successivamente, abbiamo esaminato una strategia migliore per l'aggiunta di dati necessari per una pagina master di visualizzazione per visualizzare i dati. Anziché aggiungere i dati della visualizzazione in ogni azione del controller, abbiamo aggiunto i dati della visualizzazione solo una volta all'interno di un controller dell'applicazione. In questo modo, quando si passano dati a una pagina master di visualizzazione in un'applicazione ASP.NET MVC, è possibile evitare il codice duplicato.

> [!div class="step-by-step"]
> [Precedente](creating-page-layouts-with-view-master-pages-vb.md)
