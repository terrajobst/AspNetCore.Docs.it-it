---
title: Compilazione e precompilazione delle visualizzazioni Razor
author: rick-anderson
description: Documento di riferimento che descrive come abilitare la compilazione e la precompilazione Razor MVC in applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilazione e precompilazione delle visualizzazioni Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le visualizzazioni Razor vengono compilate in fase di esecuzione quando la viene richiamata la visualizzazione. ASP.NET Core 1.1.0 e versioni successive possono facoltativamente compilare e distribuire le visualizzazioni Razor usando l'app &mdash;. Questo processo è noto come precompilazione. Per impostazione predefinita, la precompilazione è abilitata nei modelli di progetto ASP.NET Core 2.x.

> [!IMPORTANT]
> La precompilazione delle visualizzazioni Razor non è attualmente disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0. La funzionalità sarà disponibile per questo tipo di distribuzione a partire dalla versione 2.1. Per altre informazioni, vedere [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Errore di compilazione delle visualizzazioni durante la cross-compilazione per Linux in Windows.

Considerazioni sulla precompilazione:

* La precompilazione delle visualizzazioni consente di ridurre le dimensioni del bundle pubblicato e velocizzare il tempo di avvio.
* Non è possibile modificare i file Razor dopo aver precompliato le visualizzazioni. Le visualizzazioni modificate non saranno contenute nel bundle pubblicato. 

Per distribuire le visualizzazioni precompilate:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se .NET Framework è la destinazione del progetto, includere un riferimento al pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

Per impostazione predefinita, i modelli di progetto ASP.NET Core 2.x impostano in modo implicito `MvcRazorCompileOnPublish` su `true`. In questo modo il nodo può essere rimosso in modo sicuro dal file *.csproj*. Se si preferisce la modalità esplicita, è possibile impostare la proprietà `MvcRazorCompileOnPublish` su `true`. Nell'esempio *.csproj* seguente viene evidenziata questa impostazione:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Impostare `MvcRazorCompileOnPublish` su `true`e includere un riferimento al pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) eseguendo un comando simile al seguente alla radice del progetto:

```console
dotnet publish -c Release
```

Quando la precompilazione viene completata, si genera un file *<nome_progetto>.PrecompiledViews.dll* contenente le visualizzazioni Razor compilate. Ad esempio, lo screenshot seguente illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)
