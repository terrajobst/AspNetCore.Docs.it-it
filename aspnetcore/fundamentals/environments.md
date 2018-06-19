---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni sul supporto di ASP.NET Core per il controllo del comportamento delle app in ambienti diversi.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33840957"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Usare più ambienti in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core offre supporto per l'impostazione del comportamento dell'applicazione in fase di runtime mediante variabili di ambiente.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ambienti

ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'applicazione e archivia tale valore in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` può essere impostata su qualsiasi valore, ma il framework supporta [tre valori](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) e [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Se `ASPNETCORE_ENVIRONMENT` non è impostata, assume come impostazione predefinita `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Il codice precedente:

* Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando `ASPNETCORE_ENVIRONMENT` è impostata su `Development`.
* Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:

    * `Staging`
    * `Production`
    * `Staging_2`

L'[helper tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Nota: in Windows e macOS, le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole. In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.

### <a name="development"></a>Sviluppo

L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione. Ad esempio i modelli ASP.NET Core abilitano la [pagina di eccezione per sviluppatori](xref:fundamentals/error-handling#the-developer-exception-page) nell'ambiente di sviluppo.

L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto. I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.

Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server. Usare un punto e virgola tra gli URL dell'elenco:
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

Quando l'applicazione viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`. Il valore di `commandName` specifica il server Web da avviare. `commandName` può essere uno dei valori seguenti:

* IIS Express
* IIS
* Project (che avvia Kestrel)

Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):

* *launchSettings.json*, se disponibile, viene letto. Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.
* Viene visualizzato l'ambiente host.


L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

La scheda **Debug** di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web. Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.

>[!WARNING]
> Evitare di archiviare segreti in *launchSettings.json*. Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).

### <a name="production"></a>Produzione

L'ambiente di produzione deve essere configurato per ottimizzare la sicurezza, le prestazioni e affidabilità dell'applicazione. Alcune impostazioni comuni che differiscono dallo sviluppo includono:

* Memorizzazione nella cache.
* Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.
* Pagine di errore di diagnostica disabilitate.
* Pagine di errore descrittive abilitate.
* Registrazione e monitoraggio della produzione abilitati. Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Impostazione dell'ambiente

Spesso risulta utile impostare un ambiente specifico per i test. Se l'ambiente non è impostato, l'impostazione predefinita è `Production`, che disabilita la maggior parte delle funzionalità di debug.

Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.

### <a name="azure"></a>Azure

Per il Servizio app di Azure:

* Selezionare il pannello **Impostazioni applicazione**.
* Aggiungere la chiave e il valore in **Impostazioni App**.


### <a name="windows"></a>WINDOWS
Per impostare `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run) vengono usati i seguenti comandi:

**Riga di comando**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Questi comandi hanno effetto solo per la finestra corrente. Quando la finestra viene chiusa l'impostazione ASPNETCORE_ENVIRONMENT torna all'impostazione predefinita o al valore del computer predefinito. Per impostare il valore a livello globale in Windows aprire il **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`.

![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Vedere la sezione *Setting environment variables* (Impostazione di variabili di ambiente) dell'argomento [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) (Guida di riferimento per la configurazione del modulo ASP.NET Core).

**Pool di applicazioni IIS singoli**

Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolati (supportati in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS
L'impostazione dell'ambiente corrente per macOS può essere effettuata in linea durante l'esecuzione dell'applicazione

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
oppure usando `export` per impostare l'ambiente prima di eseguire l'app.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Le variabili di ambiente di livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*. Modificare il file con un editor di testo e aggiungere l'istruzione seguente.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Per le distribuzioni di Linux, usare il comando `export` della riga di comando per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente di livello computer.

### <a name="configuration-by-environment"></a>Configurazione per ambiente

Per altre informazioni, vedere [Configurazione per ambiente](xref:fundamentals/configuration/index#configuration-by-environment).

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Classe Startup e metodi basati sull'ambiente

Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app. Se esiste una classe `Startup{EnvironmentName}` tale classe viene chiamata per `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Nota: la chiamata di [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) sostituisce le sezioni di configurazione.

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) supportano versioni specifiche per l'ambiente con i formati `Configure{EnvironmentName}` e `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Configurazione](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
