---
title: Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup
author: guardrex
description: Informazioni su come migliorare un'app ASP.NET Core da un assembly esterno con un'implementazione IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 2eddfa03b28564fcca7cc098e353b05e23b7c6f6
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336258"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Migliorare un'app da un assembly esterno in ASP.NET Core con IHostingStartup

Di [Luke Latham](https://github.com/guardrex)

Un'implementazione [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (avvio dell'hosting) consente l'aggiunta di miglioramenti a un'app all'avvio da un assembly esterno. Una libreria esterna può ad esempio usare un'implementazione di avvio dell'hosting per offrire servizi o provider di configurazione aggiuntivi a un'app. `IHostingStartup`  *è disponibile in ASP.NET Core 2.0 o versioni successive.*

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Attributo HostingStartup

Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) indica la presenza di un assembly di avvio dell'hosting da attivare in fase di runtime.

L'assembly di ingresso o l'assembly contenente la classe `Startup` viene analizzato automaticamente per la ricerca dell'attributo `HostingStartup`. L'elenco di assembly in cui cercare gli attributi `HostingStartup` viene caricato in fase di runtime dalla configurazione in [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey). L'elenco di assembly da escludere dalla ricerca viene caricato da [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey). Per altre informazioni, vedere [Host Web: Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies) e [Host Web: Assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

Nell'esempio seguente lo spazio dei nomi dell'assembly di avvio dell'hosting è `StartupEnhancement`. La classe che contiene il codice di avvio dell'hosting è `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

L'attributo `HostingStartup` si trova in genere nel file della classe di implementazione `IHostingStartup` dell'assembly di avvio dell'hosting.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Individuare gli assembly di avvio dell'hosting caricati

Per individuare gli assembly di avvio dell'hosting caricati, abilitare la registrazione e controllare i log dell'app. Gli errori che si verificano durante il caricamento degli assembly vengono registrati. Gli assembly di avvio dell'hosting caricati vengono registrati a livello di debug e tutti gli errori vengono registrati.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Disabilitare il caricamento automatico di assembly di avvio dell'hosting

::: moniker range=">= aspnetcore-2.1"

Per disabilitare il caricamento automatico degli assembly di avvio dell'hosting, usare uno degli approcci seguenti:

* Per evitare il caricamento di tutti gli assembly di avvio dell'hosting, impostare uno degli elementi seguenti su `true` o `1`:
  * Impostazione di configurazione host per [impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).
  * La variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.
* Per impedire il caricamento di specifici assembly di avvio dell'hosting, impostare uno degli elementi seguenti su una stringa delimitata da punti e virgola di assembly di avvio dell'hosting da escludere all'avvio:
  * Impostazione di configurazione host per [assembly di avvio dell'hosting da escludere](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).
  * La variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Per disabilitare il caricamento automatico degli assembly di avvio dell'hosting, impostare uno degli elementi seguenti su `true` o `1`:

* Impostazione di configurazione host per [impedire l'avvio dell'hosting](xref:fundamentals/host/web-host#prevent-hosting-startup).
* La variabile di ambiente `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

::: moniker-end

Se l'impostazione di configurazione host e la variabile di ambiente sono entrambe impostate, l'impostazione host controlla il comportamento.

La disabilitazione degli assembly di avvio dell'hosting tramite l'impostazione host o la variabile di ambiente ne determina la disabilitazione globale e la possibile disabilitazione di diverse caratteristiche di un'app.

## <a name="project"></a>Progetto

Creare l'avvio dell'hosting con uno dei tipi di progetto seguenti:

* [Libreria di classi](#class-library)
* [App console senza un punto di ingresso](#console-app-without-an-entry-point)

### <a name="class-library"></a>Libreria di classi

È possibile includere un miglioramento di avvio dell'hosting in una libreria di classi. La libreria contiene un attributo `HostingStartup`.

Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include un'app Razor Pages, *HostingStartupApp*, e una libreria di classi, *HostingStartupLibrary*. La libreria di classi:

* Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`. `ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app tramite il provider di configurazione in memoria ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).
* Include un attributo `HostingStartup` che identifica lo spazio dei nomi e la classe di avvio dell'hosting.

Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe `ServiceKeyInjection` usa [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app. Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting della libreria di classi:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) include anche un progetto di pacchetto NuGet che offre un avvio dell'hosting separato, *HostingStartupPackage*. Il pacchetto ha le stesse caratteristiche della libreria di classi descritta in precedenza. Il pacchetto:

* Contiene una classe di avvio dell'hosting, `ServiceKeyInjection`, che implementa `IHostingStartup`. `ServiceKeyInjection` aggiunge una coppia di stringhe di servizio alla configurazione dell'app.
* Include un attributo `HostingStartup`.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

La pagina Indice dell'app legge ed esegue il rendering dei valori di configurazione per le due chiavi impostate dall'assembly di avvio dell'hosting del pacchetto:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>App console senza un punto di ingresso

*Questo approccio è disponibile solo per le app .NET Core, non .NET Framework.*

Un miglioramento di avvio dell'hosting dinamico che non richiede un riferimento in fase di compilazione per l'attivazione può essere fornito in un'app console senza un punto di ingresso. L'app contiene un attributo `HostingStartup`. Per creare un avvio dell'hosting dinamico:

1. Viene creata una libreria di implementazione dalla classe che contiene l'implementazione `IHostingStartup`. La libreria di implementazione viene trattata come un normale pacchetto.
1. Un'app console senza un punto di ingresso fa riferimento al pacchetto della libreria di implementazione. Viene usata un'app console perché:
   * Poiché un file di dipendenze è un asset di app eseguibile, una libreria non può fornire un file di dipendenze.
   * Una libreria non può essere aggiunta direttamente all'[archivio dei pacchetti di runtime](/dotnet/core/deploying/runtime-store) che richiede un progetto eseguibile indirizzato al runtime condiviso.
1. L'app console viene pubblicata per ottenere le dipendenze dell'avvio dell'hosting. Una conseguenza della pubblicazione dell'app console è che le dipendenze non usate vengono eliminate dal file di dipendenze.
1. L'app e il relativo file di dipendenze vengono inseriti nell'archivio dei pacchetti di runtime. Per individuare l'assembly di avvio dell'hosting e il relativo file di dipendenze, viene fatto riferimento a essi in una coppia di variabili di ambiente.

L'app console fa riferimento al pacchetto [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

Un attributo [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) identifica una classe come un'implementazione di `IHostingStartup` per il caricamento e l'esecuzione al momento della compilazione di [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Nell'esempio seguente, lo spazio dei nomi è `StartupEnhancement` e la classe è `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Una classe implementa `IHostingStartup`. Il metodo [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) della classe usa un'interfaccia [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) per aggiungere miglioramenti a un'app. Nell'assembly di avvio dell'hosting `IHostingStartup.Configure` viene chiamato dal runtime prima di `Startup.Configure` nel codice utente, il che consente al codice utente di sovrascrivere qualsiasi configurazione fornita dall'assembly di avvio dell'hosting.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Quando si compila un progetto `IHostingStartup`, il file delle dipendenze (*\*.deps.json*) imposta il percorso di `runtime` dell'assembly sulla cartella *bin*:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Viene visualizzata solo una parte del file. Il nome dell'assembly nell'esempio è `StartupEnhancement`.

## <a name="specify-the-hosting-startup-assembly"></a>Specificare l'assembly di avvio dell'hosting

Per un avvio dell'hosting fornito da una libreria di classi o da un'app console, specificare il nome dell'assembly di avvio dell'hosting nella variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`. La variabile di ambiente è un elenco di assembly delimitato da punto e virgola.

L'attributo `HostingStartup` viene cercato solo negli assembly di avvio dell'hosting. Per l'app di esempio *HostingStartupApp*, per individuare gli avvii dell'hosting descritti in precedenza, la variabile di ambiente viene impostata sul valore seguente:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

È possibile impostare l'assembly di avvio dell'hosting anche tramite l'impostazione di configurazione host [Assembly di avvio dell'hosting](xref:fundamentals/host/web-host#hosting-startup-assemblies).

Quando sono presenti più assembly di avvio dell'hosting, i relativi metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) vengono eseguiti nell'ordine in cui sono elencati gli assembly.

## <a name="activation"></a>Attivazione

Le opzioni di attivazione dell'avvio dell'hosting sono:

* [Archivio di runtime](#runtime-store) &ndash; L'attivazione non richiede un riferimento in fase di compilazione. L'app di esempio inserisce l'assembly di avvio dell'hosting e i file di dipendenze in una cartella *deployment* per facilitare la distribuzione dell'avvio dell'hosting in un ambiente con più computer. La cartella *deployment* include anche uno script PowerShell che crea o modifica le variabili di ambiente nel sistema di distribuzione per abilitare l'avvio dell'hosting.
* Riferimento in fase di compilazione richiesto per l'attivazione
  * [Pacchetto NuGet](#nuget-package)
  * [Cartella bin del progetto](#project-bin-folder)

### <a name="runtime-store"></a>Archivio di runtime

L'implementazione dell'avvio dell'hosting viene inserita nell'[archivio di runtime](/dotnet/core/deploying/runtime-store). L'app migliorata non richiede un riferimento in fase di compilazione all'assembly.

Dopo la compilazione dell'avvio dell'hosting, il file di progetto dell'avvio dell'hosting svolge la funzione di file manifesto per il comando [dotnet store](/dotnet/core/tools/dotnet-store).

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

Questo comando inserisce l'assembly di avvio dell'hosting e altre dipendenze che non fanno parte del framework condiviso nell'archivio di runtime del profilo utente in:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Se si desidera inserire l'assembly e le dipendenze per l'utilizzo globale, aggiungere l'opzione `-o|--output` al comando `dotnet store` con il percorso seguente:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Modificare e inserire il file di dipendenze dell'avvio dell'hosting**

Il percorso di runtime è specificato nel file *\*.deps.json*. Per attivare il miglioramento, l'elemento `runtime` deve specificare il percorso dell'assembly di runtime del miglioramento. Anteporre al percorso di `runtime` il prefisso `lib/<TARGET_FRAMEWORK_MONIKER>/`:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Nel codice di esempio (progetto *StartupDiagnostics*) la modifica del file *\*.deps.json* viene eseguita da uno script [PowerShell](/powershell/scripting/powershell-scripting). Lo script di PowerShell viene automaticamente attivato da una destinazione di compilazione nel file di progetto.

Il file *\*.deps.json* dell'implementazione deve trovarsi in un percorso accessibile.

Per l'uso per utente, inserire il file nella cartella *additonalDeps* delle impostazioni `.dotnet` del profilo utente:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Per l'uso globale, inserire il file nella cartella *additonalDeps* dell'installazione di .NET Core:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

La versione del framework condiviso riflette quella del runtime condiviso usato dall'app di destinazione. Il runtime condiviso è indicato nel file *\*.runtimeconfig.json*. Nell'app di esempio (*HostingStartupApp*) il runtime condiviso è specificato nel file *HostingStartupApp.runtimeconfig.json*.

**Visualizzare il file di dipendenze dell'avvio dell'hosting**

Il percorso del file *\*.deps.json* dell'implementazione è indicato nella variabile di ambiente `DOTNET_ADDITIONAL_DEPS`.

Se il file viene inserito nella cartella *.dotnet* del profilo utente, impostare il valore della variabile di ambiente su:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Se il file è inserito nella cartella di installazione di .NET Core per l'uso globale, specificare il percorso completo al file:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Per consentire all'app di esempio (*HostingStartupApp*) di trovare il file di dipendenze (*HostingStartupApp.runtimeconfig.json*), il file di dipendenze viene inserito nel profilo utente.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` sul valore seguente:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Per esempi che illustrano come impostare le variabili di ambiente per diversi sistemi operativi, vedere [Usare più ambienti](xref:fundamentals/environments).

**Distribuzione**

Per facilitare la distribuzione di un avvio dell'hosting in un ambiente con più computer, l'app di esempio crea una cartella *deplyment* nell'output pubblicato che contiene:

* L'assembly di avvio dell'hosting.
* Il file di dipendenze dell'avvio dell'hosting.
* Uno script di PowerShell che crea o modifica `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` e `DOTNET_ADDITIONAL_DEPS` per supportare l'attivazione dell'avvio dell'hosting. Eseguire lo script da un prompt dei comandi di PowerShell amministrativo del sistema di distribuzione.

### <a name="nuget-package"></a>Pacchetto NuGet

È possibile includere un miglioramento di avvio dell'hosting in un pacchetto NuGet. Il pacchetto include un attributo `HostingStartup`. I tipi di avvio dell'hosting offerti dal pacchetto vengono resi disponibili all'app usando uno degli approcci seguenti:

* Il file di progetto dell'app migliorata crea un riferimento al pacchetto per l'avvio dell'hosting nel file di progetto dell'app (un riferimento in fase di compilazione). Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*). Questo approccio si applica a un pacchetto di assembly di avvio dell'hosting pubblicato in [nuget.org](https://www.nuget.org/).
* Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).

Per altre informazioni su pacchetti NuGet e l'archivio di runtime, vedere gli argomenti seguenti:

* [Come creare un pacchetto NuGet con strumenti multipiattaforma](/dotnet/core/deploying/creating-nuget-packages)
* [Pubblicazione di pacchetti](/nuget/create-packages/publish-a-package)
* [Archivio pacchetti di runtime](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Cartella bin del progetto

È possibile inserire un avvio dell'hosting tramite un assembly distribuito tramite *bin* nell'app migliorata. I tipi di avvio dell'hosting offerti dall'assembly vengono resi disponibili all'app usando uno degli approcci seguenti:

* Il file di progetto dell'app migliorata crea un riferimento all'assembly nell'avvio dell'hosting startup (riferimento in fase di compilazione). Dopo aver inserito il riferimento in fase di compilazione, l'assembly di avvio dell'hosting e tutte le relative dipendenze vengono incorporate nel file delle dipendenze dell'app (*\*.deps.json*). Questo approccio si applica quando lo scenario di distribuzione richiede lo spostamento dell'assembly della libreria di avvio dell'hosting compilata (file DLL) nel progetto utilizzato o in un percorso accessibile dal progetto utilizzato e viene creato un riferimento in fase di compilazione all'assembly dell'avvio dell'hosting.
* Il file di dipendenze dell'avvio dell'hosting viene reso disponibile all'app migliorata come descritto nella sezione [Archivio di runtime](#runtime-store) (senza un riferimento in fase di compilazione).

## <a name="sample-code"></a>Codice di esempio

Il [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([come eseguire il download](xref:tutorials/index#how-to-download-a-sample)) illustra gli scenari di implementazione dell'avvio dell'hosting:

* Due assembly di avvio dell'hosting (librerie di classi) impostano ognuno una coppia chiave-valore di configurazione in memoria:
  * Pacchetto NuGet (*HostingStartupPackage*)
  * Libreria di classi (*HostingStartupLibrary*)
* L'avvio dell'hosting viene attivato da un assembly distribuito tramite archivio di runtime (*StartupDiagnostics*). All'avvio l'assembly aggiunge all'app due middleware che offrono informazioni di diagnostica su:
  * Servizi registrati
  * Indirizzo (schema, host, base del percorso, percorso, stringa di query)
  * Connessione (IP remoto, porta remota, IP locale, porta locale, certificato client)
  * Intestazioni della richiesta
  * Variabili di ambiente

Per eseguire l'esempio:

**Attivazione da un pacchetto NuGet**

1. Compilare il pacchetto *HostingStartupPackage* con il comando [dotnet pack](/dotnet/core/tools/dotnet-pack).
1. Aggiungere il nome dell'assembly del pacchetto *HostingStartupPackage* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. Compilare ed eseguire l'app. L'app migliorata include un riferimento al pacchetto (riferimento in fase di compilazione). `<PropertyGroup>` nel file di progetto dell'app specifica l'output del progetto del pacchetto (*../HostingStartupPackage/bin/Debug*) come origine del pacchetto. Ciò consente all'app di usare il pacchetto senza caricarlo in [nuget.org](https://www.nuget.org/). Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` del pacchetto.

Se si apportano modifiche al progetto *HostingStartupPackage* e lo si ricompila, cancellare le cache del pacchetto NuGet locale per assicurarsi che *HostingStartupApp* riceva il pacchetto aggiornato e non un pacchetto non aggiornato proveniente dalla cache locale. Per cancellare le cache NuGet locali, eseguire il comando [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) seguente:

```console
dotnet nuget locals all --clear
```

**Attivazione da una libreria di classi**

1. Compilare la libreria di classi *HostingStartupLibrary* con il comando [dotnet build](/dotnet/core/tools/dotnet-build).
1. Aggiungere il nome dell'assembly della libreria di classi di *HostingStartupLibrary* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
1. Distribuire tramite *bin* l'assembly della libreria di classi all'app copiando il file *HostingStartupLibrary.dll* dall'output compilato della libreria di classi alla cartella *bin/Debug* dell'app.
1. Compilare ed eseguire l'app. `<ItemGroup>` nel file di progetto dell'app fa riferimento all'assembly della libreria di classi (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (riferimento in fase di compilazione). Per altre informazioni, vedere le note nel file di progetto di HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Si noti che i valori di chiave di configurazione del servizio di cui è stato eseguito il rendering tramite la pagina Indice corrispondono ai valori impostati dal metodo `ServiceKeyInjection.Configure` della libreria di classi.

**Attivazione da un assembly distribuito tramite l'archivio di runtime**

1. Il progetto *StartupDiagnostics* usa [PowerShell](/powershell/scripting/powershell-scripting) per modificare il relativo file *StartupDiagnostics.deps.json*. PowerShell viene installato per impostazione predefinita in Windows a partire da Windows 7 SP1 e Windows Server 2008 R2 SP1. Per ottenere PowerShell su altre piattaforme, vedere [Installazione di Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).
1. Compilare il progetto *StartupDiagnostics*. Dopo aver compilato il progetto, una destinazione di compilazione nel file di progetto esegue automaticamente le operazioni seguenti:
   * Attiva lo script di PowerShell per modificare il file *StartupDiagnostics.deps.json*.
   * Sposta il file *StartupDiagnostics.deps.json* nella cartella *additionalDeps* del profilo utente.
1. Eseguire il comando `dotnet store` a un prompt dei comandi nella directory di avvio dell'hosting per archiviare l'assembly e le relative dipendenze nell'archivio di runtime del profilo utente:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Per Windows, il comando usa l'[identificatore di runtime (RID)](/dotnet/core/rid-catalog) `win7-x64`. Quando si specifica l'avvio dell'hosting per un runtime diverso, immettere il RID corretto.
1. Impostare le variabili di ambiente:
   * Aggiungere il nome dell'assembly di *StartupDiagnostics* alla variabile di ambiente `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.
   * In Windows impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`. In macOS/Linux impostare la variabile di ambiente `DOTNET_ADDITIONAL_DEPS` su `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, dove `<USER>` è il profilo utente che contiene l'avvio dell'hosting.
1. Eseguire l'app di esempio.
1. Richiedere l'endpoint `/services` per visualizzare i servizi registrati dell'app. Richiedere l'endpoint `/diag` per visualizzare le informazioni di diagnostica.
