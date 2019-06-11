---
title: Risolvere i problemi di ASP.NET Core in IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni Internet Information Services (IIS) di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e4c93459f2030c7c0a55ea90e0cc8c8d30b76c51
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470453"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Risolvere i problemi di ASP.NET Core in IIS

Di [Luke Latham](https://github.com/guardrex)

In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core durante l'hosting con [Internet Information Services (IIS)](/iis). Le informazioni in questo articolo si applicano all'hosting in IIS su Windows Server e Windows Desktop.

::: moniker range=">= aspnetcore-2.2"

In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug. È possibile risolvere un codice *502.5 - Errore del processo* o *500.30 - Errore di avvio* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

In Visual Studio, per impostazione predefinita un progetto ASP.NET Core usa l'hosting in [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) durante il debug. È possibile risolvere un codice *502.5 Errore del processo* che si verifica nel corso del debug in locale usando le indicazioni riportate in questo argomento.

::: moniker-end

Altri argomenti sulla risoluzione dei problemi:

<xref:host-and-deploy/azure-apps/troubleshoot>Anche se Servizio app usa il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS per ospitare le app, vedere l'argomento dedicato per istruzioni specifiche su Servizio app.

<xref:fundamentals/error-handling>Informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.

[Informazioni su come eseguire il debug con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) Questo argomento presenta le funzionalità del debugger di Visual Studio.

[Debug con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Informazioni sul supporto del debug incorporato in Visual Studio Code.

## <a name="app-startup-errors"></a>Errori di avvio delle app

### <a name="5025-process-failure"></a>502.5 Errore del processo

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare il processo dotnet back-end, ma l'avvio non riesce. In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente. Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione. Il *framework condiviso* è il set di assembly (file *DLL*) che vengono installati nel computer e cui fa riferimento un metapacchetto, ad esempio `Microsoft.AspNetCore.App`. Il riferimento del metapacchetto può specificare una versione minima richiesta. Per altre informazioni, vedere [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) (Il framework condiviso).

La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:

![Finestra del browser con la pagina 502.5 Errore del processo](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a>500.30 Errore di avvio in-process

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare .NET Core CLR In-Process, ma l'avvio non riesce. In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente. Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.

### <a name="5000-in-process-handler-load-failure"></a>500.0 Errore di caricamento del gestore in-process

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core non riesce a trovare .NET Core CLR e trova il gestore richieste In-Process (*aspnetcorev2_inprocess.dll*). Controllare che:

* L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 Errore di caricamento del gestore out-of-process

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core non riesce a trovare il gestore richieste di hosting out-of-process. Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 ANCM Failed to Find Native Dependencies (500.31 Il modulo ASP.NET Core non è riuscito a trovare le dipendenze native)

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare il runtime di .NET Core In-Process, ma l'avvio non riesce. La causa più comune di questo errore di avvio è la mancata installazione del runtime di `Microsoft.NETCore.App` o di `Microsoft.AspNetCore.App`. Se l'app viene distribuita per ASP.NET Core 3.0 e tale versione non esiste nel computer, si verifica questo errore. Ecco un esempio di messaggio di errore:

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

Il messaggio di errore elenca tutte le versioni di .NET Core installate e la versione richiesta dall'app. Per correggere l'errore, eseguire una delle operazioni seguenti:

* Installare la versione appropriata di .NET Core nel computer.
* Modificare l'app in modo che usi una versione di .NET Core presente nel computer.
* Pubblicare l'app come [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd).

Durante l'esecuzione in fase di sviluppo (con la variabile di ambiente `ASPNETCORE_ENVIRONMENT` impostata su `Development`), l'errore specifico viene scritto nella risposta HTTP. La causa di un errore di avvio del processo è disponibile anche nel [log eventi dell'applicazione](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 ANCM Failed to Load dll (500.32 Il modulo ASP.NET Core non è riuscito a caricare la DLL)

Il processo di lavoro ha esito negativo. L'app non viene avviata.

La causa più comune di questo errore è la pubblicazione dell'app per processori con architettura non compatibile. Se il processo di lavoro è in esecuzione come app a 32 bit e l'app è stata pubblicata per un'architettura a 64 bit, si verifica questo errore.

Per correggere l'errore, eseguire una delle operazioni seguenti:

* Ripubblicare l'app per processori con la stessa architettura del processo di lavoro.
* Pubblicare l'app come [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 ANCM Request Handler Load Failure (500.33 Errore di caricamento del gestore della richiesta del modulo ASP.Net Core)

Il processo di lavoro ha esito negativo. L'app non viene avviata.

L'app non fa riferimento al framework `Microsoft.AspNetCore.App`. Solo le app destinate al framework `Microsoft.AspNetCore.App` possono essere ospitate dal modulo ASP.NET Core.

Per correggere questo errore, verificare che l'app sia destinata al framework `Microsoft.AspNetCore.App`. Controllare il framework di destinazione dell'app nel file `.runtimeconfig.json`.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 ANCM Mixed Hosting Models Not Supported (500.34 Modelli di hosting misto non supportati nel modulo ASP.NET Core)

Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.

Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 ANCM Multiple In-Process Applications in same Process (500.35 Modulo ASP.NET Core: più applicazioni In-Process nello stesso processo)

Il processo di lavoro non è in grado di eseguire un'app In-Process e un'app Out-of-process nello stesso processo.

Per correggere questo errore, eseguire le app in pool di applicazioni IIS separati.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 ANCM Out-Of-Process Handler Load Failure (500.36 Modulo ASP.NET Core: Errore di caricamento del gestore Out-of-process)

Il gestore delle richieste Out-of-process, *aspnetcorev2_outofprocess.dll*, non è accanto al file *aspnetcorev2.dll*. Questo indica un'installazione danneggiata del modulo ASP.NET Core.

Per correggere questo errore, riparare l'installazione del [bundle di hosting di .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (per IIS) o di Visual Studio (per IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM Failed to Start Within Startup Time Limit (500.37 Avvio del modulo ASP.NET Core non riuscito entro il limite di tempo stabilito)

Il modulo ASP.NET Core non è stato avviato entro il limite di tempo specificato. Per impostazione predefinita, il timeout è di 120 secondi.

Questo errore può verificarsi quando si avvia un numero elevato di app nello stesso computer. Controllare i picchi di utilizzo della CPU e della memoria nel server durante l'avvio. Può essere necessario scaglionare il processo di avvio di più app.

### <a name="50030-in-process-startup-failure"></a>500.30 Errore di avvio in-process

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare il runtime di .NET Core In-Process, ma l'avvio non riesce. La causa di un errore di avvio di un processo viene in genere determinata osservando le voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="5000-in-process-handler-load-failure"></a>500.0 Errore di caricamento del gestore in-process

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Si è verificato un errore sconosciuto durante il caricamento dei componenti del modulo ASP.NET Core. Eseguire una delle azioni seguenti:

* Contattare il [supporto tecnico Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832). Selezionare **Strumenti di sviluppo** e quindi **ASP.NET Core**.
* Porre una domanda in Stack Overflow.
* Segnalare il problema nel [repository GitHub](https://github.com/aspnet/AspNetCore) di ASP.NET Core.

::: moniker-end

### <a name="500-internal-server-error"></a>500 Errore interno del server

L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.

Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta. La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*. Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente. Dal punto di vista del server, questo è corretto. L'app è stata effettivamente avviata, ma non può generare una risposta valida. Per risolvere il problema, [eseguire l'app dal prompt dei comandi](#run-the-app-at-a-command-prompt) nel server o [abilitare il log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Non è stato possibile avviare l'aggiornamento dell'applicazione. Errore: 0x800700c1

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

L'app non è stata avviata perché non è stato possibile caricarne l'assembly ( *.dll*).

Questo errore si verifica quando è presente una mancata corrispondenza del numero di bit tra l'app pubblicata e il processo w3wp/iisexpress.

Verificare che l'impostazione del pool di app a 32 bit sia corretta:

1. Selezionare il pool di app in **Pool di applicazioni** di Gestione IIS.
1. Selezionare **Impostazioni avanzate** in **Modifica pool di applicazioni** nel pannello **Azioni**.
1. Impostare **Attiva applicazioni a 32 bit**:
   * Se si sta sviluppando un'app a 32 bit (x86), impostare il valore su `True`.
   * Se si sta sviluppando un'app a 64 bit (x64), impostare il valore su `False`.

### <a name="connection-reset"></a>Reimpostazione della connessione

Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore. Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta. Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client. La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.

## <a name="default-startup-limits"></a>Limiti di avvio predefiniti

Il modulo ASP.NET Core è configurato con un valore predefinito *startupTimeLimit* di 120 secondi. Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo. Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="enable-the-aspnet-core-module-debug-log"></a>Abilitare il log di debug del modulo ASP.NET Core

Aggiungere le impostazioni del gestore seguenti al file dell'app *web.config* per abilitare i log di debug del modulo ASP.NET Core:

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Accedere al log eventi dell'applicazione:

1. Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.
1. In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.
1. Selezionare **Applicazione** per aprire il log eventi dell'applicazione.
1. Cercare gli errori associati all'app in cui si è verificato il problema. Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.

### <a name="run-the-app-at-a-command-prompt"></a>Eseguire l'app da un prompt dei comandi

Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione. È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.

#### <a name="framework-dependent-deployment"></a>Distribuzione dipendente dal framework

Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*. Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.
1. L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel. Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`. Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.

#### <a name="self-contained-deployment"></a>Distribuzione autonoma

Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app. Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.
1. L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel. Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`. Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.

### <a name="aspnet-core-module-stdout-log"></a>Log stdout del modulo ASP.NET Core

Per abilitare e visualizzare i log stdout:

1. Passare alla cartella di distribuzione del sito nel sistema host.
1. Se la cartella *logs* non è presente, creare la cartella. Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).
1. Modificare il file *web.config*. Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`. `stdout` nel percorso è il prefisso del nome del file di log. Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log. Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.
1. Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.
1. Salvare il file *web.config* aggiornato.
1. Effettuare una richiesta all'app.
1. Passare alla cartella *logs*. Trovare e aprire il log stdout più recente.
1. Esaminare il log per verificare se sono presenti errori.

> [!IMPORTANT]
> Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.

1. Modificare il file *web.config*.
1. Impostare **stdoutLogEnabled** su `false`.
1. Salvare il file.

> [!WARNING]
> La mancata disabilitazione del log stdout può causare un errore dell'app o del server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.
>
> Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enable-the-developer-exception-page"></a>Abilitare la pagina delle eccezioni per gli sviluppatori

La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo. Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet. Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*. Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errori di avvio comuni

Vedere <xref:host-and-deploy/azure-iis-errors-reference>. Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.

## <a name="obtain-data-from-an-app"></a>Ottenere dati da un'app

Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale. Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.

## <a name="create-a-dump"></a>Creare un dump

Un *dump* è uno snapshot della memoria del sistema e può essere utile per determinare la causa di un arresto anomalo dell'app, un errore di avvio o un'app lenta.

### <a name="app-crashes-or-encounters-an-exception"></a>Arresto anomalo o eccezione di un'app

Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):

1. Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`. Il pool di app deve avere accesso in scrittura alla cartella.
1. Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):
   * Se l'app usa il [modello di hosting in-process](xref:fundamentals/servers/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * Se l'app usa il [modello di hosting out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. Eseguire l'app nelle condizioni che causano l'arresto anomalo.
1. Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):
   * Se l'app usa il [modello di hosting in-process](xref:fundamentals/servers/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:

     ```console
     .\DisableDumps w3wp.exe
     ```

   * Se l'app usa il [modello di hosting out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:

     ```console
     .\DisableDumps dotnet.exe
     ```

Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente. Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.

> [!WARNING]
> I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a>L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente

Quando un'app si *blocca* (smette di rispondere ma senza arresto anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (File dump in modalità utente: scelta dello strumento migliore) per selezionare uno strumento appropriato per produrre il dump.

### <a name="analyze-the-dump"></a>Analizzare il dump

È possibile analizzare un dump usando diversi approcci. Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).

## <a name="remote-debugging"></a>Debug remoto

Vedere [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) (Eseguire il debug remoto di ASP.NET Core in un computer remoto con IIS in Visual Studio 2017) nella documentazione di Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) fornisce dati di telemetria dalle app ospitate da IIS, inclusi gli errori di registrazione e le funzionalità di creazione di report. Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app. Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-advice"></a>Suggerimenti aggiuntivi

Talvolta in un'app funzionante si verifica un problema subito dopo l'aggiornamento di .NET Core SDK nelle versioni computer o pacchetto di sviluppo all'interno dell'app. In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali. La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:

1. Eliminare le cartelle *bin* e *obj*.
1. Cancellare le cache dei pacchetti in *%UserProfile%\\.nuget\\packages* e *%LocalAppData%\\Nuget\\v3-cache*.
1. Ripristinare e ricompilare il progetto.
1. Verificare che la distribuzione precedente nel server sia stata completamente eliminata prima di ridistribuire l'app.

> [!TIP]
> Un modo pratico per cancellare le cache dei pacchetti consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.
>
> La cancellazione delle cache dei pacchetti può anche essere eseguita usando lo strumento [nuget.exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`. *nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
