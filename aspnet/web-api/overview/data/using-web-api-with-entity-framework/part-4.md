---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestione delle relazioni tra entità | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: fcfddb3d56d0be641d2df9d92c334776975621be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826157"
---
<a name="handling-entity-relations"></a>Relazioni tra entità di gestione
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

Questa sezione descrive alcuni dettagli di come Entity Framework carica entità correlate e come gestire le proprietà di navigazione circolare nelle classi di modelli. (Questa sezione fornisce informazioni di background e non è necessario per completare l'esercitazione. Se si preferisce, andare al [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Caricamento e il caricamento Lazy eager

Quando si usa Entity Framework con un database relazionale, è importante comprendere la modalità di caricamento dei dati correlati in Entity Framework.

È anche utile visualizzare le query SQL generato da EF. Per tracciare il codice SQL, aggiungere la seguente riga di codice per il `BookServiceContext` costruttore:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se si invia una richiesta GET a /api/books, restituisce JSON simile al seguente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Si noterà che la proprietà dell'autore è null, anche se il libro contiene un AuthorId valido. Ciò avviene perché EF non stia caricando le entità autore correlate. Il log di traccia della query SQL conferma questo:

[!code-console[Main](part-4/samples/sample3.sql)]

L'istruzione SELECT accetta dalla tabella dei libri e non fa riferimento la tabella di autore.

Per riferimento, ecco il metodo nel `BooksController` classe che restituisce l'elenco di libri.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Vediamo come possiamo restituire l'autore come parte dei dati JSON. Esistono tre modi per caricare i dati correlati in Entity Framework: il caricamento eager, il caricamento lazy e il caricamento esplicito. Esistono vantaggi e svantaggi con ogni tecnica, pertanto è importante comprenderne il funzionamento.

### <a name="eager-loading"></a>Caricamento eager

Con *caricamento eager*, Entity Framework carica entità correlate come parte della query del database iniziale. Per eseguire il caricamento eager, usare il **System.Data.Entity.Include** metodo di estensione.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Indica a Entity Framework per includere i dati dell'autore della query. Se si apporta questa modifica e si esegue l'app, i dati JSON sarà simile al seguente:

[!code-console[Main](part-4/samples/sample6.cmd)]

Il log di traccia indica che EF eseguita un join sulle tabelle di libro e autore.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Caricamento lazy

Con il caricamento lazy, Entity Framework carica automaticamente un'entità correlata quando la proprietà di navigazione per l'entità è dereferenziata. Per abilitare il caricamento lazy, verificare la proprietà di navigazione virtuale. Ad esempio, nella classe Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Si consideri ora il codice seguente:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando il caricamento lazy è abilitato, l'accesso alla `Author` proprietà `books[0]` fa in modo che Entity Framework eseguire query sul database per l'autore.

Il caricamento lazy richiede più trip di database, perché Entity Framework invia una query ogni volta che recupera un'entità correlata. In generale, è consigliabile il caricamento lazy disabilitato per gli oggetti da serializzare. Il serializzatore deve leggere tutte le proprietà sul modello, che attiva il caricamento di entità correlate. Ad esempio, ecco le query SQL quando Entity Framework serializza l'elenco di libri con caricamento lazy abilitata. È possibile vedere che Entity Framework esegue tre query separate per gli autori di tre.

[!code-console[Main](part-4/samples/sample10.sql)]

Esistono ancora volte quando si potrebbe voler usare il caricamento lazy. Il caricamento eager può causare Entity Framework generare un join molto complesso. O potrebbe essere necessario entità correlate per un piccolo subset di dati e il caricamento lazy risulterebbe sicuramente più efficace.

Un modo per evitare problemi di serializzazione deve serializzare gli oggetti di trasferimento di dati (DTO) invece di oggetti entità. Questo approccio verrà illustrato più avanti nell'articolo.

### <a name="explicit-loading"></a>Caricamento esplicito

Il caricamento esplicito è simile a il caricamento lazy, ad eccezione del fatto che i dati correlati di ottenere in modo esplicito nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Il caricamento esplicito offre un maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo. Per ulteriori informazioni sul caricamento esplicito, vedere [caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Le proprietà di navigazione e i riferimenti circolari

Quando definito i modelli di libro e autore, ho definito una proprietà di navigazione nel `Book` classe per la relazione autore del libro, ma non definiscono una proprietà di navigazione in altra direzione.

Cosa accade se si aggiunge la proprietà di navigazione corrispondente per il `Author` classe?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Sfortunatamente, questo costituisce un problema durante la serializzazione dei modelli. Se si caricano i dati correlati, viene creato un grafico circolare dell'oggetto.

![](part-4/_static/image1.png)

Quando il formattatore JSON o XML tenta di serializzare il grafico, verrà generata un'eccezione. I due formattatori generano messaggi di eccezione diverso. Di seguito è riportato un esempio per il formattatore JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Ecco il formattatore XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Un'unica soluzione consiste nell'usare gli oggetti DTO, che ho descritto nella sezione successiva. In alternativa, è possibile configurare i formattatori XML e JSON per gestire i cicli di grafico. Per altre informazioni, vedere [gestisce i riferimenti all'oggetto circolare](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Per questa esercitazione, non è necessario il `Author.Book` proprietà di navigazione, pertanto è possibile tralasciare.

> [!div class="step-by-step"]
> [Precedente](part-3.md)
> [Successivo](part-5.md)
