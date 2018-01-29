---
title: Risolvere i problemi principali di ASP.NET in IIS
author: guardrex
description: Come diagnosticare i problemi con le distribuzioni di IIS di App ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2e42d5904e2be8c031c5c6d4bbc104a251b699f2
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="troubleshoot-aspnet-core-on-iis"></a>Risolvere i problemi principali di ASP.NET in IIS

Di [Luke Latham](https://github.com/guardrex)

Per diagnosticare i problemi relativi alle distribuzioni di IIS:

* Esaminare l'output del browser.
* Esaminare il **registro applicazioni** del sistema con il **Visualizzatore eventi**.
* Abilitare la registrazione `stdout`. Il registro **Modulo ASP.NET Core** si trova nel percorso specificato nell'attributo *stdoutLogFile* dell'elemento `<aspNetCore>` in *web.config*. Le cartelle nel percorso fornito nel valore dell'attributo devono essere presenti nella distribuzione. Impostare *stdoutLogEnabled* a `true`. App che usano l'il `Microsoft.NET.Sdk.Web` SDK per creare il *Web. config* file predefinito di *stdoutLogEnabled* impostando su `false`manualmente forniscono il *Web. config* file o modificare il file per permettere `stdout` registrazione.

Informazioni utilizzate da tali tre origini con il [argomento fanno riferimento a errori comuni](xref:host-and-deploy/azure-iis-errors-reference) per determinare il problema. Seguire i consigli sulla risoluzione dei problemi forniti per risolvere il problema.

Molti errori comuni non vengono visualizzati nel browser, nel registro applicazioni e nel registro del modulo ASP.NET Core fino a quando non vengono superate le impostazioni *startupTimeLimit* (impostazione predefinita: 120 secondi) e *startupRetryCount* (impostazione predefinita: 2) del modulo. Attendere almeno sei minuti prima di dedurre che il modulo non è riuscito ad avviare un processo per l'applicazione.

Un modo veloce per determinare se l'app funziona correttamente è l'esecuzione dell'app direttamente su Kestrel. Se l'app è stato pubblicato come un [framework dipendente distribuzione](/dotnet/core/deploying/#framework-dependent-deployments-fdd), eseguire `dotnet <assembly_name>.dll` nella cartella di distribuzione, ovvero il percorso fisico di IIS per l'app. Se l'app è stato pubblicato come un [distribuzione indipendente](/dotnet/core/deploying/#self-contained-deployments-scd), eseguire l'app dell'eseguibile direttamente da un prompt dei comandi, `<assembly_name>.exe`, nella cartella di distribuzione. Se è in ascolto sulla porta predefinita 5000 Kestrel, l'app deve essere disponibile all'indirizzo `http://localhost:5000/`. Se l'applicazione risponde in genere in corrispondenza dell'indirizzo endpoint Kestrel, il problema è probabilmente correlate alla configurazione del proxy inverso e meno probabile che all'interno dell'app.

È un modo per determinare se il proxy inverso funziona correttamente per eseguire una richiesta semplice file statici per un foglio di stile, script o l'immagine dal file dell'app in *wwwroot* utilizzando [Middleware File statici](xref:fundamentals/static-files). Se l'app può essere utilizzato un file statico, ma le visualizzazioni MVC e altri endpoint hanno esito negativo, il problema è meno probabile che correlati alla configurazione del proxy inverso e più probabile che all'interno dell'app (ad esempio, il routing MVC o 500 Internal Server Error).

Quando viene avviato normalmente Kestrel dietro IIS, ma l'app non viene eseguito nel sistema dopo aver correttamente eseguito localmente, è possibile aggiungere temporaneamente una variabile di ambiente per *Web. config* per impostare il `ASPNETCORE_ENVIRONMENT` a `Development`. Variabile di ambiente consente fino a quando l'ambiente non è sottoposto a override in avvio dell'app, il [pagina eccezione developer](xref:fundamentals/error-handling) da visualizzare quando si esegue l'app. Variabile di ambiente per `ASPNETCORE_ENVIRONMENT` in questo modo è consigliata solo per i server di gestione temporanea e il test non sono esposti a Internet. Assicurarsi di rimuovere la variabile di ambiente dal *Web. config* al termine del file. Per informazioni sull'impostazione delle variabili di ambiente tramite *Web. config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

Nella maggior parte dei casi l'abilitazione della registrazione dell'applicazione è utile per risolvere i problemi dell'applicazione o del proxy inverso. Per altre informazioni, vedere [Registrazione](xref:fundamentals/logging/index).

L'ultimo suggerimento di risoluzione dei problemi relativo alle App che non vengono eseguiti dopo l'aggiornamento di .NET Core SDK nelle versioni di computer o un pacchetto di sviluppo all'interno dell'app. In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali. La maggior parte di questi problemi possono essere risolti automaticamente:

* L'eliminazione di `bin` e `obj` cartelle nel progetto.
* Memorizza nella cache il pacchetto di cancellazione in `%UserProfile%\.nuget\packages\` e `%LocalAppData%\Nuget\v3-cache`.
* Il ripristino e ricompilare il progetto.
* Conferma che nel server di distribuzione precedente è stato completamente eliminata prima di distribuire nuovamente l'app.

> [!TIP]
> Un modo pratico per cancellare le cache pacchetto consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.
> 
> Cancellazione delle cache dei pacchetti possono inoltre essere eseguita utilizzando il [nuget.exe](https://www.nuget.org/downloads) strumento e l'esecuzione del comando `nuget locals all -clear`. *NuGet.exe* non è un'installazione in dotazione con Windows 10 e deve essere ottenuto separatamente dal sito Web NuGet.
<!--
> [!TIP]
> A convenient way to clear package caches is to:
>
> * Obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/).
> * Add the path to *NuGet.exe* to the system PATH.
> * Execute `nuget locals all -clear` from a command prompt.
>
> Alternatively, execute `dotnet nuget locals all --clear` from a command prompt without obtaining *NuGet.exe*. -->

## <a name="additional-resources"></a>Risorse aggiuntive

* [Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
