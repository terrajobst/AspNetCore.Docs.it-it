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
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
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

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Se .NET Framework è la destinazione del progetto, includere un riferimento al pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

Per impostazione predefinita, i modelli di progetto ASP.NET Core 2.x impostano in modo implicito `MvcRazorCompileOnPublish` su `true`. In questo modo il nodo può essere rimosso in modo sicuro dal file *.csproj*. Se si preferisce la modalità esplicita, è possibile impostare la proprietà `MvcRazorCompileOnPublish` su `true`. Nell'esempio *.csproj* seguente viene evidenziata questa impostazione:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Impostare `MvcRazorCompileOnPublish` su `true`e includere un riferimento al pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish). Ad esempio, eseguire il comando seguente nella radice del progetto:

```console
dotnet publish -c Release
```

Quando la precompilazione viene completata, si genera un file *<nome_progetto>.PrecompiledViews.dll* contenente le visualizzazioni Razor compilate. Ad esempio, lo screenshot seguente illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)
