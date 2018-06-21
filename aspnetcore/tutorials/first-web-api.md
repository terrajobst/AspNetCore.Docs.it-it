---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277400"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Creare un'API Web con ASP.NET Core e Visual Studio per Windows

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività" e non un'interfaccia utente (UI).

Sono disponibili tre versioni di questa esercitazione:

* Windows: API Web con Visual Studio per Windows (questa esercitazione)
* macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Creare il progetto

Seguire questa procedura in Visual Studio:

* Scegliere **Nuovo** > **Progetto** dal menu **File**.
* Selezionare il modello **Applicazione Web ASP.NET Core**. Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.
* Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core. Selezionare il modello **API**, quindi scegliere **OK**. **Non** selezionare **Abilita supporto Docker**.

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio premere CTRL+F5 per avviare l'app. Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso. In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:

```json
["value1","value2"]
```

Se si usa Internet Explorer, viene richiesto di salvare un file *values.json*.

### <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un modello è un oggetto che rappresenta i dati nell'app. In questo caso l'unico modello è un elemento attività.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto. Selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

> [!NOTE]
> È possibile inserire le classi del modello in qualsiasi punto del progetto. ma per convenzione viene usata la cartella *Models*.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoItem* e fare clic su **Aggiungi**.

Aggiornare la classe `TodoItem` con il codice seguente:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Il database genera `Id` quando viene creato un `TodoItem`.

### <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**. Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.

Sostituire la classe con il codice seguente:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Aggiungere un controller

In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*. Selezionare **Aggiungi** > **Nuovo elemento**. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API). Assegnare alla classe il nome *TodoController* e fare clic su **Aggiungi**.

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

Sostituire la classe con il codice seguente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio premere CTRL+F5 per avviare l'app. Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso. Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
