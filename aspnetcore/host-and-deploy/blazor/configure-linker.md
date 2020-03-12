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
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Configurare il linker per ASP.NET Core Blazor

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazer webassembly esegue il collegamento [Intermediate Language (il)](/dotnet/standard/managed-code#intermediate-language--execution) durante una compilazione per eliminare il linguaggio intermedio non necessario dagli assembly di output dell'app. Il linker è disabilitato durante la compilazione nella configurazione di debug. Per abilitare il linker, le app devono essere compilate in configurazione versione. Quando si distribuiscono le app webassembly blazer, si consiglia di eseguire la compilazione in versione. 

Il collegamento di un'app ottimizza le dimensioni, ma può avere effetti negativi. Le app che usano la reflection o le funzionalità dinamiche correlate potrebbero interrompersi quando vengono tagliate perché il linker non è a conoscenza di questo comportamento dinamico e non può determinare in generale quali tipi sono necessari per la reflection in fase di esecuzione. Per tagliare tali app, il linker deve essere informato sui tipi necessari per la reflection nel codice e nei pacchetti o Framework da cui dipende l'app. 

Per assicurarsi che l'app tagliata funzioni correttamente una volta distribuita, è importante testare le build di rilascio dell'app spesso durante lo sviluppo.

Il collegamento per le app blazer può essere configurato usando le funzionalità di MSBuild seguenti:

* Configurare il collegamento a livello globale con una [proprietà di MSBuild](#control-linking-with-an-msbuild-property).
* Controllare il collegamento per ogni singolo assembly con un [file di configurazione](#control-linking-with-a-configuration-file).

## <a name="control-linking-with-an-msbuild-property"></a>Controllo del collegamento con una proprietà MSBuild

Il collegamento viene abilitato quando un'app viene compilata in `Release` configuation. Per modificare questa configurazione, configurare la proprietà `BlazorWebAssemblyEnableLinking` MSBuild nel file di progetto:

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
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

Per impostazione predefinita, la configurazione del linker di blazer per le app webassembly di Blazer rimuove le informazioni di internazionalizzazione ad eccezione delle impostazioni locali richieste in modo esplicito. La rimozione di questi assembly riduce al minimo le dimensioni dell'app.

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
| `none` (impostazione predefinita) | None                    |
| `other`          | *I18n. Other. dll*        |
| `rare`           | *I18n. Rare. dll*         |
| `west`           | *I18n. Dll occidentale*         |

Usare una virgola per separare più valori, ad esempio `mideast,west`.

Per altre informazioni, vedere [i18n: Pnetlib internationalation Framework Library (repository GitHub mono/mono)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
