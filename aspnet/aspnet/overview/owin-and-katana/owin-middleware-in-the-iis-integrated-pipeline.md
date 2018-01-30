---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: "Middleware OWIN in IIS è integrata pipeline | Documenti Microsoft"
author: Praburaj
description: "In questo articolo viene illustrato come eseguire i componenti middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare l'evento pipeline un OMC viene eseguito su. È necessario..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5f6ed1ae0309e9bdd3ca4ae229195835f20bc729
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware OWIN della pipeline integrata di IIS
====================
da [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> In questo articolo viene illustrato come eseguire i componenti middleware OWIN (OMCs) nella pipeline integrata di IIS e come impostare l'evento pipeline un OMC viene eseguito su. È necessario rivedere [una panoramica del progetto Katana](an-overview-of-project-katana.md) e [OWIN avvio classe rilevamento](owin-startup-class-detection.md) prima di leggere questa esercitazione. In questa esercitazione è stato scritto dal Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Chris Ross Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Sebbene [OWIN](an-overview-of-project-katana.md) componenti middleware (OMCs) sono progettati principalmente per l'esecuzione in una pipeline indipendente dal server, è possibile eseguire un OMC della pipeline integrata IIS nonché (**è in modalità classica *non* supportati**). Per il funzionamento della pipeline integrata di IIS, l'installazione del pacchetto di esempio dal Package Manager Console (PMC), è possibile effettuare un OMC:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Ciò significa che tutti i framework dell'applicazione, anche quelli che non sono ancora in grado di eseguire di fuori di IIS e System. Web, possono trarre vantaggio da componenti esistenti di middleware OWIN. 

> [!NOTE]
> Tutti i `Microsoft.Owin.Security.*` pacchetti shipping con il nuovo sistema di identità in Visual Studio 2013 (ad esempio: i cookie, Account Microsoft, Google, Facebook, Twitter, [Token di connessione](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, server di autorizzazione, JWT, Azure Active Directory e Active directory federation services) vengono creati come OMCs e può essere usato negli scenari self-hosted e ospitati in IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Modalità di esecuzione di Middleware OWIN in Pipeline integrata di IIS

Per le applicazioni console OWIN, la pipeline dell'applicazione creati utilizzando il [configurazione di avvio](owin-startup-class-detection.md) è impostata in base all'ordine dei componenti vengono aggiunti utilizzando la `IAppBuilder.Use` metodo. Ovvero, la pipeline OWIN nel [Katana](an-overview-of-project-katana.md) runtime elaborerà OMCs nell'ordine in cui sono stati registrati utilizzando `IAppBuilder.Use`. Nella pipeline integrata di IIS la pipeline della richiesta è costituita [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) sottoscrivere un set predefinito di eventi della pipeline, ad esempio [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)e così via.

Se si confronta un OMC a quello di un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) nel mondo ASP.NET, un OMC deve essere registrato per l'evento corretto pipeline predefinite. Ad esempio, HttpModule `MyModule` verrà richiamato quando arriva una richiesta il [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) fase della pipeline:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Affinché un OMC deve far parte di questo ordine di esecuzione stesso, basato su eventi, il [Katana](an-overview-of-project-katana.md) esegue l'analisi del codice runtime tramite il [configurazione di avvio](owin-startup-class-detection.md) e sottoscrive ognuno dei componenti middleware a un eventi pipeline integrata. Il codice di registrazione e OMC seguente consente ad esempio visualizzare la registrazione di eventi predefiniti di componenti middleware. (Per ulteriori istruzioni sulla creazione di una classe di avvio OWIN, vedere [OWIN avvio classe rilevamento](owin-startup-class-detection.md).)

1. Creare un progetto di applicazione web vuota e denominarla **owin2**.
2. Dal Package Manager Console (PMC), eseguire il comando seguente: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Aggiungere un `OWIN Startup Class` e denominarlo `Startup`. Sostituire il codice generato con il codice seguente (le modifiche vengono evidenziate):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Premere F5 per eseguire l'app.

La configurazione di avvio impostato una pipeline con middleware tre componenti, i primi due la visualizzazione di informazioni di diagnostica e l'ultima risposta agli eventi (e anche la visualizzazione di informazioni di diagnostica). Il `PrintCurrentIntegratedPipelineStage` metodo consente di visualizzare l'evento di pipeline integrata in cui viene richiamato questo middleware e un messaggio. L'output verrà visualizzato quanto segue:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Il runtime Katana eseguito il mapping di ogni componente middleware OWIN per [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) per impostazione predefinita, che corrisponde all'evento di pipeline IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Fase marcatori

È possibile contrassegnare OMCs da eseguire in specifiche fasi della pipeline utilizzando il `IAppBuilder UseStageMarker()` metodo di estensione. Per eseguire un set di componenti middleware durante una determinata fase, inserire un marcatore di fasi subito dopo l'ultimo componente impostato durante la registrazione. Esistono regole in quale fase della pipeline è possibile eseguire middleware e devono eseguire i componenti dell'ordine (le regole sono illustrate più avanti nell'esercitazione). Aggiungere il `UseStageMarker` metodo il `Configuration` codice come illustrato di seguito:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Il `app.UseStageMarker(PipelineStage.Authenticate)` chiamata consente di configurare tutti i componenti middleware registrato in precedenza (in questo caso, i due componenti diagnostica) per l'esecuzione in fase di autenticazione della pipeline. L'ultimo componente middleware (che consente di visualizzare la diagnostica e risponde alle richieste) verrà eseguito il `ResolveCache` fase (il [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) evento).

Premere F5 per eseguire l'app. La finestra di output viene illustrato quanto segue:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Fase di marcatore

Componenti di middleware Owin (OMC) possono essere configurati per eseguire gli eventi di fase della pipeline OWIN seguenti:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Per impostazione predefinita, OMCs eseguite in corrispondenza dell'ultimo evento (`PreHandlerExecute`). Ecco perché il primo codice di esempio visualizzato "PreExecuteRequestHandler".
2. È possibile utilizzare l'un `app.UseStageMarker` metodo per registrare un OMC eseguire versioni precedenti, in qualsiasi fase della pipeline OWIN elencati nel `PipelineStage` enum.
3. La pipeline OWIN e la pipeline IIS è ordinato, pertanto le chiamate a `app.UseStageMarker` deve essere in ordine. Non è possibile impostare il gestore dell'evento a un evento che precede l'ultimo evento registrato con `app.UseStageMarker`. Ad esempio, *dopo* chiamata:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

 le chiamate a `app.UseStageMarker` passando `Authenticate` o `PostAuthenticate` non sarà possibile applicare e non verrà generata alcuna eccezione. Eseguire durante la fase più recente, che per impostazione predefinita è OMCs `PreHandlerExecute`. Affinché vengano eseguiti in precedenza, vengono utilizzati i marcatori di fase. Se si specificano l'ordine marcatori di fase, si arrotondare al marcatore della precedente. In altre parole, l'aggiunta di un marcatore di fasi afferma "Esegui entro fase X". Esecuzione dell'OMC al marcatore di fasi primo aggiunto successivamente nella pipeline OWIN.
4. La prima fase il numero di chiamate al `app.UseStageMarker` wins. Ad esempio, se si passa l'ordine di `app.UseStageMarker` chiamate dall'esempio precedente:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

 Verrà visualizzata la finestra di output: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

 Il OMCs eseguiti nel `AuthenticateRequest` fase, perché l'ultima OMC registrato con il `Authenticate` evento e `Authenticate` evento precede tutti gli altri eventi.
