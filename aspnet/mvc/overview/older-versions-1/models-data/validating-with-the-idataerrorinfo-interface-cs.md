---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Convalida con l'interfaccia IDataErrorInfo (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther illustra come visualizzare i messaggi di errore di convalida personalizzata implementando l'interfaccia IDataErrorInfo in una classe di modello.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b80535db32c4567135407aeb99967bb40c279ddb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823913"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Convalida con l'interfaccia IDataErrorInfo (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther illustra come visualizzare i messaggi di errore di convalida personalizzata implementando l'interfaccia IDataErrorInfo in una classe di modello.


L'obiettivo di questa esercitazione è per indicare un approccio all'esecuzione della convalida in un'applicazione ASP.NET MVC. Informazioni su come impedire l'invio di un form HTML senza fornire valori per i campi modulo necessari. In questa esercitazione descrive come eseguire la convalida tramite l'interfaccia IErrorDataInfo.

## <a name="assumptions"></a>Presupposti

In questa esercitazione si utilizzerà il database MoviesDB e la tabella di database di film. Questa tabella contiene le colonne seguenti:

<a id="0.5_table01"></a>


| **Nome della colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


In questa esercitazione, usare Microsoft Entity Framework per generare classi modello del mio database. La classe di film generata da Entity Framework viene visualizzata nella figura 1.


[![L'entità film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: entità del filmato ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Per altre informazioni sull'utilizzo di Entity Framework per generare le classi di modello di database, vedere che l'esercitazione intitolata Creazione di classi del modello con Entity Framework.


## <a name="the-controller-class"></a>La classe Controller

Si usano controller Home per filmati elenco e creare nuovi film. Il codice per questa classe è contenuto nel listato 1.

**Listato 1 - controllers\homecontroller.cs.**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La classe controller Home nel listato 1 contiene due azioni create (). La prima azione consente di visualizzare il form HTML per la creazione di un nuovo film. La seconda azione create () esegue l'inserimento effettivo di questo nuovo film nel database. La seconda azione create () viene richiamata quando il form visualizzato per la prima azione create () viene inviato al server.

Si noti che la seconda azione create () contiene le righe di codice seguenti:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La proprietà IsValid restituisce false quando si verifica un errore di convalida. In tal caso, viene visualizzata di nuovo la visualizzazione di creazione che contiene il form HTML per la creazione di un film.

## <a name="creating-a-partial-class"></a>Creazione di una classe parziale

La classe di film viene generata da Entity Framework. È possibile visualizzare il codice per la classe Movie se si espande il file MoviesDBModel.edmx nella finestra Esplora soluzioni e aprire il file MoviesDBModel.Designer.cs nell'Editor del codice (vedere la figura 2).


[![Il codice per l'entità film](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: il codice per l'entità film ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


La classe di film è una classe parziale. Ciò significa che possiamo aggiungere un'altra classe parziale con lo stesso nome per estendere le funzionalità della classe di film. Si aggiungerà la logica di convalida per la nuova classe parziale.

Aggiungere la classe nel listato 2 nella cartella Models.

**Listato 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Si noti che la classe nel listato 2 include i *parziale* modificatore. Tutti i metodi o proprietà che si aggiunge a questa classe diventano parte della classe film generato da Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Aggiunta di metodi parziali OnChanged e OnChanging

Quando Entity Framework genera una classe di entità, Entity Framework aggiunge automaticamente alla classe i metodi parziali. Entity Framework genera OnChanging e OnChanged metodi parziali che corrispondono a ogni proprietà della classe.

Nel caso la classe di film, Entity Framework crea i metodi seguenti:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Il metodo OnChanging viene chiamato a destra prima che venga modificata la proprietà corrispondente. Il metodo OnChanged viene chiamato a destra dopo la modifica della proprietà.

È possibile sfruttare i vantaggi di questi metodi parziali per aggiungere la logica di convalida per la classe di film. La classe Movie nel listato 3 di aggiornamento verifica che le proprietà Title e direttore vengono assegnate valori non vuoti.

> [!NOTE] 
> 
> Un metodo parziale è un metodo definito in una classe che non si deve implementare. Se non si implementa un metodo parziale il compilatore rimuove la firma del metodo e tutte le chiamate al metodo pertanto vi sono previsti costi di runtime associati al metodo parziale. Nell'Editor di codice di Visual Studio, è possibile aggiungere un metodo parziale digitando la parola chiave *parziale* seguita da uno spazio per visualizzare un elenco di righe parzialmente eseguite da implementare.


**Listato 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Ad esempio, se si tenta di assegnare una stringa vuota per la proprietà Title, quindi un messaggio di errore viene assegnato a un dizionario denominato \_errori.

A questo punto, in realtà non accade nulla quando si assegna una stringa vuota per la proprietà Title e viene aggiunto un errore a privato \_campo errori. È necessario implementare l'interfaccia IDataErrorInfo per esporre questi errori di convalida per il framework ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementa l'interfaccia IDataErrorInfo

L'interfaccia IDataErrorInfo è stato incluso in .NET framework dalla prima versione. Questa interfaccia è un'interfaccia molto semplice:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Se una classe implementa l'interfaccia IDataErrorInfo, il framework di MVC ASP.NET utilizzerà questa interfaccia durante la creazione di un'istanza della classe. Ad esempio, il controller Home azione create () accetta un'istanza della classe film:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Il framework ASP.NET MVC crea l'istanza del film passato all'azione create () con uno strumento di associazione del modello (il DefaultModelBinder). Lo strumento di associazione del modello è responsabile della creazione di un'istanza dell'oggetto film associando i campi di form HTML a un'istanza dell'oggetto film.

Il DefaultModelBinder rileva se una classe implementa l'interfaccia IDataErrorInfo. Se una classe implementa questa interfaccia lo strumento individuerebbe richiama l'indicizzatore IDataErrorInfo.this per ogni proprietà della classe. Se l'indicizzatore restituisce un messaggio di errore lo strumento individuerebbe aggiunge questo messaggio di errore per lo stato del modello automaticamente.

Il DefaultModelBinder controlla anche la proprietà IDataErrorInfo.Error. Questa proprietà è lo scopo di rappresentare gli errori di convalida specifico non di proprietà associati alla classe. Potrebbe ad esempio, si desidera applicare una regola di convalida che dipende dai valori di più proprietà della classe di film. In tal caso, si restituisce un errore di convalida dalla proprietà di errore.

La classe Movie aggiornata nel listato 4 implementa l'interfaccia IDataErrorInfo.

**Listato 4 - Models\Movie.cs (implementa l'interfaccia IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Nel listato 4, controlla la proprietà dell'indicizzatore di \_raccolta errori per vedere se contiene una chiave che corrisponde al nome della proprietà passata all'indicizzatore. Se non si verificano errori di convalida associato alla proprietà viene restituita una stringa vuota.

Non devi modificare il controller Home in alcun modo per usare la classe film modificata. La pagina visualizzata nella figura 3 illustra cosa accade quando viene immesso alcun valore per i campi modulo titolo o Director.


[![La creazione automatica di metodi di azione](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: un formato con valori mancanti ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Si noti che il valore DateReleased viene convalidato automaticamente. Poiché la proprietà DateReleased non accetta valori NULL, il DefaultModelBinder genera un errore di convalida per questa proprietà automaticamente quando non dispone di un valore. Se si desidera modificare il messaggio di errore per la proprietà DateReleased quindi è necessario creare uno strumento di associazione di modelli personalizzato.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come usare l'interfaccia IDataErrorInfo per generare i messaggi di errore di convalida. In primo luogo, abbiamo creato una classe parziale di film che estende le funzionalità della classe parziale film generata da Entity Framework. Successivamente, abbiamo aggiunto la logica di convalida per i film classe OnTitleChanging() e OnDirectorChanging() metodi parziali. Infine, abbiamo implementato l'interfaccia IDataErrorInfo per esporre questi messaggi di convalida per il framework ASP.NET MVC.

> [!div class="step-by-step"]
> [Precedente](performing-simple-validation-cs.md)
> [Successivo](validating-with-a-service-layer-cs.md)
