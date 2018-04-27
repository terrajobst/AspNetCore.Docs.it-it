---
title: Riferimento di configurazione di ASP.NET modulo Core
author: guardrex
description: Informazioni su come configurare il modulo di base di ASP.NET per ospitare applicazioni ASP.NET Core.
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
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-module-configuration-reference"></a>Riferimento di configurazione di ASP.NET modulo Core

Da [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questo documento vengono fornite istruzioni su come configurare il modulo di base di ASP.NET per ospitare applicazioni ASP.NET Core. Per informazioni introduttive per il modulo di base di ASP.NET e le istruzioni di installazione, vedere il [Panoramica del modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-with-webconfig"></a>Configurazione con Web. config

Il modulo di base di ASP.NET è configurato con il `aspNetCore` sezione la `system.webServer` nodo del sito *Web. config* file.

Nell'esempio *Web. config* file viene pubblicato per un [distribuzione dipendenti dal framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) e configura il modulo di base di ASP.NET per gestire le richieste del sito:

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

Nell'esempio *Web. config* è pubblicata per una [distribuzione indipendente](/dotnet/articles/core/deploying/#self-contained-deployments-scd):

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

Quando un'applicazione viene distribuita in [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` percorso è impostato su `\\?\%home%\LogFiles\stdout`. Il percorso Salva log di stdout per il *LogFiles* cartella, ovvero una posizione automaticamente creati dal servizio.

Vedere [configurazione applicazione secondaria](xref:host-and-deploy/iis/index#sub-application-configuration) per una nota importante relativi alla configurazione di *Web. config* file nelle sottodirectory di App.

### <a name="attributes-of-the-aspnetcore-element"></a>Attributi dell'elemento aspNetCore

::: moniker range="<= aspnetcore-2.0"
| Attributo | Descrizione | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false.</p><p>Se true, il **502.5 - errore del processo di** pagina viene eliminata e la tabella codici di 502 stato nel *Web. config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | true o false.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su % ASPNETCORE_PORT % come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo di chiamare CloseHandle questo token per ogni richiesta.</p> | `true` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che consente di avviare un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato il percorso relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo intero facoltativo.</p><p>Specifica il numero di volte specificato dal processo in **processPath** può arrestarsi al minuto. Se questo limite viene superato, il modulo smetterà di avviare il processo per la parte restante del minuto.</p> | `10` |
| `requestTimeout` | <p>Attributo timespan facoltativo.</p><p>Specifica la durata per cui il modulo di base di ASP.NET attende una risposta dal processo in ascolto su % ASPNETCORE_PORT %.</p><p>Nelle versioni del modulo ASP.NET Core fornito con la versione di ASP.NET Core 2.0 o versioni precedenti, il `requestTimeout` deve essere specificato in minuti interi solo, in caso contrario, il valore predefinito è 2 minuti.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attributo intero facoltativo.</p><p>Durata in secondi che il modulo è in attesa per il file eseguibile per arrestare normalmente quando il *app_offline.htm* file è stato rilevato.</p> | `10` |
| `startupTimeLimit` | <p>Attributo intero facoltativo.</p><p>Durata in secondi che il modulo è in attesa per l'eseguibile da avviare un processo in ascolto sulla porta. Se viene superato questo limite di tempo, il modulo termina il processo. Il modulo tenta di riavviare il processo quando riceve una richiesta di nuovo e continua a tentare di riavviare il processo successivo delle richieste in ingresso, a meno che non si avvia l'app **rapidFailsPerMinute** volte negli ultimi minuto in sequenza.</p> | `120` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato **processPath** vengono reindirizzati al file specificato **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per il quale **stdout** e **stderr** dal processo specificato in **processPath** vengono registrati. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso a partire da `.` sono relativo al sito radice e tutti gli altri percorsi vengono considerati come percorsi assoluti. Tutte le cartelle nel percorso specificate devono essere presente affinché il modulo creare il file di log. Usando i delimitatori di carattere di sottolineatura, timestamp, ID processo e l'estensione di file (*log*) vengono aggiunti all'ultimo segmento del **stdoutLogFile** percorso. Se `.\logs\stdout` viene fornito come valore, un log di stdout di esempio viene salvato come *stdout_20180205194132_1934.log* nel *registri* cartella quando salvati in 2/5/2018 al 19:41:32 con un ID processo 1934.</p> | `aspnetcore-stdout` |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Attributo | Descrizione | Impostazione predefinita |
| --------- | ----------- | :-----: |
| `arguments` | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato **processPath**.</p>| |
| `disableStartUpErrorPage` | true o false.</p><p>Se true, il **502.5 - errore del processo di** pagina viene eliminata e la tabella codici di 502 stato nel *Web. config* ha la precedenza.</p> | `false` |
| `forwardWindowsAuthToken` | true o false.</p><p>Se true, il token viene inoltrato al processo figlio in ascolto su % ASPNETCORE_PORT % come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo di chiamare CloseHandle questo token per ogni richiesta.</p> | `true` |
| `processPath` | <p>Attributo stringa obbligatorio.</p><p>Percorso del file eseguibile che consente di avviare un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con `.`, viene considerato il percorso relativo alla radice del sito.</p> | |
| `rapidFailsPerMinute` | <p>Attributo intero facoltativo.</p><p>Specifica il numero di volte specificato dal processo in **processPath** può arrestarsi al minuto. Se questo limite viene superato, il modulo smetterà di avviare il processo per la parte restante del minuto.</p> | `10` |
| `requestTimeout` | <p>Attributo timespan facoltativo.</p><p>Specifica la durata per cui il modulo di base di ASP.NET attende una risposta dal processo in ascolto su % ASPNETCORE_PORT %.</p><p>Nelle versioni del modulo ASP.NET Core fornito con la versione di ASP.NET Core 2.1 o versioni successive, il `requestTimeout` viene specificato in ore, minuti e secondi.</p> | `00:02:00` |
| `shutdownTimeLimit` | <p>Attributo intero facoltativo.</p><p>Durata in secondi che il modulo è in attesa per il file eseguibile per arrestare normalmente quando il *app_offline.htm* file è stato rilevato.</p> | `10` |
| `startupTimeLimit` | <p>Attributo intero facoltativo.</p><p>Durata in secondi che il modulo è in attesa per l'eseguibile da avviare un processo in ascolto sulla porta. Se viene superato questo limite di tempo, il modulo termina il processo. Il modulo tenta di riavviare il processo quando riceve una richiesta di nuovo e continua a tentare di riavviare il processo successivo delle richieste in ingresso, a meno che non si avvia l'app **rapidFailsPerMinute** volte negli ultimi minuto in sequenza.</p> | `120` |
| `stdoutLogEnabled` | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato **processPath** vengono reindirizzati al file specificato **stdoutLogFile**.</p> | `false` |
| `stdoutLogFile` | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per il quale **stdout** e **stderr** dal processo specificato in **processPath** vengono registrati. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso a partire da `.` sono relativo al sito radice e tutti gli altri percorsi vengono considerati come percorsi assoluti. Tutte le cartelle nel percorso specificate devono essere presente affinché il modulo creare il file di log. Usando i delimitatori di carattere di sottolineatura, timestamp, ID processo e l'estensione di file (*log*) vengono aggiunti all'ultimo segmento del **stdoutLogFile** percorso. Se `.\logs\stdout` viene fornito come valore, un log di stdout di esempio viene salvato come *stdout_20180205194132_1934.log* nel *registri* cartella quando salvati in 2/5/2018 al 19:41:32 con un ID processo 1934.</p> | `aspnetcore-stdout` |
::: moniker-end

### <a name="setting-environment-variables"></a>Impostazione delle variabili di ambiente

Le variabili di ambiente possono essere specificate per il processo di `processPath` attributo. Specificare una variabile di ambiente con il `environmentVariable` elemento figlio di un `environmentVariables` elemento della raccolta. Variabili di ambiente impostate in questa sezione hanno la precedenza su sistema le variabili di ambiente.

Nell'esempio seguente imposta due variabili di ambiente. `ASPNETCORE_ENVIRONMENT` Configura l'ambiente dell'app per `Development`. Uno sviluppatore può impostare temporaneamente questo valore *Web. config* file per forzare il [pagina eccezione Developer](xref:fundamentals/error-handling) da caricare durante il debug di un'eccezione di app. `CONFIG_DIR` è un esempio di una variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto il codice che legge il valore all'avvio in modo da formare un percorso per il caricamento di file di configurazione dell'applicazione.

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
> Impostare solo la `ASPNETCORE_ENVIRONMENT` envirnonment variabile `Development` per gestione temporanea e testare i server che non sono accessibili a reti non attendibili, ad esempio Internet.

## <a name="appofflinehtm"></a>app_offline.htm

Se un file con il nome *app_offline.htm* rilevato nella directory radice di un'app, il modulo di base di ASP.NET tenta di arrestare normalmente l'applicazione e arrestare l'elaborazione delle richieste in ingresso. Se l'app è ancora in esecuzione dopo il numero di secondi definito in `shutdownTimeLimit`, il modulo di base di ASP.NET termina il processo in esecuzione.

Mentre il *app_offline.htm* file è presente, il modulo di base di ASP.NET risponde alle richieste restituendo il contenuto del *app_offline.htm* file. Quando il *app_offline.htm* file viene rimosso, la richiesta successiva avvia l'app.

## <a name="start-up-error-page"></a>Pagina di errore di avvio

Se il modulo di base di ASP.NET ha esito negativo avviare il processo di back-end o di inizio processo back-end ma ha esito negativo per l'ascolto sulla porta configurata, un *502.5 errore del processo* verrà visualizzata la pagina di codice di stato. Per non visualizzare questa pagina e tornare alla tabella codici predefinita IIS 502 stato, utilizzare il `disableStartUpErrorPage` attributo. Per ulteriori informazioni sulla configurazione di messaggi di errore personalizzati, vedere [errori HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![502.5 tabella codici di stato errore processo](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Reindirizzamento e la creazione di log

Il modulo di base di ASP.NET reindirizza i log di stdout e stderr su disco se il `stdoutLogEnabled` e `stdoutLogFile` attributi del `aspNetCore` elemento vengono impostati. Le cartelle di `stdoutLogFile` percorso deve essere presente affinché il modulo creare il file di log. Il pool di applicazioni deve avere l'accesso in scrittura al percorso in cui vengono scritti i log (utilizzare `IIS AppPool\<app_pool_name>` per fornire l'autorizzazione di scrittura).

I log non sono ruotati, a meno che non si verifica il riciclo o al riavvio del processo. È responsabilità dell'host per limitare lo spazio su disco, che utilizzano i log.

Utilizzando il log di stdout è consigliata solo per la risoluzione dei problemi di avvio delle app. Non utilizzare il log di stdout per scopi di registrazione applicazione generale. Per la routine registrazione in un'applicazione ASP.NET di base, utilizzare una libreria di registrazione che limita dimensioni file di log e ruota i log. Per ulteriori informazioni, vedere [provider di log di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

Un'estensione di timestamp e i file vengono aggiunti automaticamente quando viene creato il file di log. Il nome del file di log è costituito da aggiungendo il timestamp, ID di processo e l'estensione di file (*log*) per l'ultimo segmento del `stdoutLogFile` percorso (in genere *stdout*) delimitati da caratteri di sottolineatura. Se il `stdoutLogFile` percorso termina con *stdout*, il nome del file dispone di un log per un'app con un PID di 1934 creato in 2/5/2018 42:19:32 *stdout_20180205194132_1934.log*.

L'esempio seguente `aspNetCore` elemento consente di configurare la registrazione di stdout per un'applicazione ospitata in Azure App Service. Un percorso locale o un percorso di condivisione di rete è accettabile per l'accesso locale. Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

Vedere [configurazione con Web. config](#configuration-with-webconfig) per un esempio del `aspNetCore` elemento il *Web. config* file.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>La configurazione del proxy usa il protocollo HTTP e un token di associazione

Il proxy creato tra il modulo di base di ASP.NET e Kestrel utilizza il protocollo HTTP. Utilizzo di HTTP è un'ottimizzazione delle prestazioni, in cui il traffico tra il modulo e Kestrel effettuata in un indirizzo di loopback dall'interfaccia di rete. Non c'è alcun rischio di intercettazione il traffico tra il modulo e Kestrel da una posizione dal server.

Viene usato un token di associazione per garantire che le richieste ricevute da Kestrel siano state trasmesse tramite proxy da IIS e non provengano da un'altra origine. Il token di associazione viene creato e impostato in una variabile di ambiente (`ASPNETCORE_TOKEN`) dal modulo. Il token di associazione viene impostato anche in un'intestazione (`MSAspNetCoreToken`) in tutte le richieste trasmesse tramite proxy. Il middleware di IIS controlla ogni richiesta che riceve in modo da confermare che il valore dell'intestazione del token di associazione corrisponda al valore della variabile di ambiente. Se i valori del token non corrispondono, la richiesta viene registrata e rifiutata. La variabile di ambiente token di associazione e il traffico tra il modulo e Kestrel non sono accessibili da una posizione dal server. Senza conoscere il valore del token di associazione, un utente malintenzionato non può inviare richieste che ignorano il controllo nel middleware di IIS.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modulo Core ASP.NET con IIS condiviso

Il programma di installazione del modulo di base di ASP.NET viene eseguito con i privilegi del **sistema** account. Poiché l'account sistema locale non dispone dell'autorizzazione Modifica per il percorso della condivisione utilizzato per la configurazione condivisa di IIS, il programma di installazione riscontri un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in *fileApplicationHost.config* nella condivisione. Quando si utilizza una configurazione condivisa di IIS, seguire questi passaggi:

1. Disabilitare la configurazione condivisa di IIS.
1. Eseguire il programma di installazione.
1. Esportare aggiornato *applicationHost. config* file alla condivisione.
1. Abilitare nuovamente la configurazione condivisa di IIS.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Versione del modulo e Bundle che ospita i registri di programma di installazione

Per determinare la versione del modulo di base di ASP.NET installati:

1. Nel sistema host, passare a *%windir%\system32\inetsrv*.
1. Individuare il *aspnetcore.dll* file.
1. Il pulsante destro e selezionare **proprietà** dal menu di scelta rapida.
1. Selezionare il **dettagli** scheda. Il **versione del File** e **versione prodotto** rappresentano la versione installata del modulo.

I log del programma di installazione Hosting Bundle per il modulo sono disponibili nella *c:.\\gli utenti\\% UserName %\\AppData\\locale\\Temp*. Il file è denominato *dd_DotNetCoreWinSvrHosting__\<timestamp > _000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Percorsi dei file di modulo, schemi e configurazione

### <a name="module"></a>Modulo

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86 o amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

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

I file sono reperibile cercando *aspnetcore.dll* nel *applicationHost. config* file. Per IIS Express, il *applicationHost. config* file non esiste per impostazione predefinita. Il file viene creato in  *\<application_root >\\VS\\configurazione* per l'avvio di qualsiasi progetto di app web nella soluzione di Visual Studio.
