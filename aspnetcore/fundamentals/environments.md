---
title: "Utilizzo di più ambienti in ASP.NET Core"
author: rick-anderson
description: Scopri come ASP.NET Core fornisce supporto per il controllo del comportamento dell'app in ambienti diversi.
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 83d1593d46761b1c00aa431cfdcde59cb3b28b65
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-multiple-environments"></a>Utilizzo di più ambienti

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fornisce supporto per l'impostazione di comportamento dell'applicazione in fase di esecuzione con le variabili di ambiente.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ambienti

ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'applicazione e archivia tale valore in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT`Impostare su qualsiasi valore, ma [tre valori](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) sono supportate dal framework: [sviluppo](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [gestione temporanea](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), e [produzione](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Se `ASPNETCORE_ENVIRONMENT` non è impostata, verrà aperta `Production`.

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

Il codice precedente:

* Chiamate [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.
* Chiamate [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando il valore di `ASPNETCORE_ENVIRONMENT` è impostato uno dei seguenti:

    * `Staging`
    * `Production`
    * `Staging_2`

Il [Helper di Tag di ambiente ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) utilizza il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere markup nell'elemento:

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

Nota: In Windows e Mac OS, valori e variabili di ambiente non sono tra maiuscole e minuscole. Le variabili di ambiente Linux e i valori sono **tra maiuscole e minuscole** per impostazione predefinita.

### <a name="development"></a>Sviluppo

L'ambiente di sviluppo è possibile abilitare le funzionalità che non devono essere esposti nell'ambiente di produzione. Ad esempio, i modelli ASP.NET Core abilitano il [pagina eccezione developer](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.

L'ambiente per lo sviluppo di computer locale può essere impostata nella *Properties\launchSettings.json* file del progetto. Impostare i valori di ambiente in *launchSettings.json* sovrascrivono i valori impostati nell'ambiente di sistema.

Il codice XML seguente mostra tre profili da un *launchSettings.json* file:

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

Quando viene avviata l'applicazione con `dotnet run`, il primo profilo con `"commandName": "Project"` verrà utilizzato. Il valore di `commandName` specifica il server web da avviare. `commandName`può essere uno dei seguenti:

* IIS Express
* IIS
* Progetto (che avvia Kestrel)

Quando viene avviata un'app con `dotnet run`:

* *launchSettings.json* è di lettura, se disponibile. `environmentVariables`le impostazioni in *launchSettings.json* eseguire l'override delle variabili di ambiente.
* Viene visualizzato l'ambiente di hosting.


L'output seguente mostra un'app introduttiva `dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **Debug** scheda fornisce un'interfaccia utente grafica per modificare il *launchSettings.json* file:

![Variabili di ambiente di impostazione delle proprietà del progetto](environments/_static/project-properties-debug.png)

Le modifiche apportate ai profili di progetto non abbiano effetto, è necessario riavviare il server web. È necessario riavviare kestrel rileverà le modifiche apportate al relativo ambiente.

>[!WARNING]
> *launchSettings.json* non devono essere archiviate informazioni riservate. Il [lo strumento Gestione segreto](xref:security/app-secrets) può essere utilizzato per archiviare i segreti per lo sviluppo locale.

### <a name="production"></a>Produzione

L'ambiente di produzione deve essere configurato per ottimizzare la sicurezza, prestazioni e affidabilità dell'applicazione. Alcune impostazioni comuni che potrebbe avere un ambiente di produzione che verrebbero differisce dallo sviluppo includono:

* La memorizzazione nella cache.
* Le risorse sul lato client sono inclusi, minimizzare e potenzialmente servite da una rete CDN.
* Pagine di errore diagnostico disabilitate.
* Pagine di errore descrittivo abilitate.
* Registrazione di produzione e di monitoraggio. Ad esempio, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).

## <a name="setting-the-environment"></a>Impostazione dell'ambiente

Spesso è utile impostare un ambiente di test specifico. Se l'ambiente non è impostata, verrà aperta `Production` che disabilita la maggior parte delle funzionalità di debug.

Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.

### <a name="azure"></a>Azure

Per il servizio app di Azure:

* Selezionare il **le impostazioni dell'applicazione** blade.
* Aggiungere la chiave e il valore **impostazioni App**.


### <a name="windows"></a>WINDOWS
Per impostare il `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite `dotnet run`, vengono utilizzati i seguenti comandi

**Riga di comando**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Questi comandi diventano effettive solo per la finestra corrente. Quando la finestra viene chiusa, l'impostazione ASPNETCORE_ENVIRONMENT verrà ripristinata l'impostazione predefinita o un valore di computer. Per impostare il valore globale all'apertura di Windows il **Pannello di controllo** > **sistema** > **impostazioni di sistema avanzate** e aggiungere o modificare il `ASPNETCORE_ENVIRONMENT` valore.

![Sistema di proprietà avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Vedere il *impostare variabili di ambiente* sezione la [riferimento di configurazione di ASP.NET Core modulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) argomento.

**Per ogni Pool di applicazioni IIS**

Per impostare le variabili di ambiente per le singole applicazioni in esecuzione nel pool di applicazioni isolate (supportata in IIS 10.0 +), vedere il *comando AppCmd.exe* sezione la [le variabili di ambiente \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) argomento.

### <a name="macos"></a>macOS
L'impostazione l'ambiente corrente per macOS può essere effettuata in linea, durante l'esecuzione dell'applicazione.

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
o tramite `export` impostarlo prima di eseguire l'app.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Le variabili di ambiente di livello del computer sono impostate *.bashrc* o *.bash_profile* file. Modificare il file utilizzando un editor di testo e aggiungere l'istruzione seguente.

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Per le distribuzioni di Linux, usare il `export` comando dalla riga di comando per sessione in base a impostazioni delle variabili e *bash_profile* file per le impostazioni di ambiente di livello computer.

### <a name="configuration-by-environment"></a>Configurazione per ambiente

Vedere [configurazione dall'ambiente](xref:fundamentals/configuration/index#configuration-by-environment) per ulteriori informazioni.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Ambiente basato su metodi e classe di avvio

Quando viene avviata un'applicazione ASP.NET di base, il [classe Startup](xref:fundamentals/startup) avvia l'app. Se una classe `Startup{EnvironmentName}` esista, che verrà chiamata per tale classe `EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Nota: La chiamata [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) esegue l'override delle sezioni di configurazione.

[Configurare](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) supporta versioni specifiche dell'ambiente del form `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Configurazione](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
