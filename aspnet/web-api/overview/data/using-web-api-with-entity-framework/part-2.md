---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a>Aggiungere modelli e controller
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà le classi di modello che definiscono le entità del database. Si aggiungeranno quindi controller API Web che eseguono operazioni CRUD su tali entità.

## <a name="add-model-classes"></a>Aggiungere le classi modello

In questa esercitazione si creerà il database utilizzando l'approccio "Code First" per Entity Framework (EF). Code First, si scrivono in c# le classi che corrispondono alle tabelle di database ed EF crea il database. (Per ulteriori informazioni, vedere [approcci allo sviluppo con Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Viene innanzitutto la definizione di oggetti di dominio come POCOs (oggetti CLR normale precedente). Verrà creata la POCOs seguenti:

- Autore
- Rubrica

In Esplora soluzioni, fare clic sulla cartella di modelli. Selezionare **Aggiungi**, quindi selezionare **classe**. Assegnare alla classe il nome `Author`.

![](part-2/_static/image1.png)

Sostituire tutto il codice boilerplate in Author.cs con il codice seguente.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Aggiungere un'altra classe denominata `Book`, con il codice seguente.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Questi modelli di Entity Framework verranno utilizzati per creare tabelle di database. Per ogni modello, il `Id` proprietà diventerà una colonna chiave primaria della tabella di database.

Nella classe Book di `AuthorId` definisce una chiave esterna nel `Author` tabella. (Per motivi di semplicità, suppone che ogni libro di autore di un singolo.) Classe book contiene anche una proprietà di navigazione per correlata `Author`. È possibile utilizzare la proprietà di navigazione per accedere correlata `Author` nel codice. Informazioni sulle proprietà di navigazione nella parte 4, dire [gestisce le relazioni di entità](part-4.md).

## <a name="add-web-api-controllers"></a>Aggiungere il controller API Web

In questa sezione verrà aggiunto il controller API Web che supportano le operazioni CRUD (creare, leggere, aggiornare ed eliminare). I controller utilizzerà Entity Framework per comunicare con il livello di database.

In primo luogo, è possibile eliminare il file Controllers/ValuesController.cs. Questo file contiene un controller API Web di esempio, ma non è necessario per questa esercitazione.

![](part-2/_static/image2.png)

Successivamente, compilare il progetto. Lo scaffolding di API Web utilizza la reflection per trovare le classi di modello, pertanto è necessario l'assembly compilato.

In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **Aggiungi**, quindi selezionare **Controller**.

![](part-2/_static/image3.png)

Nel **aggiungere lo scaffolding** finestra di dialogo, seleziona "Web API 2 Controller con azioni, mediante Entity Framework". Fare clic su **Aggiungi**.

![](part-2/_static/image4.png)

Nel **Aggiungi Controller** finestra di dialogo, eseguire le operazioni seguenti:

1. Nel **classe modello** elenco a discesa, seleziona la `Author` classe. (Se non viene visualizzato nell'elenco a discesa, assicurarsi che il progetto è stato generato.)
2. Selezionare "Usa azioni asincrone del controller".
3. Lasciare il nome del controller come &quot;AuthorsController&quot;.
4. Fare clic su più (+) accanto al pulsante **classe contesto dati**.

![](part-2/_static/image5.png)

Nel **nuovo contesto dati** finestra di dialogo, lasciare il nome predefinito e fare clic su **Aggiungi**.

![](part-2/_static/image6.png)

Fare clic su **Aggiungi** per completare il **Aggiungi Controller** finestra di dialogo. La finestra di dialogo aggiunge due classi al progetto:

- `AuthorsController` definisce un controller API Web. Il controller implementa l'API REST utilizzato dai client per eseguire operazioni CRUD su nell'elenco degli autori.
- `BookServiceContext` gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, il rilevamento delle modifiche e rendere persistenti i dati al database. Eredita da `DbContext`.

![](part-2/_static/image7.png)

A questo punto, generare di nuovo il progetto. Ora accedere tramite gli stessi passaggi per aggiungere un controller di API per `Book` entità. In questo momento, selezionare `Book` per la classe di modello e selezionare esistente `BookServiceContext` classe per la classe di contesto dati. (Non creare un nuovo contesto dati). Fare clic su **Aggiungi** per aggiungere il controller.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Precedente](part-1.md)
> [Successivo](part-3.md)
