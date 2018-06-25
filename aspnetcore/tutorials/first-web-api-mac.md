---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Mac
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279610"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Creare un'API Web con ASP.NET Core e Visual Studio per Mac

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività". Non è stata costruita l'interfaccia utente.

Sono disponibili tre versioni di questa esercitazione:

* macOS: API Web con Visual Studio per Mac (questa esercitazione)
* Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Vedere [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introduzione ad ASP.NET Core MVC su macOS o Linux) per un esempio che usa un database persistente.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio selezionare **File** > **Nuova soluzione**.

![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

Selezionare **.NET Core App** > **ASP.NET Core Web API** > **Next**.

![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)

Immettere *TodoApi* in **Nome progetto**, quindi fare clic su **Crea**.

![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app. Visual Studio avvia un browser e passa a `http://localhost:5000`. Si verifica un errore HTTP 404 (Non trovato). Modificare l'URL in `http://localhost:<port>/api/values`. Vengono visualizzati i dati `ValuesController`:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Aggiungere il supporto per Entity Framework Core

Installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.

* Nel menu **Progetto** scegliere **Add NuGet Packages** (Aggiungi pacchetti NuGet al progetto).

  * In alternativa fare clic con il pulsante destro del mouse su **Dipendenze** e selezionare **Aggiungi pacchetti**.

* Immettere `EntityFrameworkCore.InMemory` nella casella di ricerca.
* Selezionare `Microsoft.EntityFrameworkCore.InMemory`, quindi selezionare **Aggiungi pacchetto**.

### <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un modello è un oggetto che rappresenta i dati nell'app. In questo caso l'unico modello è un elemento attività.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.

![Nuova cartella](first-web-api-mac/_static/folder.png)

> [!NOTE]
> È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**. Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.

Sostituire il codice generato con:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Il database genera `Id` quando viene creato un `TodoItem`.

### <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

Aggiungere una classe `TodoContext` alla cartella *Models*.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Aggiungere un controller

In Esplora soluzioni, nella cartella *Controllers* aggiungere la classe `TodoController`.

Sostituire il codice generato con il seguente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app. Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso. Si verifica un errore HTTP 404 (Non trovato). Modificare l'URL in `http://localhost:<port>/api/values`. Vengono visualizzati i dati `ValuesController`:

```json
["value1","value2"]
```

Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`. Viene restituito il codice JSON seguente:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementare le altre operazioni CRUD

Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller. Dato che questi metodi sono variazioni su un tema, il codice visualizzato di seguito evidenzia le differenze principali. Compilare il progetto dopo l'aggiunta o la modifica del codice.

### <a name="create"></a>Crea

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il metodo precedente risponde a un POST HTTP, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). L'attributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Il metodo precedente risponde a un POST HTTP, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.
::: moniker-end

Il metodo `CreatedAtRoute` restituisce una risposta 201. Si tratta della risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server. `CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta. L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato. Vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Usare Postman per inviare una richiesta di creazione

* Avviare l'app (**Esegui** > **Avvia eseguendo il debug**).
* Aprire Postman.

![Console di Postman](first-web-api/_static/pmc.png)

* Aggiornare il numero di porta nell'URL localhost.
* Impostare il metodo HTTP su *POST*.
* Fare clic sulla scheda **Body** (Corpo).
* Selezionare il pulsante di opzione **raw** (non elaborato).
* Impostare il tipo su *JSON (application/json)*.
* Immettere un corpo della richiesta con un elemento attività simile al codice JSON seguente:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Fare clic sul pulsante **Invia**.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Se dopo aver fatto clic su **Invia** non viene visualizzata nessuna risposta, disattivare l'opzione **SSL certification verification** (Verifica certificazione SSL). L'opzione è disponibile in **File** > **Impostazioni**. Fare di nuovo clic sul pulsante **Invia** dopo aver disabilitato l'impostazione.
::: moniker-end

Fare clic sulla scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta) e copiare l'intestazione **Location** (Posizione):

![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa creata. Il metodo `Create` restituisce [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Il primo parametro passato a `CreatedAtRoute` rappresenta la route denominata da usare per generare l'URL. Chiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Aggiorna

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` è simile a `Create` ma usa la richiesta HTTP PUT. La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta. Per supportare gli aggiornamenti parziali usare HTTP PATCH.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Eliminare

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
