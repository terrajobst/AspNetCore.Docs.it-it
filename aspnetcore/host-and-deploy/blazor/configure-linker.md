---
title: Configurare il linker per ASP.NET Core Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083296"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="18d11-103">Configurare il linker per ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="18d11-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="18d11-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="18d11-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="18d11-105">Blazer webassembly esegue il collegamento [Intermediate Language (il)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilazione per eliminare il linguaggio intermedio non necessario dagli assembly di output dell'app.</span><span class="sxs-lookup"><span data-stu-id="18d11-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="18d11-106">Il linker è disabilitato durante la compilazione nella configurazione di debug.</span><span class="sxs-lookup"><span data-stu-id="18d11-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="18d11-107">Per abilitare il linker, le app devono essere compilate in configurazione versione.</span><span class="sxs-lookup"><span data-stu-id="18d11-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="18d11-108">Quando si distribuiscono le app webassembly blazer, si consiglia di eseguire la compilazione in versione.</span><span class="sxs-lookup"><span data-stu-id="18d11-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="18d11-109">Il collegamento di un'app ottimizza le dimensioni, ma può avere effetti negativi.</span><span class="sxs-lookup"><span data-stu-id="18d11-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="18d11-110">Le app che usano la reflection o le funzionalità dinamiche correlate potrebbero interrompersi quando vengono tagliate perché il linker non è a conoscenza di questo comportamento dinamico e non può determinare in generale quali tipi sono necessari per la reflection in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="18d11-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="18d11-111">Per tagliare tali app, il linker deve essere informato sui tipi necessari per la reflection nel codice e nei pacchetti o Framework da cui dipende l'app.</span><span class="sxs-lookup"><span data-stu-id="18d11-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="18d11-112">Per assicurarsi che l'app tagliata funzioni correttamente una volta distribuita, è importante testare le build di rilascio dell'app spesso durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="18d11-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="18d11-113">Il collegamento per le app blazer può essere configurato usando le funzionalità di MSBuild seguenti:</span><span class="sxs-lookup"><span data-stu-id="18d11-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="18d11-114">Configurare il collegamento a livello globale con una [proprietà di MSBuild](#control-linking-with-an-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="18d11-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="18d11-115">Controllare il collegamento per ogni singolo assembly con un [file di configurazione](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="18d11-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="18d11-116">Controllo del collegamento con una proprietà MSBuild</span><span class="sxs-lookup"><span data-stu-id="18d11-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="18d11-117">Il collegamento viene abilitato quando un'app viene compilata in `Release` configuation.</span><span class="sxs-lookup"><span data-stu-id="18d11-117">Linking is enabled when an app is built in `Release` configuation.</span></span> <span data-ttu-id="18d11-118">Per modificare questa configurazione, configurare la proprietà `BlazorWebAssemblyEnableLinking` MSBuild nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="18d11-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="18d11-119">Controllare il collegamento con un file di configurazione</span><span class="sxs-lookup"><span data-stu-id="18d11-119">Control linking with a configuration file</span></span>

<span data-ttu-id="18d11-120">Controllare il collegamento per ogni singolo assembly usando un file di configurazione XML e specificando il file come un elemento MSBuild nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="18d11-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="18d11-121">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="18d11-121">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="18d11-122">Per altre informazioni, vedere [il linker: sintassi del descrittore XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="18d11-122">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="18d11-123">Configurare il linker per l'internazionalizzazione</span><span class="sxs-lookup"><span data-stu-id="18d11-123">Configure the linker for internationalization</span></span>

<span data-ttu-id="18d11-124">Per impostazione predefinita, la configurazione del linker di blazer per le app webassembly di Blazer rimuove le informazioni di internazionalizzazione ad eccezione delle impostazioni locali richieste in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="18d11-124">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="18d11-125">La rimozione di questi assembly riduce al minimo le dimensioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="18d11-125">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="18d11-126">Per controllare quali assembly I18N vengono conservati, impostare la proprietà `<MonoLinkerI18NAssemblies>` MSBuild nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="18d11-126">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="18d11-127">Valore Region</span><span class="sxs-lookup"><span data-stu-id="18d11-127">Region Value</span></span>     | <span data-ttu-id="18d11-128">Assembly dell'area mono</span><span class="sxs-lookup"><span data-stu-id="18d11-128">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="18d11-129">Tutti gli assembly inclusi</span><span class="sxs-lookup"><span data-stu-id="18d11-129">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="18d11-130">*I18n. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="18d11-130">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="18d11-131">*I18n. . Dll medio*</span><span class="sxs-lookup"><span data-stu-id="18d11-131">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="18d11-132">`none` (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="18d11-132">`none` (default)</span></span> | <span data-ttu-id="18d11-133">None</span><span class="sxs-lookup"><span data-stu-id="18d11-133">None</span></span>                    |
| `other`          | <span data-ttu-id="18d11-134">*I18n. Other. dll*</span><span class="sxs-lookup"><span data-stu-id="18d11-134">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="18d11-135">*I18n. Rare. dll*</span><span class="sxs-lookup"><span data-stu-id="18d11-135">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="18d11-136">*I18n. Dll occidentale*</span><span class="sxs-lookup"><span data-stu-id="18d11-136">*I18N.West.dll*</span></span>         |

<span data-ttu-id="18d11-137">Usare una virgola per separare più valori, ad esempio `mideast,west`.</span><span class="sxs-lookup"><span data-stu-id="18d11-137">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="18d11-138">Per altre informazioni, vedere [i18n: Pnetlib internationalation Framework Library (repository GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="18d11-138">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
