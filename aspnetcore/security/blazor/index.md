---
title: Autenticazione e autorizzazione ASP.NET Core Blazor
author: guardrex
description: Informazioni sugli scenari di autenticazione e autorizzazione Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: e9087c246f4805e5931180fa0869fc8a8d23a6c1
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885591"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="54547-103">Autenticazione e autorizzazione per ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="54547-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="54547-104">Di [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="54547-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="54547-105">ASP.NET Core supporta la configurazione e la gestione della sicurezza nelle app Blazor.</span><span class="sxs-lookup"><span data-stu-id="54547-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="54547-106">Gli scenari di sicurezza sono diversi tra le app Blazor server e Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="54547-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="54547-107">Poiché le app del server Blazor vengono eseguite nel server, i controlli delle autorizzazioni sono in grado di determinare:</span><span class="sxs-lookup"><span data-stu-id="54547-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="54547-108">Le opzioni dell'interfaccia utente presentate all'utente (ad esempio, le voci di menu disponibili per un utente).</span><span class="sxs-lookup"><span data-stu-id="54547-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="54547-109">Le regole di accesso per le aree dell'app e i componenti.</span><span class="sxs-lookup"><span data-stu-id="54547-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="54547-110">Le app webassembly Blazor vengono eseguite sul client.</span><span class="sxs-lookup"><span data-stu-id="54547-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="54547-111">L'autorizzazione viene usata *solo* per determinare quali opzioni dell'interfaccia utente visualizzare.</span><span class="sxs-lookup"><span data-stu-id="54547-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="54547-112">Poiché i controlli lato client possono essere modificati o ignorati da un utente, un'app webassembly Blazor non può applicare regole di accesso alle autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="54547-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="54547-113">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="54547-113">Authentication</span></span>

<span data-ttu-id="54547-114">Blazor usa i meccanismi di autenticazione di ASP.NET Core esistenti per stabilire l'identità dell'utente.</span><span class="sxs-lookup"><span data-stu-id="54547-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="54547-115">Il meccanismo esatto dipende dal modo in cui l'app Blazor è ospitata, il server Blazor o il webassembly Blazor.</span><span class="sxs-lookup"><span data-stu-id="54547-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="54547-116">Autenticazione server Blazor</span><span class="sxs-lookup"><span data-stu-id="54547-116">Blazor Server authentication</span></span>

<span data-ttu-id="54547-117">Le app del server Blazor operano su una connessione in tempo reale creata con SignalR.</span><span class="sxs-lookup"><span data-stu-id="54547-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="54547-118">L'[autenticazione nelle app basate su SignalR](xref:signalr/authn-and-authz) viene gestita quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="54547-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="54547-119">L'autenticazione può basarsi su un cookie o su altri bearer token.</span><span class="sxs-lookup"><span data-stu-id="54547-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="54547-120">Il modello di progetto server Blazor può configurare automaticamente l'autenticazione quando viene creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="54547-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="54547-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54547-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="54547-122">Per creare un nuovo progetto server Blazor con un meccanismo di autenticazione, seguire le linee guida di Visual Studio disponibili nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="54547-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="54547-123">Dopo aver scelto il modello **App server Blazor** nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**, selezionare **Modifica** in **Autenticazione** .</span><span class="sxs-lookup"><span data-stu-id="54547-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="54547-124">Viene visualizzata una finestra di dialogo che offre lo stesso set di meccanismi di autenticazione disponibili per altri progetti ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="54547-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="54547-125">**Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="54547-125">**No Authentication**</span></span>
* <span data-ttu-id="54547-126">È possibile archiviare **account utente singoli** &ndash; account utente:</span><span class="sxs-lookup"><span data-stu-id="54547-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="54547-127">All'interno dell'app usando il sistema di [identità](xref:security/authentication/identity) di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54547-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="54547-128">Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="54547-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="54547-129">**Account aziendali o dell'istituto di istruzione**</span><span class="sxs-lookup"><span data-stu-id="54547-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="54547-130">**Autenticazione di Windows**</span><span class="sxs-lookup"><span data-stu-id="54547-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="54547-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="54547-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="54547-132">Seguire le indicazioni Visual Studio Code nell' <xref:blazor/get-started> articolo per creare un nuovo progetto server Blazor con un meccanismo di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="54547-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="54547-133">I valori di autenticazione consentiti (`{AUTHENTICATION}`) sono indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="54547-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="54547-134">Meccanismo di autenticazione</span><span class="sxs-lookup"><span data-stu-id="54547-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="54547-135">Valore di `{AUTHENTICATION}`</span><span class="sxs-lookup"><span data-stu-id="54547-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="54547-136">Nessuna autenticazione</span><span class="sxs-lookup"><span data-stu-id="54547-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="54547-137">Utenti</span><span class="sxs-lookup"><span data-stu-id="54547-137">Individual</span></span><br><span data-ttu-id="54547-138">individuali archiviati nell'app con ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="54547-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="54547-139">Utenti</span><span class="sxs-lookup"><span data-stu-id="54547-139">Individual</span></span><br><span data-ttu-id="54547-140">individuali archiviati in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="54547-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="54547-141">Account aziendali o dell'istituto di istruzione</span><span class="sxs-lookup"><span data-stu-id="54547-141">Work or School Accounts</span></span><br><span data-ttu-id="54547-142">Autenticazione aziendale per un singolo tenant.</span><span class="sxs-lookup"><span data-stu-id="54547-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="54547-143">Account aziendali o dell'istituto di istruzione</span><span class="sxs-lookup"><span data-stu-id="54547-143">Work or School Accounts</span></span><br><span data-ttu-id="54547-144">Autenticazione aziendale per più tenant.</span><span class="sxs-lookup"><span data-stu-id="54547-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="54547-145">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="54547-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="54547-146">Il comando crea una cartella denominata con il valore fornito per il segnaposto `{APP NAME}` e il nome della cartella viene usato come nome dell'app.</span><span class="sxs-lookup"><span data-stu-id="54547-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="54547-147">Per altre informazioni, vedere il comando [dotnet new](/dotnet/core/tools/dotnet-new) nella guida per .NET Core.</span><span class="sxs-lookup"><span data-stu-id="54547-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
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

### <a name="opno-locblazor-webassembly-authentication"></a><span data-ttu-id="54547-148">autenticazione Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="54547-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="54547-149">Nelle app Blazor webassembly i controlli di autenticazione possono essere ignorati perché tutto il codice lato client può essere modificato dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="54547-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="54547-150">Lo stesso vale per tutte le tecnologie per app sul lato client, tra cui i framework JavaScript SPA o le app native per qualsiasi sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="54547-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="54547-151">Aggiungere un riferimento al pacchetto per [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) al file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="54547-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="54547-152">Le sezioni seguenti illustrano l'implementazione di un servizio `AuthenticationStateProvider` personalizzato per le app Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="54547-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="54547-153">Servizio AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="54547-153">AuthenticationStateProvider service</span></span>

Blazor<span data-ttu-id="54547-154"> app Server includono un servizio di `AuthenticationStateProvider` predefinito che ottiene i dati sullo stato di autenticazione dal `HttpContext.User`di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54547-154"> Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="54547-155">Questo è il modo in cui lo stato di autenticazione si integra con i meccanismi di autenticazione sul lato server ASP.NET Core esistenti.</span><span class="sxs-lookup"><span data-stu-id="54547-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="54547-156">`AuthenticationStateProvider` è il servizio sottostante usato dal componente `AuthorizeView` e dal componente `CascadingAuthenticationState` per ottenere lo stato di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="54547-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="54547-157">In genere non si usa `AuthenticationStateProvider` direttamente.</span><span class="sxs-lookup"><span data-stu-id="54547-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="54547-158">Usare gli approcci con il [componente AuthorizeView](#authorizeview-component) oppure [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) descritti più avanti in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="54547-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="54547-159">Lo svantaggio principale dell'uso diretto di `AuthenticationStateProvider` è che il componente non riceve alcuna notifica automaticamente se i dati relativi allo stato di autenticazione sottostanti cambiano.</span><span class="sxs-lookup"><span data-stu-id="54547-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="54547-160">Il servizio `AuthenticationStateProvider` può fornire i dati <xref:System.Security.Claims.ClaimsPrincipal> dell'utente corrente, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="54547-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
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

<span data-ttu-id="54547-161">Se `user.Identity.IsAuthenticated` è `true` e perché l'utente è un <xref:System.Security.Claims.ClaimsPrincipal>, è possibile enumerare le attestazioni e valutare l'appartenenza ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="54547-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="54547-162">Per altre informazioni sull'inserimento delle dipendenze e sui servizi, vedere <xref:blazor/dependency-injection> e <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="54547-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="54547-163">Implementare un AuthenticationStateProvider personalizzato</span><span class="sxs-lookup"><span data-stu-id="54547-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="54547-164">Se si sta compilando un'app webassembly Blazor o se la specifica dell'app richiede assolutamente un provider personalizzato, implementare un provider ed eseguire l'override di `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="54547-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
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
}
```

<span data-ttu-id="54547-165">In un'app Blazor webassembly, il servizio `CustomAuthStateProvider` è registrato in `Main` di *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="54547-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="54547-166">Usando `CustomAuthStateProvider` tutti gli utenti vengono autenticati con il nome utente `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="54547-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="54547-167">Esporre lo stato di autenticazione come un parametro a catena</span><span class="sxs-lookup"><span data-stu-id="54547-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="54547-168">Se i dati dello stato di autenticazione sono necessari per la logica procedurale, ad esempio quando si esegue un'azione attivata dall'utente, ottenere i dati dello stato di autenticazione definendo un parametro a catena di tipo `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="54547-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
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

> [!NOTE]
> <span data-ttu-id="54547-169">In un Blazor componente dell'app webassembly aggiungere lo spazio dei nomi `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="54547-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="54547-170">Se `user.Identity.IsAuthenticated` è `true`, è possibile enumerare le attestazioni e valutare l'appartenenza ai ruoli.</span><span class="sxs-lookup"><span data-stu-id="54547-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="54547-171">Configurare il `Task<AuthenticationState>` parametro di propagazione usando i componenti `AuthorizeRouteView` e `CascadingAuthenticationState` nel file *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="54547-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
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

## <a name="authorization"></a><span data-ttu-id="54547-172">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="54547-172">Authorization</span></span>

<span data-ttu-id="54547-173">Dopo l'autenticazione di un utente, vengono applicate le regole di *autorizzazione* per controllare le operazioni consentite per un utente.</span><span class="sxs-lookup"><span data-stu-id="54547-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="54547-174">L'accesso viene in genere concesso o negato in base alle condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="54547-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="54547-175">L'utente è autenticato (ha eseguito l'accesso).</span><span class="sxs-lookup"><span data-stu-id="54547-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="54547-176">L'utente è incluso in un *ruolo*.</span><span class="sxs-lookup"><span data-stu-id="54547-176">A user is in a *role*.</span></span>
* <span data-ttu-id="54547-177">L'utente ha un'*attestazione*.</span><span class="sxs-lookup"><span data-stu-id="54547-177">A user has a *claim*.</span></span>
* <span data-ttu-id="54547-178">I *criteri* sono soddisfatti.</span><span class="sxs-lookup"><span data-stu-id="54547-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="54547-179">Questi concetti sono uguali a quelli validi per un'app ASP.NET Core MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="54547-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="54547-180">Per altre informazioni sulla sicurezza di ASP.NET Core, vedere gli articoli in [Sicurezza e identità per ASP.NET Core](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="54547-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="54547-181">Componente AuthorizeView</span><span class="sxs-lookup"><span data-stu-id="54547-181">AuthorizeView component</span></span>

<span data-ttu-id="54547-182">Il componente `AuthorizeView` visualizza in modo selettivo l'interfaccia utente a seconda del fatto che l'utente sia autorizzato a visualizzarla.</span><span class="sxs-lookup"><span data-stu-id="54547-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="54547-183">Questo approccio è utile quando è necessario solo *visualizzare* dati per l'utente e non occorre usare l'identità dell'utente nella logica procedurale.</span><span class="sxs-lookup"><span data-stu-id="54547-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="54547-184">Il componente espone una variabile `context` di tipo `AuthenticationState`, che è possibile usare per accedere alle informazioni sull'utente connesso:</span><span class="sxs-lookup"><span data-stu-id="54547-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="54547-185">Se l'utente non è autenticato, è anche possibile fornire un contenuto diverso per la visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="54547-185">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
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

<span data-ttu-id="54547-186">Il contenuto dei tag `<Authorized>` e `<NotAuthorized>` può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="54547-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="54547-187">Le condizioni di autorizzazione, ad esempio i ruoli o i criteri che consentono di controllare le opzioni dell'interfaccia utente o l'accesso, sono presentate nella sezione [Autorizzazione](#authorization).</span><span class="sxs-lookup"><span data-stu-id="54547-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="54547-188">Se non sono specificate condizioni di autorizzazione, `AuthorizeView` usa criteri predefiniti e considera:</span><span class="sxs-lookup"><span data-stu-id="54547-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="54547-189">Gli utenti autenticati (che hanno eseguito l'accesso) come autorizzati.</span><span class="sxs-lookup"><span data-stu-id="54547-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="54547-190">Gli utenti non autenticati (disconnessi) come non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="54547-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="54547-191">Autorizzazione basata sui ruoli e basata sui criteri</span><span class="sxs-lookup"><span data-stu-id="54547-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="54547-192">Il componente `AuthorizeView` supporta l'autorizzazione *basata sui ruoli* oppure *basata sui criteri*.</span><span class="sxs-lookup"><span data-stu-id="54547-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="54547-193">Per l'autorizzazione basata sui ruoli, usare il parametro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="54547-193">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="54547-194">Per ulteriori informazioni, vedere <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="54547-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="54547-195">Per l'autorizzazione basata sui criteri, usare il parametro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="54547-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="54547-196">L'autorizzazione basata sulle attestazioni è un caso speciale di autorizzazione basata su criteri.</span><span class="sxs-lookup"><span data-stu-id="54547-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="54547-197">Ad esempio, è possibile definire un criterio che richiede che gli utenti abbiano una determinata attestazione.</span><span class="sxs-lookup"><span data-stu-id="54547-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="54547-198">Per ulteriori informazioni, vedere <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="54547-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="54547-199">Queste API possono essere usate nelle app Blazor server o Blazor webassembly.</span><span class="sxs-lookup"><span data-stu-id="54547-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="54547-200">Se non si specifica `Roles` o `Policy`, `AuthorizeView` usa i criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="54547-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="54547-201">Contenuto visualizzato durante l'autenticazione asincrona</span><span class="sxs-lookup"><span data-stu-id="54547-201">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="54547-202"> consente di determinare lo stato di autenticazione in *modo asincrono*.</span><span class="sxs-lookup"><span data-stu-id="54547-202"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="54547-203">Lo scenario principale per questo approccio è Blazor app webassembly che effettuano una richiesta a un endpoint esterno per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="54547-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="54547-204">Mentre è in corso l'autenticazione `AuthorizeView` non visualizza alcun contenuto per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="54547-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="54547-205">Per visualizzare contenuto durante l'autenticazione, usare l'elemento `<Authorizing>`:</span><span class="sxs-lookup"><span data-stu-id="54547-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
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

<span data-ttu-id="54547-206">Questo approccio non è in genere applicabile alle app di Blazor server.</span><span class="sxs-lookup"><span data-stu-id="54547-206">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="54547-207">le app Server Blazor conoscono lo stato di autenticazione non appena viene stabilito lo stato.</span><span class="sxs-lookup"><span data-stu-id="54547-207">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="54547-208">`Authorizing` contenuto può essere fornito in un componente `AuthorizeView` dell'app Blazor server, ma il contenuto non viene mai visualizzato.</span><span class="sxs-lookup"><span data-stu-id="54547-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="54547-209">Attributo [Authorize]</span><span class="sxs-lookup"><span data-stu-id="54547-209">[Authorize] attribute</span></span>

<span data-ttu-id="54547-210">È possibile usare l'attributo `[Authorize]` nei componenti Razor:</span><span class="sxs-lookup"><span data-stu-id="54547-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="54547-211">In un componente dell'app webassembly Blazor aggiungere lo spazio dei nomi `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) agli esempi in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="54547-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54547-212">Utilizzare solo `[Authorize]` sui componenti `@page` raggiunti tramite il router Blazor.</span><span class="sxs-lookup"><span data-stu-id="54547-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="54547-213">L'autorizzazione viene eseguita solo come un aspetto del routing e *non* per i componenti figlio di cui viene eseguito il rendering all'interno di una pagina.</span><span class="sxs-lookup"><span data-stu-id="54547-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="54547-214">Per autorizzare la visualizzazione di parti specifiche all'interno di una pagina, usare invece `AuthorizeView`.</span><span class="sxs-lookup"><span data-stu-id="54547-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="54547-215">L'attributo `[Authorize]` supporta anche l'autorizzazione basata sui ruoli o basata sui criteri.</span><span class="sxs-lookup"><span data-stu-id="54547-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="54547-216">Per l'autorizzazione basata sui ruoli, usare il parametro `Roles`:</span><span class="sxs-lookup"><span data-stu-id="54547-216">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="54547-217">Per l'autorizzazione basata sui criteri, usare il parametro `Policy`:</span><span class="sxs-lookup"><span data-stu-id="54547-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="54547-218">Se non si specifica `Roles` o `Policy`, `[Authorize]` usa i criteri predefiniti, che per impostazione predefinita considerano:</span><span class="sxs-lookup"><span data-stu-id="54547-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="54547-219">Gli utenti autenticati (che hanno eseguito l'accesso) come autorizzati.</span><span class="sxs-lookup"><span data-stu-id="54547-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="54547-220">Gli utenti non autenticati (disconnessi) come non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="54547-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="54547-221">Personalizzare il contenuto non autorizzato con il componente Router</span><span class="sxs-lookup"><span data-stu-id="54547-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="54547-222">Il componente `Router`, insieme al componente `AuthorizeRouteView`, consente all'app di specificare contenuto personalizzato se:</span><span class="sxs-lookup"><span data-stu-id="54547-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="54547-223">Non viene trovato contenuto.</span><span class="sxs-lookup"><span data-stu-id="54547-223">Content isn't found.</span></span>
* <span data-ttu-id="54547-224">L'utente non supera una condizione `[Authorize]` applicata al componente.</span><span class="sxs-lookup"><span data-stu-id="54547-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="54547-225">L'attributo `[Authorize]` viene trattato nella sezione [attributo`[Authorize]`](#authorize-attribute) .</span><span class="sxs-lookup"><span data-stu-id="54547-225">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="54547-226">L'autenticazione asincrona è in corso.</span><span class="sxs-lookup"><span data-stu-id="54547-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="54547-227">Nel modello di progetto predefinito di Blazor server il file *app. Razor* illustra come impostare il contenuto personalizzato:</span><span class="sxs-lookup"><span data-stu-id="54547-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```razor
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

<span data-ttu-id="54547-228">Il contenuto dei tag `<NotFound>`, `<NotAuthorized>`e `<Authorizing>` può includere elementi arbitrari, ad esempio altri componenti interattivi.</span><span class="sxs-lookup"><span data-stu-id="54547-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="54547-229">Se l'elemento `<NotAuthorized>` non è specificato, il `AuthorizeRouteView` usa il messaggio di fallback seguente:</span><span class="sxs-lookup"><span data-stu-id="54547-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="54547-230">Notifica per le modifiche dello stato di autenticazione</span><span class="sxs-lookup"><span data-stu-id="54547-230">Notification about authentication state changes</span></span>

<span data-ttu-id="54547-231">Se l'app determina che i dati dello stato di autenticazione sottostante sono stati modificati (ad esempio, perché l'utente si è disconnesso o un altro utente ha modificato i relativi ruoli), un `AuthenticationStateProvider` personalizzato può richiamare facoltativamente il metodo `NotifyAuthenticationStateChanged` sulla classe di base `AuthenticationStateProvider`.</span><span class="sxs-lookup"><span data-stu-id="54547-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="54547-232">Viene così inviata notifica ai consumer dei dati di stato di autenticazione (ad esempio, `AuthorizeView`) di eseguire nuovamente il rendering usando i nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="54547-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="54547-233">Logica procedurale</span><span class="sxs-lookup"><span data-stu-id="54547-233">Procedural logic</span></span>

<span data-ttu-id="54547-234">Se l'app deve controllare le regole di autorizzazione come parte della logica procedurale, usare un parametro a catena di tipo `Task<AuthenticationState>` per ottenere il <xref:System.Security.Claims.ClaimsPrincipal> dell'utente.</span><span class="sxs-lookup"><span data-stu-id="54547-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="54547-235">`Task<AuthenticationState>` può essere combinato con altri servizi, ad esempio `IAuthorizationService`, per valutare i criteri.</span><span class="sxs-lookup"><span data-stu-id="54547-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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

> [!NOTE]
> <span data-ttu-id="54547-236">In un componente dell'app webassembly Blazor aggiungere gli spazi dei nomi `Microsoft.AspNetCore.Authorization` e `Microsoft.AspNetCore.Components.Authorization`:</span><span class="sxs-lookup"><span data-stu-id="54547-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="54547-237">Autorizzazione nelle app Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="54547-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="54547-238">Nelle app Blazor webassembly è possibile ignorare i controlli di autorizzazione perché tutto il codice lato client può essere modificato dagli utenti.</span><span class="sxs-lookup"><span data-stu-id="54547-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="54547-239">Lo stesso vale per tutte le tecnologie per app sul lato client, tra cui i framework JavaScript SPA o le app native per qualsiasi sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="54547-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="54547-240">**Eseguire sempre i controlli di autorizzazione nel server all'interno degli eventuali endpoint dell'API a cui accede l'app sul lato client.**</span><span class="sxs-lookup"><span data-stu-id="54547-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="54547-241">Risolvere gli errori</span><span class="sxs-lookup"><span data-stu-id="54547-241">Troubleshoot errors</span></span>

<span data-ttu-id="54547-242">Errori comuni:</span><span class="sxs-lookup"><span data-stu-id="54547-242">Common errors:</span></span>

* <span data-ttu-id="54547-243">**Per l'autorizzazione è necessario un parametro di propagazione di tipo Task\<AuthenticationState >. Per fornire questo, provare a usare CascadingAuthenticationState.**</span><span class="sxs-lookup"><span data-stu-id="54547-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="54547-244">**Valore `null` ricevuto per `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="54547-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="54547-245">È probabile che il progetto non sia stato creato usando un modello di Blazor server con autenticazione abilitata.</span><span class="sxs-lookup"><span data-stu-id="54547-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="54547-246">Eseguire il wrapping di un `<CascadingAuthenticationState>` in una parte dell'albero dell'interfaccia utente, ad esempio in *App.razor* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="54547-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="54547-247">`CascadingAuthenticationState` fornisce il parametro a catena `Task<AuthenticationState>`, ricevuto a sua volta dal servizio di inserimento delle dipendenze `AuthenticationStateProvider` sottostante.</span><span class="sxs-lookup"><span data-stu-id="54547-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54547-248">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="54547-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
