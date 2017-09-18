---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, servizio, servizio HTTP, VS Code
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: abe088f2c9df94135209ce71540e6b345186ee70
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Creare un'API Web con ASP.NET Core MVC e Visual Studio Code in Linux, macOS e Windows

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)

In questa esercitazione si creerà un'API Web per la gestione di un elenco di elementi di tipo "attività" e non un'interfaccia utente.

Sono disponibili 3 versioni dell'esercitazione:

* macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)
* macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)
* Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Configurare l'ambiente di sviluppo

Scaricare e installare:
- [.NET Core](https://microsoft.com/net/core)
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>Creare il progetto

Da una console eseguire i comandi seguenti:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Aprire la cartella *TodoApi* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.

- Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi" e se si vuole aggiungerle.
- Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi" e se si vuole aggiungerle. Non visualizzare più questo messaggio, Non ora, Sì e anche Informazioni - esistono dipendenze non risolte - Ripristina - Chiudi](web-api-vsc/_static/vsc_restore.png)

Premere **Debug** (F5) per compilare ed eseguire il programma. In un browser passare a http://localhost:5000/api/values. Verrà visualizzato quanto segue:

`["value1","value2"]`

Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.

## <a name="add-support-for-entity-framework-core"></a>Aggiungere il supporto per Entity Framework Core

Modificare il file *TodoApi.csproj* per installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/). Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.

[!code-xml[Principale](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

Eseguire `dotnet restore` per scaricare e installare il provider di database in memoria di Entity Framework Core. È possibile eseguire `dotnet restore` dal terminale o immettere `⌘⇧P` (macOS) o `Ctrl+Shift+P` (Linux) in Visual Studio Code e quindi digitare **.NET**. Selezionare **.NET: Ripristina pacchetti**.

## <a name="add-a-model-class"></a>Aggiungere una classe del modello

Un modello è un oggetto che rappresenta i dati nell'applicazione. In questo caso l'unico modello è un elemento attività.

Aggiungere una cartella denominata *Models*. È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.

Aggiungere una classe `TodoItem` con il codice seguente:

[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Il database genera `Id` quando viene creato un `TodoItem`.

## <a name="create-the-database-context"></a>Creare il contesto di database

Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati. Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.

Aggiungere una classe `TodoContext` alla cartella *Models*:

[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Aggiungere un controller

Nella cartella *Controllers* creare una classe denominata `TodoController`. Aggiungere il codice seguente:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio Code premere F5 per avviare l'app. Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Guida di Visual Studio Code

- [Introduzione](https://code.visualstudio.com/docs)
- [Debug](https://code.visualstudio.com/docs/editor/debugging)
- [Terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Tasti di scelta rapida](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Tasti di scelta rapida di Mac](https://go.microsoft.com/fwlink/?linkid=832143)
  - [Tasti di scelta rapida di Linux](https://go.microsoft.com/fwlink/?linkid=832144)
  - [Tasti di scelta rapida di Windows](https://go.microsoft.com/fwlink/?linkid=832145)

[!INCLUDE[next steps](../includes/webApi/next.md)]


