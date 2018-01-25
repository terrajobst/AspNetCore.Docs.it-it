---
title: La migrazione da ASP.NET per ASP.NET 2.0 Core
author: isaac2004
description: "Questo documento di riferimento contiene indicazioni sulla migrazione di applicazioni già esistenti in ASP.NET MVC o Web API ad ASP.NET Core 2.0."
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: 95bedf9299b4ff65c2f520358136174c4d2c4623
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>La migrazione da ASP.NET per ASP.NET 2.0 Core

Di [Isaac Levin](https://isaaclevin.com)

Questo articolo offre una guida di riferimento per la migrazione delle applicazioni ASP.NET ad ASP.NET Core 2.0.

## <a name="prerequisites"></a>Prerequisiti

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.

## <a name="target-frameworks"></a>Framework di destinazione
I progetti di ASP.NET Core 2.0 offrono agli sviluppatori la flessibilità necessaria per scegliere .NET Core, .NET Framework o entrambi come destinazione. Vedere [Scelta di .NET Core o .NET Framework per le app server](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) per determinare quale framework di destinazione è più appropriato.

Quando la destinazione è .NET Framework, i progetti devono fare riferimento a singoli pacchetti NuGet.

La scelta di .NET Core come destinazione consente di eliminare numerosi riferimenti espliciti ai pacchetti, grazie al [metapacchetto](xref:fundamentals/metapackage) di ASP.NET 2.0 Core. Installare il metapacchetto `Microsoft.AspNetCore.All` nel progetto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Quando si usa il metapacchetto, con l'app non viene distribuito alcun pacchetto a cui si fa riferimento nel metapacchetto. L'archivio di Runtime .NET Core include queste risorse sono è precompilati per migliorare le prestazioni. Vedere le informazioni sul [metapacchetto Microsoft.AspNetCore.All per ASP.NET Core 2. x](xref:fundamentals/metapackage) per maggiori dettagli.

## <a name="project-structure-differences"></a>Differenze di struttura del progetto
Il formato di file *CSPROJ* è stato semplificato in ASP.NET Core. Alcune modifiche importanti includono:
- Inclusione esplicita dei file non è necessaria affinché possano essere considerati parte del progetto. In questo modo si riduce il rischio di conflitti di merge XML quando si lavora con team di grandi dimensioni.
- Non sono presenti riferimenti basati su GUID ad altri progetti e questo migliora la leggibilità dei file.
- Il file può essere modificato senza scaricarlo in Visual Studio:

    ![Modificare l'opzione CSPROJ del menu di scelta rapida in Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Sostituzione di file Global.asax
In ASP.NET Core è stato introdotto un nuovo meccanismo per l'avvio automatico delle app. Il punto di ingresso per le applicazioni ASP.NET è il file *Global.asax*. Attività quali la configurazione della route e le registrazioni di area e filtro vengono gestite nel file *Global.asax*.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Con questo approccio l'applicazione e il server a cui viene distribuita vengono accoppiati in un modo che interferisce con l'implementazione. Al fine di disaccoppiare gli elementi, è stata introdotta la funzionalità [OWIN](http://owin.org/) che offre un modo più semplice di usare più framework insieme. OWIN offre una pipeline per aggiungere solo i moduli necessari. L'ambiente host accetta una funzione di [avvio](xref:fundamentals/startup) per configurare i servizi e la pipeline delle richieste dell'applicazione. `Startup` registra un set di middleware con l'applicazione. Per ogni richiesta, l'applicazione chiama ognuno dei componenti middleware con il puntatore iniziale di un elenco collegato a un set esistente di gestori. Ogni componente middleware può aggiungere uno o più gestori alla pipeline di gestione delle richieste. Questa operazione viene eseguita tramite la restituzione di un riferimento al gestore che è il nuovo inizio dell'elenco. Ogni gestore è responsabile della memorizzazione e della chiamata del gestore successivo nell'elenco. Con ASP.NET Core, il punto di ingresso a un'applicazione è `Startup` e non esiste più una dipendenza da *Global.asax*. Quando si usa OWIN con .NET Framework, usare una pipeline simile alla seguente:

[!code-csharp[Main](samples/webapi-owin.cs)]

Ciò consente di configurare le route predefinite e impostare come predefinito XmlSerialization per Json. Aggiungere altro middleware a questa pipeline in base alle esigenze, ad esempio caricamento di servizi, impostazioni di configurazione, file statici e così via.

ASP.NET Core usa un approccio simile, ma non si basa su OWIN per gestire la voce. Invece, che viene eseguita tramite il *Program.cs* `Main` metodo (simile alle applicazioni console) e `Startup` viene caricato tramite tale posizione.

[!code-csharp[Main](samples/program.cs)]

`Startup` deve includere un metodo `Configure`. In `Configure` aggiungere il middleware necessario alla pipeline. Nell'esempio seguente, estratto dal modello di sito Web predefinito, vengono usati vari metodi di estensione per configurare la pipeline con il supporto per:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Pagine di errore
* File statici
* ASP.NET Core MVC
* identità

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

L'host e applicazione sono stati disaccoppiati e questo offre la possibilità di passare a una piattaforma diversa in futuro.

**Nota:** per un riferimento più dettaglio all'avvio e al middleware di ASP.NET Core, vedere l'articolo sull'[avvio in ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Archiviazione delle configurazioni
ASP.NET supporta le impostazioni di archiviazione. Tali impostazioni vengono usate, ad esempio, per supportare l'ambiente in cui vengono distribuite le applicazioni. Una prassi comune era archiviare tutte le coppie chiave-valore personalizzate nella sezione `<appSettings>` del file *Web.config*:

[!code-xml[Main](samples/webconfig-sample.xml)]

Le applicazioni leggono queste impostazioni usando la raccolta `ConfigurationManager.AppSettings` nello spazio dei nomi `System.Configuration`:

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core è in grado di archiviare i dati di configurazione per l'applicazione in tutti i file e di caricarli come parte dell'avvio automatico del middleware. Il file predefinito usato nei modelli di progetto è *appSettings.json*:

[!code-json[Main](samples/appsettings-sample.json)]

Il caricamento del file in un'istanza di `IConfiguration` all'interno dell'applicazione viene eseguito in *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

L'app legge da `Configuration` per ottenere le impostazioni:

[!code-csharp[Main](samples/read-appsettings.cs)]

Sono disponibili estensioni di questo approccio che aumentano l'efficacia del processo, ad esempio l'uso dell'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per caricare un servizio con questi valori. L'approccio con inserimento delle dipendenze offre un set fortemente tipizzato di oggetti di configurazione.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Nota:** per informazioni più dettagliate sulla configurazione di ASP.NET Core, vedere l'articolo sulla [configurazione in ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Inserimento delle dipendenze nativo
Un obiettivo importante nella compilazione di applicazioni scalabili di grandi dimensioni è l'accoppiamento libero di componenti e servizi. [Inserimento di dipendenze](xref:fundamentals/dependency-injection) è una tecnica comune per raggiungere questo obiettivo, ed è un componente nativo di ASP.NET Core.

Nelle applicazioni ASP.NET, gli sviluppatori si affidano a una libreria di terze parti per implementare l'inserimento delle dipendenze. Una di queste librerie è [Unity](https://github.com/unitycontainer/unity) e fa parte di Modelli e procedure Microsoft. 

Un esempio di configurazione dell'inserimento delle dipendenze con Unity è l'implementazione di `IDependencyResolver` che esegue il wrapping di un oggetto `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Creare un'istanza di `UnityContainer`, registrare il servizio e impostare il sistema di risoluzione delle dipendenze di `HttpConfiguration` sulla nuova istanza di `UnityResolver` per il contenitore:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Inserire `IProductRepository` dove necessario:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Poiché l'inserimento delle dipendenze fa parte di ASP.NET Core, è possibile aggiungere il servizio nel metodo `ConfigureServices` di *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

Il repository può essere inserito in qualsiasi posizione, analogamente a Unity.

**Nota:** per un riferimento dettagliato all'inserimento delle dipendenze in ASP.NET Core, vedere l'introduzione all'[inserimento delle dipendenze in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Gestione dei file statici
Una parte importante dello sviluppo Web è la possibilità di distribuire asset statici sul lato client. Gli esempi più comuni di file statici sono HTML, CSS, Javascript e immagini. Questi file devono essere salvati nella posizione di pubblicazione dell'app (o della rete CDN) con riferimenti che ne consentano il caricamento da parte di una richiesta. Questo processo è stato modificato in ASP.NET Core.

In ASP.NET i file statici vengono archiviati in directory diverse e viene fatto riferimento ai file nelle viste.

In ASP.NET Core i file statici vengono archiviati nella "radice Web" (*&lt;radice contenuto&gt;/wwwroot*), a meno che la configurazione non sia diversa. I file vengono caricati nella pipeline delle richieste chiamando il metodo di estensione `UseStaticFiles` da `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Nota:** se la destinazione è .NET Framework, installare il pacchetto NuGet `Microsoft.AspNetCore.StaticFiles`.

Ad esempio, un asset immagine nella cartella *wwwroot/images* è accessibile al browser in corrispondenza di una posizione come `http://<app>/images/<imageFileName>`.

**Nota:** per informazioni più dettagliate sulla gestione dei file statici in ASP.NET Core, vedere l'[introduzione all'uso dei file statici in ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Risorse aggiuntive
* [Portabilità in .NET Core - Librerie](https://docs.microsoft.com/dotnet/core/porting/libraries)
