---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: La convalida in ASP.NET Web API del modello | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a>Convalida del modello in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando un client invia dati per l'API web, spesso si desidera convalidare i dati prima di eseguire qualsiasi elaborazione. In questo articolo viene illustrato come annotare i modelli, utilizzare le annotazioni per la convalida dei dati e gestire gli errori di convalida in web API.

## <a name="data-annotations"></a>Annotazioni dei dati

In ASP.NET Web API, è possibile utilizzare gli attributi di [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi per impostare le regole di convalida per le proprietà nel modello. Prendere in considerazione il modello seguente:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se è stata utilizzata la convalida del modello in ASP.NET MVC, questo dovrebbe essere noti. Il **necessari** attributo indica che il `Name` proprietà non deve essere null. Il **intervallo** attributo afferma che `Weight` deve essere compreso tra 0 e 999.

Si supponga che un client invia una richiesta POST con la rappresentazione JSON seguente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

È possibile vedere che il client non include il `Name` proprietà, che è contrassegnato come richiesto. Quando API Web Converte la stringa JSON in un `Product` istanza, convalida il `Product` contro gli attributi di convalida. Nell'ambito dell'azione del controller, è possibile controllare se il modello è valido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Convalida del modello non garantisce che i dati del client sono protetta. Convalida aggiuntiva potrebbe essere necessario in altri livelli dell'applicazione. (Ad esempio, il livello di dati potrebbe imporre vincoli di chiave esterna). L'esercitazione [tramite Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) esamina alcuni di questi problemi.

**"Registrazione insufficiente"**: la registrazione si verifica quando il client esclude alcune proprietà. Si supponga, ad esempio, che il client invia le operazioni seguenti:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

In questo caso, il client non ha specificato i valori per `Price` o `Weight`. Il formattatore JSON assegna un valore predefinito pari a zero per le proprietà mancanti.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Lo stato del modello è valido, perché un valore valido per queste proprietà è pari a zero. Se si tratta di un problema dipende dallo scenario. Ad esempio, in un'operazione di aggiornamento, è consigliabile distinguere tra "0" e "non impostata." Per imporre ai client per impostare un valore, impostare la proprietà ammette valori null e impostare il **necessari** attributo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Registrazione eccessiva"**: un client può inoltre inviare *più* dati tempo del previsto. Ad esempio:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

In questo caso, la stringa JSON include una proprietà ("colori") che non esiste nel `Product` modello. In questo caso, il formattatore JSON ignora semplicemente questo valore. (Il formattatore XML non uguale). Registrazione eccessiva può causare problemi se il modello contiene le proprietà che si deve essere di sola lettura. Ad esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Non si desidera aggiornare il `IsAdmin` proprietà ed elevare i loro agli amministratori. La strategia più sicura consiste nell'utilizzare una classe modello che corrisponde esattamente a ciò che il client è autorizzato a inviare:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Post di blog di Brad Wilson "[vs di convalida dell'Input. Modello di convalida in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"presenta una trattazione la registrazione e la registrazione in eccesso. Sebbene il post su ASP.NET MVC 2, i problemi sono ancora rilevanti per l'API Web.


## <a name="handling-validation-errors"></a>Gestione degli errori di convalida

API Web non restituisce un errore automaticamente al client durante la convalida non riesce. È responsabilità dell'azione del controller per controllare lo stato del modello e di rispondere in modo appropriato.

È anche possibile creare un filtro di azione per controllare lo stato del modello prima che venga richiamato l'azione del controller. Il codice seguente viene illustrato un esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se si verifica un errore di convalida del modello, questo filtro restituisce una risposta HTTP che contiene gli errori di convalida. In tal caso, l'azione del controller non viene richiamato.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Per applicare il filtro a tutti i controller API Web, aggiungere un'istanza del filtro per il **HttpConfiguration.Filters** raccolta durante la configurazione:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Un'altra opzione consiste nell'impostare il filtro come attributo nel controller singoli o le azioni del controller:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
