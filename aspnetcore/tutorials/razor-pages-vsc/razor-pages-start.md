---
title: Introduzione a pagine Razor ASP.NET Core in Visual Studio Code
author: rick-anderson
description: Informazioni di base sulla compilazione di un'app Web pagine Razor ASP.NET Core con Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244853"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Introduzione a pagine Razor ASP.NET Core in Visual Studio Code

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:razor-pages/index) prima di iniziare questa esercitazione. Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

Da un terminale eseguire i comandi seguenti:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

I comandi precedenti usano [.NET Core CLI](/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor. Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Aprire il progetto

Premere Ctrl + C per arrestare l'applicazione.

Da Visual Studio Code (VS Code), selezionare **File > Apri cartella**, quindi selezionare la cartella *RazorPagesMovie*.

- Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano in "RazorPagesMovie" e se si vuole aggiungerle.
- Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".

### <a name="launch-the-app"></a>Avviare l'app

Scegliere **Avvia senza eseguire debug** dal menu **Debug**. In alternativa, è possibile premere il tasto di scelta rapida visualizzato accanto all'opzione di menu. Il tasto di scelta rapida varia a seconda del sistema operativo.

Nella prossima esercitazione viene aggiunto un modello al progetto. 

> [!div class="step-by-step"]
> [Avanti: Aggiunta di un modello](xref:tutorials/razor-pages-vsc/model)  
