---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestione delle relazioni di entità | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="handling-entity-relations"></a>Gestione delle relazioni di entità
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

Questa sezione descrive alcuni dettagli del modo in cui EF carica entità correlate e come gestire le proprietà di navigazione circolare nelle classi del modello. (Questa sezione fornisce informazioni di background e non è necessario per completare l'esercitazione. Se si preferisce, andare al [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Caricamento e il caricamento Lazy eager

Quando si utilizza Entity Framework con un database relazionale, è importante comprendere la modalità di caricamento dati correlati in Entity Framework.

È inoltre utile visualizzare le query SQL che genera l'errore Entity Framework. Per tracciare SQL, aggiungere la seguente riga di codice per il `BookServiceContext` costruttore:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se si invia una richiesta GET a /api/books, restituisce JSON simile al seguente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Si noterà che nella proprietà Author è null, anche se il runbook contiene un AuthorId valido. Ciò avviene perché EF non è possibile caricare le entità correlate di autore. Questo conferma che il log di traccia della query SQL:

[!code-console[Main](part-4/samples/sample3.sql)]

L'istruzione SELECT accetta dalla tabella di documentazione e non fanno riferimento alla tabella di autore.

Per riferimento, di seguito è il metodo di `BooksController` classe che restituisce l'elenco di libri.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Di seguito viene illustrato come è possibile tornare all'autore come parte dei dati JSON. Esistono tre modi per caricare i dati correlati in Entity Framework: il caricamento non differito, il caricamento lazy e caricamento esplicito. Esistono vantaggi e svantaggi con ogni tecnica, è importante comprenderne il funzionamento.

### <a name="eager-loading"></a>Caricamento eager

Con *caricamento eager*, EF carica entità correlate come parte della query del database iniziale. Per eseguire il caricamento non differito, utilizzare il **System.Data.Entity.Include** metodo di estensione.

[!code-csharp[Main](part-4/samples/sample5.cs)]

In questo modo EF per includere i dati di autore della query. Se si apporta questa modifica e di eseguire l'app, ora i dati JSON sono simile al seguente:

[!code-console[Main](part-4/samples/sample6.cmd)]

Il log di traccia Mostra di EF eseguito un join sulle tabelle di libro e autore.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Caricamento lazy

Con il caricamento differito, EF carica automaticamente un'entità correlata quando la proprietà di navigazione per l'entità viene dereferenziata. Per abilitare il caricamento differito, impostare la proprietà di navigazione virtuale. Ad esempio, nella classe Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Si consideri ora il codice seguente:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando il caricamento lazy è abilitato, l'accesso di `Author` proprietà `books[0]` fa sì che Entity Framework eseguire query sul database per l'autore.

Caricamento lazy richiede più trip di database, in quanto EF invia una query ogni volta che viene recuperata un'entità correlata. In genere, si desidera il caricamento lazy disabilitato per gli oggetti da serializzare. Il serializzatore deve leggere tutte le proprietà sul modello, che attiva il caricamento delle entità correlate. Ad esempio, ecco le query SQL EF serializza l'elenco di libri con abilitato del caricamento lazy. È possibile vedere che EF rende tre query separate per tre autori.

[!code-console[Main](part-4/samples/sample10.sql)]

Sono ancora presenti volte quando si potrebbe desiderare di utilizzare il caricamento lazy. Caricamento eager può causare EF generare un join molto complesso. O potrebbe essere necessario entità correlate per un piccolo subset di dati e il caricamento lazy sarebbe più efficiente.

È di un modo per evitare problemi di serializzazione per serializzare oggetti di trasferimento di dati DTO anziché oggetti entità. Questo approccio vi mostrerò avanti in questo articolo.

### <a name="explicit-loading"></a>Caricamento esplicito

Caricamento esplicito è simile al caricamento lazy, ad eccezione del fatto che sia possibile ottenere in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Caricamento esplicito offre maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo. Per ulteriori informazioni sul caricamento esplicito, vedere [durante il caricamento delle entità correlate](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Le proprietà di navigazione e i riferimenti circolari

Definendo i modelli di libro e l'autore, sono state definite proprietà di navigazione nel `Book` classe per la relazione libro autore, ma non definita una proprietà di navigazione in altra direzione.

Cosa accade se si aggiunge la proprietà di navigazione corrispondente per la `Author` classe?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Sfortunatamente, questo costituisce un problema durante la serializzazione dei modelli. Se si caricano i dati correlati, viene creato un grafico circolare dell'oggetto.

![](part-4/_static/image1.png)

Quando il formattatore JSON o XML tenta di serializzare il grafico, verrà generata un'eccezione. I due formattatori generano messaggi di eccezione diverso. Di seguito è riportato un esempio per il formattatore JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Di seguito è riportato il formattatore XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Una soluzione consiste nell'utilizzare DTO, cui è descritta nella sezione successiva. In alternativa, è possibile configurare i formattatori JSON e XML per la gestione di cicli di grafico. Per ulteriori informazioni, vedere [la gestione di riferimenti circolari oggetto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Per questa esercitazione, non è necessario il `Author.Book` proprietà di navigazione, pertanto è possibile tralasciare.

> [!div class="step-by-step"]
> [Precedente](part-3.md)
> [Successivo](part-5.md)
