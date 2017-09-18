---
title: Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio Code
keywords: ASP.NET Core, pagine Razor, scaffolding, Entity Framework Core, EF, EF Core, database, mac, macOS, Visual Studio Code, Code
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 096a60dae171ac5dbfa935be4c16e0903d8f5bb3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio Code

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione. Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

Da un terminale eseguire i comandi seguenti:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

I comandi precedenti usano [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor. Aprire un browser all'indirizzo http://localhost:5000/ per visualizzare l'applicazione.

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Aprire il progetto

Premere Ctrl + C per arrestare l'applicazione.

Da Visual Studio Code (VS Code), selezionare **File > Apri cartella**, quindi selezionare la cartella *RazorPagesMovie*.

- Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano in "RazorPagesMovie" e se si vuole aggiungerle.
- Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".

### <a name="launch-the-app"></a>Avviare l'app

Premere CTRL+F5 per avviare l'app senza debug. In alternativa, scegliere **Avvia senza eseguire debug** dal menu **Debug**.

Nella prossima esercitazione viene aggiunto un modello al progetto. 

>[!div class="step-by-step"]
[Avanti: Aggiunta di un modello](xref:tutorials/razor-pages-vsc/model)  
