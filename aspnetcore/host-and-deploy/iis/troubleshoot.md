---
title: Risolvere i problemi principali di ASP.NET in IIS
author: guardrex
description: Come diagnosticare i problemi con le distribuzioni di Internet Information Services (IIS) di App ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e44892d2022ca1a176cee9d027e220e196c6572d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Risolvere i problemi principali di ASP.NET in IIS

Di [Luke Latham](https://github.com/guardrex)

In questo articolo vengono fornite istruzioni su come diagnosticare un ASP.NET Core problema avvio app durante l'hosting con [Internet Information Services (IIS)](/iis). Le informazioni in questo articolo si applicano all'hosting in IIS su Windows Server e Windows Desktop.

In Visual Studio, impostazione predefinita un progetto ASP.NET Core [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting durante il debug. Oggetto *502.5 errore del processo* che si verifica quando il debug in locale può essere risolverli utilizzando le indicazioni riportate in questo argomento.

Altri argomenti sulla risoluzione dei problemi:

[Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)  
Anche se viene utilizzato il servizio App di [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) e IIS per le applicazioni host, vedere l'argomento dedicato per istruzioni specifiche di servizio App.

[Gestione degli errori](xref:fundamentals/error-handling)  
Informazioni su come gestire gli errori nelle applicazioni ASP.NET Core durante lo sviluppo in un sistema locale.

[Informazioni sul debug tramite Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger)  
In questo argomento vengono presentate le funzionalità del debugger di Visual Studio.

## <a name="app-startup-errors"></a>Errori di avvio delle App

**502.5 Errore processo**  
Il processo di lavoro ha esito negativo. Non si avvia l'app.

Il modulo di base di ASP.NET tenta di avviare il processo di lavoro, ma non viene avviato. Può determinare la causa di un errore di avvio del processo in genere dalle voci nella [registro eventi dell'applicazione](#application-event-log) e [log di stdout ASP.NET Core modulo](#aspnet-core-module-stdout-log).

Il *502.5 errore del processo* pagina di errore viene restituito quando un errore di configurazione di hosting o app fa sì che il processo di lavoro esito negativo:

![Visualizzazione della pagina di errore del processo 502.5 finestra del browser](troubleshoot/_static/process-failure-page.png)

**500 Errore interno del Server**  
Avvio dell'app, ma un errore impedisce al server di soddisfare la richiesta.

Questo errore si verifica all'interno del codice dell'app durante l'avvio o durante la creazione di una risposta. La risposta non può contenere alcun contenuto o la risposta potrebbe essere visualizzato come un *500 Internal Server Error* nel browser. Registro eventi dell'applicazione in genere indica che l'app è stato avviato normalmente. Dalla prospettiva del server, che non è corretto. L'app è stato avviato, ma non può generare una risposta valida. [Eseguire l'app al prompt dei comandi](#run-the-app-at-a-command-prompt) nel server o [abilitare il log di stdout ASP.NET Core modulo](#aspnet-core-module-stdout-log) per risolvere il problema.

**Reimpostazione della connessione**

Se si verifica un errore dopo le intestazioni vengono inviate, è troppo tardi per il server inviare un **500 Internal Server Error** quando si verifica un errore. Ciò accade spesso quando si verifica un errore durante la serializzazione di oggetti complessi, di una risposta. Questo tipo di errore viene visualizzato come un *reimpostazione della connessione* errore nel client. [Registrazione applicazioni](xref:fundamentals/logging/index) può consentire di risolvere questi tipi di errori.

## <a name="default-startup-limits"></a>Limiti di avvio predefiniti

Il modulo di base di ASP.NET è configurato con un valore predefinito *startupTimeLimit* di 120 secondi. Quando si mantiene il valore predefinito, un'app potrebbe richiedere fino a due minuti per l'avvio prima che il modulo Registra un errore del processo. Per informazioni sulla configurazione del modulo, vedere [attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

## <a name="troubleshoot-app-startup-errors"></a>Risolvere gli errori di avvio delle app

### <a name="application-event-log"></a>Registro eventi dell'applicazione

Il registro eventi applicazioni di accesso:

1. Aprire il menu Start, cercare **Visualizzatore eventi**, quindi selezionare il **Visualizzatore eventi** app.
1. In **Visualizzatore eventi**, aprire il **i registri di Windows** nodo.
1. Selezionare **applicazione** per aprire il registro eventi dell'applicazione.
1. Cercare gli errori associati all'app di esito negativo. Gli errori presentano un valore di *modulo AspNetCore IIS* o *modulo di IIS Express AspNetCore* nel *origine* colonna.

### <a name="run-the-app-at-a-command-prompt"></a>Eseguire l'app a un prompt dei comandi

Molti errori di avvio non producano informazioni utili nel registro eventi dell'applicazione. È possibile individuare la causa di determinati errori eseguendo l'app a un prompt dei comandi nel sistema host.

**Distribuzione dipendenti dal framework**

Se l'applicazione è un [distribuzione dipendenti dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):

1. Al prompt dei comandi, passare alla cartella di distribuzione ed eseguire l'app tramite l'esecuzione di assembly dell'applicazione con *dotnet.exe*. Il comando seguente, sostituire il nome dell'assembly dell'applicazione per \<nome_assembly >: `dotnet .\<assembly_name>.dll`.
1. L'output dall'app, che mostra gli eventuali errori, della console verrà scritti nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta per l'app, effettuare una richiesta per l'host e la porta in cui è in ascolto Kestrel. Tramite l'host predefinito e post, effettuare una richiesta per `http://localhost:5000/`. Se l'applicazione risponde in genere in corrispondenza dell'indirizzo endpoint Kestrel, il problema è probabilmente correlate alla configurazione del proxy inverso e meno probabile che all'interno dell'app.

**Distribuzione autonoma**

Se l'applicazione è un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd):

1. Al prompt dei comandi, passare alla cartella di distribuzione ed eseguire file eseguibile dell'applicazione. Il comando seguente, sostituire il nome dell'assembly dell'applicazione per \<nome_assembly >: `<assembly_name>.exe`.
1. L'output dall'app, che mostra gli eventuali errori, della console verrà scritti nella finestra della console.
1. Se gli errori si verificano quando si effettua una richiesta per l'app, effettuare una richiesta per l'host e la porta in cui è in ascolto Kestrel. Tramite l'host predefinito e post, effettuare una richiesta per `http://localhost:5000/`. Se l'applicazione risponde in genere in corrispondenza dell'indirizzo endpoint Kestrel, il problema è probabilmente correlate alla configurazione del proxy inverso e meno probabile che all'interno dell'app.

### <a name="aspnet-core-module-stdout-log"></a>Log di stdout ASP.NET modulo Core

Per abilitare e visualizzare i log di stdout:

1. Passare alla cartella di distribuzione del sito nel sistema host.
1. Se il *registri* cartella non è presente, creare la cartella. Per istruzioni sull'abilitazione di MSBuild per creare il *registri* cartella nella distribuzione automaticamente, vedere il [struttura di Directory](xref:host-and-deploy/directory-structure) argomento.
1. Modificare il *Web. config* file. Impostare **stdoutLogEnabled** a `true` e modificare il **stdoutLogFile** percorso affinché faccia riferimento al *registri* cartella (ad esempio, `.\logs\stdout`). `stdout` nel percorso è il prefisso di nome file di log. Un timestamp, id di processo e l'estensione di file vengono aggiunti automaticamente quando il log viene creato. Utilizzando `stdout` denominato come il prefisso del nome file, un file di log tipici *stdout_20180205184032_5412.log*. 
1. Salvare l'aggiornamento *Web. config* file.
1. Effettuare una richiesta per l'app.
1. Passare il *registri* cartella. Trovare e aprire il log di stdout più recente.
1. Esaminare il log degli errori.

**Importante**: Disabilitare la registrazione di stdout quando la risoluzione dei problemi è stata completata.

1. Modificare il *Web. config* file.
1. Impostare **stdoutLogEnabled** a `false`.
1. Salvare il file.

> [!WARNING]
> Mancata possibilità di disabilitazione del log di stdout può causare un errore di applicazione o server. Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.
>
> Per la routine registrazione in un'applicazione ASP.NET di base, utilizzare una libreria di registrazione che limita dimensioni file di log e ruota i log. Per ulteriori informazioni, vedere [provider di log di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).

## <a name="enabling-the-developer-exception-page"></a>Abilitare la pagina di eccezione per sviluppatori

Il `ASPNETCORE_ENVIRONMENT` [variabile di ambiente può essere aggiunto a Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo. Purché l'ambiente non è sottoposto a override in avvio dell'app da `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di [pagina eccezione Developer](xref:fundamentals/error-handling) da visualizzare quando si esegue l'app.

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

Variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'utilizzo di gestione temporanea e il test di server che non sono esposti a Internet. Rimuovere la variabile di ambiente dal *Web. config* file dopo la risoluzione dei problemi. Per informazioni sull'impostazione delle variabili di ambiente in *Web. config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

## <a name="common-startup-errors"></a>Errori più comuni di avvio 

Vedere il [riferimento per gli errori comune ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference). La maggior parte dei problemi comuni che impediscono l'avvio dell'app sono descritte nell'argomento di riferimento.

## <a name="slow-or-hanging-app"></a>App lento o bloccati

Quando un'app risponde lentamente o si blocca una richiesta, ottenere e analizzare un [file di dump](/visualstudio/debugger/using-dump-files). File di dump possono essere ottenuti utilizzando uno degli strumenti seguenti:

* [ProcDump](/sysinternals/downloads/procdump)
* [DebugDiag](https://www.microsoft.com/download/details.aspx?id=49924)
* WinDbg: [scaricare strumenti di debug per Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [debug WinDbg utilizzando](/windows-hardware/drivers/debugger/debugging-using-windbg)

## <a name="remote-debugging"></a>Debug remoto

Vedere [remoto il Debug di ASP.NET Core in un Computer remoto di IIS in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) nella documentazione di Visual Studio.

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/) fornisce dati di telemetria da app ospitate da IIS, inclusi errori di registrazione e le funzionalità di reporting. Application Insights può segnalare solo in caso di errori che si verifica dopo l'avvio dell'app quando diventano disponibili le funzionalità di registrazione dell'app. Per ulteriori informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="additional-troubleshooting-advice"></a>Suggerimenti sulla risoluzione dei problemi aggiuntive

In alcuni casi un'applicazione funziona ha esito negativo immediatamente dopo l'aggiornamento il SDK di base .NET nelle versioni di computer o un pacchetto di sviluppo all'interno dell'app. In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali. La maggior parte di questi problemi può essere risolto seguendo queste istruzioni:

1. Eliminare il *bin* e *obj* cartelle.
1. Cancella la cache il pacchetto *% UserProfile %\\opportunità\\pacchetti* e *% LocalAppData %\\Nuget\\v3 cache*.
1. Ripristinare e ricompilare il progetto.
1. Verificare che nel server di distribuzione precedente è stata eliminata completamente prima di ridistribuire l'applicazione.

> [!TIP]
> Un modo pratico per cancellare le cache pacchetto consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.
> 
> Cancellazione delle cache dei pacchetti possono inoltre essere eseguita utilizzando il [nuget.exe](https://www.nuget.org/downloads) strumento e l'esecuzione del comando `nuget locals all -clear`. *NuGet.exe* non è un'installazione in dotazione con il sistema operativo desktop di Windows e devono essere ottenuti separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione alla gestione degli errori in ASP.NET Core](xref:fundamentals/error-handling)
* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)
