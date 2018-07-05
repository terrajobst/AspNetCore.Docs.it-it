---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Accesso ai dati e modelli | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 4 illustra i modelli e accesso ai dati.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 023350e882afe049ce3800921825b1b2bec8e415
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818953"
---
<a name="part-4-models-and-data-access"></a>Parte 4: Modelli e accesso ai dati
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 4 illustra i modelli e accesso ai dati.


Finora, abbiamo abbiamo stato passando solo "dati fittizi" verso i controller per i modelli di visualizzazione. È ora possibile collegare un database effettivo. In questa esercitazione si parlerà come usare SQL Server Compact Edition (spesso chiamati SQL CE) come il motore di database. SQL CE è un database gratuito incorporato, file basati su che non richiede alcuna installazione o configurazione, che rende molto semplice per lo sviluppo locale.

## <a name="database-access-with-entity-framework-code-first"></a>Accesso al database con Entity Framework Code First

Si userà il supporto di Entity Framework (EF) che è incluso in progetti ASP.NET MVC 3 per eseguire query e aggiornare il database. Entity Framework è un oggetto flessibile relazionale mapping (ORM) dei dati API che consente agli sviluppatori di eseguire query e aggiornare i dati archiviati in un database in una modalità orientata agli oggetti.

Entity Framework versione 4 supporta un paradigma di sviluppo denominato code first. Code first consente di creare l'oggetto modello mediante la scrittura di classi semplice (noto anche come POCO da "plain-old" CLR Object) e può anche creare il database in tempo reale dalle classi.

### <a name="changes-to-our-model-classes"></a>Modifiche per le classi di modello

La funzionalità per la creazione del database in Entity Framework verrà usato in questa esercitazione. Prima di procedere, tuttavia, creiamo lievi modifiche per le classi di modello di componente aggiuntivo di alcuni aspetti che verrà usato in un secondo momento.

#### <a name="adding-the-artist-model-classes"></a>Aggiunta di classi del modello artista

Nostro album verranno associati alle creazioni degli artisti, quindi si aggiungerà una classe di modello semplice per descrivere un artista. Aggiungere una nuova classe nella cartella Models denominato Artist.cs usando il codice illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aggiornare le classi di modello

Aggiornare la classe di Album come illustrato di seguito.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Successivamente, apportare le modifiche seguenti alla classe del genere.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Aggiunta dell'App\_cartella di dati

Si aggiungerà un'App\_directory dei dati per il progetto per contenere i file di database SQL Server Express. App\_dati sono una directory speciale in ASP.NET che ha già le autorizzazioni di accesso di sicurezza corrette per l'accesso al database. Dal menu progetto, selezionare Aggiungi cartella ASP.NET, quindi App\_dei dati.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Creazione di una stringa di connessione nel file Web. config

Si aggiungerà alcune righe al file di configurazione del sito Web in modo che Entity Framework sia in grado di connettersi al database. Fare doppio clic sul file Web. config situato nella radice del progetto.

![](mvc-music-store-part-4/_static/image2.png)

Scorrere fino alla fine di questo file e aggiungere una &lt;connectionStrings&gt; sezione immediatamente sopra l'ultima riga, come illustrato di seguito.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Aggiunta di una classe di contesto

Fare doppio clic su cartella Models e aggiungere una nuova classe denominata MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Questa classe verrà rappresentano il contesto del database, Entity Framework e verrà gestire la creazione, lettura, operazioni aggiornamento ed eliminazione per noi. Seguito è riportato il codice per questa classe.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Questo è tutto, non vi è alcun altro configurazione speciale interfacce, e così via. Con l'estensione della classe base DbContext, la classe MusicStoreEntities è in grado di gestire le operazioni di database per noi. A questo punto, abbiamo che collegato, è possibile aggiungere alcune altre proprietà per le classi di modello possa sfruttare i vantaggi di alcune informazioni aggiuntive nel nostro database.

### <a name="adding-our-store-catalog-data"></a>Aggiungere i dati di catalogo di archivio

Si sarà possibile avvalersi di una funzionalità in Entity Framework che consente di aggiungere dati di "valore di inizializzazione" a un database appena creato. Ciò popola preventivamente il nostro catalogo di archivio con un elenco di generi e gli artisti album. Il download MvcMusicStore Assets.zip - che comprendeva i file di progettazione del sito usati in precedenza in questa esercitazione - dispone di un file di classe con questi dati di valore di inizializzazione, che si trova in una cartella denominata codice.

All'interno del codice / cartella Models, individuare il file SampleData.cs e rilasciarlo nella cartella Models nel progetto, come illustrato di seguito.

![](mvc-music-store-part-4/_static/image4.png)

A questo punto è necessario aggiungere una riga di codice per indicare a Entity Framework su tale classe SampleData. Fare doppio clic sul file Global. asax nella radice del progetto per aprirlo e aggiungere la riga seguente all'inizio dell'applicazione\_Start (metodo).

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

A questo punto, abbiamo completato le operazioni necessarie per configurare Entity Framework per il progetto.

## <a name="querying-the-database"></a>Esecuzione di query sul database

A questo punto è possibile aggiornare il StoreController in modo che invece di usare "dati fittizio" chiama invece nel database per eseguire query di tutte le informazioni. Si inizierà dichiarando un campo nel **StoreController** per conservare un'istanza della classe MusicStoreEntities, denominata storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aggiornamento dell'indice Store per eseguire query sul database

La classe MusicStoreEntities è gestita da Entity Framework ed espone una proprietà di raccolta per ogni tabella nel database. Aggiorniamo azione Index del nostro StoreController per recuperare tutti i generi dal database. In precedenza abbiamo ottenuto questo come hardcoded i dati di stringa. A questo punto è possibile invece usare semplicemente il contesto di Entity Framework Generes raccolta:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Nessuna modifica desidera essere eseguiti per il modello di vista poiché viene comunque restituito il StoreIndexViewModel stesso viene restituite prima - viene semplicemente restituito dei dati in tempo reale dal database ora.

Quando si esegue nuovamente il progetto e visitare l'URL "/ Store", si vedrà ora un elenco di tutti i generi dal database:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>L'aggiornamento Store individuare e informazioni dettagliate per usare i dati in tempo reale

Con/Store/Sfoglia? genre =*[some-genre]* metodo di azione, viene eseguita la ricerca per genere in base al nome. È previsto solo un risultato, poiché sono sempre non dovrebbero essere presenti due voci per lo stesso nome Genre e quindi è possibile usare il. Estensione Single () nelle query LINQ per eseguire query per l'oggetto Genre appropriato simile al seguente (non digitare questo ancora):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Il metodo Single accetta un'espressione Lambda come un parametro che specifica che si desidera un singolo oggetto Genre tale che il relativo nome corrisponde al valore che abbiamo definito. Nel caso precedente, è corso il caricamento un singolo oggetto Genre con un valore di nome corrispondente Disco.

Articolo verrà fornita una funzionalità di Entity Framework che consente di indicare altre entità correlate che si desidera caricare anche quando l'oggetto Genre viene recuperato. Questa funzionalità è denominata forma del risultato di Query e ci permette di ridurre il numero di volte in cui che è necessario accedere al database per recuperare tutte le informazioni che necessarie. Si vuole recuperare preventivamente gli album per Genre vengono recuperati, in modo che la query da includere da Genres.Include("Albums") per indicare che si desidera anche album correlati verrà aggiornata. Questo è più efficiente, poiché consente di recuperare i nostri Genre Album dati e nella richiesta singolo database.

Con le spiegazioni disponibili dall'area di lavoro, ecco come appare l'azione del controller Sfoglia aggiornata:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

È ora possibile aggiornare la Store esplorazione della vista per visualizzare album che sono disponibili in ogni genere. Aprire il modello di visualizzazione (trovato nel /Views/Store/Browse.cshtml) e aggiungere un elenco puntato di album come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Esecuzione dell'applicazione e passare a/Store/Sfoglia? genre = Mostra Jazz che i risultati sono ora rimosse dal database, visualizzazione di tutti gli album nel genere selezionato.

![](mvc-music-store-part-4/_static/image2.jpg)

Si renderà la stessa modifica al nostro /Store/dettagli / [id] URL e sostituire i dati fittizi con una query sul database che carica un Album il cui ID corrisponde al valore di parametro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Esecuzione dell'applicazione e scegliendo /Store/Details/1 mostra che i risultati sono ora rimosse dal database.

![](mvc-music-store-part-4/_static/image5.png)

Ora che la pagina dettagli Store è configurata per la visualizzazione di un album dall'ID di Album, aggiorniamo il **esplorare** Visualizza il collegamento alla visualizzazione dettagli. Si userà HTML. ActionLink, esattamente come è stato fatto per creare un collegamento dall'indice Store per Store passare alla fine della sezione precedente. Di seguito è riportata l'origine completa per la visualizzazione di esplorazione.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

A questo punto siamo in grado di passare dalla nostra pagina Store a una pagina Genre, che elenca gli album disponibili, e facendo clic su un album è possibile visualizzare i dettagli per tale album.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-3.md)
> [Successivo](mvc-music-store-part-5.md)
