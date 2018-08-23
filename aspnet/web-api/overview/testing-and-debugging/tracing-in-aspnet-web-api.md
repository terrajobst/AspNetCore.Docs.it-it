---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Traccia nelle API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come abilitare la traccia nell'API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 697e7e91ae2d9d5712d9306a291635793063117b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833398"
---
<a name="tracing-in-aspnet-web-api-2"></a>Traccia in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Quando si tenta di eseguire il debug di un'applicazione basata su web, non vi è alcuna sostituzione di un set di log di traccia valido. Questa esercitazione illustra come abilitare la traccia nell'API Web ASP.NET. È possibile usare questa funzionalità per tracciare il framework API Web del funzionamento di prima e dopo aver richiamato il controller. È anche possibile usarlo per tracciare il proprio codice.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (funziona anche con Visual Studio 2015)
> - API Web 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Abilitare la traccia in Web API System. Diagnostics

In primo luogo, si creerà un nuovo progetto di applicazione Web ASP.NET. In Visual Studio, dai **File** dal menu **New**, quindi **progetto**. Sotto **modelli**, **Web**, selezionare **applicazione Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Scegliere il modello di progetto API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Dal **degli strumenti** dal menu **Library Package Manager**, quindi **Console di gestione pacchetti**.

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Il primo comando consente di installare il pacchetto più recente di traccia API Web. Aggiorna anche i pacchetti di API Web di base. Il secondo comando Aggiorna il pacchetto WebApi.WebHost alla versione più recente.

> [!NOTE]
> Se si vuole destinare una versione specifica di API Web, usare l'opzione - flag di versione quando si installa il pacchetto di analisi.


Aprire il file WebApiConfig.cs nell'App\_cartella di avvio. Aggiungere il codice seguente per il **registrare** (metodo).

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Questo codice aggiunge il [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe per la pipeline di Web API. Il **SystemDiagnosticsTraceWriter** classe scrive tracce [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Per visualizzare le tracce, eseguire l'applicazione nel debugger. Nel browser, passare a `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Le istruzioni di traccia vengono scritti nella finestra di Output in Visual Studio. (Dal **View** dal menu **Output**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

In quanto **SystemDiagnosticsTraceWriter** scrive tracce **Trace**, è possibile registrare i listener di traccia aggiuntivi, ad esempio, per scrivere le tracce in un file di log. Per altre informazioni sui writer di traccia, vedere la [listener di traccia](https://msdn.microsoft.com/library/4y5y10s7.aspx) argomento in MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurazione SystemDiagnosticsTraceWriter

Il codice seguente viene illustrato come configurare il writer di traccia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Sono disponibili due impostazioni che è possibile controllare:

- IsVerbose: Se è false, ogni traccia contiene informazioni minime. Se true, le tracce includono altre informazioni.
- MinimumLevel: Imposta il livello minimo di traccia. Livelli di traccia, in ordine, sono Debug, Info, Warn, errore ed errore irreversibile.

## <a name="adding-traces-to-your-web-api-application"></a>Aggiunta di tracce per l'applicazione API Web

Aggiunta di un writer di traccia offre l'accesso immediato alle tracce create tramite la pipeline di API Web. È inoltre possibile utilizzare il writer di traccia per tracciare il proprio codice:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Per ottenere il writer di traccia, chiamare **HttpConfiguration.Services.GetTraceWriter**. Da un controller, questo metodo è accessibile tramite il **ApiController.Configuration** proprietà.

Per creare una traccia, è possibile chiamare il **ITraceWriter.Trace** metodo direttamente, ma la [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) classe definisce alcuni metodi di estensione che sono più descrittivi. Ad esempio, il **Info** metodo illustrato in precedenza crea una traccia con livello di traccia **Info**.

## <a name="web-api-tracing-infrastructure"></a>Infrastruttura di traccia API Web

Questa sezione descrive come scrivere un writer di traccia personalizzato per l'API Web.

Il pacchetto Microsoft.AspNet.WebApi.Tracing si basa su un'infrastruttura di traccia più generale in API Web. Invece di usare Microsoft.AspNet.WebApi.Tracing, è anche possibile collegare in qualche altra libreria di analisi/effettuato l'accesso, ad esempio [NLog](http://nlog-project.org/) oppure [log4net](http://logging.apache.org/log4net/).

Per raccogliere le tracce, implementare il **ITraceWriter** interfaccia. Ecco un esempio semplice:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Il **ITraceWriter.Trace** metodo crea un oggetto trace. Il chiamante specifica un livello di traccia e di categoria. La categoria può essere qualsiasi stringa definita dall'utente. L'implementazione di **traccia** deve eseguire le operazioni seguenti:

1. Creare una nuova **TraceRecord**. Inizializzarlo con la richiesta, categoria e livello di traccia, come illustrato. Questi valori vengono forniti dal chiamante.
2. Richiama il *traceAction* delegare. All'interno di questo delegato, il chiamante dovrà completare nel resto del **TraceRecord**.
3. Scrivere il **TraceRecord**, usando tutte le tecniche che si desidera che la registrazione. Nell'esempio illustrato di seguito chiama semplicemente **Trace**.

## <a name="setting-the-trace-writer"></a>Impostare il Writer di traccia

Per abilitare la traccia, è necessario configurare Web API da usare il **ITraceWriter** implementazione. Eseguire questa operazione tramite il **HttpConfiguration** dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Writer di traccia solo uno possono essere attivi. Per impostazione predefinita, set di API Web un &quot;no-op&quot; traccia che non esegue alcuna operazione. (Il &quot;no-op&quot; utilità di traccia esistente in modo che non deve controllare se il writer di traccia è il codice di tracciatura **null** prima della scrittura di una traccia.)

## <a name="how-web-api-tracing-works"></a>Come Web API traccia Works

Traccia in Web API Usa Usa un'API Web in un *facciata* pattern: quando la traccia è abilitata, API Web esegue il wrapping di diverse parti di pipeline della richiesta con le classi che eseguono chiamate di traccia.

Ad esempio, quando si seleziona un controller, la pipeline Usa il **IHttpControllerSelector** interfaccia. Con la traccia abilitata, il pipleline inserisce una classe che implementa **IHttpControllerSelector** ma through delle chiamate per l'implementazione reale:

![Traccia API Web Usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

I vantaggi di questa progettazione includono:

- Se non si aggiunge un writer di traccia, i componenti di traccia non vengono create istanze e non abbiano alcun impatto sulle prestazioni.
- Se si sostituiscono servizi predefiniti, ad esempio **IHttpControllerSelector** con la propria implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.

È inoltre possibile sostituire l'intero framework traccia API Web con il proprio framework personalizzati, sostituendo il valore predefinito **ITraceManager** servizio:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementare **ITraceManager.Initialize** per inizializzare il sistema di analisi. Tenere presente che questo sostituisce il *intera* framework di traccia, tra cui tutto il codice di traccia che è incorporato in API Web.
