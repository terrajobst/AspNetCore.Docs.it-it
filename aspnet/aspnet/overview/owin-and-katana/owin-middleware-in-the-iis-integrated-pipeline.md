---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware OWIN in IIS integrati pipeline | Microsoft Docs
author: Praburaj
description: Questo articolo illustra come eseguire i componenti middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare un OMC l'evento della pipeline viene eseguita in. Deve...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 6124bcdaeeb0d4342cbde0d3ca52d55f76a953ab
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576466"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN nella pipeline integrata di IIS
====================
dal [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questo articolo illustra come eseguire i componenti middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare un OMC l'evento della pipeline viene eseguita in. È necessario rivedere [una panoramica del progetto Katana](an-overview-of-project-katana.md) e [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md) prima di leggere questa esercitazione. Questa esercitazione è stato scritto da Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Sebbene [OWIN](an-overview-of-project-katana.md) componenti middleware (OMCs) sono progettati principalmente per l'esecuzione in una pipeline senza server, è possibile eseguire un OMC nella pipeline integrata di IIS nonché (**è in modalità classica *non* supportato**). Un OMC può essere reso a funzionare nella pipeline integrata di IIS installando il pacchetto seguente dal Package Manager Console (console di gestione pacchetti):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Ciò significa che tutti i framework di applicazione, anche quelli che non sono ancora in grado di eseguire di fuori di IIS e System. Web, possono trarre vantaggio da componenti del middleware OWIN esistenti. 

> [!NOTE]
> Tutti i `Microsoft.Owin.Security.*` i pacchetti di distribuzione con il nuovo sistema di identità in Visual Studio 2013 (ad esempio: i cookie, Account Microsoft, Google, Facebook, Twitter, [Bearer Token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, server di autorizzazione, token JWT, Azure Active Directory e Active directory federation services) vengono creati come OMCs e può essere usato in scenari self-hosted e ospitato in IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Modalità di esecuzione di Middleware OWIN nella Pipeline integrata di IIS

Per le applicazioni console OWIN, la pipeline dell'applicazione compilata usando il [configurazione di avvio](owin-startup-class-detection.md) è impostata in base all'ordine dei componenti vengono aggiunti utilizzando la `IAppBuilder.Use` (metodo). Vale a dire, la pipeline OWIN nel [Katana](an-overview-of-project-katana.md) runtime elaborerà OMCs nell'ordine in cui sono stati registrati usando `IAppBuilder.Use`. Nella pipeline integrata di IIS la pipeline delle richieste è costituito [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) sottoscritto, ad esempio un set predefinito degli eventi di pipeline [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)e così via.

Se si confronta un OMC a quello di un' [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) del mondo di ASP.NET, un OMC deve essere registrato per l'evento corretto pipeline predefinite. Ad esempio, HttpModule `MyModule` verrà richiamato quando viene ricevuta una richiesta il [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fase della pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Affinché un OMC a far parte di questo ordinamento per l'esecuzione stessa, basato su eventi, il [Katana](an-overview-of-project-katana.md) esegue l'analisi del codice runtime tramite il [configurazione di avvio](owin-startup-class-detection.md) e sottoscrive ognuno dei componenti middleware a un eventi pipeline integrata. Il seguente codice di registrazione e OMC consente, ad esempio, vedere la registrazione dell'evento predefinito dei componenti middleware. (Per istruzioni sulla creazione di una classe di avvio OWIN più dettagliate, vedere [rilevamento della classe di avvio OWIN](owin-startup-class-detection.md).)

1. Creare un progetto di applicazione web vuota e denominarla **owin2**.
2. Dal pacchetto Manager Console (console di gestione pacchetti), eseguire il comando seguente: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Aggiungere un `OWIN Startup Class` e denominarlo `Startup`. Sostituire il codice generato con il codice seguente (le modifiche sono evidenziate):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Premere F5 per eseguire l'app.

La configurazione di avvio consente di impostare una pipeline con i componenti middleware tre, i primi due visualizzando le informazioni di diagnostica e il penultimo risponde agli eventi (e anche la visualizzazione di informazioni di diagnostica). Il `PrintCurrentIntegratedPipelineStage` metodo consente di visualizzare l'evento pipeline integrata questo middleware viene richiamato sulla e un messaggio. Le finestre di output visualizza quanto segue:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Il runtime Katana eseguito il mapping di ogni componente middleware OWIN per [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) per impostazione predefinita, che corrisponde all'evento di pipeline IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcatori di fase

È possibile contrassegnare OMCs da eseguire in specifiche fasi della pipeline usando la `IAppBuilder UseStageMarker()` metodo di estensione. Per eseguire un set di componenti middleware durante una specifica fase, inserire un marcatore di fasi subito dopo l'ultimo componente è un set durante la registrazione. Esistono regole in quale fase della pipeline è possibile eseguire middleware e devono eseguire i componenti dell'ordine (le regole sono illustrate più avanti nell'esercitazione). Aggiungere il `UseStageMarker` metodo di `Configuration` codice come illustrato di seguito:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Il `app.UseStageMarker(PipelineStage.Authenticate)` chiamata consente di configurare tutti i componenti middleware registrato in precedenza (in questo caso, i due componenti di diagnostica) per l'esecuzione in fase di autenticazione della pipeline. L'ultimo componente middleware, che consente di visualizzare la diagnostica e risponde alle richieste, verrà eseguito nel `ResolveCache` fase (la [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) eventi).

Premere F5 per eseguire l'app. La finestra di output mostra quanto segue:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Regole di marcatore di fasi

Componenti del middleware Owin (OMC) possono essere configurati per eseguire gli eventi di fase della pipeline OWIN seguenti:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Per impostazione predefinita, OMCs eseguite in corrispondenza dell'ultimo evento (`PreHandlerExecute`). Ecco perché il primo codice di esempio visualizzato "PreExecuteRequestHandler".
2. È possibile usare l'una `app.UseStageMarker` metodo per registrare un OMC venga eseguito in precedenza, in qualsiasi fase della pipeline OWIN elencati nel `PipelineStage` enum.
3. La pipeline OWIN e la pipeline IIS sono ordinati, pertanto le chiamate a `app.UseStageMarker` deve essere in ordine. Non è possibile impostare il gestore eventi a un evento che precede l'ultimo evento registrato con `app.UseStageMarker`. Ad esempio, *dopo* chiamata:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   le chiamate a `app.UseStageMarker` passando `Authenticate` o `PostAuthenticate` non saranno rispettata e non verrà generata alcuna eccezione. Eseguire durante la fase più recente, che per impostazione predefinita è OMCs `PreHandlerExecute`. I marcatori di fase vengono usati per rendere vengano eseguiti in precedenza. Se si specificano i marcatori di fase non in ordine, vengono arrotondate al marcatore della precedente. In altre parole, l'aggiunta di un marcatore di fasi afferma "Esegui non oltre fase X". Esecuzione del OMC al marcatore di fasi meno recente aggiunto successivamente nella pipeline OWIN.
4. Alla prima fase il numero di chiamate al `app.UseStageMarker` wins. Ad esempio, se si inverte l'ordine di `app.UseStageMarker` chiamate in base all'esempio precedente:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Verrà visualizzata la finestra di output: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Il OMCs tutte eseguite nel `AuthenticateRequest` fase, perché l'ultima OMC registrato con il `Authenticate` evento e il `Authenticate` eventi precede tutti gli altri eventi.
