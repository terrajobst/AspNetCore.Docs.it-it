---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Convalida del modello in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393096"
---
<a name="model-validation-in-aspnet-web-api"></a>Convalida del modello in API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Quando un client invia dati all'API Web, spesso si desidera convalidare i dati prima di eseguire qualsiasi elaborazione. Questo articolo illustra come annotare i modelli, usare le annotazioni per la convalida dei dati e la gestione degli errori di convalida nell'API web.

## <a name="data-annotations"></a>Annotazioni dei dati

Nell'API Web ASP.NET, è possibile usare gli attributi dal [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) dello spazio dei nomi per impostare le regole di convalida per le proprietà nel modello. Si consideri il modello seguente:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se è stata usata la convalida del modello in ASP.NET MVC, si dovrebbe avere familiarità. Il **necessari** attributo indica che il `Name` proprietà non può essere null. Il **Range** attributo indica che `Weight` deve essere compreso tra 0 e 999.

Si supponga che un client invia una richiesta POST con la rappresentazione JSON seguente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

È possibile vedere che il client non comprendeva i `Name` proprietà, che è contrassegnato come obbligatorio. Quando Web API converte il codice JSON in una `Product` istanza, vengono convalidate le `Product` contro gli attributi di convalida. Nell'ambito dell'azione del controller, è possibile controllare se il modello è valido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Convalida del modello non garantisce che i dati del client sono sicuri. Potrebbe essere necessaria una convalida aggiuntiva in altri livelli dell'applicazione. (Ad esempio, il livello di dati potrebbe imporre vincoli di chiave esterna). L'esercitazione [uso di API Web con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) esamina alcuni di questi problemi.

**"Pubblicando correggerà"**: correggerà la registrazione si verifica quando il client esclude alcune proprietà. Si supponga, ad esempio, che il client invia la seguente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

In questo caso, il client non sono stati specificati valori per `Price` o `Weight`. Il formattatore JSON assegna un valore predefinito pari a zero per le proprietà mancante.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Lo stato del modello è valido, poiché lo zero è un valore valido per queste proprietà. Se si tratta di un problema dipende dallo scenario. Ad esempio, in un'operazione di aggiornamento, si potrebbe voler distinguere tra "0" e "non impostata." Per forzare i client per impostare un valore, impostare la proprietà che ammette valori null e viene impostata la **necessari** attributo:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Overposting"**: un client può anche inviare *altre* dati quanto ci si aspetterebbe. Ad esempio:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

In questo caso, il codice JSON include una proprietà ("colore") che non esiste nel `Product` modello. In questo caso, il formattatore JSON ignora semplicemente questo valore. (Il formattatore XML esegue la stessa). Evitare l'overposting può causare problemi se il modello contiene le proprietà che si devono essere di sola lettura. Ad esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Preferibile che gli utenti per l'aggiornamento di `IsAdmin` proprietà ed elevare se stesso agli amministratori. La strategia più sicura consiste nell'usare una classe di modello che corrisponde esattamente a ciò che il client è autorizzato a inviare:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Post di blog di Brad Wilson "[vs: convalida dell'Input. Convalida del modello in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"dispone per una trattazione correggerà la registrazione, evitare l'overposting. Anche se il post è su ASP.NET MVC 2, i problemi sono ancora rilevanti per l'API Web.


## <a name="handling-validation-errors"></a>Gestione degli errori di convalida

API Web non restituire un errore automaticamente al client quando si verifica un errore di convalida. È responsabilità dell'azione del controller per verificare lo stato del modello e rispondere in modo appropriato.

È anche possibile creare un filtro azioni per controllare lo stato del modello prima che venga richiamato l'azione del controller. Il codice seguente viene illustrato un esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se si verifica un errore di convalida del modello, questo filtro restituisce una risposta HTTP che contiene gli errori di convalida. In tal caso, non viene richiamata l'azione del controller.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Per applicare il filtro selezionato a tutti i controller API Web, aggiungere un'istanza del filtro per il **HttpConfiguration.Filters** raccolta durante la configurazione:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Un'altra opzione consiste nell'impostare il filtro come attributo in singoli controller o azioni del controller:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
