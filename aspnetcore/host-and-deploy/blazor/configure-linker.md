---
title: Configurare il linker per ASP.NET Core Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: cf017ec6d6de3c5848b866b0c29781f283c5de44
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168004"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="f1e30-103">Configurare il linker per ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="f1e30-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="f1e30-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1e30-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f1e30-105">Blazor esegue il collegamento del [linguaggio intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante ogni compilazione in modalità versione per rimuovere il livello intermedio non necessario dagli assembly di output dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1e30-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a Release build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="f1e30-106">Controllare il collegamento degli assembly adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1e30-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="f1e30-107">Disabilitare il collegamento a livello globale con una [proprietà di MSBuild](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="f1e30-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="f1e30-108">Controllare il collegamento per ogni singolo assembly con un [file di configurazione](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="f1e30-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="f1e30-109">Disabilitare il collegamento con una proprietà di MSBuild</span><span class="sxs-lookup"><span data-stu-id="f1e30-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="f1e30-110">Il collegamento è abilitato per impostazione predefinita in modalità versione quando viene compilata un'app, operazione che include la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f1e30-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="f1e30-111">Per disabilitare il collegamento per tutti gli assembly, impostare la proprietà di MSBuild `BlazorLinkOnBuild` su `false` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="f1e30-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="f1e30-112">Controllare il collegamento con un file di configurazione</span><span class="sxs-lookup"><span data-stu-id="f1e30-112">Control linking with a configuration file</span></span>

<span data-ttu-id="f1e30-113">Controllare il collegamento per ogni singolo assembly usando un file di configurazione XML e specificando il file come un elemento MSBuild nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="f1e30-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="f1e30-114">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="f1e30-114">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/aspnet/Blazor/issues/239
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

<span data-ttu-id="f1e30-115">Per altre informazioni, vedere [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Linker IL: sintassi del descrittore xml).</span><span class="sxs-lookup"><span data-stu-id="f1e30-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
