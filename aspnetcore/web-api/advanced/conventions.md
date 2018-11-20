---
title: Usare le convenzioni dell'API Web
author: pranavkm
description: Informazioni sulle convenzioni dell'API Web in ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 023b8d09511aa42966e2a7d1c85e407bb6e79b0f
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635394"
---
# <a name="use-web-api-conventions"></a>Usare le convenzioni dell'API Web

ASP.NET Core 2.2 introduce una modalità per l'estrazione di [documentazione dell'API](xref:tutorials/web-api-help-pages-using-swagger) comune e l'applicazione della documentazione a più azioni o controller oppure a tutti i controller inclusi in un assembly. Le convenzioni dell'API Web sostituiscono l'aggiunta di [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) alle singole azioni. La soluzione consente di definire i tipi restituiti "tradizionali" più comuni e i codici di stato restituiti dall'azione, con una modalità per selezionare il metodo della convenzione che si applica a un'azione.

Per impostazione predefinita, ASP.NET Core MVC 2.2 include il set di convenzioni predefinite `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Le convenzioni sono basate sul controller di cui ASP.NET Core esegue lo scaffolding. Se le azioni seguono il criterio prodotto dallo scaffolding, è possibile usare le convenzioni predefinite.

In fase di runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> riconosce le convenzioni. `ApiExplorer` è l'astrazione MVC per comunicare con i generatori di documenti Open API. Gli attributi della convenzione applicata vengono associati a un'azione e sono inclusi nella documentazione Swagger dell'azione. Anche gli analizzatori di API supportano le convenzioni. Se l'azione non è convenzionale (ad esempio, se restituisce un codice di stato non documentato dalla convenzione applicata), genera un avviso richiedendo di documentarla.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Applicare le convenzioni dell'API Web

Esistono tre modi per applicare una convenzione. Le convenzioni non sono componibili: ogni azione può essere associata a una sola convenzione. Le convenzioni più specifiche (illustrate di seguito) hanno la precedenza su quelle meno specifiche. Quando si applicano a un'azione due o più convenzioni con la stessa priorità, la selezione è non deterministica. Per applicare una convenzione a un'azione sono disponibili le opzioni seguenti, dalla più specifica alla meno specifica:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; È valida per azioni singole, specifica il tipo di convenzione e il metodo della convenzione applicato. Nell'esempio seguente, il metodo della convenzione `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` viene applicato all'azione `Update`:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un controller &mdash; Applica il tipo di convenzione a tutte le azioni del controller. I metodi convenzione sono provvisti di hint che determinano le azioni alle quali possono essere applicati (dettagli nel contesto della creazione di convenzioni). Ad esempio:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applicato a un an assembly &mdash; Applica il tipo di convenzione a tutti i controller dell'assembly corrente. Ad esempio:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Creare convenzioni dell'API Web

Una convenzione è un tipo statico con metodi. Questi metodi vengono annotati con attributi `[ProducesResponseType]` o `[ProducesDefaultResponseType]`.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Se si applica questa convenzione a un assembly, il metodo della convenzione viene applicato a qualsiasi azione con nome `Find` che ha esattamente un parametro con nome `id` e non ha altri attributi di metadati più specifici.

Oltre a `[ProducesResponseType]` e `[ProducesDefaultResponseType]`, è possibile applicare `[ApiConventionNameMatch]` e `[ApiConventionTypeMatch]` al metodo convenzione che determina i metodi ai quali vengono applicati. Ad esempio:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* L'opzione `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` applicata al metodo indica che la convenzione può corrispondere a qualsiasi azione con prefisso "Find". Sono inclusi metodi come `Find`, `FindPet` o `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applicato al parametro indica che la convenzione può corrispondere a metodi che hanno esattamente un parametro che termina con l'identificatore del suffisso. Sono inclusi parametri come `id` o `petId`. `ApiConventionTypeMatch` può essere applicato in modo analogo ai tipi, per vincolare il tipo del parametro. È possibile usare un argomento `params[]` per indicare i parametri rimanenti, per i quali non è necessario che venga rilevata una corrispondenza esplicita.
