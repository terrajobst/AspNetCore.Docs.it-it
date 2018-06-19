---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Accesso ai dati e i modelli | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 4 riguarda l'accesso ai dati e modelli.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879478"
---
<a name="part-4-models-and-data-access"></a>Parte 4: Accesso ai dati e i modelli
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 4 riguarda l'accesso ai dati e modelli.


Fino a questo punto, è stato stato passando solo "dati fittizi" dai nostri controller per i modelli di visualizzazione. Ora si è pronti per collegare un database reale. In questa esercitazione parleremo illustrato come utilizzare SQL Server Compact Edition (spesso chiamato SQL CE) come il motore di database. SQL CE è un database gratuito incorporato, i file in base che non richiede una configurazione, che rende molto semplice per lo sviluppo locale o l'installazione.

## <a name="database-access-with-entity-framework-code-first"></a>Accesso al database con Entity Framework Code-First

Verrà usato il supporto di Entity Framework (EF) incluso in progetti ASP.NET MVC 3 per eseguire query e aggiornare il database. Entity Framework è un oggetto flessibile relazionale mapping API che consente agli sviluppatori di query e aggiornare i dati archiviati in un database in modalità orientata agli oggetti di dati (ORM).

Entity Framework versione 4 supporta un paradigma di sviluppo chiamato prima di codice. Prima di codice consente di creare l'oggetto modello mediante la scrittura di classi semplice (noto anche come POCO da oggetti CLR "plain-old") e può anche creare il database al momento dalle classi.

### <a name="changes-to-our-model-classes"></a>Modifiche per il modello di classi

In questa esercitazione, si verrà sfruttando la funzionalità di creazione del database in Entity Framework. Prima di procedere è tuttavia verifichiamo alcune lievi modifiche per il nostro classi del modello per aggiungere alcuni elementi che verrà usato in un secondo momento.

#### <a name="adding-the-artist-model-classes"></a>Aggiunta di classi del modello artista

Il nostro album verrà associato a artisti, pertanto verrà aggiunto una classe di modello semplice per descrivere un artista. Aggiungere una nuova classe nella cartella Models denominato Artist.cs utilizzando il codice riportato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aggiornare il nostro classi modello

Aggiornare la classe Album come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Successivamente, effettuare gli aggiornamenti seguenti alla classe genere.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Aggiunta dell'App\_cartella dati

Verrà aggiunto una App\_directory dei dati al progetto per l'archiviazione dei file di database SQL Server Express. App\_dati sono una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database. Dal menu progetto, selezionare Aggiungi cartella ASP.NET, quindi App\_dati.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Creazione di una stringa di connessione nel file Web. config

Si aggiungerà alcune righe al file di configurazione del sito Web in modo che Entity Framework sia in grado di connettersi al database. Fare doppio clic sul file Web. config situato nella radice del progetto.

![](mvc-music-store-part-4/_static/image2.png)

Scorrere fino alla fine di questo file e aggiungere un &lt;connectionStrings&gt; sezione direttamente sopra l'ultima riga, come illustrato di seguito.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Aggiunta di una classe di contesto

Fare clic sulla cartella di modelli e aggiungere una nuova classe denominata MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Questa classe verrà rappresenta il contesto di database di Entity Framework ed eseguiranno gestire la creazione, leggere, aggiornare ed eliminare operazioni per Microsoft. Il codice per questa classe è illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Questo è tutto, non c'è alcun altro configurazione speciali interfacce, e così via. Estendendo la classe di base DbContext, la classe MusicStoreEntities è in grado di gestire le nostre operazioni di database per noi. Ora che abbiamo che agganciato, aggiungere alcune proprietà aggiuntive per il nostro classi del modello per sfruttare alcune delle informazioni aggiuntive nel database.

### <a name="adding-our-store-catalog-data"></a>Aggiungere i dati del catalogo di archivio

Si sarà possibile avvalersi di una funzionalità in Entity Framework e aggiunge i dati di "valore di inizializzazione" a un database appena creato. Questo verrà pre-popolare il catalogo di archivio con un elenco di generi, artisti e album. Il download MvcMusicStore Assets.zip - che include i file di progettazione del sito utilizzati in precedenza in questa esercitazione, con un file di classe con dati questo valore di inizializzazione, che si trova in una cartella denominata codice.

All'interno del codice / cartella Models, individuare il file SampleData.cs e rilasciarla nella cartella modelli del progetto, come illustrato di seguito.

![](mvc-music-store-part-4/_static/image4.png)

È necessario aggiungere una riga di codice per informare tale classe SampleData di Entity Framework. Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente all'inizio dell'applicazione\_Start (metodo).

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

A questo punto, abbiamo completato il lavoro necessario per configurare Entity Framework per il progetto.

## <a name="querying-the-database"></a>Esecuzione di query sul database

Ora si aggiorna il nostro StoreController in modo che anziché "dati fittizio" chiami invece al database per eseguire una query tutte le informazioni di. Inizieremo con la dichiarazione di un campo nel **StoreController** per contenere un'istanza della classe MusicStoreEntities, denominata storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aggiornamento dell'indice di archivio per eseguire query sul database

La classe MusicStoreEntities viene mantenuta da Entity Framework ed espone una proprietà di raccolta per ogni tabella nel database. Si aggiorna azione di indice del nostro StoreController per recuperare tutti i generi nel database. In precedenza è eseguita questa operazione a livello di codice i dati di stringa. Ora è possibile invece utilizzare il contesto di Entity Framework Generes raccolta:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nessuna modifica desidera essere eseguite per il modello di visualizzazione poiché il StoreIndexViewModel stesso è restituiti prima - viene semplicemente restituito dati in tempo reale dal database ora viene comunque restituito.

Quando si esegue nuovamente il progetto e visitare l'URL "e delle archiviazioni", è ora verrà visualizzato un elenco di tutti i generi nel database:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aggiornamento di archivio Sfoglia e i dettagli per utilizzare i dati in tempo reale

Con l'archivio/Sfoglia? genre =*[alcune genre]* metodo di azione, si sta cercando un genere in base al nome. È previsto solo un risultato, poiché non dovrebbero essere sempre due voci per lo stesso nome Genre e pertanto è possibile utilizzare il. Estensione Single() nelle query LINQ per eseguire query per l'oggetto appropriato genere simile al seguente (non digitare quanto segue ancora):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

L'unico metodo accetta un'espressione Lambda come un parametro, che indica che si desidera che un singolo oggetto Genre in modo che il nome corrisponde al valore, viene definito. Nel caso precedente, ci stiamo durante il caricamento di un singolo oggetto Genre con un valore di nome corrispondente a Disco.

L'utente verrà reindirizzato a una funzionalità di Entity Framework che consente di indicare altre entità correlate desideriamo caricare anche quando l'oggetto Genre viene recuperato. Questa funzionalità viene chiamata forma del risultato di Query e consente di ridurre il numero di volte in cui che è necessario accedere al database per recuperare tutte le informazioni che necessarie. Si desidera recuperare preventivamente gli album per genere che è possibile recuperare, pertanto verrà aggiornata la query per includere da Genres.Include("Albums") per indicare che si desidera anche album correlati. Si tratta di una più efficiente, poiché consente di recuperare i dati del genere e di Album nella richiesta singolo database.

Con spiegazioni dall'area di lavoro, ecco come apparirà la l'azione del controller Sfoglia aggiornata:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

È ora possibile aggiornare l'archivio di esplorazione della vista per visualizzare album disponibili in ogni genere. Aprire il modello di visualizzazione (presente in /Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album, come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

In esecuzione l'applicazione e la selezione di archivio/Sfoglia? genre = Mostra Jazz che i risultati sono ora rimosse dal database, la visualizzazione di tutti gli album questo genere selezionato.

![](mvc-music-store-part-4/_static/image2.jpg)

Verrà effettuata la stessa modifica per il nostro /Store/dettagli / [id] URL e sostituire i dati fittizi con una query sul database che carica un Album il cui ID corrisponde al valore di parametro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

In esecuzione l'applicazione e la selezione di /Store/Details/1 mostra che i risultati sono ora rimosse dal database.

![](mvc-music-store-part-4/_static/image5.png)

Dopo che la pagina Dettagli archivio è impostata per visualizzare un album dall'ID di Album, si aggiorna il **Sfoglia** Visualizza il collegamento alla visualizzazione dettagli. Si utilizzerà HTML. ActionLink, esattamente come è stato fatto per creare un collegamento dall'indice di archivio per archivio passare alla fine della sezione precedente. Di seguito è riportata l'origine completa per la vista di esplorazione.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

A questo punto siamo in grado di individuare la pagina archivio una pagina del genere, che elenca gli album di disponibili, e facendo clic su un album è possibile visualizzare i dettagli per tale album.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)
