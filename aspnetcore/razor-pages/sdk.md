---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 872d90662494735dc0e4caa01c46fcdcc2606bc6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660069"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Panoramica

Il [!INCLUDE[](~/includes/2.1-SDK.md)] include `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Il Razor SDK:

::: moniker range=">= aspnetcore-3.0"

* È necessario per compilare, creare un pacchetto e pubblicare progetti contenenti file [Razor](xref:mvc/views/razor) per ASP.NET Core progetti basati su MVC o [Blazer](xref:blazor/index) .
* Include un set di destinazioni, proprietà ed elementi predefiniti che consentono di personalizzare la compilazione dei file Razor ( *. cshtml* o *. Razor*).

Razor SDK include `Content` elementi con `Include` attributi impostati sui modelli di `**\*.cshtml` e `**\*.razor` glob. I file corrispondenti vengono pubblicati.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Consente di standardizzare l'esperienza in termini di compilazione, creazione dei pacchetti e pubblicazione dei progetti contenenti file [Razor](xref:mvc/views/razor) per i progetti ASP.NET Core basati su MVC.
* Include un set di destinazioni, proprietà e di elementi predefiniti che consentono di personalizzare la compilazione dei file Razor.

Razor SDK include un elemento di `Content` con un attributo `Include` impostato sul modello `**\*.cshtml` glob. I file corrispondenti vengono pubblicati.

::: moniker-end

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Usare Razor SDK

La maggior parte delle App web non è necessario fare riferimento esplicitamente al SDK di Razor.

::: moniker range=">= aspnetcore-3.0"

Per usare Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages, è consigliabile iniziare con il modello di progetto libreria di classi Razor (RCL). Un RCL usato per compilare file blazer (*Razor*) richiede almeno un riferimento al pacchetto [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . Un RCL usato per compilare visualizzazioni o pagine Razor (file con*estensione cshtml* ) richiede almeno la destinazione `netcoreapp3.0` o versione successiva e ha una `FrameworkReference` al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) nel file di progetto.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Per usare il Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages:

* Usare `Microsoft.NET.Sdk.Razor` anziché `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* In genere, è necessario un riferimento al pacchetto `Microsoft.AspNetCore.Mvc` per ricevere dipendenze aggiuntive necessarie per compilare e compilare Razor Pages e visualizzazioni Razor. Come minimo, il progetto deve aggiungere i riferimenti ai pacchetti per:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  Il pacchetto di `Microsoft.AspNetCore.Razor.Design` fornisce le attività di compilazione Razor e le destinazioni per il progetto.

  I pacchetti precedenti sono inclusi in `Microsoft.AspNetCore.Mvc`. Il markup seguente illustra un file di progetto che usa il SDK di Razor per generare i file Razor per un'app ASP.NET Core Razor Pages:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> I pacchetti `Microsoft.AspNetCore.Razor.Design` e `Microsoft.AspNetCore.Mvc.Razor.Extensions` sono inclusi nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Tuttavia, la versione meno `Microsoft.AspNetCore.App` riferimento al pacchetto fornisce un metapacchetto all'app che non include la versione più recente di `Microsoft.AspNetCore.Razor.Design`. I progetti devono fare riferimento a una versione coerente di `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`), in modo che siano incluse le correzioni più recenti della fase di compilazione per Razor. Per altre informazioni, vedere [questo problema in GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Proprietà

Le proprietà seguenti controllano il comportamento del Razor SDK come parte della compilazione del progetto:

* `RazorCompileOnBuild` &ndash; quando `true`, compila ed emette l'assembly Razor come parte della compilazione del progetto. L'impostazione predefinita è `true`.
* `RazorCompileOnPublish` &ndash; quando `true`, compila ed emette l'assembly Razor come parte della pubblicazione del progetto. L'impostazione predefinita è `true`.

Le proprietà e gli elementi nella tabella seguente vengono utilizzati per configurare input e output per il SDK di Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> A partire da ASP.NET Core 3,0, le visualizzazioni MVC o Razor Pages non vengono gestite per impostazione predefinita se le proprietà `RazorCompileOnBuild` o `RazorCompileOnPublish` MSBuild nel file di progetto sono disabilitate. Le applicazioni devono aggiungere un riferimento esplicito al pacchetto [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) se l'app si basa sulla compilazione di runtime per elaborare i file con *estensione cshtml* .

::: moniker-end

| Items | Descrizione |
| ----- | ----------- |
| `RazorGenerate` | Elementi Item (file con*estensione cshtml* ) che sono input per la generazione di codice. |
| `RazorComponent` | Elementi elemento (file*Razor* ) che sono input per la generazione di codice del componente Razor. |
| `RazorCompile` | Elementi Item (file con*estensione cs* ) che sono input per le destinazioni di compilazione Razor. Usare questa `ItemGroup` per specificare i file aggiuntivi da compilare nell'assembly Razor. |
| `RazorTargetAssemblyAttribute` | Elementi della voce usati per attributi di generazione del codice per l'assembly Razor. Ad esempio:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementi aggiunti come risorse incorporate nell'assembly generato Razor. |

| Proprietà | Descrizione |
| -------- | ----------- |
| `RazorTargetName` | Nome di file (senza estensione) dell'assembly generato da Razor. |
| `RazorOutputPath` | Directory di output di Razor. |
| `RazorCompileToolset` | Usato per determinare il set di strumenti utilizzato per compilare l'assembly Razor. I valori validi sono `Implicit`, `RazorSDK` e `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Il valore predefinito è `true`. Quando `true`, include i file *Web. config*, *. JSON*e *. cshtml* come contenuto nel progetto. Quando si fa riferimento tramite `Microsoft.NET.Sdk.Web`, vengono inclusi anche i file in *wwwroot* e nei file di configurazione. |
| `EnableDefaultRazorGenerateItems` | Quando è `true`, include i file *cshtml* degli elementi di `Content` negli elementi di `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Quando `true`, genera un file con *estensione cs* contenente gli attributi specificati da `RazorAssemblyAttribute` e include il file nell'output di compilazione. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Quando è `true`, aggiunge un set predefinito di attributi degli assembly a `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Quando `true`, `RazorGenerate` copia i file di elementi (con*estensione cshtml*) nella directory di pubblicazione. In genere, i file Razor non sono necessari per un'app pubblicata se partecipano alla compilazione in fase di compilazione o in fase di pubblicazione. L'impostazione predefinita è `false`. |
| `CopyRefAssembliesToPublishDirectory` | Quando è `true`, copiare gli elementi dell'assembly di riferimento nella directory di pubblicazione. In genere, gli assembly di riferimento non sono necessari per un'app pubblicata se compilazione Razor si verifica in fase di compilazione o in fase di pubblicazione. Impostare su `true` se l'app pubblicata richiede la compilazione del runtime. Ad esempio, impostare il valore su `true` se l'app modifica i file *cshtml* in fase di esecuzione o usa le visualizzazioni incorporate. L'impostazione predefinita è `false`. |
| `IncludeRazorContentInPack` | Quando `true`, tutti gli elementi di contenuto Razor (file con*estensione cshtml* ) vengono contrassegnati per l'inclusione nel pacchetto NuGet generato. L'impostazione predefinita è `false`. |
| `EmbedRazorGenerateSources` | Quando è `true`, aggiunge gli elementi (*cshtml*) di RazorGenerate come file incorporati nell'assembly Razor generato. L'impostazione predefinita è `false`. |
| `UseRazorBuildServer` | Quando è `true`, usa un processo del server di compilazione permanente per ripartire il lavoro di generazione del codice. Valore predefinito è il valore di `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Quando `true`, l'SDK genera attributi aggiuntivi usati da MVC in fase di esecuzione per eseguire l'individuazione delle parti dell'applicazione. |
| `DefaultWebContentItemExcludes` | Modello glob per gli elementi elemento che devono essere esclusi dal gruppo di elementi `Content` nei progetti destinati a Web o Razor SDK |
| `ExcludeConfigFilesFromBuildOutput` | Quando `true`, i file con *estensione config* e *JSON* non vengono copiati nella directory dell'output di compilazione. |
| `AddRazorSupportForMvc` | Quando `true`, configura Razor SDK per aggiungere il supporto per la configurazione MVC richiesta quando si compilano applicazioni contenenti visualizzazioni MVC o Razor Pages. Questa proprietà viene impostata in modo implicito per i progetti .NET Core 3,0 o versioni successive destinate a Web SDK |
| `RazorLangVersion` | Versione della lingua Razor di destinazione. |

Per altre informazioni sulle proprietà, vedere [Proprietà di MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Server di destinazione

Il Razor SDK definisce due obiettivi principali:

* `RazorGenerate` codice &ndash; genera file con *estensione cs* da elementi `RazorGenerate` elemento. Utilizzare la proprietà `RazorGenerateDependsOn` per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.
* `RazorCompile` &ndash; compila i file con *estensione cs* generati in un assembly Razor. Usare il `RazorCompileDependsOn` per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.
* `RazorComponentGenerate` codice &ndash; genera file con *estensione cs* per gli elementi `RazorComponent` elemento. Utilizzare la proprietà `RazorComponentGenerateDependsOn` per specificare destinazioni aggiuntive che possono essere eseguite prima o dopo questa destinazione.

### <a name="runtime-compilation-of-razor-views"></a>Compilazione runtime di visualizzazioni Razor

* Per impostazione predefinita, Razor SDK non pubblica gli assembly di riferimento necessari per eseguire la compilazione runtime. Vengono pertanto generati errori di compilazione nel caso in cui il modello di applicazione si basi su una compilazione runtime&mdash;ad esempio, l'app usa visualizzazioni incorporate o modifica le visualizzazioni dopo aver pubblicato l'app. Impostare `CopyRefAssembliesToPublishDirectory` su `true` per continuare con la pubblicazione degli assembly di riferimento.

* Per un'app Web, verificare che l'app sia destinata a `Microsoft.NET.Sdk.Web` SDK.

## <a name="razor-language-version"></a>Versione della lingua Razor

Quando la destinazione è `Microsoft.NET.Sdk.Web` SDK, la versione della lingua Razor viene dedotta dalla versione del Framework di destinazione dell'app. Per i progetti destinati a `Microsoft.NET.Sdk.Razor` SDK o nel raro caso in cui l'app richieda una versione diversa della lingua Razor rispetto al valore derivato, è possibile configurare una versione impostando la proprietà `<RazorLangVersion>` nel file di progetto dell'app:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

La versione del linguaggio Razor è strettamente integrata con la versione del runtime per cui è stata creata. La destinazione di una versione di linguaggio non progettata per il runtime non è supportata e probabilmente genera errori di compilazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Aggiunte al formato csproj per .NET Core](/dotnet/core/tools/csproj)
* [Elementi di progetto MSBuild comuni](/visualstudio/msbuild/common-msbuild-project-items)
