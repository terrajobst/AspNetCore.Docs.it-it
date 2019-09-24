---
title: Configurare il linker per ASP.NET Core Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: d3dd69e263e88ca1fc301eefc0da186a023aa96f
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211591"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Configurare il linker per ASP.NET Core Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor esegue il collegamento del [linguaggio intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante ogni compilazione in modalità versione per rimuovere il livello intermedio non necessario dagli assembly di output dell'app.

Controllare il collegamento degli assembly adottando uno degli approcci seguenti:

* Disabilitare il collegamento a livello globale con una [proprietà di MSBuild](#disable-linking-with-a-msbuild-property).
* Controllare il collegamento per ogni singolo assembly con un [file di configurazione](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Disabilitare il collegamento con una proprietà di MSBuild

Il collegamento è abilitato per impostazione predefinita in modalità versione quando viene compilata un'app, operazione che include la pubblicazione. Per disabilitare il collegamento per tutti gli assembly, impostare la proprietà di MSBuild `BlazorLinkOnBuild` su `false` nel file di progetto:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Controllare il collegamento con un file di configurazione

Controllare il collegamento per ogni singolo assembly usando un file di configurazione XML e specificando il file come un elemento MSBuild nel file di progetto:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.xml*:

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

Per altre informazioni, vedere [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Linker IL: sintassi del descrittore xml).
