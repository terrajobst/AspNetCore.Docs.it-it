---
title: Risolvere i problemi di ASP.NET Core in IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni Internet Information Services (IIS) di app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2b23bf8230f7a1c207ef7870da098ffb0c597fd5
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225447"
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

<xref:host-and-deploy/azure-apps/troubleshoot>  
Anche se Servizio app usa il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e IIS per ospitare le app, vedere l'argomento dedicato per istruzioni specifiche su Servizio app.

<xref:fundamentals/error-handling>  
Informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.

[Informazioni sul debug tramite Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
Questo argomento presenta le funzionalità del debugger di Visual Studio.

[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) (Debug con Visual Studio Code)  
Informazioni sul supporto del debug incorporato in Visual Studio Code.

## <a name="app-startup-errors"></a>Errori di avvio delle app

**502.5 Errore del processo**  
Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare il processo dotnet back-end, ma l'avvio non riesce. In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log). 

Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente. Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.

La pagina di errore *502.5 Errore del processo* viene restituita quando il processo di lavoro ha esito negativo a causa di un errore di configurazione dell'hosting o dell'app:

![Finestra del browser con la pagina 502.5 Errore del processo](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

**500.30 Errore di avvio In-Process**

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core prova ad avviare .NET Core CLR In-Process, ma l'avvio non riesce. In genere è possibile determinare la causa di un errore di avvio del processo dalle voci nel [log eventi dell'applicazione](#application-event-log) e nel [log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log). 

Una condizione di errore comune è dovuta alla configurazione non corretta dell'app perché come destinazione viene specificata una versione del framework condiviso ASP.NET Core, che non è presente. Controllare le versioni del framework condiviso ASP.NET Core installate nel computer di destinazione.

**500.0 Errore di caricamento del gestore In-Process**

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core non riesce a trovare .NET Core CLR e trova il gestore richieste In-Process (*aspnetcorev2_inprocess.dll*). Controllare che:

* L'app specifichi come destinazione il pacchetto NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) o il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Le versione del framework condiviso ASP.NET Core specificata come destinazione dall'app sia installata nel computer di destinazione.

**500.0 Errore di caricamento del gestore out-of-process**

Il processo di lavoro ha esito negativo. L'app non viene avviata.

Il modulo ASP.NET Core non riesce a trovare il gestore richieste di hosting out-of-process. Verificare che *aspnetcorev2_outofprocess.dll* sia presente in una sottocartella accanto ad *aspnetcorev2.dll*. 

::: moniker-end

**500 Errore interno del server**  
L'app viene avviata, ma un errore impedisce al server di soddisfare la richiesta.

Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta. La risposta potrebbe non avere alcun contenuto o essere visualizzata nel browser come un codice *500 Errore interno del server*. Il log eventi dell'applicazione in genere indica che l'app è stata avviata normalmente. Dal punto di vista del server, questo è corretto. L'app è stata effettivamente avviata, ma non può generare una risposta valida. Per risolvere il problema, [eseguire l'app dal prompt dei comandi](#run-the-app-at-a-command-prompt) nel server o [abilitare il log stdout del modulo ASP.NET Core](#aspnet-core-module-stdout-log).

**Reimpostazione della connessione**

Se si verifica un errore dopo l'invio delle intestazioni, è troppo tardi perché il server possa inviare un codice **500 Errore interno del server** quando si verifica un errore. Questo spesso accade quando si verifica un errore durante la serializzazione di oggetti complessi per una risposta. Questo tipo di errore viene visualizzato come un errore di *reimpostazione della connessione* nel client. La [registrazione delle applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.

## <a name="default-startup-limits"></a>Limiti di avvio predefiniti

Il modulo ASP.NET Core è configurato con un valore predefinito *startupTimeLimit* di 120 secondi. Quando si mantiene il valore predefinito, l'avvio di un'app potrebbe richiedere fino a due minuti prima che il modulo registri un errore del processo. Per informazioni sulla configurazione del modulo, vedere [Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Accedere al log eventi dell'applicazione:

1. Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.
1. In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.
1. Selezionare **Applicazione** per aprire il log eventi dell'applicazione.
1. Cercare gli errori associati all'app in cui si è verificato il problema. Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.

### <a name="run-the-app-at-a-command-prompt"></a>Eseguire l'app da un prompt dei comandi

Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione. È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.

**Distribuzione dipendente dal framework**

Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*. Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.
1. L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel. Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`. Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione del proxy inverso e meno probabile che sia interno all'app.

**Distribuzione autonoma**

Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app. Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.
1. L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel. Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`. Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione del proxy inverso e meno probabile che sia interno all'app.

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

**Importante**: Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.

1. Modificare il file *web.config*.
1. Impostare **stdoutLogEnabled** su `false`.
1. Salvare il file.

> [!WARNING]
> La mancata disabilitazione del log stdout può causare un errore dell'app o del server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.
>
> Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione. Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Abilitazione della pagina delle eccezioni per gli sviluppatori

La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo. Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.

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

L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet. Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*. Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errori di avvio comuni 

Vedere <xref:host-and-deploy/azure-iis-errors-reference>. Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.

## <a name="slow-or-hanging-app"></a>App lenta o bloccata

Quando un'app risponde lentamente o si blocca durante una richiesta, ottenere e analizzare un [file dump](/visualstudio/debugger/using-dump-files). I file dump possono essere ottenuti usando uno degli strumenti seguenti:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [download degli strumenti di debug per Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [debug tramite WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Debug remoto

Vedere [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) (Eseguire il debug remoto di ASP.NET Core in un computer remoto con IIS in Visual Studio 2017) nella documentazione di Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) fornisce dati di telemetria dalle app ospitate da IIS, inclusi gli errori di registrazione e le funzionalità di creazione di report. Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app. Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Altre indicazioni sulla risoluzione dei problemi

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

* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
