---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4ce808ec4241ab2fc3c2fb81c3fdb15dd853cd90
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342276"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Creare un'API Web con ASP.NET Core e Visual Studio Code

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività". Non è stata costruita un'interfaccia utente.

Sono disponibili tre versioni di questa esercitazione:

* macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)
* macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Creare il progetto

Da una console eseguire i comandi seguenti:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

In Visual Studio Code (VS Code) viene aperta la cartella *TodoApi*. Selezionare il file *Startup.cs*.

* Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi" e se si vuole aggiungerle.
* Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi" e se si vuole aggiungerle. Non chiedere più, Non ora, Sì](web-api-vsc/_static/vsc_restore.png)

Premere **Debug** (F5) per compilare ed eseguire il programma. In un browser passare a http://localhost:5000/api/values. Verrà visualizzato l'output seguente:

```json
["value1","value2"]
```

Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.

## <a name="add-support-for-entity-framework-core"></a>Aggiungere il supporto per Entity Framework Core

:::moniker range=">= aspnetcore-2.1"

La creazione di un nuovo progetto in ASP.NET Core 2.1 o versioni successive aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) nel file *TodoApi.csproj*. Aggiungere l'attributo `Version`, se non è già specificato.

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

La creazione di un nuovo progetto in ASP.NET Core 2.0 aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) nel file *TodoApi.csproj*:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

Non occorre installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separatamente. Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.

## <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un modello è un oggetto che rappresenta i dati nell'app. In questo caso l'unico modello è un elemento attività.

Aggiungere una cartella denominata *Modelli*. È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

Aggiungere una classe `TodoItem` con il codice seguente:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Il database genera `Id` quando viene creato un `TodoItem`.

## <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

Aggiungere una classe `TodoContext` alla cartella *Models*:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Aggiungere un controller

Nella cartella *Controllers* creare una classe denominata `TodoController`. Sostituire il contenuto con il codice seguente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio Code premere F5 per avviare l'app. Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Guida di Visual Studio Code

* [Introduzione](https://code.visualstudio.com/docs)
* [Debug](https://code.visualstudio.com/docs/editor/debugging)
* [Terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Tasti di scelta rapida](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [Tasti di scelta rapida di macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Tasti di scelta rapida di Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Tasti di scelta rapida di Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
