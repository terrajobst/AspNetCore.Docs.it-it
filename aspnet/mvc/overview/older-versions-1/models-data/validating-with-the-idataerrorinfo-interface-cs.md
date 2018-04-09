---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Convalida con l'interfaccia IDataErrorInfo (c#) | Documenti Microsoft
author: StephenWalther
description: Stephen Walther viene illustrato come visualizzare i messaggi di errore di convalida personalizzate implementando l'interfaccia IDataErrorInfo in una classe di modello.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Convalida con l'interfaccia IDataErrorInfo (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther viene illustrato come visualizzare i messaggi di errore di convalida personalizzate implementando l'interfaccia IDataErrorInfo in una classe di modello.


L'obiettivo di questa esercitazione è illustrare un approccio per l'esecuzione di convalida in un'applicazione ASP.NET MVC. Informazioni su come impedire l'invio di un form HTML senza fornire valori per i campi modulo necessari. In questa esercitazione, informazioni su come eseguire la convalida tramite l'interfaccia IErrorDataInfo.

## <a name="assumptions"></a>Presupposti

In questa esercitazione userà il database MoviesDB e la tabella di database di filmati. Questa tabella contiene le colonne seguenti:

<a id="0.5_table01"></a>


| **Nome colonna** | **Tipo di dati** | **Consenti valori null** |
| --- | --- | --- |
| Id | Int | False |
| Titolo | Nvarchar (100) | False |
| Director | Nvarchar (100) | False |
| DateReleased | DateTime | False |


In questa esercitazione, usare Microsoft Entity Framework per generare classi di modello il database. La classe di film generata da Entity Framework viene visualizzata nella figura 1.


[![L'entità di film](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: entità del filmato ([fare clic per visualizzare l'immagine ingrandita](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Per ulteriori informazioni sull'utilizzo di Entity Framework per generare le classi di modello di database, vedere che l'esercitazione intitolata Creazione di classi del modello con Entity Framework.


## <a name="the-controller-class"></a>La classe Controller

Si utilizzano controller Home per filmati elenco e creare nuovi film. Il codice per questa classe è contenuto in elenco 1.

**Elenco 1 - controllers\homecontroller.cs.**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La classe controller Home nel listato 1 contiene due azioni di metodo di creazione. La prima azione consente di visualizzare il form HTML per la creazione di un nuovo film. La seconda azione di metodo di creazione esegue l'inserimento effettivo del filmato nuovo nel database. La seconda azione di metodo di creazione viene richiamata quando il form visualizzato per la prima azione di metodo di creazione viene inviato al server.

Si noti che la seconda azione di metodo di creazione contiene le righe di codice seguente:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La proprietà IsValid restituisce false quando si verifica un errore di convalida. In tal caso, viene visualizzata di nuovo la visualizzazione di creazione che contiene il form HTML per la creazione di un film.

## <a name="creating-a-partial-class"></a>Creazione di una classe parziale

La classe film viene generata da Entity Framework. Se si espande il file MoviesDBModel.edmx nella finestra Esplora soluzioni e apre il file MoviesDBModel.Designer.cs nell'Editor di codice, è possibile visualizzare il codice per la classe filmato (vedere la figura 2).


[![Il codice per l'entità di film](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: il codice per l'entità filmato ([fare clic per visualizzare l'immagine ingrandita](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


La classe film è una classe parziale. Ciò significa che è possibile aggiungere un'altra classe parziale con lo stesso nome per estendere la funzionalità della classe film. La logica di convalida verrà aggiunto alla nuova classe parziale.

Aggiungere la classe nel listato 2 nella cartella Models.

**Il listato 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Si noti che la classe nel listato 2 include i *parziale* modificatore. Tutti i metodi o proprietà che si aggiunge a questa classe diventano parte della classe film generata da Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Aggiunta di metodi di OnChanged parziale e OnChanging

Quando Entity Framework genera una classe di entità, alla classe di metodi parziali Entity Framework aggiunge automaticamente. Entity Framework genera OnChanging e OnChanged metodi parziali che corrispondono a ogni proprietà della classe.

Nel caso della classe di film, Entity Framework consente di creare i metodi seguenti:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Il metodo OnChanging viene chiamato destra prima che venga modificata la proprietà corrispondente. Metodo OnChanged viene chiamato destra dopo la modifica della proprietà.

È possibile utilizzare questi metodi parziali per aggiungere la logica di convalida per la classe di film. L'aggiornamento classe film listato 3 verifica che le proprietà del titolo e Director vengono assegnate valori non vuoti.

> [!NOTE] 
> 
> Un metodo parziale è un metodo definito in una classe che non è necessario implementare. Se non si implementa un metodo parziale il compilatore rimuove la firma del metodo e tutte le chiamate al metodo pertanto vi sono costi in fase di esecuzione associati al metodo parziale. Nell'Editor di codice di Visual Studio, è possibile aggiungere un metodo parziale digitando la parola chiave *parziale* seguiti da uno spazio per visualizzare un elenco di parziali per implementare.


**Elenco di 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Ad esempio, se si tenta di assegnare una stringa vuota per la proprietà Title, quindi un messaggio di errore viene assegnato a un dizionario denominato \_errori.

A questo punto, in realtà non accade nulla quando si assegna una stringa vuota per la proprietà Title e viene aggiunto un errore per privato \_campo errori. È necessario implementare l'interfaccia IDataErrorInfo per esporre tali errori di convalida per il framework di MVC ASP.NET.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementazione dell'interfaccia IDataErrorInfo

IDataErrorInfo (interfaccia) è stato parte di .NET framework dalla prima versione. Questa interfaccia è un'interfaccia molto semplice:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Se una classe implementa l'interfaccia IDataErrorInfo, il framework di MVC ASP.NET utilizzerà questa interfaccia durante la creazione di un'istanza della classe. Ad esempio, il controller Home azione metodo di creazione accetta un'istanza della classe filmato:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Il framework di MVC ASP.NET crea l'istanza del filmato passato all'azione del metodo di creazione usando un raccoglitore di modelli (il DefaultModelBinder). Lo strumento di associazione è responsabile per la creazione di un'istanza dell'oggetto film associando i campi di form HTML a un'istanza dell'oggetto film.

Il DefaultModelBinder rileva una classe implementa l'interfaccia IDataErrorInfo o meno. Se una classe implementa questa interfaccia lo strumento di associazione del modello richiama l'indicizzatore IDataErrorInfo.this per ogni proprietà della classe. Se l'indicizzatore restituisce un messaggio di errore lo strumento di associazione aggiunge questo messaggio di errore per automaticamente lo stato del modello.

Il DefaultModelBinder controlla anche la proprietà IDataErrorInfo.Error. Questa proprietà è destinata a rappresentano errori di convalida specifici non di proprietà associati alla classe. Potrebbe ad esempio, si desidera applicare una regola di convalida che dipende dai valori di più proprietà della classe film. In tal caso, si restituisce un errore di convalida della proprietà di errore.

La classe film aggiornata nel listato 4 implementa l'interfaccia IDataErrorInfo.

**Listato 4 - Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

Listato 4, controlla la proprietà dell'indicizzatore di \_raccolta di errori per vedere se contiene una chiave che corrisponde al nome della proprietà passati all'indicizzatore. Se non si verificano errori di convalida associato alla proprietà viene restituita una stringa vuota.

Non è necessario modificare il controller Home in alcun modo per utilizzare la classe film modificata. La pagina visualizzata nella figura 3 viene illustrato cosa accade quando viene immesso alcun valore per i titolo o Director i campi del form.


[![La creazione automatica di metodi di azione](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: un form con i valori mancanti ([fare clic per visualizzare l'immagine ingrandita](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Si noti che il valore DateReleased viene convalidato automaticamente. Poiché la proprietà DateReleased non accetta valori NULL, il DefaultModelBinder genera automaticamente un errore di convalida per questa proprietà quando non dispone di un valore. Se si desidera modificare il messaggio di errore per la proprietà DateReleased è necessario creare un raccoglitore di modelli personalizzati.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come utilizzare l'interfaccia IDataErrorInfo per generare messaggi di errore di convalida. In primo luogo, è creata una classe film parziale che estende la funzionalità di classe parziale film generata da Entity Framework. Successivamente, è stata aggiunta logica di convalida per i film classe OnTitleChanging() e OnDirectorChanging() metodi parziali. Infine, abbiamo implementato IDataErrorInfo (interfaccia) per esporre questi messaggi di convalida per il framework di MVC ASP.NET.

> [!div class="step-by-step"]
> [Precedente](performing-simple-validation-cs.md)
> [Successivo](validating-with-a-service-layer-cs.md)
