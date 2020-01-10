---
title: Configurare il linker per ASP.NET Core Blazor
author: guardrex
description: Informazioni su come controllare il linker del linguaggio intermedio (IL) quando si compila un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: cdf506f0c0fa720df64e59342d352ef41271d24b
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2020
ms.locfileid: "75866046"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a>Configurare il linker per ASP.NET Core Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor esegue il collegamento [Intermediate Language (il)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilazione per rimuovere il linguaggio intermedio non necessario dagli assembly di output dell'app.

Controllare il collegamento degli assembly adottando uno degli approcci seguenti:

* Disabilitare il collegamento a livello globale con una [proprietà di MSBuild](#disable-linking-with-a-msbuild-property).
* Controllare il collegamento per ogni singolo assembly con un [file di configurazione](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Disabilitare il collegamento con una proprietà di MSBuild

Il collegamento è abilitato per impostazione predefinita quando viene compilata un'app, che include la pubblicazione. Per disabilitare il collegamento per tutti gli assembly, impostare la proprietà di MSBuild `BlazorLinkOnBuild` su `false` nel file di progetto:

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

Per altre informazioni, vedere [il linker: sintassi del descrittore XML](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).

### <a name="configure-the-linker-for-internationalization"></a>Configurare il linker per l'internazionalizzazione

Per impostazione predefinita, la configurazione del linker Blazorper le app Blazor webassembly rimuove le informazioni di internazionalizzazione ad eccezione delle impostazioni locali richieste in modo esplicito. La rimozione di questi assembly riduce al minimo le dimensioni dell'app.

Per controllare quali assembly I18N vengono conservati, impostare la proprietà `<MonoLinkerI18NAssemblies>` MSBuild nel file di progetto:

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| Valore Region     | Assembly dell'area mono    |
| ---------------- | ----------------------- |
| `all`            | Tutti gli assembly inclusi |
| `cjk`            | *I18n. CJK. dll*          |
| `mideast`        | *I18n. . Dll medio*      |
| `none` (impostazione predefinita) | nessuna                    |
| `other`          | *I18n. Other. dll*        |
| `rare`           | *I18n. Rare. dll*         |
| `west`           | *I18n. Dll occidentale*         |

Usare una virgola per separare più valori, ad esempio `mideast,west`.

Per altre informazioni, vedere [i18n: Pnetlib internazionalizzazione Framework libreria (repository GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
