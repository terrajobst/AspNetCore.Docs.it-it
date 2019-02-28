---
title: Configurare il linker per Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: 7c53e7912ec3b0ae471ea38777f874f55a32487d
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647941"
---
# <a name="configure-the-linker-for-blazor"></a>Configurare il linker per Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor esegue il collegamento del [linguaggio intermedio (IL)](/dotnet/standard/managed-code#intermediate-language--execution) durante ogni compilazione in modalità versione per rimuovere IL non necessario dagli assembly di output.

È possibile controllare il collegamento degli assembly con uno degli approcci seguenti:

* Disabilitare il collegamento a livello globale con una proprietà di MSBuild.
* Controllare il collegamento per ogni singolo assembly con un file di configurazione.

## <a name="disable-linking-with-an-msbuild-property"></a>Disabilitare il collegamento con una proprietà di MSBuild

Il collegamento è abilitato per impostazione predefinita in modalità versione quando viene compilata un'app, operazione che include la pubblicazione. Per disabilitare il collegamento per tutti gli assembly, impostare la proprietà di MSBuild `<BlazorLinkOnBuild>` su `false` nel file di progetto:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Controllare il collegamento con un file di configurazione

Il collegamento può essere controllato per ogni singolo assembly usando un file di configurazione XML e specificando il file come un elemento MSBuild nel file di progetto.

Di seguito è riportato un esempio di file di configurazione (*Linker.xml*):

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

Per altre informazioni sul formato di file per il file di configurazione, vedere [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Linker IL: sintassi del descrittore xml).

Specificare il file di configurazione nel file di progetto con l'elemento `BlazorLinkerDescriptor`:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
