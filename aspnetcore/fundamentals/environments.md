---
title: "Utilizzo di più ambienti in ASP.NET Core"
author: ardalis
description: Scopri come ASP.NET Core fornisce supporto per il controllo del comportamento dell'app in ambienti diversi.
keywords: ASP.NET di base, le impostazioni di ambiente, ASPNETCORE_ENVIRONMENT
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 054b3e9f1e2bcfe1e4a75eca4d9dc6326ee6e44f
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="working-with-multiple-environments"></a>Utilizzo di più ambienti

Da [Steve Smith](https://ardalis.com/)

ASP.NET Core fornisce supporto per il controllo del comportamento dell'app in ambienti diversi, ad esempio sviluppo, gestione temporanea e produzione. Le variabili di ambiente vengono utilizzate per indicare l'ambiente di runtime, consentendo all'app di essere configurato per tale ambiente.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Sviluppo, gestione temporanea, produzione

ASP.NET Core fa riferimento a un particolare [variabile di ambiente](https://github.com/aspnet/Home/wiki), `ASPNETCORE_ENVIRONMENT` per descrivere l'ambiente in cui è in esecuzione l'applicazione. Questa variabile può essere impostata su qualsiasi valore desiderato, ma vengono utilizzati tre valori per convenzione: `Development`, `Staging`, e `Production`. Si noterà questi valori utilizzati negli esempi e i modelli forniti con ASP.NET Core.

Impostazione dell'ambiente corrente può essere rilevata a livello di codice all'interno dell'applicazione. Inoltre, è possibile utilizzare l'ambiente [helper di tag](../mvc/views/tag-helpers/index.md) includere determinate sezioni del [vista](../mvc/views/index.md) in base all'ambiente dell'applicazione corrente.

Nota: In Windows e Mac OS, il nome dell'ambiente specificato viene fatta distinzione tra maiuscole e minuscole. Se si imposta la variabile `Development` o `development` o `DEVELOPMENT` i risultati saranno uguali. Tuttavia, Linux è un **tra maiuscole e minuscole** sistema operativo per impostazione predefinita. Le variabili di ambiente, i nomi di file e le impostazioni richiedono distinzione maiuscole/minuscole.

### <a name="development"></a>Sviluppo

Deve trattarsi di ambiente utilizzata quando si sviluppa un'applicazione. In genere viene utilizzato per abilitare le funzionalità che non si desidera rendere disponibili quando si esegue l'app nell'ambiente di produzione, ad esempio il [pagina eccezione developer](xref:fundamentals/error-handling#the-developer-exception-page).

Se si utilizza Visual Studio, è possibile configurare l'ambiente nei profili di debug del progetto. Eseguire il debug di profili di specificare il [server](xref:fundamentals/servers/index) da utilizzare quando si avvia l'applicazione e delle variabili di ambiente da impostare. Il progetto può avere più profili di debug che impostare variabili di ambiente in modo diverso. Gestire i profili usando il **Debug** scheda della finestra del progetto di applicazione web **proprietà** menu. I valori impostati nelle proprietà del progetto sono persistenti nel *launchSettings.json* file ed è inoltre possibile configurare profili modificando direttamente il file.

Il profilo per IIS Express è illustrato di seguito:

![Variabili di ambiente di impostazione delle proprietà del progetto](environments/_static/project-properties-debug.png)

Ecco un `launchSettings.json` file che include i profili per `Development` e `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Le modifiche apportate ai profili di progetto non abbiano effetto, è necessario riavviare il server web utilizzato (in particolare, Kestrel devono essere riavviati prima che rileva le modifiche apportate al relativo ambiente).

>[!WARNING]
> Le variabili di ambiente archiviato in *launchSettings.json* non sono protetti in alcun modo e farà parte del repository di codice sorgente per il progetto, se si usa uno. **Non memorizzare mai le credenziali o altri dati riservati in questo file.** Se occorre una posizione in cui archiviare tali dati, utilizzare il *Manager segreto* strumento descritto in [archiviazione sicura di segreti dell'app durante lo sviluppo](../security/app-secrets.md#security-app-secrets).

### <a name="staging"></a>Gestione temporanea

Per convenzione, un `Staging` ambiente è un ambiente di pre-produzione usato per il test finale prima della distribuzione nell'ambiente di produzione. In teoria, le sue caratteristiche fisiche devono rispecchiare quello di produzione, in modo che si verificano prima eventuali problemi che potrebbero verificarsi nell'ambiente di produzione nell'ambiente di gestione temporanea, in cui possono essere risolti senza impatto sugli utenti.

### <a name="production"></a>Produzione

Il `Production` ambiente è l'ambiente in cui l'applicazione viene eseguita quando è in tempo reale e utilizzati dagli utenti finali. Questo ambiente deve essere configurato per ottimizzare la sicurezza, prestazioni e affidabilità dell'applicazione. Alcune impostazioni comuni che potrebbe avere un ambiente di produzione che verrebbero differisce dallo sviluppo includono:

* Attivare la memorizzazione nella cache

* Assicurarsi che tutte le risorse sul lato client sono inclusi, minimizzare e potenzialmente servite da una rete CDN

* Disattivare la diagnostica ErrorPages

* Attivare le pagine di errore descrittivo

* Abilitare la registrazione e monitoraggio di produzione (ad esempio, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

Questo non deve essere un elenco completo. È consigliabile evitare la dispersione controlli dell'ambiente in molte parti dell'applicazione. Al contrario, l'approccio consigliato è eseguire tali controlli all'interno dell'applicazione `Startup` classi ove possibile

## <a name="setting-the-environment"></a>Impostazione dell'ambiente

Il metodo per l'impostazione dell'ambiente dipende dal sistema operativo.

### <a name="windows"></a>Windows
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

**Web. config**

Vedere il *impostare variabili di ambiente* sezione la [riferimento di configurazione di ASP.NET Core modulo](xref:hosting/aspnet-core-module#setting-environment-variables) argomento.

**Per ogni Pool di applicazioni IIS**

Se è necessario impostare le variabili di ambiente per le singole app in esecuzione nel pool di applicazioni isolate (supportate in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento.

### <a name="macos"></a>MacOS
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

## <a name="determining-the-environment-at-runtime"></a>Determinare l'ambiente in fase di esecuzione

Il `IHostingEnvironment` servizio fornisce l'astrazione fondamentale per l'utilizzo di ambienti. Questo servizio viene fornito da ASP.NET hosting dei livelli e possono essere inserite nella logica di avvio tramite [Dependency Injection](dependency-injection.md). Il modello di sito web ASP.NET Core in Visual Studio utilizza questo approccio per caricare i file di configurazione specifici dell'ambiente (se presente) e per personalizzare le impostazioni di gestione degli errori dell'applicazione. In entrambi i casi, questo comportamento viene ottenuto facendo riferimento all'ambiente specificato chiamando `EnvironmentName` o `IsEnvironment` nell'istanza di `IHostingEnvironment` passato al metodo appropriato.

> [!NOTE]
> Se è necessario verificare se l'applicazione è in esecuzione in un ambiente specifico, utilizzare `env.IsEnvironment("environmentname")` poiché correttamente lo ignora maiuscole (invece di controllare se `env.EnvironmentName == "Development"` ad esempio).

Ad esempio, è possibile utilizzare il codice seguente nel metodo di configurazione per la gestione degli errori specifici di ambiente del programma di installazione:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Se l'app è in esecuzione in un `Development` ambiente, quindi si abilita il supporto di runtime necessario per utilizzare la funzionalità "BrowserLink" in Visual Studio, le pagine di errore specifico di sviluppo (che in genere non devono essere eseguite nell'ambiente di produzione) ed errore di database speciale pagine (che forniscono un modo per applicare le migrazioni e devono pertanto essere utilizzate solo in fase di sviluppo). In caso contrario, se l'app non è in esecuzione in un ambiente di sviluppo, una pagina di gestione degli errori standard è configurata per essere visualizzato in risposta a eventuali eccezioni non gestite.

Potrebbe essere necessario determinare quale contenuto da inviare al client in fase di esecuzione, a seconda dell'ambiente corrente. Ad esempio, in un ambiente di sviluppo si forniscono in genere non ridotta a icona script e fogli di stile, che rende il debug. Ambienti di produzione e di test devono essere utilizzato per le versioni minimizzate e in genere da una rete CDN. È possibile farlo usando l'ambiente [helper di tag](../mvc/views/tag-helpers/intro.md). L'helper di tag di ambiente restituirà il relativo contenuto solo se l'ambiente corrente corrisponde a uno degli ambienti specificati utilizzando il `names` attributo.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Per iniziare a utilizzare gli helper di tag nell'applicazione, vedere [introduzione per gli helper di Tag](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Convenzioni di avvio

ASP.NET di base supporta un approccio basato sulle convenzioni per la configurazione di avvio di un'applicazione in base all'ambiente corrente. Anche a livello di codice, è possibile controllare il comportamento dell'applicazione in base a quale ambiente è in, che consente di creare e gestire le convenzioni.

Quando si avvia un'applicazione ASP.NET di base, il `Startup` classe viene utilizzata per avviare l'applicazione, caricare il impostazioni di configurazione e così via ([ulteriori informazioni sull'avvio di ASP.NET](startup.md)). Tuttavia, se esiste una classe denominata `Startup{EnvironmentName}` (ad esempio `StartupDevelopment`) e `ASPNETCORE_ENVIRONMENT` variabile di ambiente corrispondente, tale nome sarà quello `Startup` classe viene invece utilizzata. Di conseguenza, è possibile configurare `Startup` per lo sviluppo, ma disporre di un' `StartupProduction` che viene utilizzata quando si esegue l'app nell'ambiente di produzione. O viceversa.

> [!NOTE]
> La chiamata `WebHostBuilder.UseStartup<TStartup>()` esegue l'override delle sezioni di configurazione.

Oltre a utilizzare un completamente separate `Startup` classe in base all'ambiente corrente, è anche possibile apportare modifiche di configurazione dell'applicazione all'interno di un `Startup` classe. Il `Configure()` e `ConfigureServices()` metodi supportano le versioni specifiche dell'ambiente simile al `Startup` classe stessa, nel formato `Configure{EnvironmentName}()` e `Configure{EnvironmentName}Services()`. Se si definisce un metodo `ConfigureDevelopment()` verrà chiamato anziché `Configure()` quando l'ambiente è impostata per lo sviluppo. Analogamente, `ConfigureDevelopmentServices()` deve essere chiamato anziché `ConfigureServices()` nello stesso ambiente.

## <a name="summary"></a>Riepilogo

ASP.NET Core fornisce una serie di funzionalità e le convenzioni che consentono agli sviluppatori di controllare facilmente il comportamento delle applicazioni in ambienti diversi. Quando si pubblica un'applicazione dallo sviluppo alla gestione temporanea alla produzione, set di variabili di ambiente in modo appropriato per l'ambiente di consentire per l'ottimizzazione dell'applicazione per l'utilizzo di debug, test o di produzione, come appropriato.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione](configuration.md)

* [Introduzione agli helper tag](../mvc/views/tag-helpers/intro.md)
