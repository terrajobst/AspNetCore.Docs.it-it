---
title: Aggiungere le funzionalità di app con una configurazione specifica della piattaforma in ASP.NET Core
author: guardrex
description: Informazioni su come aggiungere funzionalità a un'applicazione ASP.NET di base da un assembly esterno usando un'implementazione IHostingStartup.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 9dd7774a1885a9c6c702b5b46fa1f88c86f7f7ac
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="add-app-features-with-a-platform-specific-configuration-in-aspnet-core"></a>Aggiungere le funzionalità di app con una configurazione specifica della piattaforma in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementazione consente l'aggiunta di funzionalità a un'app all'avvio da un assembly esterno all'esterno dell'app `Startup` classe. Ad esempio, è possibile utilizzare una libreria di strumenti esterni un `IHostingStartup` implementazione per fornire servizi a un'app o provider di configurazione aggiuntive. `IHostingStartup` *è disponibile in ASP.NET Core 2.0 e versioni successive.*

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Individuare l'assembly di avvio hosting caricati

Per individuare gli assembly di avvio di hosting caricato dall'applicazione o dalle librerie, abilitare la registrazione e controllare i registri di applicazione. Vengono registrati gli errori che si verificano durante il caricamento di assembly. Caricare gli assembly di avvio di hosting sono connessi a livello di Debug e tutti gli errori vengono registrati.

Le operazioni di lettura di app di esempio di [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in un `string` di matrice e viene visualizzato il risultato nella pagina di indice dell'app:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Disabilitare il caricamento automatico di hosting di assembly di avvio

Esistono due modi per disabilitare il caricamento automatico di hosting di assembly di avvio:

* Impostare il [impedire l'avvio di Hosting](xref:fundamentals/hosting#prevent-hosting-startup) ospitare l'impostazione di configurazione.
* Impostare il `ASPNETCORE_preventHostingStartup` variabile di ambiente.

Quando l'impostazione dell'host o la variabile di ambiente è impostata su `true` o `1`, hosting gli assembly di avvio non vengono caricati automaticamente. Se entrambi sono impostati, l'host controlla il comportamento.

La disabilitazione di hosting gli assembly di avvio usando la variabile di ambiente o impostazione host disabilitati a livello globale e può disabilitare la funzionalità diverse di un'app. Non è attualmente possibile disabilitare selettivamente un assembly di avvio hosting aggiunto da una libreria a meno che la libreria offre un proprio opzione di configurazione. Una versione futura offrono la possibilità di disabilitare selettivamente l'hosting degli assembly di avvio (vedere [GitHub emettere aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Implementare le funzionalità di IHostingStartup

### <a name="create-the-assembly"></a>Creare l'assembly

Un `IHostingStartup` funzionalità viene distribuito come un assembly in base a un'applicazione console senza un punto di ingresso. I riferimenti agli assembly di [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) pacchetto:

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

Oggetto [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attributo identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione quando si compila il [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Nell'esempio seguente, lo spazio dei nomi è `StartupFeature`, e la classe è `StartupFeatureHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

Una classe implementa `IHostingStartup`. La classe [configura](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) metodo utilizza un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere funzionalità a un'app:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Quando si compila un `IHostingStartup` file di progetto, le dipendenze (*\*. deps.json*) imposta il `runtime` percorso dell'assembly per il *bin* cartella:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Viene visualizzata solo una parte del file. Nome dell'assembly nell'esempio `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Aggiornare il file di dipendenze

Il percorso di runtime è specificato nella  *\*. deps.json* file. Su attivo, la funzionalità di `runtime` elemento è necessario specificare il percorso dell'assembly di runtime della funzionalità. Prefisso di `runtime` posizione con `lib/netcoreapp2.0/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

Nell'applicazione di esempio, la modifica del  *\*. deps.json* file viene eseguita da un [PowerShell](/powershell/scripting/powershell-scripting) script. Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.

### <a name="feature-activation"></a>Attivazione della caratteristica

**Inserire il file di assembly**

Il `IHostingStartup` deve essere il file di assembly dell'implementazione *bin*-distribuito nell'app o inserito nel [archivio runtime](/dotnet/core/deploying/runtime-store):

Per l'utilizzo per singolo utente, inserire l'assembly nell'archivio di runtime del profilo utente in:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Per l'utilizzo globale, inserire l'assembly nell'archivio di runtime dell'installazione di .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Quando si distribuisce l'assembly all'archivio di runtime, il file di simboli può essere distribuito anche ma non è necessario usare la funzionalità.

**Inserire il file di dipendenze**

L'implementazione  *\*. deps.json* file deve essere in un percorso accessibile.

Per l'utilizzo per singolo utente, inserire il file nella `additonalDeps` cartella del profilo utente `.dotnet` impostazioni: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Per l'utilizzo globale, inserire il file nella `additonalDeps` cartella dell'installazione di .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Prendere nota della versione, `2.0.0`, corrisponde alla versione del runtime condiviso che usa l'app di destinazione. La fase di esecuzione condiviso viene visualizzato nel  *\*. runtimeconfig.json* file. Nell'applicazione di esempio, il runtime condiviso viene specificato nella *HostingStartupSample.runtimeconfig.json* file.

**Set di variabili di ambiente**

Impostare le variabili di ambiente seguenti nel contesto dell'applicazione che utilizza la funzionalità.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Vengono analizzati solo gli assembly di avvio di hosting per il `HostingStartupAttribute`. Il nome dell'assembly dell'implementazione viene fornito nella variabile di ambiente. Questo valore viene impostato l'app di esempio `StartupDiagnostics`.

Il valore può essere impostato anche tramite il [Hosting gli assembly di avvio](xref:fundamentals/hosting#hosting-startup-assemblies) ospitare l'impostazione di configurazione.

DOTNET\_AGGIUNTIVI\_DEPS

Il percorso dell'implementazione  *\*. deps.json* file.

Se il file si trova il profilo utente *.dotnet* cartella da utilizzare per ogni utente:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Se il file si trova nell'installazione di .NET Core per l'utilizzo generale, specificare il percorso completo del file:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

Questo valore viene impostato l'app di esempio:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Per esempi di come impostare le variabili di ambiente per diversi sistemi operativi, vedere [funziona con più ambienti](xref:fundamentals/environments).

## <a name="sample-app"></a>App di esempio

Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) utilizza `IHostingStartup` per creare uno strumento di diagnostica. Lo strumento aggiunge due middlewares all'app all'avvio che forniscono informazioni di diagnostica:

* Registrazione di servizi
* Indirizzo: schema, host, percorso di base, percorso, stringa di query
* Connessione: remoto IP, porta remota, IP locale, porta locale, il certificato client
* Le intestazioni di richiesta
* Variabili di ambiente

Per eseguire l'esempio:

1. Il progetto di avvio diagnostica utilizza [PowerShell](/powershell/scripting/powershell-scripting) per modificare il relativo *StartupDiagnostics.deps.json* file. PowerShell viene installato per impostazione predefinita in un sistema operativo Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1. Per ottenere PowerShell su altre piattaforme, vedere [installazione di Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compilare il progetto di avvio di diagnostica. Una destinazione di compilazione nel file di progetto:
   * Consente di spostare l'assembly e file di archivio di runtime del profilo utente di simboli.
   * Genera lo script di PowerShell per modificare il *StartupDiagnostics.deps.json* file.
   * Sposta il *StartupDiagnostics.deps.json* file del profilo utente `additionalDeps` cartella.
3. Impostare le variabili di ambiente:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Eseguire l'app di esempio.
5. Richiedere il `/services` endpoint per l'App registrato di servizi. Richiedere il `/diag` endpoint per visualizzare le informazioni di diagnostica.
