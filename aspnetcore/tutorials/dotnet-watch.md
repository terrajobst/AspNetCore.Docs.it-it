---
title: Sviluppo di app ASP.NET Core con dotnet watch
author: rick-anderson
description: Illustra come usare dotnet watch.
keywords: ASP.NET Core, usando dotnet watch
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Sviluppo di app ASP.NET Core con dotnet watch


Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` è uno strumento che esegue un comando `dotnet` quando i file di origine vengono modificati. Ad esempio, una modifica di file può attivare la compilazione, i test o la distribuzione.

In questa esercitazione si usa un'app Web API esistente con due endpoint: uno restituisce una somma e uno restituisce un prodotto. Il metodo del prodotto contiene un bug che verrà corretto nel corso di questa esercitazione.

Scaricare l'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Contiene due progetti, `WebApp` (un'app Web) e `WebAppTests` (unit test per l'app Web).

In una console passare alla cartella WebApp ed eseguire i comandi seguenti:

- `dotnet restore`
- `dotnet run`

L'output della console visualizzerà messaggi simili al seguente che indicano che l'app è in esecuzione e in attesa di richieste:

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

In un Web browser passare a `http://localhost:5000/api/math/sum?a=4&b=5`; il risultato dovrebbe essere `9`.

Passare all'API del prodotto (`http://localhost:5000/api/math/product?a=4&b=5`), viene restituito `9`, non `20` come previsto. La correzione verrà apportata in una fase successiva dell'esercitazione.

## <a name="add-dotnet-watch-to-a-project"></a>Aggiungere `dotnet watch` a un progetto

- Aggiungere `Microsoft.DotNet.Watcher.Tools` al file *.csproj*:
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- Eseguire `dotnet restore`.

## <a name="running-dotnet-commands-using-dotnet-watch"></a>Esecuzione di comandi `dotnet` usando `dotnet watch`

Qualsiasi comando `dotnet` può essere eseguito con `dotnet watch`, ad esempio:

| Comando | Comando con watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

Eseguire `dotnet watch run` nella cartella `WebApp`. L'output della console indicherà che `watch` è stato avviato.

## <a name="making-changes-with-dotnet-watch"></a>Apportare modifiche ai dati con `dotnet watch`

Assicurarsi che `dotnet watch` sia in esecuzione.

Correggere il bug nel metodo `Product` di `MathController` in modo che restituisca il prodotto e non la somma.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Salvare il file. L'output della console visualizzerà i messaggi che indicano che `dotnet watch` ha rilevato una modifica del file e ha riavviato l'app.

Verificare che `http://localhost:5000/api/math/product?a=4&b=5` restituisca il risultato corretto.

## <a name="running-tests-using-dotnet-watch"></a>Esecuzione di test usando `dotnet watch`

- Modificare il metodo `Product` di `MathController` di nuovo in modo che restituisca la somma e salvare il file.
- In una finestra di comando passare alla cartella `WebAppTests`.
- Eseguire `dotnet restore`
- Eseguire `dotnet watch test`. Viene visualizzato l'output che indica che un test non è stato superato e che il watcher è in attesa di modifiche ai file:

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- Correggere il codice del metodo `Product` in modo che restituisca il prodotto. Salvare il file.

`dotnet watch` rileva la modifica ai file ed esegue di nuovo i test. L'output della console indicherà che i test sono stati superati.

## <a name="dotnet-watch-in-github"></a>dotnet-watch in GitHub

dotnet-watch fa parte del [repository DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools) GitHub.

La [sezione MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) del file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) spiega come dotnet-watch può essere configurato dal file di progetto MSBuild che viene controllato. Il file [Leggimi di dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contiene informazioni su dotnet-watch non trattate in questa esercitazione.
