---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Creare oggetti di trasferimento dati DTO | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878685"
---
<a name="create-data-transfer-objects-dtos"></a>Creare oggetti di trasferimento dati (DTO)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

Diritto a questo punto, l'API web espone le entità di database per il client. Il client riceve i dati che è mappata direttamente a tabelle di database. Tuttavia, che non è sempre una buona idea. Talvolta si desidera modificare la forma dei dati inviati al client. Può, ad esempio, essere necessario:

- Rimuovere i riferimenti circolari (vedere la sezione precedente).
- Nascondere determinate proprietà che non dovrebbero per visualizzare i client.
- Omettere alcune proprietà per ridurre le dimensioni di payload.
- Convertire oggetti grafici che contengono gli oggetti annidati, in modo da renderli più utile per i client.
- Evitare di "eccesso di registrazione" vulnerabilità. (Vedere [la convalida del modello](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) per una descrizione della registrazione in eccesso.)
- Separare il livello di servizio dal livello del database.

A tale scopo, è possibile definire un *oggetto di trasferimento dati* (DTO). Un DTO è un oggetto che definisce come i dati verranno inviati in rete. Di seguito viene illustrato il funzionamento con l'entità Book. Nella cartella Models, aggiungere due classi DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Il `BookDetailDTO` classe sono incluse tutte le proprietà del modello di libro, con la differenza che `AuthorName` è una stringa che conterrà il nome dell'autore. Il `BookDTO` classe contiene un subset di proprietà da `BookDetailDTO`.

Successivamente, sostituire i due metodi GET di `BooksController` (classe), con le versioni che restituiscono DTO. Si userà il LINQ **selezionare** istruzione da convertire in DTO provenienti da entità Book.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Ecco il codice SQL generato dalla nuova `GetBooks` metodo. È possibile vedere che Entity Framework traduce LINQ **selezionare** in un'istruzione SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Infine, modificare il `PostBook` per restituire un DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In questa esercitazione viene convertito in DTO manualmente nel codice. Un'altra opzione consiste nell'utilizzare una libreria come [AutoMapper](http://automapper.org/) che gestisce automaticamente la conversione.
> 
> [!div class="step-by-step"]
> [Precedente](part-4.md)
> [Successivo](part-6.md)
