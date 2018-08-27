---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "42909957"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)] include MSBuild SDK di `Microsoft.NET.Sdk.Razor` (Razor SDK). Il Razor SDK:

* Consente di standardizzare l'esperienza in termini di compilazione, creazione dei pacchetti e pubblicazione dei progetti contenenti file [Razor](xref:mvc/views/razor) per i progetti ASP.NET Core basati su MVC.
* Include un set di destinazioni, proprietà e di elementi predefiniti che consentono di personalizzare la compilazione dei file Razor.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Uso di Razor SDK

La maggior parte delle app Web non deve fare espressamente riferimento a Razor SDK. 

Per usare il Razor SDK per compilare librerie di classi contenenti visualizzazioni Razor o Razor Pages:

* Usare `Microsoft.NET.Sdk.Razor` anziché `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* In genere serve un riferimento al pacchetto a `Microsoft.AspNetCore.Mvc` per importare le dipendenze aggiuntive necessarie per generare e compilare visualizzazioni Razor o Razor Pages. Come minimo, è necessario che il progetto aggiunga riferimenti al pacchetto a:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 I pacchetti precedenti sono inclusi in `Microsoft.AspNetCore.Mvc`. Il markup seguente illustra un file *csproj* elementare che utilizza il Razor SDK per compilare i file Razor per un'app Razor Pages di ASP.NET Core:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Proprietà

Le proprietà seguenti controllano il comportamento del Razor SDK come parte della compilazione del progetto:

* `RazorCompileOnBuild`: Quando è `true`, compila e crea l'assembly Razor come parte della compilazione del progetto. Il valore predefinito è `true`.
* `RazorCompileOnPublish`: Quando è `true`, compila e crea l'assembly Razor come parte della pubblicazione del progetto. Il valore predefinito è `true`.

Le proprietà e gli elementi seguenti vengono utilizzati per configurare gli input e l'output del Razor SDK:

| Elementi                                         | Descrizione                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Elementi della voce (file *cshtml*) che sono input per le destinazioni di generazione del codice. |
| RazorCompile                                  | Elementi della voce (file cs) che sono input per le destinazioni di compilazione Razor. Usare questo ItemGroup per specificare ulteriori file da compilare nell'assembly Razor. |
| RazorTargetAssemblyAttribute                  | Elementi della voce usati per attributi di generazione del codice per l'assembly Razor. Ad esempio:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Elementi della voce aggiunti come risorse incorporate all'assembly Razor generato |

| Proprietà                                      | Descrizione                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nome di file (senza estensione) dell'assembly generato da Razor. | 
| RazorOutputPath                               | Directory di output di Razor.                                      |
| RazorCompileToolset                           | Usato per determinare il set di strumenti utilizzato per compilare l'assembly Razor. I valori validi sono `Implicit` e `PrecompilationTool`. |
| EnableDefaultContentItems                     | Quando è `true`, include alcuni tipi di file, ad esempio file *cshtml*, come contenuto nel progetto. Quando il riferimento è fatto tramite Microsoft.NET.Sdk.Web, include anche tutti i file in *wwwroot* e i file di configurazione.         |
| EnableDefaultRazorGenerateItems               | Quando è `true`, include i file *cshtml* degli elementi di `Content` negli elementi di `RazorGenerate`. |
| GenerateRazorTargetAssemblyInfo               | Quando è `true`, genera un file *cs* contenente attributi specificati da `RazorAssemblyAttribute` e li include nell'output compilato. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Quando è `true`, aggiunge un set predefinito di attributi degli assembly a `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Quando è `true`, copia i file (*cshtml*) degli elementi RazorGenerate nella directory di pubblicazione. In genere i file Razor non sono necessari per un'applicazione pubblicata se sono stati inclusi nella compilazione in fase di compilazione o di pubblicazione. Il valore predefinito è `false`. |
| CopyRefAssembliesToPublishDirectory            | Quando è `true`, copiare gli elementi dell'assembly di riferimento nella directory di pubblicazione. In genere gli assembly di riferimento non sono necessari per un'applicazione pubblicata se la compilazione di Razor avviene in fase di compilazione o di pubblicazione. Impostare su `true` se l'applicazione pubblicata richiede la compilazione runtime, ad esempio, modifica i file cshtml in fase di esecuzione oppure utilizza viste incorporate. Il valore predefinito è `false`. |
| IncludeRazorContentInPack                      | Quando è `true`, tutti gli elementi del contenuto Razor (file *cshtml*) verranno contrassegnati per l'inclusione nel pacchetto NuGet generato. Il valore predefinito è `false`. |
| EmbedRazorGenerateSources | Quando è `true`, aggiunge gli elementi (*cshtml*) di RazorGenerate come file incorporati nell'assembly Razor generato. Il valore predefinito è `false`. |
| UseRazorBuildServer                           | Quando è `true`, usa un processo del server di compilazione permanente per ripartire il lavoro di generazione del codice. Valore predefinito è il valore di `UseSharedCompilation`. |

### <a name="targets"></a>Destinazioni
Il Razor SDK definisce due obiettivi principali:

* `RazorGenerate`: il codice genera i file *cs* dagli elementi della voce RazorGenerate. Usa la proprietà `RazorGenerateDependsOn` per specificare le altre destinazioni che possono essere eseguite prima o dopo questa destinazione.
* `RazorCompile`: compila i file *cs* generati in un assembly Razor. Usare `RazorCompileDependsOn` per specificare le altre destinazioni che possono eseguite prima o dopo questa destinazione.

### <a name="runtime-compilation-of-razor-views"></a>Compilazione runtime di visualizzazioni Razor

* Per impostazione predefinita, Razor SDK non pubblica gli assembly di riferimento necessari per eseguire la compilazione runtime. Vengono pertanto generati errori di compilazione nel caso in cui il modello di applicazione si basi su una compilazione runtime&mdash;ad esempio, l'app usa visualizzazioni incorporate o modifica le visualizzazioni dopo aver pubblicato l'app. Impostare `CopyRefAssembliesToPublishDirectory` su `true` per continuare con la pubblicazione degli assembly di riferimento.

* Per le applicazioni Web, verificare che l'app sia destinata all'SDK `Microsoft.NET.Sdk.Web`.
