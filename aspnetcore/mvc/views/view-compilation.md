---
title: Compilazione e precompilazione dei file Razor in ASP.NET Core
author: rick-anderson
description: Informazioni sui vantaggi della precompilazione dei file Razor e su come eseguire la precompilazione dei file Razor in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248186"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilazione del file Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. I file Razor vengono compilati sia in fase di compilazione che in fase di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk).

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
> Lo strumento di precompilazione verrà rimosso in ASP.NET Core 3.0. È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).
>
> Razor SDK è attivo solo se nel file di progetto non sono impostate proprietà specifiche di precompilazione. Se ad esempio si imposta la proprietà `MvcRazorCompileOnPublish` del file con estensione *csproj* su `true`, Razor SDK viene disabilitato.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Se il progetto è destinato a .NET Framework, installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

I modelli di progetto ASP.NET Core 2.x impostano in modo implicito la proprietà `MvcRazorCompileOnPublish` su `true` per impostazione predefinita. Di conseguenza, questo elemento può essere rimosso senza problemi dal file con estensione *csproj*.

> [!IMPORTANT]
> Lo strumento di precompilazione verrà rimosso in ASP.NET Core 3.0. È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).
>
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

## <a name="recompile-razor-files-on-change"></a>Ricompilare i file Razor in caso di modifica

La proprietà <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> di <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> ottiene o imposta un valore che determina se i file Razor (visualizzazioni Razor e Razor Pages) vengono ricompilati e aggiornati in caso di modifica dei file nel disco.

Se impostato su `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) controlla le modifiche apportate ai file Razor nelle istanze di <xref:Microsoft.Extensions.FileProviders.IFileProvider> configurate.

Il valore predefinito è `true` per:

* App ASP.NET Core 2.1 o versioni precedenti.
* App ASP.NET Core 2.2 o versioni precedenti nell'ambiente di sviluppo.

La proprietà <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> è associata a un'opzione di compatibilità e può presentare un comportamento diverso a seconda della versione di compatibilità configurata per l'app. La configurazione dell'app tramite l'impostazione di <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> ha priorità rispetto al valore implicito della versione di compatibilità dell'app.

Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o versioni precedenti, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> è impostata su `true` a meno che il valore non sia configurato in modo esplicito.

Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o versioni successive, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> è impostata su `false` a meno che l'ambiente non sia quello di sviluppo o il valore non sia configurato in modo esplicito.

Per indicazioni ed esempi di impostazione della versione di compatibilità dell'app, vedere <xref:mvc/compatibility-version>.

## <a name="additional-resources"></a>Risorse aggiuntive

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
