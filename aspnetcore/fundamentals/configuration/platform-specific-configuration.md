---
title: Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup
author: guardrex
description: Informazioni su come migliorare un'app ASP.NET Core da un assembly esterno con un'implementazione IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278083"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup

Di [Luke Latham](https://github.com/guardrex)

Un' implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app. Una libreria di strumenti esterna può ad esempio usare un'implementazione `IHostingStartup` per offrire servizi o provider di configurazione aggiuntivi a un'app. `IHostingStartup` *è disponibile in ASP.NET Core 2.0 e versioni successive.*

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Individuare gli assembly di avvio dell'hosting caricati

Per individuare gli assembly di avvio dell'hosting caricati dall'app o dalle librerie, abilitare la registrazione e controllare i registri applicazioni. Gli errori che si verificano durante il caricamento degli assembly vengono registrati. Gli assembly di avvio dell'hosting caricati vengono registrati a livello di debug e tutti gli errori vengono registrati.

L'app di esempio legge [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in una matrice `string` e visualizza il risultato nella pagina di indice dell'app:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Disabilitare il caricamento automatico di assembly di avvio dell'hosting

Per disabilitare il caricamento automatico di assembly di avvio dell'hosting sono disponibili due modi:

* Usare l'impostazione di configurazione host per [Impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Impostare la variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Quando l'impostazione host o la variabile di ambiente è impostata su `true` o `1`, gli assembly di avvio dell'hosting non vengono caricati automaticamente. Se entrambe sono impostate, il comportamento viene controllato dall'impostazione host.

La disabilitazione degli assembly di avvio dell'hosting tramite l'impostazione host o la variabile di ambiente ne determina la disabilitazione globale e la possibile disabilitazione di diverse caratteristiche di un'app. Attualmente non è possibile disabilitare in modo selettivo un assembly di avvio dell'hosting aggiunto da una libreria, a meno che questa non offra una propria opzione di configurazione. Una versione futura offrirà la possibilità di disabilitare in modo selettivo gli assembly di avvio dell'hosting (vedere il [problema aspnet/Hosting n. 1243 in GitHub](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementare IHostingStartup

### <a name="create-the-assembly"></a>Creare l'assembly

Viene distribuito un miglioramento `IHostingStartup` come assembly basato su un'applicazione console senza un punto di ingresso. L'assembly fa riferimento al pacchetto [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione al momento della compilazione di [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Nell'esempio seguente, lo spazio dei nomi è `StartupEnhancement` e la classe è `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Una classe implementa `IHostingStartup`. Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe usa un'interfaccia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app. Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Quando si compila un progetto `IHostingStartup`, il file delle dipendenze (*\*.deps.json*) imposta il percorso di `runtime` dell'assembly sulla cartella *bin*:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Viene visualizzata solo una parte del file. Il nome dell'assembly nell'esempio è `StartupEnhancement`.

### <a name="update-the-dependencies-file"></a>Aggiornare il file delle dipendenze

Il percorso di runtime è specificato nel file *\*.deps.json*. Per attivare il miglioramento, l'elemento `runtime` deve specificare il percorso dell'assembly di runtime del miglioramento. Anteporre al percorso di `runtime` il prefisso `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Nell'app di esempio la modifica del file *\*.deps.json* viene eseguita da uno script di [PowerShell](/powershell/scripting/powershell-scripting). Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.

### <a name="enhancement-activation"></a>Attivazione del miglioramento

**Inserire il file di assembly**

Il file di assembly dell'implementazione `IHostingStartup` deve essere distribuito alla cartella *bin* nell'app o inserito nell'[archivio di runtime](/dotnet/core/deploying/runtime-store):

Per l'uso per utente, inserire l'assembly nell'archivio di runtime del profilo utente in:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Per l'uso globale, inserire l'assembly nell'archivio di runtime dell'installazione di .NET Core:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Quando si distribuisce l'assembly all'archivio di runtime è possibile che venga distribuito anche il file di simboli che tuttavia non è necessario per il funzionamento del miglioramento.

**Inserire il file delle dipendenze**

Il file *\*.deps.json* dell'implementazione deve trovarsi in un percorso accessibile.

Per l'uso per utente, inserire il file nella cartella `additonalDeps` delle impostazioni `.dotnet` del profilo utente:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Per l'uso globale, inserire il file nella cartella `additonalDeps` dell'installazione di .NET Core:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

La versione del framework condiviso riflette quella del runtime condiviso usato dall'app di destinazione. Il runtime condiviso è indicato nel file *\*.runtimeconfig.json*. Nell'app di esempio il runtime condiviso è specificato nel file *HostingStartupSample.runtimeconfig.json*.

**Impostare le variabili di ambiente**

Impostare le variabili di ambiente seguenti nel contesto dell'app che usa il miglioramento.

ASPNETCORE_HOSTINGSTARTUPASSEMBLIES

`HostingStartupAttribute` viene cercato solo negli assembly di avvio dell'hosting. Il nome di assembly dell'implementazione viene specificato in questa variabile di ambiente. L'app di esempio imposta questo valore su `StartupDiagnostics`.

È possibile impostare il valore anche tramite l'impostazione di configurazione host [Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies).

Quando sono presenti più assembly di avvio dell'hosting, i relativi metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) vengono eseguiti nell'ordine in cui sono elencati gli assembly.

DOTNET_ADDITIONAL_DEPS

Il percorso del file *\*.deps.json* dell'implementazione.

Se il file è inserito nella cartella *.dotnet* del profilo utente per l'uso per utente:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Se il file è inserito nella cartella di installazione di .NET Core per l'uso globale, specificare il percorso completo al file:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

L'app di esempio imposta questo valore su:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Per esempi che illustrano come impostare le variabili di ambiente per diversi sistemi operativi, vedere [Usare più ambienti](xref:fundamentals/environments).

## <a name="sample-app"></a>App di esempio

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)) usa `IHostingStartup` per creare uno strumento di diagnostica. All'avvio dell'app, lo strumento aggiunge due middleware che offrono informazioni di diagnostica:

* Servizi registrati
* Indirizzo: schema, host, base del percorso, percorso, stringa di query
* Connessione: IP remoto, porta remota, IP locale, porta locale, certificato client
* Intestazioni della richiesta
* Variabili di ambiente

Per eseguire l'esempio:

1. Il progetto Startup Diagnostic usa [PowerShell](/powershell/scripting/powershell-scripting) per modificare il file *StartupDiagnostics.deps.json*. PowerShell viene installato per impostazione predefinita nel sistema operativo Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1. Per ottenere PowerShell su altre piattaforme, vedere [Installazione di Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Compilare il progetto Startup Diagnostic. Una destinazione di compilazione nel file di progetto:
   * Sposta l'assembly e i file di simboli nell'archivio di runtime del profilo utente.
   * Attiva lo script di PowerShell per modificare il file *StartupDiagnostics.deps.json*.
   * Sposta il file *StartupDiagnostics.deps.json* nella cartella `additionalDeps` del profilo utente.
3. Impostare le variabili di ambiente:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Eseguire l'app di esempio.
5. Richiedere l'endpoint `/services` per visualizzare i servizi registrati dell'app. Richiedere l'endpoint `/diag` per visualizzare le informazioni di diagnostica.
