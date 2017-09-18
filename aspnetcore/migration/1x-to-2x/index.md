---
title: Migrazione da ASP.NET Core 1.x a 2.0
author: scottaddie
description: "In questo articolo vengono illustrati i prerequisiti e i passaggi più comuni per la migrazione di un progetto di ASP.NET Core 1.x su ASP.NET 2.0 Core."
keywords: ASP.NET Core, eseguire la migrazione
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>Migrazione da ASP.NET Core 1.x a 2.0

Di [Scott Addie](https://github.com/scottaddie)

In questo articolo si procederà con l'aggiornamento di un progetto ASP.NET Core 1.x esistente ad ASP.NET 2.0 Core. La migrazione dell'applicazione ad ASP.NET 2.0 Core consente di sfruttare le [molte nuove caratteristiche e i miglioramenti delle prestazioni](https://go.microsoft.com/fwlink/?linkid=854094). 

Le applicazioni esistenti di ASP.NET Core 1.x si basano su modelli di progetto specifico della versione. Quando il framework di ASP.NET Core si evolve, i modelli del progetto fanno la stessa cosa, così come il codice di avvio in essi contenuti. Oltre ad aggiornare il framework di ASP.NET Core, è necessario aggiornare il codice dell'applicazione.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Prerequisiti
Vedere [Introduzione ad ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Aggiornare Moniker della versione di .NET Framework di destinazione (TFM, Target Framework Moniker)
I progetti destinati a .NET Core devono usare il [TFM](/dotnet/standard/frameworks#referring-to-frameworks) di una versione successiva o uguale a .NET Core 2.0. Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `netcoreapp2.0`:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]

I progetti destinati a .NET Framework devono usare il TFM di una versione successiva o uguale a .NET Framework 4.6.1. Cercare il nodo `<TargetFramework>` nel file *csproj*, e sostituire il testo interno con `net461`:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET core 2.0 offre una quantità di superficie maggiore rispetto a .NET Core 1.x. Se il destinatario è .NET Framework esclusivamente a causa della mancanza di API in .NET Core 1.x, avere .NET Core 2.0 come destinatario potrebbe funzionare.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Aggiornare la versione di .NET Core SDK in global.json
Se la soluzione si basa su un file [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) per avere come destinatario una specifica versione di .NET Core SDK, aggiornare la relativa proprietà `version` per usare la versione 2.0 installata nel computer:

[!code-json[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Aggiornare i riferimenti del pacchetto
Il file *csproj* in un progetto di 1.x elenca ogni pacchetto NuGet usato dal progetto.

In un progetto ASP.NET Core 2.0 destinato a .NET Core 2.0, un singolo riferimento del [metapacchetto](xref:fundamentals/metapackage) nel file *csproj* sostituisce la raccolta di pacchetti:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]

Tutte le funzionalità di ASP.NET Core 2.0 e di Entity Framework Core 2.0 sono incluse nel metapacchetto.

I progetti di ASP.NET Core 2.0 con destinazione .NET Framework devono fare riferimento a singoli pacchetti NuGet. Aggiornamento dell'attributo `Version` di ogni `<PackageReference />` nodo a 2.0.0.

Ad esempio, ecco l'elenco dei `<PackageReference />` nodi usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Framework:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Strumenti dell'interfaccia della riga di comando di .NET Core
Nel file *.csproj* aggiornare l'attributo `Version` di ogni nodo `<DotNetCliToolReference />` a 2.0.0.

Ad esempio, ecco l'elenco degli strumenti dell'interfaccia della riga di comando usati in un progetto ASP.NET Core 2.0 tipico destinato a .NET Core 2.0:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Rinominare la proprietà di Fallback di destinazione del pacchetto
Il file *csproj* di un progetto 1.x ha usato un nodo `PackageTargetFallback` e una variabile:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]

Rinominare il nodo e la variabile a `AssetTargetFallback`:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Aggiornare il metodo Main in Program.cs
Nei progetti di 1.x, il metodo `Main` di *Program.cs* era simile al seguente:

[!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

Nei progetti 2.0, il metodo `Main` di *Program.cs* è stato semplificato:

[!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]

L'adozione di questo nuovo modello 2.0 è altamente consigliabile ed è necessario affinché le funzionalità del prodotto come [Migrazioni di Entity Framework](xref:data/ef-mvc/migrations) funzionino. Ad esempio, l'esecuzione di `Update-Database` dalla finestra della Console di Gestione pacchetti o di `dotnet ef database update` dalla riga di comando (nei progetti convertiti in ASP.NET Core 2.0) genera l'errore seguente:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>Revisionare le impostazioni di compilazione della vista Razor
Tempi di avvio dell'applicazione più rapidi e aggregazioni pubblicate più piccole sono di importanza fondamentale per l'utente. Per questi motivi, [la compilazione della vista Razor](xref:mvc/views/view-compilation) è abilitata per impostazione predefinita in ASP.NET Core 2.0.

L'impostazione della proprietà `MvcRazorCompileOnPublish` su true non è più necessaria. A meno che non si stia disattivando la compilazione della vista, la proprietà può essere rimossa dal file *csproj*.

Quando la destinazione è .NET Framework, è necessario fare comunque riferimento in modo esplicito al pacchetto NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) nel file *csproj*:

[!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Basarsi sulle funzionalità di Application Insights "Light-Up"
Un semplice programma di installazione di strumentazione delle prestazioni dell'applicazione è importante. È ora possibile basarsi sulle nuove funzionalità "light-up" di [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponibili negli strumenti di Visual Studio 2017.

Per impostazione predefinita, i progetti ASP.NET Core 1.1 creati in Visual Studio 2017 hanno aggiunto Application Insights. Se non si usa Application Insights SDK direttamente, di fuori di *Program.cs* e *Startup.cs*, seguire questa procedura:

1. Rimuovere il nodo `<PackageReference />` seguente dal file *.csproj*:
    
    [!code-xml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]

2. Rimuovere la chiamata del metodo di estensione `UseApplicationInsights` da *Program.cs*:

    [!code-csharp[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Rimuovere la chiamata di API sul lato client di Application Insights da *_Layout.cshtml*. Comprende le due righe di codice seguenti:

    [!code-cshtml[Principale](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]

Se si usa Application Insights SDK direttamente, continuare a farlo. Il [metapacchetto](xref:fundamentals/metapackage) 2.0 comprende la versione più recente di Application Insights, pertanto viene visualizzato un errore di downgrade del pacchetto se si fa riferimento a una versione precedente.

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>Adottare i miglioramenti di Autenticazione/Identità
ASP.NET Core 2.0 ha un nuovo modello di autenticazione e un numero di modifiche significative per ASP.NET Identity Core. Se il progetto è stato creato con Account utente abilitati o se è stata aggiunta manualmente l'autenticazione o l'identità, vedere [Migrazione di autenticazione e identità ad ASP.NET 2.0 Core](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Risorse aggiuntive
- [Modifiche importanti apportate in ASP.NET Core 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
