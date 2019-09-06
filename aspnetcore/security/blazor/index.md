---
title: Autenticazione e autorizzazione per ASP.NET Core Blazor
author: guardrex
description: Informazioni sugli scenari di autenticazione e autorizzazione per Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2019
uid: security/blazor/index
ms.openlocfilehash: 8714acbeb6e8a00992a601030811b24f53426b82
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310519"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="42299-103">Autenticazione e autorizzazione per ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="42299-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="42299-104">Di [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="42299-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="42299-105">ASP.NET Core supporta la configurazione e la gestione della sicurezza nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="42299-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="42299-106">Gli scenari di sicurezza sono diversi per le app Blazor sul lato server e sul lato client.</span><span class="sxs-lookup"><span data-stu-id="42299-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="42299-107">Dato che le app sul lato server Blazor vengono eseguite nel server, i controlli di autorizzazione consentono di determinare:</span><span class="sxs-lookup"><span data-stu-id="42299-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="42299-108">Le opzioni dell'interfaccia utente presentate all'utente (ad esempio, le voci di menu disponibili per un utente).</span><span class="sxs-lookup"><span data-stu-id="42299-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="42299-109">Le regole di accesso per le aree dell'app e i componenti.</span><span class="sxs-lookup"><span data-stu-id="42299-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="42299-110">Le app sul lato client Blazor eseguite nel client.</span><span class="sxs-lookup"><span data-stu-id="42299-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="42299-111">L'autorizzazione viene usata *solo* per determinare quali opzioni dell'interfaccia utente visualizzare.</span><span class="sxs-lookup"><span data-stu-id="42299-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="42299-112">Dato che i controlli sul lato client possono essere modificati o ignorati dall'utente, un'app sul lato client Blazor non può imporre regole di accesso di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="42299-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="42299-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="42299-113">Authentication</span></span>

<span data-ttu-id="42299-114">Blazor usa i meccanismi di autenticazione di ASP.NET Core esistenti per stabilire l'identità dell'utente.</span><span class="sxs-lookup"><span data-stu-id="42299-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="42299-115">Il meccanismo esatto dipende dal modo in cui è ospitata l'app Blazor, ovvero sul lato server o sul lato client.</span><span class="sxs-lookup"><span data-stu-id="42299-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="42299-116">Autenticazione sul lato server Blazor</span><span class="sxs-lookup"><span data-stu-id="42299-116">Blazor server-side authentication</span></span>

<span data-ttu-id="42299-117">Le app sul lato server Blazor operano su una connessione in tempo reale creata tramite SignalR.</span><span class="sxs-lookup"><span data-stu-id="42299-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="42299-118">L'[autenticazione nelle app basate su SignalR](xref:signalr/authn-and-authz) viene gestita quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="42299-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="42299-119">L'autenticazione può basarsi su un cookie o su altri bearer token.</span><span class="sxs-lookup"><span data-stu-id="42299-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="42299-120">Il modello di progetto sul lato server Blazor può configurare l'autenticazione per l'utente quando viene creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="42299-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42299-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42299-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="42299-122">Seguire le istruzioni per Visual Studio nell'articolo <xref:blazor/get-started> per creare un nuovo progetto sul lato server Blazor con un meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="42299-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="42299-123">Dopo aver scelto il modello **App server Blazor** nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**, selezionare **Modifica** in **Autenticazione** .</span><span class="sxs-lookup"><span data-stu-id="42299-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="42299-124">Viene visualizzata una finestra di dialogo che offre lo stesso set di meccanismi di autenticazione disponibili per altri progetti ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="42299-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="42299-125">**Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="42299-125">**No Authentication**</span></span>
* <span data-ttu-id="42299-126">**Account utente individuali** &ndash; Gli account utente possono essere archiviati:</span><span class="sxs-lookup"><span data-stu-id="42299-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="42299-127">All'interno dell'app usando il sistema di [identità](xref:security/authentication/identity) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42299-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="42299-128">Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="42299-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="42299-129">**Account aziendali o dell'istituto di istruzione**</span><span class="sxs-lookup"><span data-stu-id="42299-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="42299-130">**Autenticazione di Windows**</span><span class="sxs-lookup"><span data-stu-id="42299-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="42299-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="42299-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="42299-132">Seguire le istruzioni per Visual Studio Code nell'articolo <xref:blazor/get-started> per creare un nuovo progetto sul lato server Blazor con un meccanismo di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="42299-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="42299-133">I valori di autenticazione consentiti (`{AUTHENTICATION}`) sono indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="42299-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="42299-134">Meccanismo di autenticazione</span><span class="sxs-lookup"><span data-stu-id="42299-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="42299-135">Valore di `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="42299-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="42299-136">Nessuna autenticazione</span><span class="sxs-lookup"><span data-stu-id="42299-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="42299-137">Utenti</span><span class="sxs-lookup"><span data-stu-id="42299-137">Individual</span></span><br><span data-ttu-id="42299-138">individuali archiviati nell'app con ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="42299-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="42299-139">Utenti</span><span class="sxs-lookup"><span data-stu-id="42299-139">Individual</span></span><br><span data-ttu-id="42299-140">individuali archiviati in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="42299-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="42299-141">Account aziendali o dell'istituto di istruzione</span><span class="sxs-lookup"><span data-stu-id="42299-141">Work or School Accounts</span></span><br><span data-ttu-id="42299-142">Autenticazione aziendale per un singolo tenant.</span><span class="sxs-lookup"><span data-stu-id="42299-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="42299-143">Account aziendali o dell'istituto di istruzione</span><span class="sxs-lookup"><span data-stu-id="42299-143">Work or School Accounts</span></span><br><span data-ttu-id="42299-144">Autenticazione aziendale per più tenant.</span><span class="sxs-lookup"><span data-stu-id="42299-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="42299-145">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="42299-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="42299-146">Il comando crea una cartella denominata con il valore fornito per il segnaposto `{APP NAME}` e il nome della cartella viene usato come nome dell'app.</span><span class="sxs-lookup"><span data-stu-id="42299-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="42299-147">Per altre informazioni, vedere il comando [dotnet new](/dotnet/core/tools/dotnet-new) nella guida per .NET Core.</span><span class="sxs-lookup"><span data-stu-id="42299-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="42299-148">Autenticazione sul lato client Blazor</span><span class="sxs-lookup"><span data-stu-id="42299-148">Blazor client-side authentication</span></span>

<span data-ttu-id="42299-149">Nelle app sul lato client Blazor i controlli di autenticazione possono essere ignorati perché tutto il codice sul lato client può essere modificato dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="42299-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="42299-150">Lo stesso vale per tutte le tecnologie per app sul lato client, tra cui i framework JavaScript SPA o le app native per qualsiasi sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="42299-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="42299-151">L'implementazione di un servizio `AuthenticationStateProvider` personalizzato per le app sul lato client Blazor è illustrato nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="42299-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="42299-152">Servizio AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="42299-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="42299-153">Le app sul lato server Blazor includono un servizio `AuthenticationStateProvider` predefinito che ottiene i dati relativi allo stato di autenticazione da `HttpContext.User` di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42299-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="42299-154">Questo è il modo in cui lo stato di autenticazione si integra con i meccanismi di autenticazione sul lato server ASP.NET Core esistenti.</span><span class="sxs-lookup"><span data-stu-id="42299-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="42299-155">`AuthenticationStateProvider` è il servizio sottostante usato dal componente `AuthorizeView` e dal componente `CascadingAuthenticationState` per ottenere lo stato di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="42299-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="42299-156">In genere non si usa `AuthenticationStateProvider` direttamente.</span><span class="sxs-lookup"><span data-stu-id="42299-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="42299-157">Usare gli approcci con il [componente AuthorizeView](#authorizeview-component) oppure [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) descritti più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="42299-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="42299-158">Lo svantaggio principale dell'uso diretto di `AuthenticationStateProvider` è che il componente non riceve alcuna notifica automaticamente se i dati relativi allo stato di autenticazione sottostanti cambiano.</span><span class="sxs-lookup"><span data-stu-id="42299-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="42299-159">Il servizio `AuthenticationStateProvider` può fornire i dati <xref:System.Security.Claims.ClaimsPrincipal> dell'utente corrente, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="42299-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="42299-160">Se `user.Identity.IsAuthenticated` è `true` e perché l'utente è un <xref:System.Security.Claims.ClaimsPrincipal>, è possibile enumerare le attestazioni e valutare l'appartenenza ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="42299-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="42299-161">Per altre informazioni sull'inserimento delle dipendenze e sui servizi, vedere <xref:blazor/dependency-injection> e <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="42299-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="42299-162">Implementare un AuthenticationStateProvider personalizzato</span><span class="sxs-lookup"><span data-stu-id="42299-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="42299-163">Se si crea un'app sul lato client Blazor o se per la specifica dell'app è assolutamente necessario un provider personalizzato, implementare un provider ed eseguire l'override di `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="42299-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

<span data-ttu-id="42299-164">Il servizio `CustomAuthStateProvider` è registrato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="42299-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="42299-165">Usando `CustomAuthStateProvider` tutti gli utenti vengono autenticati con il nome utente `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="42299-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="42299-166">Esporre lo stato di autenticazione come un parametro a catena</span><span class="sxs-lookup"><span data-stu-id="42299-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="42299-167">Se i dati dello stato di autenticazione sono necessari per la logica procedurale, ad esempio quando si esegue un'azione attivata dall'utente, ottenere i dati dello stato di autenticazione definendo un parametro a catena di tipo `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="42299-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="42299-168">Se `user.Identity.IsAuthenticated` è `true`, è possibile enumerare le attestazioni e valutare l'appartenenza ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="42299-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="42299-169">Configurare il `Task<AuthenticationState>` parametro di propagazione utilizzando `AuthorizeRouteView` i `CascadingAuthenticationState` componenti e:</span><span class="sxs-lookup"><span data-stu-id="42299-169">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a><span data-ttu-id="42299-170">Authorization</span><span class="sxs-lookup"><span data-stu-id="42299-170">Authorization</span></span>

<span data-ttu-id="42299-171">Dopo l'autenticazione di un utente, vengono applicate le regole di *autorizzazione* per controllare le operazioni consentite per un utente.</span><span class="sxs-lookup"><span data-stu-id="42299-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="42299-172">L'accesso viene in genere concesso o negato in base alle condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="42299-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="42299-173">L'utente è autenticato (ha eseguito l'accesso).</span><span class="sxs-lookup"><span data-stu-id="42299-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="42299-174">L'utente è incluso in un *ruolo*.</span><span class="sxs-lookup"><span data-stu-id="42299-174">A user is in a *role*.</span></span>
* <span data-ttu-id="42299-175">L'utente ha un'*attestazione*.</span><span class="sxs-lookup"><span data-stu-id="42299-175">A user has a *claim*.</span></span>
* <span data-ttu-id="42299-176">I *criteri* sono soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="42299-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="42299-177">Questi concetti sono uguali a quelli validi per un'app ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="42299-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="42299-178">Per altre informazioni sulla sicurezza di ASP.NET Core, vedere gli articoli in [Sicurezza e identità per ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="42299-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="42299-179">Componente AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="42299-179">AuthorizeView component</span></span>

<span data-ttu-id="42299-180">Il componente `AuthorizeView` visualizza in modo selettivo l'interfaccia utente a seconda del fatto che l'utente sia autorizzato a visualizzarla.</span><span class="sxs-lookup"><span data-stu-id="42299-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="42299-181">Questo approccio è utile quando è necessario solo *visualizzare* dati per l'utente e non occorre usare l'identità dell'utente nella logica procedurale.</span><span class="sxs-lookup"><span data-stu-id="42299-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="42299-182">Il componente espone una variabile `context` di tipo `AuthenticationState`, che è possibile usare per accedere alle informazioni sull'utente connesso:</span><span class="sxs-lookup"><span data-stu-id="42299-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="42299-183">Se l'utente non è autenticato, è anche possibile fornire un contenuto diverso per la visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="42299-183">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="42299-184">Il contenuto di `<Authorized>` e `<NotAuthorized>` può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="42299-184">The content of `<Authorized>` and `<NotAuthorized>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="42299-185">Le condizioni di autorizzazione, ad esempio i ruoli o i criteri che consentono di controllare le opzioni dell'interfaccia utente o l'accesso, sono presentate nella sezione [Autorizzazione](#authorization).</span><span class="sxs-lookup"><span data-stu-id="42299-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="42299-186">Se non sono specificate condizioni di autorizzazione, `AuthorizeView` usa criteri predefiniti e considera:</span><span class="sxs-lookup"><span data-stu-id="42299-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="42299-187">Gli utenti autenticati (che hanno eseguito l'accesso) come autorizzati.</span><span class="sxs-lookup"><span data-stu-id="42299-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="42299-188">Gli utenti non autenticati (disconnessi) come non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="42299-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="42299-189">Autorizzazione basata sui ruoli e basata sui criteri</span><span class="sxs-lookup"><span data-stu-id="42299-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="42299-190">Il componente `AuthorizeView` supporta l'autorizzazione *basata sui ruoli* oppure *basata sui criteri*.</span><span class="sxs-lookup"><span data-stu-id="42299-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="42299-191">Per l'autorizzazione basata sui ruoli, usare il parametro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="42299-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="42299-192">Per altre informazioni, vedere <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="42299-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="42299-193">Per l'autorizzazione basata sui criteri, usare il parametro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="42299-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="42299-194">L'autorizzazione basata sulle attestazioni è un caso speciale di autorizzazione basata su criteri.</span><span class="sxs-lookup"><span data-stu-id="42299-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="42299-195">Ad esempio, è possibile definire un criterio che richiede che gli utenti abbiano una determinata attestazione.</span><span class="sxs-lookup"><span data-stu-id="42299-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="42299-196">Per altre informazioni, vedere <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="42299-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="42299-197">Queste API possono essere usate nelle app sul lato server o sul lato client Blazor.</span><span class="sxs-lookup"><span data-stu-id="42299-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="42299-198">Se non si specifica `Roles` o `Policy`, `AuthorizeView` usa i criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="42299-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="42299-199">Contenuto visualizzato durante l'autenticazione asincrona</span><span class="sxs-lookup"><span data-stu-id="42299-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="42299-200">Blazor consente di determinare lo stato di autenticazione *in modo asincrono*.</span><span class="sxs-lookup"><span data-stu-id="42299-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="42299-201">Lo scenario principale per questo approccio è rappresentato dalle app sul lato client Blazor che effettuano una richiesta a un endpoint esterno per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="42299-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="42299-202">Mentre è in corso l'autenticazione `AuthorizeView` non visualizza alcun contenuto per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="42299-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="42299-203">Per visualizzare contenuto durante l'autenticazione, usare l'elemento `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="42299-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="42299-204">Questo approccio non è in genere applicabile alle app sul lato server Blazor.</span><span class="sxs-lookup"><span data-stu-id="42299-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="42299-205">Le app sul lato server Blazor conoscono lo stato di autenticazione non appena viene stabilito.</span><span class="sxs-lookup"><span data-stu-id="42299-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="42299-206">È possibile fornire contenuto `Authorizing` nel componente `AuthorizeView` di un'app sul lato server Blazor, ma il contenuto non viene mai visualizzato.</span><span class="sxs-lookup"><span data-stu-id="42299-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="42299-207">Attributo [Authorize]</span><span class="sxs-lookup"><span data-stu-id="42299-207">[Authorize] attribute</span></span>

<span data-ttu-id="42299-208">Proprio come un'app può usare `[Authorize]` con un controller MVC o una pagina Razor, è possibile usare `[Authorize]` anche con Razor Components:</span><span class="sxs-lookup"><span data-stu-id="42299-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="42299-209">Usare `[Authorize]` solo per i componenti `@page` raggiunti tramite il componente Router di Blazor.</span><span class="sxs-lookup"><span data-stu-id="42299-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="42299-210">L'autorizzazione viene eseguita solo come un aspetto del routing e *non* per i componenti figlio di cui viene eseguito il rendering all'interno di una pagina.</span><span class="sxs-lookup"><span data-stu-id="42299-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="42299-211">Per autorizzare la visualizzazione di parti specifiche all'interno di una pagina, usare invece `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="42299-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="42299-212">Potrebbe essere necessario aggiungere `@using Microsoft.AspNetCore.Authorization` al componente o al file *_Imports.razor* per la compilazione del componente.</span><span class="sxs-lookup"><span data-stu-id="42299-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="42299-213">L'attributo `[Authorize]` supporta anche l'autorizzazione basata sui ruoli o basata sui criteri.</span><span class="sxs-lookup"><span data-stu-id="42299-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="42299-214">Per l'autorizzazione basata sui ruoli, usare il parametro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="42299-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="42299-215">Per l'autorizzazione basata sui criteri, usare il parametro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="42299-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="42299-216">Se non si specifica `Roles` o `Policy`, `[Authorize]` usa i criteri predefiniti, che per impostazione predefinita considerano:</span><span class="sxs-lookup"><span data-stu-id="42299-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="42299-217">Gli utenti autenticati (che hanno eseguito l'accesso) come autorizzati.</span><span class="sxs-lookup"><span data-stu-id="42299-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="42299-218">Gli utenti non autenticati (disconnessi) come non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="42299-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="42299-219">Personalizzare il contenuto non autorizzato con il componente Router</span><span class="sxs-lookup"><span data-stu-id="42299-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="42299-220">Il `Router` componente, insieme `AuthorizeRouteView` al componente, consente all'app di specificare contenuto personalizzato se:</span><span class="sxs-lookup"><span data-stu-id="42299-220">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="42299-221">Non viene trovato contenuto.</span><span class="sxs-lookup"><span data-stu-id="42299-221">Content isn't found.</span></span>
* <span data-ttu-id="42299-222">L'utente non supera una condizione `[Authorize]` applicata al componente.</span><span class="sxs-lookup"><span data-stu-id="42299-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="42299-223">L'attributo `[Authorize]` viene presentato nella sezione [Attributo [Authorize]](#authorize-attribute).</span><span class="sxs-lookup"><span data-stu-id="42299-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="42299-224">L'autenticazione asincrona è in corso.</span><span class="sxs-lookup"><span data-stu-id="42299-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="42299-225">Nel modello di progetto sul lato server Blazor predefinito, il file *App.razor* dimostra come impostare il contenuto personalizzato:</span><span class="sxs-lookup"><span data-stu-id="42299-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="42299-226">Il contenuto di `<NotFound>`, `<NotAuthorized>` e `<Authorizing>` può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="42299-226">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="42299-227">Se `<NotAuthorized>` non è specificato `<AuthorizeRouteView>` , utilizza il seguente messaggio di fallback:</span><span class="sxs-lookup"><span data-stu-id="42299-227">If `<NotAuthorized>` isn't specified, the `<AuthorizeRouteView>` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="42299-228">Notifica per le modifiche dello stato di autenticazione</span><span class="sxs-lookup"><span data-stu-id="42299-228">Notification about authentication state changes</span></span>

<span data-ttu-id="42299-229">Se l'app determina che i dati dello stato di autenticazione sottostante sono stati modificati (ad esempio, perché l'utente si è disconnesso o un altro utente ha modificato i relativi ruoli), un `AuthenticationStateProvider` personalizzato può richiamare facoltativamente il metodo `NotifyAuthenticationStateChanged` sulla classe di base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="42299-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="42299-230">Viene così inviata notifica ai consumer dei dati di stato di autenticazione (ad esempio, `AuthorizeView`) di eseguire nuovamente il rendering usando i nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="42299-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="42299-231">Logica procedurale</span><span class="sxs-lookup"><span data-stu-id="42299-231">Procedural logic</span></span>

<span data-ttu-id="42299-232">Se l'app deve controllare le regole di autorizzazione come parte della logica procedurale, usare un parametro a catena di tipo `Task<AuthenticationState>` per ottenere il <xref:System.Security.Claims.ClaimsPrincipal> dell'utente.</span><span class="sxs-lookup"><span data-stu-id="42299-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="42299-233">`Task<AuthenticationState>` può essere combinato con altri servizi, ad esempio `IAuthorizationService`, per valutare i criteri.</span><span class="sxs-lookup"><span data-stu-id="42299-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="42299-234">Autorizzazione nelle app sul lato client Blazor</span><span class="sxs-lookup"><span data-stu-id="42299-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="42299-235">Nelle app sul lato client Blazor i controlli di autorizzazione possono essere ignorati perché tutto il codice sul lato client può essere modificato dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="42299-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="42299-236">Lo stesso vale per tutte le tecnologie per app sul lato client, tra cui i framework JavaScript SPA o le app native per qualsiasi sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="42299-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="42299-237">**Eseguire sempre i controlli di autorizzazione nel server all'interno degli eventuali endpoint dell'API a cui accede l'app sul lato client.**</span><span class="sxs-lookup"><span data-stu-id="42299-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="42299-238">Risolvere gli errori</span><span class="sxs-lookup"><span data-stu-id="42299-238">Troubleshoot errors</span></span>

<span data-ttu-id="42299-239">Errori comuni:</span><span class="sxs-lookup"><span data-stu-id="42299-239">Common errors:</span></span>

* <span data-ttu-id="42299-240">**Per l'autorizzazione è richiesto un parametro a catena di tipo Task\<AuthenticationState>. Valutare la possibilità di usare CascadingAuthenticationState per specificarlo.**</span><span class="sxs-lookup"><span data-stu-id="42299-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="42299-241">**Valore `null` ricevuto per `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="42299-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="42299-242">È probabile che il progetto non sia stato creato con un modello Blazor sul lato server con l'autenticazione abilitata.</span><span class="sxs-lookup"><span data-stu-id="42299-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="42299-243">Eseguire il wrapping di un `<CascadingAuthenticationState>` in una parte dell'albero dell'interfaccia utente, ad esempio in *App.razor* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="42299-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="42299-244">`CascadingAuthenticationState` fornisce il parametro a catena `Task<AuthenticationState>`, ricevuto a sua volta dal servizio di inserimento delle dipendenze `AuthenticationStateProvider` sottostante.</span><span class="sxs-lookup"><span data-stu-id="42299-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="42299-245">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42299-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server-side>
* <xref:security/authentication/windowsauth>
