---
title: Configurare il linker per Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668064"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="d08ef-103">Configurare il linker per Blazor</span><span class="sxs-lookup"><span data-stu-id="d08ef-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="d08ef-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d08ef-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="d08ef-105">Blazor esegue il collegamento del [linguaggio intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante ogni compilazione in modalità versione per rimuovere IL non necessario dagli assembly di output.</span><span class="sxs-lookup"><span data-stu-id="d08ef-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="d08ef-106">È possibile controllare il collegamento degli assembly con uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="d08ef-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="d08ef-107">Disabilitare il collegamento a livello globale con una proprietà di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="d08ef-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="d08ef-108">Controllare il collegamento per ogni singolo assembly con un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d08ef-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="d08ef-109">Disabilitare il collegamento con una proprietà di MSBuild</span><span class="sxs-lookup"><span data-stu-id="d08ef-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="d08ef-110">Il collegamento è abilitato per impostazione predefinita in modalità versione quando viene compilata un'app, operazione che include la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d08ef-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="d08ef-111">Per disabilitare il collegamento per tutti gli assembly, impostare la proprietà di MSBuild `<BlazorLinkOnBuild>` su `false` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="d08ef-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="d08ef-112">Controllare il collegamento con un file di configurazione</span><span class="sxs-lookup"><span data-stu-id="d08ef-112">Control linking with a configuration file</span></span>

<span data-ttu-id="d08ef-113">Il collegamento può essere controllato per ogni singolo assembly usando un file di configurazione XML e specificando il file come un elemento MSBuild nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="d08ef-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="d08ef-114">Di seguito è riportato un esempio di file di configurazione (*Linker.xml*):</span><span class="sxs-lookup"><span data-stu-id="d08ef-114">The following is an example configuration file (*Linker.xml*):</span></span>

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

<span data-ttu-id="d08ef-115">Per altre informazioni sul formato di file per il file di configurazione, vedere [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor) (Linker IL: sintassi del descrittore xml).</span><span class="sxs-lookup"><span data-stu-id="d08ef-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="d08ef-116">Specificare il file di configurazione nel file di progetto con l'elemento `BlazorLinkerDescriptor`:</span><span class="sxs-lookup"><span data-stu-id="d08ef-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
