---
title: Sviluppo di app ASP.NET Core con dotnet watch
author: rick-anderson
description: Questa esercitazione illustra come installare e usare lo strumento watcher per file dell'interfaccia della riga di comando di .NET Core (dotnet watch) in un'applicazione ASP.NET Core.
keywords: ASP.NET Core, usando dotnet watch
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: e7f01a649f240b6b57118c53314ab82f7f36f2eb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Sviluppo di app ASP.NET Core con dotnet watch

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` è uno strumento che esegue un comando dell'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools) quando i file di origine vengono modificati. Ad esempio, una modifica di file può attivare la compilazione, l'esecuzione di test o la distribuzione.

In questa esercitazione si usa un'app Web API esistente con due endpoint: uno restituisce una somma e uno restituisce un prodotto. Il metodo del prodotto contiene un bug che verrà corretto nel corso di questa esercitazione.

Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Contiene due progetti: *WebApp* (un'API Web di ASP.NET Core) e *WebAppTests* (unit test per l'API Web).

In una shell dei comandi passare alla cartella *WebApp* ed eseguire il comando seguente:

```console
dotnet run
```

L'output della console visualizza messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

In un Web browser passare a `http://localhost:<port number>/api/math/sum?a=4&b=5`. Dovrebbe essere visualizzato il risultato `9`.

Passare all'API del prodotto (`http://localhost:<port number>/api/math/product?a=4&b=5`). Restituisce `9` e non `20` come previsto. La correzione verrà apportata in una fase successiva dell'esercitazione.

## <a name="add-dotnet-watch-to-a-project"></a>Aggiungere `dotnet watch` a un progetto

1. Aggiungere un riferimento al pacchetto `Microsoft.DotNet.Watcher.Tools` nel file *.csproj*:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. Installare il pacchetto `Microsoft.DotNet.Watcher.Tools` eseguendo il comando seguente:
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>Esecuzione dei comandi dell'interfaccia della riga di comando di .NET Core con `dotnet watch`

Qualsiasi [comando dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools#cli-commands) può essere eseguito con `dotnet watch`. Ad esempio:

| Comando | Comando con watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

Eseguire `dotnet watch run` nella cartella *WebApp*. L'output della console indica che `watch` è stato avviato.

## <a name="making-changes-with-dotnet-watch"></a>Apportare modifiche ai dati con `dotnet watch`

Assicurarsi che `dotnet watch` sia in esecuzione.

Correggere il bug nel metodo `Product` di *MathController.cs* in modo che restituisca il prodotto e non la somma:

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Salvare il file. L'output della console indica che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.

Verificare che `http://localhost:<port number>/api/math/product?a=4&b=5` restituisca il risultato corretto.

## <a name="running-tests-using-dotnet-watch"></a>Esecuzione di test usando `dotnet watch`

1. Modificare il metodo `Product` di *MathController.cs* di nuovo in modo che restituisca la somma e salvare il file.
1. In una shell dei comandi passare alla cartella *WebAppTests*.
1. Eseguire `dotnet restore`.
1. Eseguire `dotnet watch test`. L'output indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Correggere il codice del metodo `Product` in modo che restituisca il prodotto. Salvare il file.

`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test. L'output della console indica che i test sono stati superati.

## <a name="dotnet-watch-in-github"></a>dotnet-watch in GitHub

dotnet-watch fa parte del [repository DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) GitHub.

La [sezione MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) spiega come dotnet-watch può essere configurato dal file di progetto MSBuild che viene controllato. Il file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene informazioni su dotnet-watch non trattate in questa esercitazione.
