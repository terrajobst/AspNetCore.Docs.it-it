---
title: La migrazione da ASP.NET per ASP.NET 2.0 Core
author: isaac2004
description: In questo documento fornisce materiale sussidiario per migrazione ASP.NET MVC o Web API delle applicazioni esistenti per ASP.NET 2.0 Core.
keywords: La migrazione di componenti di base, MVC ASP.NET
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>La migrazione da ASP.NET per ASP.NET 2.0 Core

Da [Isaac Levin](https://isaaclevin.com)

In questo articolo viene utilizzato come una Guida di riferimento per la migrazione delle applicazioni ASP.NET ad ASP.NET Core 2.0.

## <a name="prerequisites"></a>Prerequisiti

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.

## <a name="target-frameworks"></a>Framework di destinazione
Progetti di componenti di base di ASP.NET 2.0 offrono agli sviluppatori la flessibilità di destinazione di .NET Core, .NET Framework o entrambi. Vedere [scelta tra .NET Core e .NET Framework per applicazioni server](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) per determinare quali framework di destinazione è più appropriato.

Quando la destinazione è .NET Framework, è necessario fare riferimento a singoli pacchetti NuGet progetti.

Destinato a .NET Core consente di eliminare numerosi riferimenti pacchetto esplicita, grazie ai componenti di base di ASP.NET 2.0 [metapackage](xref:fundamentals/metapackage). Installare il `Microsoft.AspNetCore.All` metapackage nel progetto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Quando viene utilizzato il metapackage, nessun pacchetto a cui fa riferimento il metapackage viene distribuito con l'app. L'archivio di Runtime .NET Core include queste risorse, e sono precompilati per migliorare le prestazioni. Vedere [metapackage Microsoft.AspNetCore.All per ASP.NET Core 2. x](xref:fundamentals/metapackage) per ulteriori dettagli.

## <a name="project-structure-differences"></a>Differenze di struttura di progetto
Il *csproj* formato di file è stato semplificato in ASP.NET Core. Alcune modifiche importanti includono:
- Esplicita inclusione di file non è necessaria affinché possano essere considerate parte del progetto. Questo riduce il rischio di conflitti di unione XML quando si lavora in team di grandi dimensioni.
- Non sono presenti riferimenti basato su GUID per altri progetti, che migliora la leggibilità di file.
- Il file può essere modificato senza scaricarlo in Visual Studio:

    ![Modificare l'opzione del menu contesto CSPROJ in Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Sostituzione di file Global. asax
È stato introdotto un nuovo meccanismo per l'avvio di un'applicazione ASP.NET Core. Il punto di ingresso per le applicazioni ASP.NET è il *Global. asax* file. Attività quali la configurazione delle route e le registrazioni di area e di filtro vengono gestite nel *Global. asax* file.

[!code-csharp[Principale](samples/globalasax-sample.cs)]

Collegato a questo approccio l'applicazione e il server a cui è stato distribuito in modo che interferisce con l'implementazione. Nel tentativo di separare, [OWIN](http://owin.org/) è stata introdotta per fornire un modo più semplice da utilizzare insieme più Framework. OWIN fornisce una pipeline per aggiungere solo i moduli necessari. L'ambiente host ha un [avvio](xref:fundamentals/startup) funzione per configurare servizi e delle pipeline delle richieste dell'applicazione. `Startup`Registra un set di middleware con l'applicazione. Per ogni richiesta, l'applicazione chiama ognuno dei componenti middleware con il puntatore head di un elenco collegato a un set esistente di gestori. Ogni componente del middleware è possibile aggiungere uno o più gestori per la gestione della pipeline delle richieste. Questa operazione viene eseguita tramite la restituzione di un riferimento al gestore che è il nuovo inizio dell'elenco. Ogni gestore è responsabile per la memorizzazione e richiamare il gestore successivo nell'elenco. Il punto di ingresso a un'applicazione ASP.NET di base, è `Startup`, e non è una dipendenza *Global. asax*. Quando si utilizza OWIN con .NET Framework, è possibile utilizzare il seguente come una pipeline:

[!code-csharp[Principale](samples/webapi-owin.cs)]

Ciò consente di configurare le route predefinite e il valore predefinito XmlSerialization su Json. Aggiungere altro Middleware per questa pipeline in base alle esigenze (il caricamento di servizi, le impostazioni di configurazione, file statici, ecc.).

ASP.NET Core viene utilizzato un approccio simile, ma non si basi su OWIN per gestire la voce. Invece, che viene eseguita tramite il *Program.cs* `Main` metodo (simile alle applicazioni console) e `Startup` viene caricato tramite tale posizione.

[!code-csharp[Principale](samples/program.cs)]

`Startup`deve includere un `Configure` metodo. In `Configure`, aggiungere il middleware necessario per la pipeline. Nell'esempio seguente (dal modello sito web predefinito), i diversi metodi di estensione vengono utilizzati per configurare la pipeline con il supporto per:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Pagine di errore
* File statici
* Componenti di base di ASP.NET MVC
* Identità

[!code-csharp[Principale](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

L'host e applicazione sono state separate, che fornisce la flessibilità di transizione a una piattaforma diversa in futuro.

**Nota:** per un riferimento più dettaglio di avvio ASP.NET di base e Middleware, vedere [avvio ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Archiviazione delle configurazioni
ASP.NET supporta le impostazioni di archiviazione. Queste impostazioni vengono utilizzate, ad esempio, per supportare l'ambiente in cui sono state distribuite le applicazioni. È pratica comune per archiviare tutte le coppie chiave-valore personalizzate nel `<appSettings>` sezione la *Web. config* file:

[!code-xml[Principale](samples/webconfig-sample.xml)]

Le applicazioni leggono queste impostazioni utilizzando il `ConfigurationManager.AppSettings` insieme il `System.Configuration` dello spazio dei nomi:

[!code-csharp[Principale](samples/read-webconfig.cs)]

ASP.NET Core è possibile archiviare i dati di configurazione per l'applicazione in tutti i file e caricarli come parte di avvio automatico di middleware. Il file predefinito utilizzato nei modelli di progetto è *appSettings. JSON*:

[!code-json[Principale](samples/appsettings-sample.json)]

Caricare il file in un'istanza di `IConfiguration` all'interno dell'applicazione viene eseguita in *Startup.cs*:

[!code-csharp[Principale](samples/startup-builder.cs)]

L'app legge da `Configuration` per ottenere le impostazioni:

[!code-csharp[Principale](samples/read-appsettings.cs)]

Sono disponibili estensioni di questo approccio per rendere più affidabile, ad esempio utilizzando il processo [Dependency Injection](xref:fundamentals/dependency-injection) (DI) per caricare un servizio con questi valori. L'approccio DI fornisce un set di fortemente tipizzata di oggetti di configurazione.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Nota:** per la documentazione più dettagliata alla configurazione di ASP.NET Core, vedere [configurazione in ASP.NET Core](xref:fundamentals/configuration).

## <a name="native-dependency-injection"></a>Inserimento di dipendenze nativo
Un importante obiettivo durante la compilazione di applicazioni di grandi dimensioni, scalabile è dell'accoppiamento debole di componenti e servizi. [Inserimento di dipendenze](xref:fundamentals/dependency-injection) è una tecnica comune per raggiungere questo obiettivo, ed è un componente nativo di ASP.NET Core.

Nelle applicazioni ASP.NET, gli sviluppatori si basano su una libreria di terze parti per implementare l'inserimento di dipendenze. È una di queste librerie [Unity](https://github.com/unitycontainer/unity), fornito da Microsoft Patterns & Practices. 

Implementazione di un esempio di configurazione di inserimento di dipendenze con Unity `IDependencyResolver` che esegue il wrapping di un `UnityContainer`:

[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Creare un'istanza del `UnityContainer`, registrare il servizio e impostare il resolver di dipendenza di `HttpConfiguration` nella nuova istanza di `UnityResolver` per il contenitore:

[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Inserire `IProductRepository` dove necessario:

[!code-csharp[Principale](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Poiché l'inserimento di dipendenze fa parte di ASP.NET Core, è possibile aggiungere il servizio nel `ConfigureServices` metodo *Startup.cs*:

[!code-csharp[Principale](samples/configure-services.cs)]

Il repository può essere inserito in qualsiasi posizione, analogamente a Unity.

**Nota:** per un riferimento dettagliato per l'inserimento di dipendenze in ASP.NET Core, vedere [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Gestione dei file statici
Una parte importante dello sviluppo web è la possibilità di distribuire asset statico, sul lato client. Gli esempi più comuni dei file statici sono HTML, CSS, Javascript e immagini. Questi file devono essere salvati nella posizione di pubblicazione dell'app (o della rete CDN) e a cui fa riferimento in modo che può essere caricati da una richiesta. Questo processo è stato modificato in ASP.NET Core.

In ASP.NET, file statici sono archiviati in directory diverse e a cui fa riferimento nelle viste.

In ASP.NET Core, file statici vengono archiviati nella radice web"" (*&lt;contenuto radice&gt;/wwwroot*), salvo configurazione diversa. I file vengono caricati nella pipeline delle richieste richiamando il `UseStaticFiles` il metodo di estensione da `Startup.Configure`:

[!code-csharp[Principale](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

**Nota:** se la destinazione di .NET Framework, installare il pacchetto NuGet `Microsoft.AspNetCore.StaticFiles`.

Ad esempio, una risorsa immagine nel *wwwroot/immagini* cartella sia accessibile al browser in corrispondenza della posizione, ad esempio `http://<app>/images/<imageFileName>`.

**Nota:** per la documentazione più dettagliata di gestione dei file statici in ASP.NET Core, vedere [Introduzione all'utilizzo dei file statici di ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Risorse aggiuntive
* [Importazione di librerie per .NET Core](https://docs.microsoft.com/dotnet/core/porting/libraries)
