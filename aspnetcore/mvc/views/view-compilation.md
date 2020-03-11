---
title: Compilazione del file Razor in ASP.NET Core
author: rick-anderson
description: Informazioni sulla compilazione dei file Razor in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661105"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilazione del file Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. La pubblicazione dei file Razor in fase di compilazione non è supportata. I file Razor possono essere facoltativamente compilati in fase di pubblicazione e distribuiti con l'app&mdash;usando lo strumento di precompilazione.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Un file Razor viene compilato in fase di runtime, quando viene richiamata la pagina Razor o la visualizzazione MVC associata. I file Razor vengono compilati sia in fase di compilazione che in fase di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

I file Razor con estensione *cshtml* vengono compilati in fase di compilazione e di pubblicazione tramite [Razor SDK](xref:razor-pages/sdk). Facoltativamente, è possibile abilitare la compilazione in fase di runtime configurando l'applicazione.

::: moniker-end

## <a name="razor-compilation"></a>Compilazione Razor

::: moniker range=">= aspnetcore-3.0"

La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK. Quando abilitata, la compilazione in fase di runtime funge da complemento alla compilazione in fase di build, consentendo di aggiornare i file Razor in caso di modifica.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

La compilazione di file Razor in fase di build e pubblicazione è abilitata per impostazione predefinita da Razor SDK. La modifica dei file Razor dopo che sono stati aggiornati è supportata in fase di compilazione. Per impostazione predefinita, vengono distribuiti con l'app solo i file *Views.dll* compilati e non i file *cshtml* o gli assembly di riferimento necessari per compilare i file Razor con l'app.

> [!IMPORTANT]
> Lo strumento di precompilazione è stato deprecato e verrà rimosso in ASP.NET Core 3.0. È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).
>
> Razor SDK è attivo solo se nel file di progetto non sono impostate proprietà specifiche di precompilazione. Se ad esempio si imposta la proprietà *del file con estensione*csproj`MvcRazorCompileOnPublish` su `true`, Razor SDK viene disabilitato.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Se il progetto è destinato a .NET Framework, installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.

I modelli di progetto ASP.NET Core 2.x impostano in modo implicito la proprietà `MvcRazorCompileOnPublish` su `true` per impostazione predefinita. Di conseguenza, questo elemento può essere rimosso senza problemi dal file con estensione *csproj*.

> [!IMPORTANT]
> Lo strumento di precompilazione è stato deprecato e verrà rimosso in ASP.NET Core 3.0. È consigliabile eseguire la migrazione a [Razor SDK](xref:razor-pages/sdk).
>
> La precompilazione dei file Razor non è disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Impostare la proprietà `MvcRazorCompileOnPublish` su `true`e installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish). Ad esempio, eseguire il comando seguente nella radice del progetto:

```dotnetcli
dotnet publish -c Release
```

Al termine della compilazione, viene prodotto un file *\<nome_progetto>.PrecompiledViews.dll* contenente i file Razor compilati. Lo screenshot seguente, ad esempio, illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>Compilazione runtime

::: moniker range="= aspnetcore-2.1"

La compilazione in fase di build è integrata dalla compilazione in fase di runtime dei file Razor. ASP.NET Core MVC ricompilerà i file Razor quando il contenuto di un file *cshtml* cambia.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

La compilazione in fase di build è integrata dalla compilazione in fase di runtime dei file Razor. Il <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Ottiene o imposta un valore che determina se i file Razor (visualizzazioni Razor e Razor Pages) vengono ricompilati e aggiornati in caso di modifica dei file sul disco.

Il valore predefinito è `true` per:

* Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o versioni precedenti
* Se la versione di compatibilità dell'app è impostata su <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o versioni successive e l'app è nell'ambiente di sviluppo <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. In altre parole, i file Razor non vengono ricompilati in un ambiente non di sviluppo a meno che non sia stato impostato in modo esplicito <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>.

Per indicazioni ed esempi di impostazione della versione di compatibilità dell'app, vedere <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Per abilitare la compilazione in fase di esecuzione per tutti gli ambienti e le modalità di configurazione:

1. Installare il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).

1. Aggiornare il metodo `Startup.ConfigureServices` del progetto in modo da includere una chiamata a `AddRazorRuntimeCompilation`. Ad esempio:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>Abilitare in modo condizionale la compilazione del runtime

È possibile abilitare la compilazione di runtime in modo che sia disponibile solo per lo sviluppo locale. L'abilitazione condizionale in questo modo garantisce che l'output pubblicato:

* USA viste compilate.
* Dimensioni minori.
* Non Abilita i controlli file nell'ambiente di produzione.

Per abilitare la compilazione in fase di esecuzione in base all'ambiente e alla modalità di configurazione:

1. Fare riferimento in modo condizionale al pacchetto [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) in base al valore di `Configuration` attivo:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. Aggiornare il metodo `Startup.ConfigureServices` del progetto in modo da includere una chiamata a `AddRazorRuntimeCompilation`. Eseguire in modo condizionale `AddRazorRuntimeCompilation` in modo che venga eseguito in modalità di debug solo quando la variabile `ASPNETCORE_ENVIRONMENT` è impostata su `Development`:

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

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
