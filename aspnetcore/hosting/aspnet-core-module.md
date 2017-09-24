---
title: Riferimento di configurazione di ASP.NET modulo Core
author: guardrex
description: Come configurare il modulo di base di ASP.NET per ospitare applicazioni ASP.NET Core.
keywords: ASP.NET Core, ancm, modulo di base, iis, la registrazione di stdout, variabile di ambiente, var env, subapplication, subapp, appoffline, app_offline, 502, dello schema
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: ac52b791e02ce52da35fe8d599465076d251b4da
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2017
---
# <a name="aspnet-core-module-configuration-reference"></a>Riferimento di configurazione di ASP.NET modulo Core

Da [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Questo documento vengono fornite informazioni dettagliate su come configurare il modulo di base di ASP.NET per ospitare applicazioni ASP.NET Core. Per informazioni introduttive per il modulo di base di ASP.NET e le istruzioni di installazione, vedere il [Panoramica del modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).

## <a name="configuration-via-webconfig"></a>Configurazione tramite Web. config

Il modulo di base di ASP.NET viene configurato tramite un sito o applicazione *Web. config* file e ha il proprio `aspNetCore` sezione di configurazione all'interno di `system.webServer`. Di seguito è riportato un esempio *Web. config* file che il `Microsoft.NET.Sdk.Web` SDK fornirà quando il progetto viene pubblicato per un [framework dipendente distribuzione](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) con segnaposto per il `processPath` e `arguments`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Il *Web. config* esempio riportato di seguito è per un [distribuzione indipendente](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) per il [Azure App Service](https://azure.microsoft.com/services/app-service/). Per ulteriori informazioni, vedere [la pubblicazione in IIS](xref:publishing/iis). Vedere [configurazione delle applicazioni secondario](xref:publishing/iis#configuration-of-sub-applications) per una nota importante relativi alla configurazione di *Web. config* file secondari di applicazioni.

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a>Attributi dell'elemento aspNetCore

| Attributo | Descrizione |
| --- | --- |
| processPath | <p>Attributo stringa obbligatorio.</p><p>Percorso dell'eseguibile da avvia un processo in ascolto delle richieste HTTP. I percorsi relativi sono supportati. Se il percorso inizia con '.', il percorso viene considerato come relativo alla radice del sito.</p><p>Non è previsto alcun valore predefinito.</p> |
| arguments | <p>Attributo stringa facoltativo.</p><p>Argomenti per l'eseguibile specificato **processPath**.</p><p>Il valore predefinito è una stringa vuota.</p> |
| startupTimeLimit | <p>Attributo intero facoltativo.</p><p>Durata in secondi di attesa per l'eseguibile da avviare un processo in ascolto sulla porta il modulo. Se viene superato questo limite di tempo, il modulo verrà terminare il processo. Il modulo tenterà di avviare nuovamente il processo quando riceve una richiesta di nuovo e continuerà a tentare di riavviare il processo successivo delle richieste in ingresso, a meno che non viene avviato l'applicazione **rapidFailsPerMinute** numero di volte in cui nell'ultimo minuto in sequenza.</p><p>Il valore predefinito è 120.</p> |
| shutdownTimeLimit | <p>Attributo intero facoltativo.</p><p>Durata in secondi per cui il modulo di attesa per il file eseguibile per arrestare normalmente quando il *app_offline.htm* file è stato rilevato.</p><p>Il valore predefinito è 10.</p> |
| rapidFailsPerMinute | <p>Attributo intero facoltativo.</p><p>Specifica il numero di volte specificato dal processo in **processPath** può arrestarsi al minuto. Se questo limite viene superato, il modulo smetterà di avviare il processo per la parte restante del minuto.</p><p>Il valore predefinito è 10.</p> |
| requestTimeout | <p>Attributo timespan facoltativo.</p><p>Specifica la durata per cui il modulo di base di ASP.NET attenderà una risposta dal processo in ascolto su % ASPNETCORE_PORT %.</p><p>Il valore predefinito è "00:02:00".</p> |
| stdoutLogEnabled | <p>Attributo booleano facoltativo.</p><p>Se true, **stdout** e **stderr** per il processo specificato **processPath** verranno reindirizzati al file specificato **stdoutLogFile**.</p><p>Il valore predefinito è false.</p> |
| stdoutLogFile | <p>Attributo stringa facoltativo.</p><p>Specifica il percorso relativo o assoluto per il quale **stdout** e **stderr** dal processo specificato in **processPath** verranno registrati. I percorsi relativi sono relativi alla radice del sito. Qualsiasi percorso inizia con '.' sarà relativo alla radice del sito e tutti gli altri percorsi verranno considerati come percorsi assoluti. Tutte le cartelle nel percorso specificate devono essere presente affinché il modulo creare il file di log. L'ID del processo, timestamp (*yyyyMdhms*) e l'estensione di file (*log*) con un carattere di sottolineatura delimitatori vengono aggiunti all'ultimo segmento del **stdoutLogFile** fornito.</p><p>Il valore predefinito è `aspnetcore-stdout`.</p> |
| forwardWindowsAuthToken | true o false.</p><p>Se true, il token verrà inoltrato al processo figlio in ascolto su % ASPNETCORE_PORT % come un'intestazione 'MS-ASPNETCORE-WINAUTHTOKEN' per ogni richiesta. È responsabilità del processo di chiamare CloseHandle questo token per ogni richiesta.</p><p>Il valore predefinito è true.</p> |
| disableStartUpErrorPage | true o false.</p><p>Se true, il **502.5 - errore del processo di** pagina verrà eliminata e la tabella codici di 502 stato nel *Web. config* avrà la precedenza.</p><p>Il valore predefinito è false.</p> |

### <a name="setting-environment-variables"></a>Impostazione delle variabili di ambiente

Il modulo di base di ASP.NET consente di specificare le variabili di ambiente per il processo specificato nella `processPath` attributo specificandoli in uno o più `environmentVariable` gli elementi figlio di un `environmentVariables` elemento della raccolta nel `aspNetCore` elemento. Variabili di ambiente impostate in questa sezione hanno la precedenza su sistema le variabili di ambiente per il processo.

Nell'esempio seguente imposta due variabili di ambiente. `ASPNETCORE_ENVIRONMENT`verrà configurato l'ambiente dell'applicazione per `Development`. Uno sviluppatore può impostare temporaneamente questo valore *Web. config* file per forzare il [pagina eccezione developer](xref:fundamentals/error-handling) da caricare durante il debug di un'eccezione di app. `CONFIG_DIR`è un esempio di una variabile di ambiente definita dall'utente, in cui lo sviluppatore ha scritto codice che verrà letto il valore all'avvio in modo da formare un percorso per caricare il file di configurazione dell'applicazione.

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

## <a name="appofflinehtm"></a>App_offline.htm

Se si inserisce un file con il nome *app_offline.htm* alla radice di una directory dell'applicazione web, il modulo di base di ASP.NET sarà tenta di arrestare normalmente l'app e arrestare l'elaborazione delle richieste in ingresso. Se l'app è ancora in esecuzione `shutdownTimeLimit` numero di secondi, il modulo di base di ASP.NET sarà terminare il processo in esecuzione.

Mentre il *app_offline.htm* file è presente, il modulo di base di ASP.NET risponderà alle richieste restituendo il contenuto del *app_offline.htm* file. Una volta il *app_offline.htm* file viene rimosso, alla successiva richiesta di caricamento dell'applicazione, che quindi risponde alle richieste.

## <a name="start-up-error-page"></a>Pagina di errore di avvio

Se il modulo di base di ASP.NET non è possibile avviare il processo di back-end o l'avvio di processo di back-end ma ha esito negativo per l'ascolto sulla porta configurata, si visualizzeranno una pagina di codice di stato HTTP 502.5. Per non visualizzare questa pagina e tornare alla tabella codici predefinita IIS 502 stato, utilizzare il `disableStartUpErrorPage` attributo. Per ulteriori informazioni sulla configurazione di messaggi di errore personalizzati, vedere [errori HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).

![Pagina stato 502](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Reindirizzamento e la creazione di log

Il modulo di base di ASP.NET reindirizza `stdout` e `stderr` log su disco se si imposta la `stdoutLogEnabled` e `stdoutLogFile` gli attributi del `aspNetCore` elemento. Le cartelle di `stdoutLogFile` percorso deve essere presente affinché il modulo creare il file di log. Un'estensione di timestamp e il file verrà aggiunto automaticamente quando viene creato il file di log. Log non vengono ruotati, a meno che non si verifica il riciclo o al riavvio del processo. È responsabilità dell'host per limitare lo spazio su disco, che utilizzano i log. Utilizzo di `stdout` log è consigliato solo per la risoluzione dei problemi di avvio dell'applicazione e non per scopi di registrazione generali dell'applicazione.

Il nome del file di log è costituito da aggiungendo l'ID processo (PID), timestamp (*yyyyMdhms*) e l'estensione di file (*log*) per l'ultimo segmento del `stdoutLogFile` percorso (in genere *stdout *) delimitati da caratteri di sottolineatura. Ad esempio se il `stdoutLogFile` percorso termina con *stdout*, un log per un'app con un PID di 10652 creato in 10/8/2017 12:05:02 ha il nome del file *stdout_10652_20178101252.log*.

Di seguito è riportato un esempio `aspNetCore` elemento che configura `stdout` registrazione. Il `stdoutLogFile` illustrato nell'esempio di percorso è appropriato per il servizio App di Azure. Un percorso locale o un percorso di condivisione di rete è accettabile per l'accesso locale. Verificare che l'identità dell'utente AppPool disponga dell'autorizzazione di scrittura per il percorso specificato.

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>Modulo Core ASP.NET con IIS condiviso

Il programma di installazione del modulo di base di ASP.NET viene eseguito con i privilegi del **sistema** account. Poiché l'account sistema locale non dispone dell'autorizzazione Modifica per il percorso della condivisione utilizzata per la configurazione condivisa di IIS, il programma di installazione verrà raggiunto un errore di accesso negato durante il tentativo di configurare le impostazioni del modulo in * applicationHost. config* nella condivisione.

È la soluzione non supportata per disabilitare la configurazione condivisa di IIS, eseguire il programma di installazione e l'esportazione aggiornato *applicationHost. config* alla condivisione di file e abilitare nuovamente la configurazione condivisa di IIS.

## <a name="module-schema-and-configuration-file-locations"></a>Percorsi dei file di modulo, schemi e configurazione

### <a name="module"></a>Modulo

**IIS (x86 o amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86 o amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * % ProgramFiles (x86) %\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.Xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Configurazione

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

È possibile cercare *aspnetcore.dll* nel *applicationHost. config* file. Per IIS Express, il *applicationHost. config* file non esiste per impostazione predefinita. Il file viene creato in *{radice dell'applicazione}\.vs\config* quando si avvia un progetto di applicazione web nella soluzione di Visual Studio.
