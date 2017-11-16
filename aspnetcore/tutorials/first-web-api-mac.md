---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Mac
description: Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.openlocfilehash: 6bbd5e332e395928d8f79888ecf190f7f59a4bbc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

In questa esercitazione si creerà un'API Web per la gestione di un elenco di elementi di tipo "attività" e non un'interfaccia utente.

Sono disponibili 3 versioni dell'esercitazione:

* macOS: API Web con Visual Studio per Mac (questa esercitazione)
* Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Vedere [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introduzione ad ASP.NET Core MVC su Mac o Linux) per un esempio che usa un database persistente.

## <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:

- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva
- [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio selezionare **File > Nuova soluzione**.

![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

Selezionare **.NET Core App (App .NET Core) > ASP.NET Core Web API (API Web ASP.NET Core) > Avanti**.

![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)

Immettere **TodoApi** in **Nome progetto**, quindi selezionare Crea.

![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app. Visual Studio avvia un browser e passa a `http://localhost:5000`. Si verifica un errore HTTP 404 (Non trovato).  Modificare l'URL in `http://localhost:port/api/values`. Vengono visualizzati i dati `ValuesController`:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Aggiungere il supporto per Entity Framework Core

Installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/). Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.

* Nel menu **Progetto** scegliere **Add NuGet Packages** (Aggiungi pacchetti NuGet al progetto). 

  *  In alternativa fare clic con il pulsante destro del mouse su **Dipendenze** e selezionare **Aggiungi pacchetto**.

* Immettere `EntityFrameworkCore.InMemory` nella casella di ricerca.
* Selezionare `Microsoft.EntityFrameworkCore.InMemory`, quindi selezionare **Aggiungi pacchetto**.

### <a name="add-a-model-class"></a>Aggiungere una classe modello

Un modello è un oggetto che rappresenta i dati nell'applicazione. In questo caso l'unico modello è un elemento attività.

Aggiungere una cartella denominata *Models*. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.

![Nuova cartella](first-web-api-mac/_static/folder.png)

Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

Aggiungere una classe `TodoItem`. Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi > Nuovo file > Generale > Classe vuota**. Assegnare il nome `TodoItem` alla classe, quindi selezionare **Nuovo**.

Sostituire il codice generato con:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Il database genera il `Id` quando viene creato un `TodoItem`.

### <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

Aggiungere una classe `TodoContext` alla cartella *Models*.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Aggiungere un controller

In Esplora soluzioni, nella cartella *Controllers* aggiungere la classe `TodoController`.

Sostituire il codice generato con il seguente codice (e aggiungere le parentesi graffe di chiusura):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app. Visual Studio viene avviato un browser e passa a `http://localhost:port`, dove *port* è un numero di porta selezionato a caso. Si verifica un errore HTTP 404 (Non trovato).  Modificare l'URL in `http://localhost:port/api/values`. Vengono visualizzati i dati `ValuesController`:

```
["value1","value2"]
```

Passare al controller `Todo` all'indirizzo `http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementare le altre operazioni CRUD

Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller. Dato che si tratta di variazioni di un tema, di seguito viene mostrato il codice evidenziando le differenze principali. Compilare il progetto dopo l'aggiunta o la modifica del codice.

### <a name="create"></a>Crea

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Questo è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). L'attributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.

Il metodo `CreatedAtRoute` restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server. `CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Usare Postman per inviare una richiesta di creazione

* Avviare l'app (**Esegui > Avvia eseguendo il debug**).
* Avviare Postman.

![Console di Postman](first-web-api/_static/pmc.png)

* Impostare il metodo HTTP su `POST`.
* Selezionare il pulsante di opzione **Body** (Corpo).
* Selezionare il pulsante di opzione **raw** (non elaborato).
* Impostare il tipo su JSON
* Nell'editor chiave-valore immettere un elemento attività, ad esempio:

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Selezionare **Send** (Invia).

* Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):

![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmget.png)

È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa appena creata. Richiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Aggiorna

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` è simile a `Create` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta. Per supportare gli aggiornamenti parziali usare HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminare

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Passaggi successivi

* [Routing alle azioni del controller](xref:mvc/controllers/routing)
* Per informazioni sulla distribuzione dell'API, vedere [Pubblicazione e distribuzione](../publishing/index.md).
* [Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
