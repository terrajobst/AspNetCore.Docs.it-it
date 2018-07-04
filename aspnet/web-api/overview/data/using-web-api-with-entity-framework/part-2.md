---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Aggiungere modelli e controller | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: f127f239bcc665f71976bb34f6d3387f8e0b72a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393149"
---
<a name="add-models-and-controllers"></a>Aggiungere modelli e controller
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà le classi di modello che definiscono le entità del database. Si aggiungeranno quindi controller API Web che eseguono operazioni CRUD su tali entità.

## <a name="add-model-classes"></a>Aggiungere le classi del modello

In questa esercitazione si creerà il database usando l'approccio "Code First" per Entity Framework (EF). Code First, si scrivono in c# le classi che corrispondono alle tabelle di database ed Entity Framework crea il database. (Per altre informazioni, vedere [approcci di sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Si inizia definendo gli oggetti di dominio come oggetti poco (plain-old CLR Object). Si creerà gli oggetti poco seguenti:

- Autore
- Libro

In Esplora soluzioni, pulsante destro del mouse, fare clic sulla cartella modelli. Selezionare **Add**, quindi selezionare **classe**. Assegnare alla classe il nome `Author`.

![](part-2/_static/image1.png)

Sostituire tutto il codice boilerplate in Author.cs con il codice seguente.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Aggiungere un'altra classe denominata `Book`, con il codice seguente.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework, tali modelli verranno utilizzati per creare tabelle di database. Per ogni modello, il `Id` proprietà diventerà la colonna chiave primaria della tabella di database.

Nella classe Book, il `AuthorId` definisce una chiave esterna nel `Author` tabella. (Per motivi di semplicità, suppongo che ogni libro ha un singolo autore.) La classe book contiene inoltre una proprietà di navigazione per i relativi `Author`. È possibile utilizzare la proprietà di navigazione per accedere correlato `Author` nel codice. Informazioni sulle proprietà di navigazione nella parte 4, dico [gestione delle relazioni tra entità](part-4.md).

## <a name="add-web-api-controllers"></a>Aggiungere controller API Web

In questa sezione si aggiungerà il controller API Web che supportano le operazioni CRUD (creare, leggere, aggiornare ed eliminare). Il controller userà Entity Framework per comunicare con il livello di database.

In primo luogo, è possibile eliminare il file Controllers/ValuesController.cs. Questo file contiene un controller API Web di esempio, ma non è necessario per questa esercitazione.

![](part-2/_static/image2.png)

Successivamente, compilare il progetto. Lo scaffolding API Web Usa la reflection per trovare le classi del modello, pertanto è necessario l'assembly compilato.

In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **Add**, quindi selezionare **Controller**.

![](part-2/_static/image3.png)

Nel **Add Scaffold** finestra di dialogo, seleziona "Web API 2 Controller con azioni, che usa Entity Framework". Fare clic su **Aggiungi**.

![](part-2/_static/image4.png)

Nel **Aggiungi Controller** finestra di dialogo, eseguire le operazioni seguenti:

1. Nel **classe modello** elenco a discesa, seleziona il `Author` classe. (Se non viene visualizzato nell'elenco a discesa, assicurarsi che è stato compilato il progetto.)
2. Controllare "Azioni del controller asincrono Usa".
3. Lasciare il nome controller &quot;AuthorsController&quot;.
4. Fare clic su più (+) accanto al pulsante **alla classe contesto dati**.

![](part-2/_static/image5.png)

Nel **nuovo contesto dati** finestra di dialogo, lasciare il nome predefinito e fare clic su **Add**.

![](part-2/_static/image6.png)

Fare clic su **Add** per completare il **Aggiungi Controller** finestra di dialogo. La finestra di dialogo aggiunge due classi al progetto:

- `AuthorsController` definisce un controller API Web. Il controller implementa l'API REST usata dai client per eseguire operazioni CRUD nell'elenco degli autori.
- `BookServiceContext` gestisce oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, il rilevamento delle modifiche e rendere persistenti i dati nel database. Eredita da `DbContext`.

![](part-2/_static/image7.png)

A questo punto, compilare di nuovo il progetto. Passare ora tramite la stessa procedura per aggiungere un controller API per `Book` entità. Selezionare questa volta `Book` per la classe di modello e selezionare esistente `BookServiceContext` classe per la classe del contesto dati. (Non creare un nuovo contesto dati). Fare clic su **Add** per aggiungere il controller.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Precedente](part-1.md)
> [Successivo](part-3.md)
