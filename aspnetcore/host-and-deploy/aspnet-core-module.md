---
title: Guida di riferimento per la configurazione del modulo ASP.NET Core
author: guardrex
description: Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 954841a1b1465c80e60d5745ad9e22294a88fdf4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483747"
---
# <a name="aspnet-core-module-configuration-reference"></a>Guida di riferimento per la configurazione del modulo ASP.NET Core

Di [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questo documento fornisce istruzioni su come configurare il modulo ASP.NET Core per l'hosting di app ASP.NET Core. Per un'introduzione al modulo ASP.NET Core e istruzioni per l'installazione, vedere la [panoramica del modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configurazione con web.config

Il modulo ASP.NET Core viene configurato tramite la sezione `aspNetCore` del nodo `system.webServer` nel file *web.config* del sito.

Il file *web.config* seguente viene pubblicato per una [distribuzione dipendente dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo ASP.NET Core per gestire le richieste del sito:

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

Il file *web.config* seguente viene pubblicato per una [distribuzione autonoma](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Quando un'app viene distribuita in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), il percorso `stdoutLogFile` è impostato su `\\?\%home%\LogFiles\stdout`. Il percorso salva i log stdout nella cartella *LogFiles*, ovvero una posizione creata automaticamente dal servizio.

Vedere [Configurazione delle applicazioni secondarie](xref:host-and-deploy/iis/index#sub-application-configuration) per una nota importante relativa alla configurazione di *web.config* file nelle app secondarie.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributi dell'elemento aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Attributo | Descrizione | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato in **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false.</p><p>Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | true o false.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</p> | `true` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**. Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</p> | `10` |
| `requestTimeout` | <p>Attributo Timespan facoltativo.</p><p>Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</p><p>Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.0 o versioni precedenti, `requestTimeout` deve essere specificato solo in minuti interi. In caso contrario, verrà usato il valore predefinito di 2 minuti.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</p> | `10` |
| `startupTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile. Se questo limite di tempo viene superato, il modulo termina il processo. Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</p> | `120` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti. Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log. Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**. Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Attributo | Descrizione | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato in **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false.</p><p>Se true, la pagina **502.5 - Errore del processo** non viene visualizzata e la tabella codici di stato 502 configurata in *web.config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | true o false.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su %ASPNETCORE_PORT% come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo chiamare CloseHandle su questo token per ogni richiesta.</p> | `true` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo Integer facoltativo.</p><p>Specifica il numero di arresti anomali al minuto per il processo specificato in **processPath**. Se questo limite viene superato, il modulo smette di avviare il processo per la parte restante del minuto.</p> | `10` |
| `requestTimeout` | <p>Attributo Timespan facoltativo.</p><p>Specifica la durata per cui il modulo ASP.NET Core attende una risposta dal processo in ascolto su %ASPNETCORE_PORT%.</p><p>Nelle versioni del modulo ASP.NET Core fornito con ASP.NET Core 2.1 o versioni successive, `requestTimeout` viene specificato in ore, minuti e secondi.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende che il file eseguibile venga arrestato normalmente quando viene rilevato il file *app_offline.htm*.</p> | `10` |
| `startupTimeLimit` | <p>Attributo Integer facoltativo.</p><p>Durata in secondi per cui il modulo attende l'avvio di un processo in ascolto sulla porta da parte del file eseguibile. Se questo limite di tempo viene superato, il modulo termina il processo. Il modulo tenta di avviare nuovamente il processo quando riceve una nuova richiesta e continua a tentare di riavviare il processo alle successive richieste in ingresso, a meno che non risulti impossibile avviare l'app un numero di volte pari a **rapidFailsPerMinute** nell'ultimo minuto continuo.</p> | `120` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato in **processPath** vengono reindirizzati al file specificato in **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per cui vengono registrati **stdout** e **stderr** dal processo specificato in **processPath**. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso che inizia con `.` è relativo al sito radice e tutti gli altri percorsi vengono trattati come percorsi assoluti. Le eventuali cartelle specificate nel percorso devono essere già esistenti affinché il modulo possa creare il file di log. Usando il carattere di sottolineatura come delimitatore, il timestamp, l'ID processo e l'estensione del file (*.log*) vengono aggiunti all'ultimo segmento del percorso **stdoutLogFile**. Se si specifica `.\logs\stdout` come valore, un log stdout di esempio salvato il 5/2/2018 alle 19:41:32 con un ID processo 1934 viene salvato come *stdout_20180205194132_1934.log* nella cartella *logs*.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Impostazione delle variabili di ambiente

È possibile specificare le variabili di ambiente per il processo nell'attributo `processPath`. Specificare una variabile di ambiente con l'elemento figlio `environmentVariable` di un elemento della raccolta `environmentVariables`. Le variabili di ambiente impostate in questa sezione hanno la precedenza sulle variabili di ambiente di sistema.

Nell'esempio seguente vengono impostate due variabili di ambiente. `ASPNETCORE_ENVIRONMENT` configura l'ambiente dell'app su `Development`. Uno sviluppatore può impostare temporaneamente questo valore nel file *web.config* per forzare il caricamento della [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) durante il debug di un'eccezione dell'app. `CONFIG_DIR` è un esempio di variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento del file di configurazione dell'app.

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

> [!WARNING]
> Impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` su `Development` solo in server di gestione temporanea e test che non sono accessibili da reti non attendibili, ad esempio Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Se un file denominato *app_offline.htm* viene rilevato nella directory radice di un'app, il modulo ASP.NET Core tenta di arrestare normalmente l'app e arresta l'elaborazione delle richieste in ingresso. Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo ASP.NET Core termina il processo in esecuzione.

Mentre il file *app_offline.htm* è presente, il modulo ASP.NET Core risponde alle richieste restituendo il contenuto del file *app_offline.htm*. Quando il file *app_offline.htm* viene rimosso, la richiesta successiva avvia l'app.

## <a name="start-up-error-page"></a>Pagina di errore di avvio

Se il modulo ASP.NET Core non riesce ad avviare il processo di back-end o il processo di back-end viene avviato ma non riesce a restare in ascolto sulla porta configurata, verrà visualizzata la tabella codici di stato *502.5 Errore del processo*. Per non visualizzare questa pagina e tornare alla tabella codici di stato 502 predefinita di IIS, usare l'attributo `disableStartUpErrorPage`. Per altre informazioni sulla configurazione di messaggi di errore personalizzati, vedere [Errori HTTP `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).

![Tabella codici di stato 502.5 Errore del processo](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Creazione e reindirizzamento dei log

Il modulo ASP.NET Core reindirizza su disco i log stdout e stderr se sono impostati gli attributi `stdoutLogEnabled` e `stdoutLogFile` dell'elemento `aspNetCore`. Le eventuali cartelle nel percorso `stdoutLogFile` devono essere già esistenti affinché il modulo possa creare il file di log. Il pool di app deve avere accesso in scrittura alla posizione in cui vengono scritti i log (usare `IIS AppPool\<app_pool_name>` per specificare l'autorizzazione di scrittura).

I log non vengono ruotati, a meno che non si verifichi il riciclo/riavvio del processo. È responsabilità del provider di servizi di hosting limitare lo spazio su disco usato dai log.

L'uso del log stdout è consigliato solo per la risoluzione dei problemi di avvio delle app. Non usare il log stdout per scopi di registrazione generale delle app. Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

Un timestamp e l'estensione del file vengono aggiunti automaticamente al momento della creazione del file di log. Il nome del file di log è composto aggiungendo il timestamp, l'ID processo e l'estensione del file (*.log*) all'ultimo segmento del percorso `stdoutLogFile` (in genere *stdout*), con caratteri di sottolineatura come delimitatori. Se il percorso `stdoutLogFile` termina con *stdout*, un log per un'app con un PID 1934 creata il 5/2/2018 alle 19:42:32 sarà denominato *stdout_20180205194132_1934.log*.

L'elemento di esempio `aspNetCore` seguente configura la registrazione stdout per un'app ospitata in Servizio app di Azure. Un percorso locale o un percorso di una condivisione di rete è accettabile per la registrazione locale. Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Vedere [Configurazione con web.config](#configuration-with-webconfig) per un esempio dell'elemento `aspNetCore` nel file *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configurazione del proxy usa il protocollo HTTP e un token di associazione

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

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configurazione

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

È possibile individuare i file eseguendo una ricerca di *aspnetcore.dll* nel file *applicationHost.config*. Per IIS Express, il file *applicationHost.config* non esiste per impostazione predefinita. Il file viene creato in *\<application_root>\\.vs\\config* all'avvio di qualsiasi progetto di app Web nella soluzione Visual Studio.
