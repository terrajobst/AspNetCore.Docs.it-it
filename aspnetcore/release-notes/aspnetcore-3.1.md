---
title: Novità di ASP.NET Core 3,1
author: rick-anderson
description: Informazioni sulle nuove funzionalità di ASP.NET Core 3,1.
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: f375022ad3ebdea2990f626320ef295926f88c22
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447438"
---
# <a name="whats-new-in-aspnet-core-31"></a>Novità di ASP.NET Core 3,1

In questo articolo vengono evidenziate le modifiche più significative in ASP.NET Core 3,1 con i collegamenti alla documentazione pertinente.

## <a name="partial-class-support-for-razor-components"></a>Supporto delle classi parziali per i componenti Razor

I componenti Razor sono ora generati come classi parziali. Il codice per un componente Razor può essere scritto usando un file code-behind definito come classe parziale anziché definire tutto il codice per il componente in un unico file. Per ulteriori informazioni, vedere [supporto di classi parziali](xref:blazor/components#partial-class-support).

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor Helper Tag Component e passare i parametri ai componenti di primo livello

In Blazor con ASP.NET Core 3,0, i componenti sono stati sottoposti a rendering in pagine e visualizzazioni usando un helper HTML (`Html.RenderComponentAsync`). In ASP.NET Core 3,1 eseguire il rendering di un componente da una pagina o da una vista con il nuovo Helper tag di componente:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

L'helper HTML rimane supportato in ASP.NET Core 3,1, ma è consigliabile usare l'helper Tag Component.

le app Server Blazor possono ora passare parametri ai componenti di primo livello durante il rendering iniziale. In precedenza era possibile passare i parametri solo a un componente di primo livello con [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static). Con questa versione sono supportati sia [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) che [RenderModel. ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) . Qualsiasi valore di parametro specificato viene serializzato come JSON e incluso nella risposta iniziale.

Ad esempio, prerenderizzare un componente `Counter` con un importo di incremento (`IncrementAmount`):

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Per altre informazioni, vedere [integrare i componenti nelle app Razor Pages e MVC](xref:blazor/integrate-components).

## <a name="support-for-shared-queues-in-httpsys"></a>Supporto per le code condivise in HTTP. sys

[Http. sys](xref:fundamentals/servers/httpsys) supporta la creazione di code di richieste anonime. In ASP.NET Core 3,1 è stata aggiunta la possibilità di creare o connettersi a una coda di richieste HTTP. sys denominata esistente. La creazione o la connessione a una coda di richieste HTTP. sys denominata esistente Abilita scenari in cui il processo del controller HTTP. sys proprietario della coda è indipendente dal processo del listener. Questa indipendenza rende possibile la conservazione delle connessioni esistenti e delle richieste accodate tra i riavvii del processo del listener:

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a>Modifiche di rilievo per i cookie navigava sullostesso sito

Il comportamento dei cookie navigava sullostesso sito è stato modificato in modo da riflettere le modifiche imminenti del browser. Questo può influire su scenari di autenticazione come AzureAd, OpenIdConnect o WsFederation. Per altre informazioni, vedere <xref:security/samesite>.

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a>Impedisci azioni predefinite per gli eventi nelle app Blazor

Usare l'attributo `@on{EVENT}:preventDefault` Directive per impedire l'azione predefinita per un evento. Nell'esempio seguente viene impedita l'azione predefinita di visualizzazione del carattere della chiave nella casella di testo:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

Per ulteriori informazioni, vedere [Impedisci azioni predefinite](xref:blazor/event-handling#prevent-default-actions).

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a>Arresta la propagazione degli eventi nelle app Blazor

Usare l'attributo della direttiva `@on{EVENT}:stopPropagation` per arrestare la propagazione degli eventi. Nell'esempio seguente la selezione della casella di controllo impedisce la propagazione degli eventi click dal `<div>` figlio al `<div>`padre:

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

Per altre informazioni, vedere [arrestare la propagazione degli eventi](xref:blazor/event-handling#stop-event-propagation).

## <a name="detailed-errors-during-opno-locblazor-app-development"></a>Errori dettagliati durante lo sviluppo di app Blazor

Quando un'app Blazor non funziona correttamente durante lo sviluppo, la ricezione di informazioni dettagliate sugli errori dall'app è utile per la risoluzione dei problemi e per risolvere il problema. Quando si verifica un errore, Blazor app visualizzano una barra dorata nella parte inferiore della schermata:

* Durante lo sviluppo, la barra dorata indirizza l'utente alla console del browser, in cui è possibile visualizzare l'eccezione.
* In produzione, la barra dorata informa l'utente che si è verificato un errore e consiglia di aggiornare il browser.

Per ulteriori informazioni, vedere [errori dettagliati durante lo sviluppo](xref:blazor/handle-errors#detailed-errors-during-development).
