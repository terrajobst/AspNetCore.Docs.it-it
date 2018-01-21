---
title: Inserimento di dipendenze nelle viste
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: cade61b1ebdb2b845b07117384475638c0227f7f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-views"></a>Inserimento di dipendenze nelle viste

Da [Steve Smith](https://ardalis.com/)

Supporta ASP.NET Core [inserimento di dipendenze](xref:fundamentals/dependency-injection) nelle viste. Questo può essere utile per i servizi di visualizzazione specifica, ad esempio localizzazione o dati necessari solo per il popolamento di elementi di visualizzazione. È consigliabile mantenere [separazione delle problematiche](http://deviq.com/separation-of-concerns/) tra il controller e visualizzazioni. La maggior parte dei dati di che visualizzano le visualizzazioni devono essere passata dal controller.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="a-simple-example"></a>Un esempio semplice

È possibile inserire un servizio in una visualizzazione utilizzando il `@inject` direttiva. È possibile considerare `@inject` come aggiunta di una proprietà per la visualizzazione e la proprietà utilizzando DI popolamento.

La sintassi per `@inject`:`@inject <type> <name>`

Un esempio di `@inject` in azione:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Consente di visualizzare un elenco di `ToDoItem` istanze, insieme a un riepilogo che mostra statistiche generali. Il riepilogo è popolato da inserita `StatisticsService`. Questo servizio è registrato per l'inserimento di dipendenze in `ConfigureServices` in *Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

Il `StatisticsService` esegue i calcoli sul set di `ToDoItem` istanze, a cui accede tramite un repository:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Il repository di esempio utilizza una raccolta in memoria. L'implementazione illustrato in precedenza (che agisce su tutti i dati in memoria) non è consigliata per i set di dati di grandi dimensioni, a cui si accede in remoto.

Nell'esempio vengono visualizzati i dati del modello associato alla vista e il servizio inserito nella vista:

![Per visualizzare l'elenco di elementi totali, completare gli elementi, la priorità Media e un elenco di attività con i relativi livelli di priorità e i valori booleani che indica il completamento.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Inserimento dei dati di ricerca

Attacco intrusivo nel codice di visualizzazione può essere utile per popolare le opzioni di elementi dell'interfaccia utente, ad esempio elenchi a discesa. Si consideri un modulo di profilo utente che include opzioni per la specifica relativa al sesso, stato e altre preferenze. Rendering di questo tipo un form utilizzando un approccio MVC standard richiederebbe il controller per richiedere dati di servizi di accesso per ognuno di questi set di opzioni e quindi compilare un modello o `ViewBag` con ogni set di opzioni da associare.

Un approccio alternativo inserisce i servizi direttamente nella visualizzazione per ottenere le opzioni. In questo modo viene ridotta la quantità di codice necessario per il controller, spostare la logica di costruzione di questo elemento di visualizzazione nella vista stessa. Azione del controller per visualizzare un form di modifica del profilo è sufficiente passare il form l'istanza di profilo:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

Il form HTML utilizzato per aggiornare queste preferenze include elenchi a discesa per tre delle proprietà:

![Aggiornare la vista profilo con un modulo che consente l'immissione di nome, sesso, stato e il colore preferito.](dependency-injection/_static/updateprofile.png)

Questi elenchi vengono popolati da un servizio che è stato inserito nella visualizzazione:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

Il `ProfileOptionsService` è un servizio a livello di interfaccia utente progettato per fornire solo i dati necessari per questo form:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Non dimenticare di registrare tipi richiesto tramite l'inserimento di dipendenze nel `ConfigureServices` metodo *Startup.cs*.

## <a name="overriding-services"></a>Si esegue l'override di servizi

Oltre l'inserimento di nuovi servizi, questa tecnica può essere usata anche per eseguire l'override di servizi precedentemente inseriti in una pagina. La figura seguente mostra tutti i campi disponibili nella pagina utilizzato nel primo esempio:

![Menu di scelta rapida di IntelliSense in un oggetto tipizzato elenco dei campi Html, componente, StatsService e Url simbolo @](dependency-injection/_static/razor-fields.png)

Come si può notare, i campi predefiniti includono `Html`, `Component`, e `Url` (così come il `StatsService` che abbiamo inserito). Se ad esempio si desidera sostituire l'helper HTML predefinita con la propria, è possibile farlo facilmente con `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Se si desidera estendere i servizi esistenti, è possibile utilizzare questa tecnica semplicemente durante ereditare o disposizione testo per l'implementazione esistente con il proprio.

## <a name="see-also"></a>Vedere anche

* Blog di Simon Timms: [recupero dei dati di ricerca nella visualizzazione](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
