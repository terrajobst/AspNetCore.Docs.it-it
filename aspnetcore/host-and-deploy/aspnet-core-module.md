---
title: Guida di riferimento per la configurazione del modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ad73d89ffa3a8a3625c6e248efaad821e1b4d0a
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121557"
---
# <a name="aspnet-core-module-configuration-reference"></a>Guida di riferimento per la configurazione del modulo ASP.NET Core

Di [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Justin Kotalik](https://github.com/jkotalik)

Questo documento fornisce istruzioni su come configurare il modulo ASP.NET Core per l'hosting di app ASP.NET Core. Per un'introduzione al modulo ASP.NET Core e istruzioni per l'installazione, vedere la [panoramica del modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a>Modello di hosting

Per le app che vengono eseguite in .NET Core 2.2 o versione successiva, il modulo supporta un modello di hosting in-process, che garantisce prestazioni migliori rispetto al tipo di hosting a proxy inverso (out-of-process). Per ulteriori informazioni, vedere <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.

L'hosting in-process acconsente esplicitamente le app esistenti, mentre i modelli [dotnet new](/dotnet/core/tools/dotnet-new) usano per impostazione predefinita il modello di hosting in-process per tutti gli scenari IIS e IIS Express.

Per configurare un'app per l'hosting In-Process, aggiungere la proprietà `<AspNetCoreHostingModel>` al file di progetto dell'app (ad esempio, *MyApp.csproj*), usando il valore `InProcess` (l'hosting out-of-process viene impostato con `outofprocess`):

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

In caso di hosting in-process, vengono applicate le caratteristiche seguenti:

* Un server HTTP di IIS (`IISHttpServer`) viene utilizzato al posto del server [Kestrel](xref:fundamentals/servers/kestrel). Il server HTTP di IIS (`IISHttpServer`) è un'altra implementazione <xref:Microsoft.AspNetCore.Hosting.Server.IServer> che converte le richieste native di IIS in richieste gestite di ASP.NET Core da sottoporre all'app per l'elaborazione.

* L'[attributo requestTimeout ](#attributes-of-the-aspnetcore-element) non viene applicato all'hosting in-process.

* Non è supportata la condivisione di un pool di app tra le app. Usare un pool di app per ogni app.

* Quando si usa la [Distribuzione Web ](/iis/publish/using-web-deploy/introduction-to-web-deploy) o si inserisce manualmente un [file app_offline.htm nella distribuzione](xref:host-and-deploy/iis/index#locked-deployment-files), l'app potrebbe non arrestarsi immediatamente se una connessione è aperta. Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.

* L'architettura, vale a dire il numero di bit dell'app, e il runtime installato (x64 o x86) devono corrispondere all'architettura del pool di app.

* Se si configura l'host dell'app manualmente con `WebHostBuilder` (non con [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host)) e l'app viene sempre eseguita direttamente nel server di Kestrel (self-hosted), chiamare `UseKestrel` prima di chiamare `UseIISIntegration`. Se l'ordine viene invertito, l'host non verrà avviato.

* Le disconnessioni del client vengono rilevate. Il token di annullamento [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) viene cancellato quando il client si disconnette.

* <xref:System.IO.Directory.GetCurrentDirectory*> restituisce la directory di lavoro del processo avviato da IIS invece che la directory dell'app (ad esempio, *C:\Windows\System32\inetsrv* per *w3wp.exe*).

  Per vedere il codice di esempio che imposta la directory corrente dell'app, vedere la [classe CurrentDirectoryHelpers](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs). Chiamare il metodo `SetCurrentDirectory`. Le chiamate successive a <xref:System.IO.Directory.GetCurrentDirectory*> specificano la directory dell'app.

### <a name="hosting-model-changes"></a>Modifiche del modello di hosting

Se l'impostazione `hostingModel` viene modificata nel file *web.config* (descritto nella sezione [Configurazione con web.config](#configuration-with-webconfig)), il modulo ricicla il processo di lavoro per IIS.

Per IIS Express, il modulo non ricicla il processo di lavoro. Attiva invece un arresto normale del processo di IIS Express corrente. La richiesta successiva all'app attiva un nuovo processo di IIS Express.

### <a name="process-name"></a>Nome processo

`Process.GetCurrentProcess().ProcessName` dichiara `w3wp`/`iisexpress` (In-Process) o `dotnet` (out-of-process).

::: moniker-end

## <a name="configuration-with-webconfig"></a>Configurazione con web.config

Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.

Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet" 
                  arguments=".\MyApp.dll" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe" 
                  stdoutLogEnabled="false" 
                  stdoutLogFile=".\logs\stdout" 
                  hostingModel="InProcess" />
    </system.webServer>
  </location>
</configuration>
```

La proprietà <xref:System.Configuration.SectionInformation.InheritInChildApplications*> è impostata su `false` per indicare che le impostazioni specificate nell'elemento [\<percorso>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) non sono ereditate da app che risiedono in una sottodirectory dell'app.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`. Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.

Per informazioni sulla configurazione delle applicazioni secondarie IIS, vedere <xref:host-and-deploy/iis/index#sub-applications>.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributi dell'elemento aspNetCore

::: moniker range=">= aspnetcore-2.2"

| Attributo | Description | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato in **processPath**.</p> | |
| `disableStartUpErrorPage` | <p>Attributo booleano facoltativo.</p><p>Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attributo booleano facoltativo.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</p> | `true` |
| `hostingModel` | <p>Attributo stringa facoltativo.</p><p>Specifica il modello di hosting in-process (`InProcess`) o out-of-process (`OutOfProcess`).</p> | `OutOfProcess` |
| `processesPerApplication` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</p><p>&dagger;Per l'hosting in-process, il valore è limitato a `1`.</p> | Valore predefinito: `1`<br>Min: `1`<br>Max: `100`&dagger; |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**. Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</p><p>Non supportato con l'hosting in-process.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Attributo Timespan facoltativo.</p><p>Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</p><p>Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</p><p>Non è applicabile all'hosting in-process. Per l'hosting in-process, il modulo resta in attesa che l'app elabori la richiesta.</p> | Valore predefinito: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile. Se questo limite di tempo viene superato, il modulo termina il processo. Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</p><p>Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</p> | Valore predefinito: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti. Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log. Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**. Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Attributo | Description | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato in **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Attributo booleano facoltativo.</p><p>Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attributo booleano facoltativo.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</p> | `true` |
| `processesPerApplication` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</p> | Valore predefinito: `1`<br>Min: `1`<br>Max: `100` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**. Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Attributo Timespan facoltativo.</p><p>Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</p><p>Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</p> | Valore predefinito: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile. Se questo limite di tempo viene superato, il modulo termina il processo. Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</p><p>Un valore pari a 0 (zero) **non** è considerato un timeout infinito.</p> | Valore predefinito: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti. Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log. Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**. Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| Attributo | Description | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato in **processPath**.</p>| |
| `disableStartUpErrorPage` | <p>Attributo booleano facoltativo.</p><p>Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Attributo booleano facoltativo.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</p> | `true` |
| `processesPerApplication` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di istanze del processo specificato nell'impostazione **processPath** che può essere riattivato per ogni app.</p> | Valore predefinito: `1`<br>Min: `1`<br>Max: `100` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**. Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `100` |
| `requestTimeout` | <p>Attributo Timespan facoltativo.</p><p>Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</p><p>Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.0 o versioni precedenti, `requestTimeout` deve essere specificato solo in minuti interi. In caso contrario, verrà usato il valore predefinito di 2 minuti.</p> | Valore predefinito: `00:02:00`<br>Min: `00:00:00`<br>Max: `360:00:00` |
| `shutdownTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</p> | Valore predefinito: `10`<br>Min: `0`<br>Max: `600` |
| `startupTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile. Se questo limite di tempo viene superato, il modulo termina il processo. Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</p> | Valore predefinito: `120`<br>Min: `0`<br>Max: `3600` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti. Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log. Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**. Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Impostazione delle variabili di ambiente

È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`. Specificare una variabile di ambiente con l'elemento figlio `environmentVariable` di un elemento della raccolta `environmentVariables`. Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.

Nell'esempio seguente vengono impostate due variabili di ambiente. `ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`. Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app. `CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso. Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.

Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*. Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.

::: moniker range=">= aspnetcore-2.2"

Quando si usa il modello di hosting out-of-process, l'app potrebbe non arrestarsi immediatamente se una connessione è aperta. Ad esempio, una connessione WebSocket può ritardare l'arresto dell'app.

::: moniker-end

## <a name="start-up-error-page"></a>Pagina di errore di avvio

::: moniker range=">= aspnetcore-2.2"

Sia l'hosting In-Process che quello out-of-process producono pagine di errore personalizzate quando non riescono ad avviare l'app.

Se il modulo ASP.NET Core non riesce a trovare il gestore richieste In-Process o out-of-process, viene visualizzata una tabella codici di stato *500.0 - Errore di caricamento del gestore In-Process/out-of-process*.

Per l'hosting In-Process, se il modulo ASP.NET Core non riesce ad avviare l'app, viene visualizzata una tabella codici di stato *500.30 - Errore di avvio*.

Per l'hosting out-of-process, se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*.

Per non visualizzare questa pagina e tornare alla tabella codici di stato 5xx predefinita di IIS, usare l'attributo `disableStartUpErrorPage`. Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 - Errore del processo*. Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`. Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

::: moniker-end

## <a name="log-creation-and-redirection"></a>Creazione e reindirizzamento dei log

Il modulo ASP.NET Core reindirizza su disco l'output della console stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`. Le eventuali cartelle nel percorso `stdoutLogFile` devono essere già esistenti affinché il modulo possa creare il file di log. Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).

I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo. È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.

L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app. Non usare il log stdout per scopi di registrazione generale delle app. Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log. Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file (*.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori. Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.

::: moniker range=">= aspnetcore-2.2"

Se `stdoutLogEnabled` è false, gli errori che si verificano all'avvio dell'app vengono acquisiti ed emessi nel log eventi fino a 30 KB. Dopo l'avvio, tutti i log aggiuntivi vengono rimossi.

::: moniker-end

L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure. Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale. Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="enhanced-diagnostic-logs"></a>Log di diagnostica avanzati

Il modulo ASP.NET Core può essere configurato per restituire log di diagnostica avanzata. Aggiungere l'elemento `<handlerSettings>` all'elemento `<aspNetCore>` in *web.config*. L'impostazione di `debugLevel` su `TRACE` restituisce una maggior fedeltà dei dati diagnostici:

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="InProcess">
  <handlerSettings>
    <handlerSetting name="debugFile" value="aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

I valori di livello debug (`debugLevel`) possono includere sia il livello che il percorso.

Livelli (in ordine dal meno dettagliato al più dettagliato):

* ERROR
* WARNING
* INFO
* TRACE

Posizioni (sono consentite più posizioni):

* CONSOLE
* EVENTLOG
* FILE

Le impostazioni del gestore possono essere specificate anche tramite le variabili di ambiente:

* `ASPNETCORE_MODULE_DEBUG_FILE` &ndash; Percorso del file di logo di debug. (Impostazione predefinita: *aspnetcore-debug.log*)
* `ASPNETCORE_MODULE_DEBUG` &ndash; Impostazione del livello di debug.

> [!WARNING]
> **Non** lasciare la registrazione del debug abilitata nella distribuzione per un tempo superiore a quello necessario alla risoluzione di problema. Le dimensioni del log non sono limitate. Se si lascia abilitato il log di debug, lo spazio disponibile su disco può esaurirsi e il server o il servizio app può registrare un arresto anomalo.

::: moniker-end

Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configurazione del proxy usa il protocollo HTTP e un token di associazione

::: moniker range=">= aspnetcore-2.2"

*Si applica solo all'hosting out-of-process.*

::: moniker-end

Il proxy creato tra il modulo ASP.NET Core e Kestrel usa il protocollo HTTP. L'uso di HTTP è un'ottimizzazione delle prestazioni in cui il traffico tra il modulo e Kestrel avviene in un indirizzo di loopback dell'interfaccia di rete. Non vi è alcun rischio di intercettazione del traffico tra il modulo e Kestrel da una posizione esterna al server.

Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine. Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo. Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste trasmesse tramite proxy. Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente. Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata. La variabile di ambiente del token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione esterna al server. Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modulo ASP.NET Core con configurazione condivisa di IIS

Il programma di installazione del modulo ASP.NET Core viene eseguito con i privilegi dell'account **SYSTEM**. Poiché l'account di sistema locale non dispone dell'autorizzazione di modifica per il percorso della condivisione usato dalla configurazione condivisa di IIS, il programma di installazione rileva un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in *applicationHost.config* nella condivisione. Quando si usa una configurazione condivisa di IIS, attenersi ai passaggi riportati di seguito:

1. Disabilitare la configurazione condivisa di IIS.
1. Eseguire il programma di installazione.
1. Esportare il file *applicationHost.config* aggiornato nella condivisione.
1. Abilitare di nuovo la configurazione condivisa di IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Versione del modulo e log del programma di installazione del bundle di hosting

Per determinare la versione del modulo ASP.NET Core installato:

1. Nel sistema host passare a *%windir%\System32\inetsrv*.
1. Individuare il file *aspnetcore.dll*.
1. Fare clic con il pulsante destro del mouse sul file e scegliere **Proprietà** dal menu di scelta rapida.
1. Selezionare la scheda **Dettagli**. **Versione file** e **Versione prodotto** rappresentano la versione installata del modulo.

I log del programma di installazione del bundle di hosting per il modulo sono disponibili in *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Percorsi dei file di modulo, schema e configurazione

### <a name="module"></a>Modulo

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

   * %Programmi%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

   * %Programmi(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

::: moniker range=">= aspnetcore-2.2"

   * %Programmi%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

   * %Programmi(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll

::: moniker-end

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml

::: moniker-end
**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

::: moniker range=">= aspnetcore-2.2"

   * %Programmi%\IIS Express\config\schema\aspnetcore_schema_v2.xml

::: moniker-end

### <a name="configuration"></a>Configurazione

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * %Programmi%\IIS Express\config\templates\PersonalWebServer\applicationHost.config

È possibile trovare i file eseguendo una ricerca di *aspnetcore* nel file *applicationHost.config*.
