---
title: Usare più ambienti in ASP.NET Core
author: rick-anderson
description: Informazioni su come controllare il comportamento di app ASP.NET Core in più ambienti.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2019
uid: fundamentals/environments
ms.openlocfilehash: a0e6d62f352a886a9bc051813a21d94c1605a1ce
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087039"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Usare più ambienti in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core configura il comportamento dell'app in base all'ambiente di runtime e tramite una variabile di ambiente.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Ambienti

ASP.NET Core legge la variabile di ambiente `ASPNETCORE_ENVIRONMENT` all'avvio dell'app e ne archivia il valore in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). La variabile `ASPNETCORE_ENVIRONMENT` può essere impostata su qualsiasi valore, ma il framework supporta [tre valori](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) e [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Se la variabile `ASPNETCORE_ENVIRONMENT` non viene impostata, il valore predefinito è `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Il codice precedente:

* Chiama [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) quando `ASPNETCORE_ENVIRONMENT` è impostato su `Development`.
* Chiama [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) quando il valore di `ASPNETCORE_ENVIRONMENT` è uno dei seguenti:

  * `Staging`
  * `Production`
  * `Staging_2`

L'[helper per tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) usa il valore di `IHostingEnvironment.EnvironmentName` per includere o escludere il markup nell'elemento:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

In Windows e macOS le variabili di ambiente e i relativi valori non applicano la distinzione tra maiuscole e minuscole. In Linux le variabili di ambiente e i relativi valori **applicano la distinzione tra maiuscole e minuscole** per impostazione predefinita.

### <a name="development"></a>Sviluppo

L'ambiente di sviluppo può abilitare funzionalità che non devono essere esposte nell'ambiente di produzione. Ad esempio i modelli ASP.NET Core abilitano la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling#developer-exception-page) nell'ambiente di sviluppo.

L'ambiente di sviluppo computer locale può essere impostato nel file *Properties\launchSettings.json* del progetto. I valori di ambiente in *launchSettings.json* sostituiscono i valori impostati nell'ambiente di sistema.

Il codice JSON seguente visualizza tre profili di un file *launchSettings.json*:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> La proprietà `applicationUrl` in *launchSettings.json* può specificare un elenco di URL di server. Usare un punto e virgola tra gli URL dell'elenco:
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

Quando l'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run) viene usato il primo profilo con `"commandName": "Project"`. Il valore di `commandName` specifica il server Web da avviare. `commandName` può avere uno dei valori seguenti:

* `IISExpress`
* `IIS`
* `Project` (che avvia Kestrel)

Quando un'app viene avviata con [dotnet run](/dotnet/core/tools/dotnet-run):

* *launchSettings.json*, se disponibile, viene letto. Le impostazioni `environmentVariables` in *launchSettings.json* sostituiscono le variabili di ambiente.
* Viene visualizzato l'ambiente host.

L'output seguente visualizza un'app avviata con [dotnet run](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

La scheda **Debug** nelle proprietà di progetto di Visual Studio visualizza una GUI per la modifica del file *launchSettings.json*:

![Proprietà progetto - Impostazione delle variabili di ambiente](environments/_static/project-properties-debug.png)

È possibile che le modifiche apportate ai profili di progetto abbiano effetto solo dopo il riavvio del server Web. Perché Kestrel rilevi le modifiche apportate al suo ambiente, deve essere riavviato.

> [!WARNING]
> Evitare di archiviare segreti in *launchSettings.json*. Per l'archiviazione di segreti per lo sviluppo locale, usare lo [strumento Secret Manager](xref:security/app-secrets).

Quando si usa [Visual Studio Code](https://code.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *.vscode/launch.json*. Nell'esempio seguente l'ambiente viene impostato su `Development`:

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

Un file *.vscode/launch.json* del progetto non viene letto quando si avvia l'app con `dotnet run` come accade per il file *Properties/launchSettings.json*. Quando si avvia un'app in un ambiente di sviluppo che non ha un file *launchSettings.json*, impostare una variabile di ambiente o un argomento della riga di comando sul comando `dotnet run`.

### <a name="production"></a>Produzione

È necessario che l'ambiente di produzione sia configurato per ottimizzare la sicurezza, le prestazioni e l'affidabilità dell'app. Alcune impostazioni comuni che differiscono dallo sviluppo includono:

* Memorizzazione nella cache.
* Risorse lato client in bundle, minimizzate e potenzialmente offerte da una rete CDN.
* Pagine di errore di diagnostica disabilitate.
* Pagine di errore descrittive abilitate.
* Registrazione e monitoraggio della produzione abilitati. Ad esempio [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Impostare l'ambiente

Spesso risulta utile impostare un ambiente specifico per i test. Se l'ambiente non viene impostato, il valore predefinito è `Production`, che disabilita la maggior parte delle funzionalità di debug. Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.

### <a name="azure-app-service"></a>Servizio app di Azure

Per impostare l'ambiente in [Servizio app di Azure](https://azure.microsoft.com/services/app-service/), attenersi alla procedura seguente:

1. Selezionare l'app dal pannello **Servizi app**.
1. Nel gruppo **IMPOSTAZIONI** selezionare il pannello **Impostazioni dell'applicazione**.
1. Nell'area **Impostazioni dell'applicazione** selezionare **Aggiungi nuova impostazione**.
1. In **Immettere un nome** specificare `ASPNETCORE_ENVIRONMENT`. In **Immettere un valore** specificare l'ambiente (ad esempio, `Staging`).
1. Selezionare la casella di controllo **Impostazione slot** per lasciare invariata l'impostazione dell'ambiente sullo slot corrente quando gli slot di distribuzione vengono scambiati. Per altre informazioni, vedere [Documentazione di Azure: Impostazioni incluse nello scambio](/azure/app-service/web-sites-staged-publishing).
1. Selezionare **Salva** nella parte superiore del pannello.

Servizio app di Azure riavvia automaticamente l'app quando un'impostazione dell'app (una variabile di ambiente) viene aggiunta, modificata o eliminata nel portale di Azure.

### <a name="windows"></a>WINDOWS

Per impostare la variabile `ASPNETCORE_ENVIRONMENT` per la sessione corrente, se l'app viene avviata tramite [dotnet run](/dotnet/core/tools/dotnet-run), vengono usati i comandi seguenti:

**Prompt dei comandi**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Questi comandi hanno effetto solo per la finestra corrente. Quando la finestra viene chiusa, l'impostazione `ASPNETCORE_ENVIRONMENT` viene ripristinata sull'impostazione predefinita o sul valore del computer.

Per impostare il valore a livello globale in Windows, usare uno degli approcci seguenti:

* Scegliere **Pannello di controllo** > **Sistema** > **Impostazioni di sistema avanzate** e aggiungere o modificare il valore `ASPNETCORE_ENVIRONMENT`:

  ![Proprietà di sistema avanzate](environments/_static/systemsetting_environment.png)

  ![Variabile di ambiente ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* Aprire un prompt dei comandi di amministrazione e usare il comando `setx` o aprire un prompt dei comandi di PowerShell di amministrazione e usare `[Environment]::SetEnvironmentVariable`:

  **Prompt dei comandi**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  L'opzione `/M` indica di impostare la variabile di ambiente a livello del sistema. Se non viene usata l'opzione `/M`, la variabile di ambiente viene impostata per l'account utente.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  Il valore dell'opzione `Machine` indica di impostare la variabile di ambiente a livello del sistema. Se non viene usata l'opzione `User`, la variabile di ambiente viene impostata per l'account utente.

Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` è impostata a livello globale, viene applicata per `dotnet run` in tutte le finestre di comando aperte dopo l'impostazione del valore.

**web.config**

Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` con *web.config*, vedere la sezione *Impostazione delle variabili di ambiente* di <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.

::: moniker range=">= aspnetcore-2.2"

**File di progetto o profilo di pubblicazione**

**Per le distribuzioni di Windows IIS:** Includere la proprietà `<EnvironmentName>` nel profilo di pubblicazione (*.pubxml*) o nel file di progetto. Questo approccio imposta l'ambiente in *web.config* quando viene pubblicato il progetto:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

**Pool di applicazioni IIS singoli**

Per impostare la variabile di ambiente `ASPNETCORE_ENVIRONMENT` per un'app in esecuzione in un pool di applicazioni isolato (supportato in IIS 10.0 o versioni successive), vedere la sezione *AppCmd.exe* dell'argomento [Variabili di ambiente &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe). Quando la variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene impostata per un pool di app, il suo valore esegue l'override di un'impostazione a livello di sistema.

> [!IMPORTANT]
> Durante l'hosting di un'app in IIS, quando si aggiunge o si modifica la variabile di ambiente `ASPNETCORE_ENVIRONMENT`, usare uno degli approcci seguenti per fare in modo che il nuovo valore venga selezionato dalle app:
>
> * Eseguire `net stop was /y` seguito da `net start w3svc` da un prompt dei comandi.
> * Riavviare il server.

### <a name="macos"></a>macOS

L'impostazione dell'ambiente corrente per macOS può essere eseguita in linea durante l'esecuzione dell'app:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

In alternativa, impostare l'ambiente tramite `export` prima di eseguire l'app:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Le variabili di ambiente a livello computer sono impostate nel file con estensione *bashrc* o *bash_profile*. Modificare il file usando un qualsiasi editor di testo. Aggiungere l'istruzione seguente:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Per le distribuzioni di Linux, usare il comando `export` al prompt dei comandi per le impostazioni delle variabili basate sulla sessione e il file *bash_profile* per le impostazioni di ambiente a livello di computer.

### <a name="configuration-by-environment"></a>Configurazione per ambiente

Per caricare la configurazione dall'ambiente, sono consigliabili:

* file *appSettings* (*appsettings.\< Ambiente>.json*). Vedere [Configurazione: Provider di configurazione dei file](xref:fundamentals/configuration/index#file-configuration-provider).
* Variabili di ambiente (impostate in ogni sistema in cui è ospitata l'app). Vedere [Configurazione: Provider di configurazione dei file](xref:fundamentals/configuration/index#file-configuration-provider) e [Archiviazione sicura di segreti dell'app durante lo sviluppo: Variabili di ambiente](xref:security/app-secrets#environment-variables).
* Secret Manager (solo nell'ambiente di sviluppo). Vedere <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Classe Startup e metodi basati sull'ambiente

### <a name="startup-class-conventions"></a>Convenzioni delle classi di avvio

Quando viene avviata un'app ASP.NET Core, la [classe Startup](xref:fundamentals/startup) avvia l'app. L'app può definire classi `Startup` separate per i diversi ambienti (ad esempio, `StartupDevelopment`) e la classe `Startup` appropriata viene selezionata durante il runtime. La classe il cui suffisso di nome corrisponde all'ambiente corrente ha la priorità. Se non viene trovata una classe `Startup{EnvironmentName}` corrispondente, viene usata la classe `Startup`.

Per implementare le classi `Startup` basate sull'ambiente, creare una classe `Startup{EnvironmentName}` per ogni ambiente in uso e una classe `Startup` di fallback:

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

Usare l'overload [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) che accetta un nome di assembly:

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a>Convenzioni dei metodi di avvio

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) supportano versioni specifiche per l'ambiente con i formati `Configure<EnvironmentName>` e `Configure<EnvironmentName>Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
