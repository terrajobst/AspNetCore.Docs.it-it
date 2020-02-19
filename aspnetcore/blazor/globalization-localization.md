---
title: ASP.NET Core Blazor la globalizzazione e la localizzazione
author: guardrex
description: Informazioni su come rendere i componenti Razor accessibili agli utenti in più impostazioni cultura e lingue.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453268"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a><span data-ttu-id="446ef-103">ASP.NET Core Blazor la globalizzazione e la localizzazione</span><span class="sxs-lookup"><span data-stu-id="446ef-103">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="446ef-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="446ef-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="446ef-105">I componenti Razor possono essere resi accessibili agli utenti in più impostazioni cultura e lingue.</span><span class="sxs-lookup"><span data-stu-id="446ef-105">Razor components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="446ef-106">Sono disponibili i seguenti scenari di globalizzazione e localizzazione di .NET:</span><span class="sxs-lookup"><span data-stu-id="446ef-106">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="446ef-107">. Sistema di risorse di NET</span><span class="sxs-lookup"><span data-stu-id="446ef-107">.NET's resources system</span></span>
* <span data-ttu-id="446ef-108">Formattazione di numeri e date specifiche delle impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="446ef-108">Culture-specific number and date formatting</span></span>

<span data-ttu-id="446ef-109">Sono attualmente supportati un set limitato di scenari di localizzazione di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="446ef-109">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="446ef-110">`IStringLocalizer<>` *è supportata* nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="446ef-110">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="446ef-111">la localizzazione `IHtmlLocalizer<>`, `IViewLocalizer<>`e le annotazioni dei dati sono ASP.NET Core scenari MVC e **non sono supportate** nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="446ef-111">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="446ef-112">Per ulteriori informazioni, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="446ef-112">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="446ef-113">Globalizzazione</span><span class="sxs-lookup"><span data-stu-id="446ef-113">Globalization</span></span>

<span data-ttu-id="446ef-114">la funzionalità `@bind` di Blazoresegue formati e analizza i valori per la visualizzazione in base alle impostazioni cultura correnti dell'utente.</span><span class="sxs-lookup"><span data-stu-id="446ef-114">Blazor's `@bind` functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="446ef-115">È possibile accedere alle impostazioni cultura correnti dalla proprietà <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="446ef-115">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="446ef-116">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) viene usato per i tipi di campo seguenti (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="446ef-116">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="446ef-117">I tipi di campo precedenti:</span><span class="sxs-lookup"><span data-stu-id="446ef-117">The preceding field types:</span></span>

* <span data-ttu-id="446ef-118">Vengono visualizzati utilizzando le regole di formattazione appropriate basate su browser.</span><span class="sxs-lookup"><span data-stu-id="446ef-118">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="446ef-119">Non può contenere testo in formato libero.</span><span class="sxs-lookup"><span data-stu-id="446ef-119">Can't contain free-form text.</span></span>
* <span data-ttu-id="446ef-120">Fornire le caratteristiche di interazione utente in base all'implementazione del browser.</span><span class="sxs-lookup"><span data-stu-id="446ef-120">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="446ef-121">I tipi di campo seguenti hanno requisiti di formattazione specifici e non sono attualmente supportati da Blazor perché non sono supportati da tutti i browser principali:</span><span class="sxs-lookup"><span data-stu-id="446ef-121">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="446ef-122">`@bind` supporta il parametro `@bind:culture` per fornire un <xref:System.Globalization.CultureInfo?displayProperty=fullName> per l'analisi e la formattazione di un valore.</span><span class="sxs-lookup"><span data-stu-id="446ef-122">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="446ef-123">Non è consigliabile specificare impostazioni cultura quando si usano i tipi di campo `date` e `number`.</span><span class="sxs-lookup"><span data-stu-id="446ef-123">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="446ef-124">`date` e `number` hanno un supporto incorporato Blazor che fornisce le impostazioni cultura richieste.</span><span class="sxs-lookup"><span data-stu-id="446ef-124">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="446ef-125">Localizzazione</span><span class="sxs-lookup"><span data-stu-id="446ef-125">Localization</span></span>

<span data-ttu-id="446ef-126">le app di Blazor server sono localizzate usando il [middleware di localizzazione](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="446ef-126">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="446ef-127">Il middleware seleziona le impostazioni cultura appropriate per gli utenti che richiedono risorse dall'app.</span><span class="sxs-lookup"><span data-stu-id="446ef-127">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="446ef-128">Le impostazioni cultura possono essere impostate utilizzando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="446ef-128">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="446ef-129">Cookie</span><span class="sxs-lookup"><span data-stu-id="446ef-129">Cookies</span></span>](#cookies)
* [<span data-ttu-id="446ef-130">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="446ef-130">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="446ef-131">Per altre informazioni ed esempi, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="446ef-131">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="446ef-132">Configurare il linker per l'internazionalizzazione (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="446ef-132">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="446ef-133">Per impostazione predefinita, la configurazione del linker Blazorper le app Blazor webassembly rimuove le informazioni di internazionalizzazione ad eccezione delle impostazioni locali richieste in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="446ef-133">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="446ef-134">Per ulteriori informazioni e istruzioni sul controllo del comportamento del linker, vedere <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="446ef-134">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="446ef-135">Cookie</span><span class="sxs-lookup"><span data-stu-id="446ef-135">Cookies</span></span>

<span data-ttu-id="446ef-136">Un cookie di impostazioni cultura di localizzazione può salvare in maniera permanente le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="446ef-136">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="446ef-137">Il cookie viene creato tramite il metodo `OnGet` della pagina host dell'app (*pages/host. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="446ef-137">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="446ef-138">Il middleware di localizzazione legge il cookie nelle richieste successive per impostare le impostazioni cultura dell'utente.</span><span class="sxs-lookup"><span data-stu-id="446ef-138">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="446ef-139">L'uso di un cookie garantisce che la connessione WebSocket possa propagare correttamente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="446ef-139">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="446ef-140">Se gli schemi di localizzazione sono basati sul percorso dell'URL o sulla stringa di query, lo schema potrebbe non essere in grado di funzionare con WebSocket, quindi non è possibile salvare in modo permanente le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="446ef-140">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="446ef-141">Pertanto, l'utilizzo di un cookie di impostazioni cultura di localizzazione è l'approccio consigliato.</span><span class="sxs-lookup"><span data-stu-id="446ef-141">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="446ef-142">Qualsiasi tecnica può essere utilizzata per assegnare impostazioni cultura se le impostazioni cultura vengono rese permanente in un cookie di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="446ef-142">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="446ef-143">Se l'app dispone già di uno schema di localizzazione stabilito per ASP.NET Core lato server, continuare a usare l'infrastruttura di localizzazione esistente dell'app e impostare il cookie delle impostazioni cultura di localizzazione nello schema dell'app.</span><span class="sxs-lookup"><span data-stu-id="446ef-143">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="446ef-144">Nell'esempio seguente viene illustrato come impostare le impostazioni cultura correnti in un cookie che può essere letto dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="446ef-144">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="446ef-145">Creare un file *pages/host. cshtml. cs* con il contenuto seguente nell'app Blazor server:</span><span class="sxs-lookup"><span data-stu-id="446ef-145">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="446ef-146">La localizzazione viene gestita dall'app nella sequenza di eventi seguente:</span><span class="sxs-lookup"><span data-stu-id="446ef-146">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="446ef-147">Il browser invia una richiesta HTTP iniziale all'app.</span><span class="sxs-lookup"><span data-stu-id="446ef-147">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="446ef-148">Le impostazioni cultura vengono assegnate dal middleware di localizzazione.</span><span class="sxs-lookup"><span data-stu-id="446ef-148">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="446ef-149">Il metodo `OnGet` in *_Host. cshtml. cs* rende permanente le impostazioni cultura di un cookie come parte della risposta.</span><span class="sxs-lookup"><span data-stu-id="446ef-149">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="446ef-150">Il browser apre una connessione WebSocket per creare una sessione interattiva di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="446ef-150">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="446ef-151">Il middleware di localizzazione legge il cookie e assegna le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="446ef-151">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="446ef-152">La sessione del server Blazor inizia con le impostazioni cultura corrette.</span><span class="sxs-lookup"><span data-stu-id="446ef-152">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="446ef-153">Fornire l'interfaccia utente per scegliere le impostazioni cultura</span><span class="sxs-lookup"><span data-stu-id="446ef-153">Provide UI to choose the culture</span></span>

<span data-ttu-id="446ef-154">Per consentire a un utente di selezionare le impostazioni cultura, è consigliabile un *approccio basato su Reindirizzamento* .</span><span class="sxs-lookup"><span data-stu-id="446ef-154">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="446ef-155">Il processo è simile a quello che accade in un'app Web quando un utente tenta di accedere a una risorsa protetta&mdash;l'utente viene reindirizzato a una pagina di accesso e quindi reindirizzato di nuovo alla risorsa originale.</span><span class="sxs-lookup"><span data-stu-id="446ef-155">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="446ef-156">L'app rende permanente le impostazioni cultura selezionate dall'utente tramite un reindirizzamento a un controller.</span><span class="sxs-lookup"><span data-stu-id="446ef-156">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="446ef-157">Il controller imposta le impostazioni cultura selezionate dall'utente in un cookie e reindirizza di nuovo l'utente all'URI originale.</span><span class="sxs-lookup"><span data-stu-id="446ef-157">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="446ef-158">Stabilire un endpoint HTTP nel server per impostare le impostazioni cultura selezionate dall'utente in un cookie ed eseguire di nuovo il reindirizzamento all'URI originale:</span><span class="sxs-lookup"><span data-stu-id="446ef-158">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="446ef-159">Usare il risultato dell'azione `LocalRedirect` per impedire gli attacchi di reindirizzamento aperti.</span><span class="sxs-lookup"><span data-stu-id="446ef-159">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="446ef-160">Per ulteriori informazioni, vedere <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="446ef-160">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="446ef-161">Il componente seguente mostra un esempio di come eseguire il reindirizzamento iniziale quando l'utente seleziona le impostazioni cultura:</span><span class="sxs-lookup"><span data-stu-id="446ef-161">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="446ef-162">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="446ef-162">Additional resources</span></span>

* <xref:fundamentals/localization>
