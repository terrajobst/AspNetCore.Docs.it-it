---
title: Compilazione e precompilazione delle visualizzazioni Razor in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la compilazione e precompilazione delle visualizzazioni Razor MVC nelle app ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Compilazione del file Razor con estensione cshtml in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le visualizzazioni Razor vengono compilate in fase di esecuzione quando la viene richiamata la visualizzazione. ASP.NET Core 2.1.0 o versioni successive compila le visualizzazioni in fase di generazione e pubblicazione usando [Razor SDK](/aspnetcore/mvc/razor-pages/sdk). In ASP.NET Core 1.1 e ASP.NET Core 2.0 le visualizzazioni possono essere facoltativamente compilate in fase di pubblicazione e distribuite con l'app &mdash; usando lo strumento di precompilazione. 



Considerazioni sulla precompilazione:

* La precompilazione delle visualizzazioni consente di ridurre le dimensioni del bundle pubblicato e velocizzare il tempo di avvio.
* Non è possibile modificare i file Razor dopo aver precompliato le visualizzazioni. Le visualizzazioni modificate non saranno contenute nel bundle pubblicato. 

Per distribuire le visualizzazioni precompilate:

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
La compilazione di file Razor in fase di generazione e pubblicazione è abilitata per impostazione predefinita da Razor SDK. La modifica dei file Razor dopo essere stati aggiornati è supportata in fase di generazione. Per impostazione predefinita, vengono distribuite solo *Views.dll* compilate con l'applicazione. Non viene distribuito nessun file cshtml. 
    
> [!IMPORTANT]
> Razor SDK è efficace solo quando non sono impostate proprietà specifiche di precompilazione nel file di progetto. Ad esempio, se si imposta `MvcRazorCompileOnPublish` nel file *con estensione csproj*, Razor SDK sarà disabilitato.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Se .NET Framework è la destinazione del progetto, includere un riferimento al pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

Per impostazione predefinita, i modelli di progetto ASP.NET Core 2.x impostano in modo implicito `MvcRazorCompileOnPublish` su `true`. In questo modo il nodo può essere rimosso in modo sicuro dal file *con estensione csproj*.
    
> [!IMPORTANT]
> La precompilazione delle visualizzazioni Razor non è disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0. 

Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish). Ad esempio, eseguire il comando seguente nella radice del progetto:

```console
dotnet publish -c Release
```

Quando la precompilazione viene completata, si genera un file *<nome_progetto>.PrecompiledViews.dll* contenente le visualizzazioni Razor compilate. Ad esempio, lo screenshot seguente illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Impostare `MvcRazorCompileOnPublish` su `true`e includere un riferimento al pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

