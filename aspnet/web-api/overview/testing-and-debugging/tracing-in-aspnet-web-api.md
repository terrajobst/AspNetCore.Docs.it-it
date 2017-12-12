---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Analisi in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: Viene illustrato come abilitare la traccia di ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>Analisi in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Quando si sta tentando di eseguire il debug di un'applicazione basata su web, non si verifica alcuna sostituzione per un set ottimo di log di traccia. In questa esercitazione viene illustrato come abilitare la traccia di ASP.NET Web API. È possibile utilizzare questa funzionalità per il framework Web API cosa prima e dopo che viene richiamato il controller di traccia. È possibile inoltre utilizzare per tracciare il proprio codice.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (funziona anche con Visual Studio 2015)
> - Web API 2
> - [Italiano](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Abilitare la traccia in Web API System. Diagnostics

In primo luogo, si creerà un nuovo progetto applicazione Web ASP.NET. In Visual Studio, dal **File** dal menu **New**, quindi **progetto**. In **modelli**, **Web**selezionare **applicazione Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Scegliere il modello di progetto API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi **pacchetto Console di gestione**.

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Il primo comando installa il pacchetto di analisi Web API più recente. Aggiorna anche i pacchetti di Web API core. Il secondo comando Aggiorna il pacchetto WebApi.WebHost alla versione più recente.

> [!NOTE]
> Se si desidera utilizzare una versione specifica dell'API Web, utilizzare il parametro - flag di versione quando si installa il pacchetto di analisi.


Aprire il file WebApiConfig.cs nell'App\_cartella di avvio. Aggiungere il codice seguente per il **registrare** metodo.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Questo codice aggiunge il [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe per la pipeline di Web API. Il **SystemDiagnosticsTraceWriter** classe scrive tracce a [Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

Per visualizzare le tracce, eseguire l'applicazione nel debugger. Nel browser, passare a `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Le istruzioni di traccia vengono scritti nella finestra di Output in Visual Studio. (Dal **vista** dal menu **Output**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Poiché **SystemDiagnosticsTraceWriter** scrive tracce a **Trace**, è possibile registrare i listener di traccia aggiuntivi, ad esempio, per scrivere le tracce in un file di log. Per ulteriori informazioni sui writer di traccia, vedere il [listener di traccia](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) su MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configurazione SystemDiagnosticsTraceWriter

Il codice seguente viene illustrato come configurare il writer di traccia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Sono disponibili due impostazioni che è possibile controllare:

- IsVerbose: Se è false, ogni traccia contiene informazioni minime. Se true, le tracce includere altre informazioni.
- MinimumLevel: Imposta il livello minimo di traccia. Livelli di traccia, in ordine, sono di Debug, Info, Warn, errore ed errore irreversibile.

## <a name="adding-traces-to-your-web-api-application"></a>Aggiunta di tracce per l'applicazione Web API

Aggiunta di un writer di traccia consente di accedere immediatamente per le tracce create tramite la pipeline di Web API. Inoltre, è possibile utilizzare il writer di traccia per il proprio codice di traccia:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Per ottenere il writer di traccia, chiamare **HttpConfiguration.Services.GetTraceWriter**. Da un controller, questo metodo è accessibile tramite il **ApiController.Configuration** proprietà.

Per creare una traccia, è possibile chiamare il **ITraceWriter.Trace** metodo direttamente, ma la [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) classe definisce alcuni metodi di estensione che sono più descrittivi. Ad esempio, il **Info** metodo illustrato in precedenza viene creata una traccia con livello di traccia **Info**.

## <a name="web-api-tracing-infrastructure"></a>Infrastruttura di analisi Web API

In questa sezione viene descritto come scrivere un writer di traccia personalizzato per l'API Web.

Il pacchetto italiano è incorporato nell'infrastruttura di traccia più generale in Web API. Anziché utilizzare italiano, è inoltre possibile collegare in qualche altra libreria di analisi/effettuato l'accesso, ad esempio [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).

Per raccogliere le tracce, implementare il **ITraceWriter** interfaccia. Di seguito è riportato un esempio:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Il **ITraceWriter.Trace** metodo crea una traccia. Il chiamante specifica un livello di traccia e di categoria. La categoria può essere qualsiasi stringa definita dall'utente. L'implementazione di **traccia** deve eseguire le operazioni seguenti:

1. Creare un nuovo **TraceRecord**. Inizializzare con la richiesta, categoria e livello di traccia, come illustrato. Questi valori vengono forniti dal chiamante.
2. Richiamare il *traceAction* delegato. All'interno di questo tipo di delegato, il chiamante deve compilare il resto del **TraceRecord**.
3. Scrivere il **TraceRecord**, utilizzando qualsiasi tecnica di registrazione desiderato. Nell'esempio illustrato di seguito chiama semplicemente **Trace**.

## <a name="setting-the-trace-writer"></a>Impostare il Writer di traccia

Per abilitare la traccia, è necessario configurare l'API Web per utilizzare il **ITraceWriter** implementazione. È possibile tramite il **HttpConfiguration** dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Writer di traccia solo uno può essere attivo. Per impostazione predefinita, set di API Web un &quot;NOOP&quot; traccia che non esegue alcuna operazione. (Il &quot;NOOP&quot; traccia esiste in modo che l'analisi del codice non è necessario controllare se il writer di traccia è **null** prima della scrittura di una traccia.)

## <a name="how-web-api-tracing-works"></a>Ricerca Web API Tracing Works

Traccia negli utilizzi di API Web viene utilizzata un'API Web in un *facciata* motivo: quando è attivata, API Web esegue il wrapping di varie parti della pipeline di richieste con le classi che eseguono chiamate di traccia.

Ad esempio, quando si seleziona un controller, la pipeline utilizza il **IHttpControllerSelector** interfaccia. Con la traccia è attivata, il pipleline inserisce una classe che implementa **IHttpControllerSelector** ma chiamate all'implementazione reale:

![Traccia API Web Usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

I vantaggi di questa progettazione includono:

- Se non si aggiunge un writer di traccia, i componenti di traccia non vengono create istanze e non abbiano alcun impatto sulle prestazioni.
- Se si sostituiscono servizi predefiniti, ad esempio **IHttpControllerSelector** con un'implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.

È inoltre possibile sostituire l'intero framework traccia API Web con propri framework personalizzato, sostituendo il valore predefinito **ITraceManager** servizio:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementare **ITraceManager.Initialize** per inizializzare il sistema di traccia. Tenere presente che questa impostazione sostituisce il *intero* framework di traccia, inclusi tutto il codice di traccia che viene compilato nell'API Web.
