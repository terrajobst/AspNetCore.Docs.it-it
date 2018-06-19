---
title: Compilazione e precompilazione dei file Razor in ASP.NET Core
author: rick-anderson
description: Informazioni sui vantaggi della precompilazione dei file Razor e su come eseguire la precompilazione dei file Razor in un'app ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336278"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilazione del file Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
Un file Razor viene compilato in fase di runtime, quando viene richiamata la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app &mdash;usando lo strumento di precompilazione.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. I file Razor vengono compilati sia in fase di compilazione che in fase di pubblicazione tramite [Razor SDK](xref:mvc/razor-pages/sdk).
::: moniker-end

## <a name="precompilation-considerations"></a>Considerazioni sulla precompilazione

Ecco gli effetti collaterali della precompilazione dei file Razor:

* Dimensioni inferiori del bundle pubblicato
* Minore tempo di avvio
* Non è possibile modificare i file Razor&mdash;il contenuto associato è assente dal bundle pubblicato.

## <a name="deploy-precompiled-files"></a>Distribuire file precompilati

::: moniker range=">= aspnetcore-2.1"
La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK. La modifica dei file Razor dopo che sono stati aggiornati è supportata in fase di compilazione. Per impostazione predefinita, con l'app viene distribuito solo il file *Views.dll* compilato. Non viene distribuito alcun file con estensione *cshtml*.

> [!IMPORTANT]
> Razor SDK è attivo solo se nel file di progetto non sono impostate proprietà specifiche di precompilazione. Se ad esempio si imposta la proprietà `MvcRazorCompileOnPublish` del file con estensione *csproj* su `true`, Razor SDK viene disabilitato.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Se il progetto è destinato a .NET Framework, installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

I modelli di progetto ASP.NET Core 2.x impostano in modo implicito la proprietà `MvcRazorCompileOnPublish` su `true` per impostazione predefinita. Di conseguenza, questo elemento può essere rimosso senza problemi dal file con estensione *csproj*.

> [!IMPORTANT]
> La precompilazione dei file Razor non è disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Impostare la proprietà `MvcRazorCompileOnPublish` su `true`e installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish). Ad esempio, eseguire il comando seguente nella radice del progetto:

```console
dotnet publish -c Release
```

Al termine della compilazione, viene prodotto un file *<nome_progetto>.PrecompiledViews.dll* contenente i file Razor compilati. Lo screenshot seguente, ad esempio, illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end