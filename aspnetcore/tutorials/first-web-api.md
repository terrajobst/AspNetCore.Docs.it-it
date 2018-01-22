---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Creare un'API Web con ASP.NET Core e Visual Studio per Windows

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività" e non un'interfaccia utente (UI).

Sono disponibili 3 versioni dell'esercitazione:

* Windows: API Web con Visual Studio per Windows (questa esercitazione)
* macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[install 2.0](../includes/install2.0.md)]

Vedere [questo PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) per la versione ASP.NET Core 1.1.

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio selezionare il menu **File** > **Nuovo** > **Progetto**.

Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**. Assegnare il nome `TodoApi` al progetto e scegliere **OK**.

![Finestra di dialogo Nuovo progetto](first-web-api/_static/new-project.png)

Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi**, selezionare il modello **API Web**. Scegliere **OK**. **Non** selezionare **Abilita supporto Docker**.

![Finestra di dialogo Nuova Applicazione Web ASP.NET Core con il modello di progetto API Web selezionato tra i modelli ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio premere CTRL+F5 per avviare l'app. Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso. In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un modello è un oggetto che rappresenta i dati nell'app. In questo caso l'unico modello è un elemento attività.

Aggiungere una cartella denominata "Models". In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto. ma per convenzione viene usata la cartella *Models*.

Aggiungere una classe `TodoItem`. Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare il nome `TodoItem` alla classe, quindi selezionare **Aggiungi**.

Aggiornare la classe `TodoItem` con il codice seguente:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Il database genera `Id` quando viene creato un `TodoItem`.

### <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

Aggiungere una classe `TodoContext`. Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**. Assegnare il nome `TodoContext` alla classe, quindi selezionare **Aggiungi**.

Sostituire la classe con il codice seguente:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Aggiungere un controller

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*. Selezionare **Aggiungi** > **Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **Classe controller API Web**. Assegnare alla classe il nome `TodoController`.

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

Sostituire la classe con il codice seguente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio premere CTRL+F5 per avviare l'app. Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso. Passare al controller `Todo` all'indirizzo `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

