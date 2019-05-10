---
title: Autenticare gli utenti con WS-Federation in ASP.NET Core
author: chlowell
description: Questa esercitazione viene illustrato come utilizzare WS-Federation in un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897568"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="87f70-103">Autenticare gli utenti con WS-Federation in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87f70-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="87f70-104">Questa esercitazione illustra come consentire agli utenti di accedere con un provider di autenticazione WS-Federation, ad esempio Active Directory Federation Services (ADFS) o [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="87f70-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="87f70-105">Usa l'app di esempio ASP.NET Core 2.0 descritto in [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="87f70-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="87f70-106">Per le app ASP.NET Core 2.0, WS-Federation supporto viene fornito da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="87f70-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="87f70-107">Questo componente viene trasferito dal [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) e condivide molti dei meccanismi del componente.</span><span class="sxs-lookup"><span data-stu-id="87f70-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="87f70-108">Tuttavia, i componenti sono diversi in un paio di modi importanti.</span><span class="sxs-lookup"><span data-stu-id="87f70-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="87f70-109">Per impostazione predefinita, il middleware di nuovo:</span><span class="sxs-lookup"><span data-stu-id="87f70-109">By default, the new middleware:</span></span>

* <span data-ttu-id="87f70-110">Non consentire l'accesso non richiesto.</span><span class="sxs-lookup"><span data-stu-id="87f70-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="87f70-111">Questa funzionalità del protocollo WS-Federation è vulnerabile agli attacchi XSRF.</span><span class="sxs-lookup"><span data-stu-id="87f70-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="87f70-112">Tuttavia, può essere abilitata con la `AllowUnsolicitedLogins` opzione.</span><span class="sxs-lookup"><span data-stu-id="87f70-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="87f70-113">Non controlla ogni post del form per messaggi di accesso.</span><span class="sxs-lookup"><span data-stu-id="87f70-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="87f70-114">Solo le richieste per il `CallbackPath` vengono verificate per l'opzione sign `CallbackPath` per impostazione predefinita `/signin-wsfed` ma può essere modificato tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="87f70-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="87f70-115">Questo percorso può essere condivisa con altri provider di autenticazione, consentendo il [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opzione.</span><span class="sxs-lookup"><span data-stu-id="87f70-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="87f70-116">Registrare l'app con Active Directory</span><span class="sxs-lookup"><span data-stu-id="87f70-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="87f70-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="87f70-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="87f70-118">Aprire il server **Aggiunta guidata attendibilità componente** dalla console di gestione di ad FS:</span><span class="sxs-lookup"><span data-stu-id="87f70-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Aggiunta guidata attendibilità Relying Party: Introduzione](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="87f70-120">Scegliere di immettere manualmente i dati:</span><span class="sxs-lookup"><span data-stu-id="87f70-120">Choose to enter data manually:</span></span>

![Aggiunta guidata attendibilità Relying Party: Seleziona l'origine dati](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="87f70-122">Immettere un nome visualizzato per la relying party.</span><span class="sxs-lookup"><span data-stu-id="87f70-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="87f70-123">Il nome non è importante per l'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87f70-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="87f70-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) manca il supporto per la crittografia dei token, in modo che non si configura un certificato di crittografia di token:</span><span class="sxs-lookup"><span data-stu-id="87f70-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Aggiunta guidata attendibilità Relying Party: Configurare il certificato](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="87f70-126">Abilitare il supporto per il protocollo passivo WS-Federation, usando l'URL dell'app.</span><span class="sxs-lookup"><span data-stu-id="87f70-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="87f70-127">Verificare che la porta sia corretta per l'app:</span><span class="sxs-lookup"><span data-stu-id="87f70-127">Verify the port is correct for the app:</span></span>

![Aggiunta guidata attendibilità Relying Party: Configura URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="87f70-129">Deve trattarsi di un URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="87f70-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="87f70-130">IIS Express, è possibile fornire un certificato autofirmato per l'hosting dell'app durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="87f70-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="87f70-131">Kestrel richiede la configurazione manuale del certificato.</span><span class="sxs-lookup"><span data-stu-id="87f70-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="87f70-132">Vedere le [documentazione di Kestrel](xref:fundamentals/servers/kestrel) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="87f70-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="87f70-133">Fare clic su **successivo** attraverso il resto della procedura guidata e **Chiudi** alla fine.</span><span class="sxs-lookup"><span data-stu-id="87f70-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="87f70-134">ASP.NET Core Identity richiede un **ID nome** di attestazione.</span><span class="sxs-lookup"><span data-stu-id="87f70-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="87f70-135">Aggiungere uno dal **Modifica regole attestazione** finestra di dialogo:</span><span class="sxs-lookup"><span data-stu-id="87f70-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Modifica regole attestazione](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="87f70-137">Nel **Nell'Aggiunta guidata regole attestazione di trasformazione**, lasciare l'impostazione predefinita **inviare attributi LDAP come attestazioni** modello selezionato e fare clic su **Next**.</span><span class="sxs-lookup"><span data-stu-id="87f70-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="87f70-138">Aggiungere un regola mapping il **SAM-Account-Name** attributo LDAP per il **ID nome** attestazione in uscita:</span><span class="sxs-lookup"><span data-stu-id="87f70-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Aggiunta guidata regole attestazione di trasformazione: Configura regola attestazione](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="87f70-140">Fare clic su **Finish** > **OK** nel **Edit Claim Rules** finestra.</span><span class="sxs-lookup"><span data-stu-id="87f70-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="87f70-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87f70-141">Azure Active Directory</span></span>

* <span data-ttu-id="87f70-142">Passare al pannello registrazioni per l'app del tenant AAD.</span><span class="sxs-lookup"><span data-stu-id="87f70-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="87f70-143">Fare clic su **registrazione nuova applicazione**:</span><span class="sxs-lookup"><span data-stu-id="87f70-143">Click **New application registration**:</span></span>

![Azure Active Directory: Registrazioni per l'App](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="87f70-145">Immettere un nome per la registrazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="87f70-145">Enter a name for the app registration.</span></span> <span data-ttu-id="87f70-146">Non è importante per l'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87f70-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="87f70-147">Immettere l'URL dell'app è in ascolto su come le **Sign-on di URL**:</span><span class="sxs-lookup"><span data-stu-id="87f70-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Creare una registrazione di app](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="87f70-149">Fare clic su **endpoint** e notare il **documento metadati federazione** URL.</span><span class="sxs-lookup"><span data-stu-id="87f70-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="87f70-150">Si tratta del middleware WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="87f70-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: Endpoint](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="87f70-152">Passare alla nuova registrazione di app.</span><span class="sxs-lookup"><span data-stu-id="87f70-152">Navigate to the new app registration.</span></span> <span data-ttu-id="87f70-153">Fare clic su **impostazioni** > **proprietà** e assicurarsi di annotare il **URI ID App**.</span><span class="sxs-lookup"><span data-stu-id="87f70-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="87f70-154">Si tratta del middleware WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="87f70-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Proprietà registrazione dell'App](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="87f70-156">Aggiungere WS-Federation come provider di accesso esterno per ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="87f70-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="87f70-157">Aggiungere una dipendenza da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al progetto.</span><span class="sxs-lookup"><span data-stu-id="87f70-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="87f70-158">Aggiungere WS-Federation per `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="87f70-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="87f70-159">Accedi con WS-Federation</span><span class="sxs-lookup"><span data-stu-id="87f70-159">Log in with WS-Federation</span></span>

<span data-ttu-id="87f70-160">Passare all'app e fare clic sui **Accedi** collegamento nell'intestazione della barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="87f70-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="87f70-161">È disponibile un'opzione per accedere con WsFederation: ![Pagina di accesso](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="87f70-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="87f70-162">Con ad FS come provider, il pulsante reindirizza a una pagina di accesso di ad FS: ![Pagina di accesso ad FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="87f70-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="87f70-163">Con Azure Active Directory come provider, il pulsante reindirizza a una pagina di accesso di AAD: ![Pagina di accesso di AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="87f70-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="87f70-164">Un accesso aggiuntivo ha esito positivo per un nuovo utente reindirizza alla pagina di registrazione utente dell'app: ![Pagina di registrazione](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="87f70-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="87f70-165">Uso di WS-Federation senza ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="87f70-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="87f70-166">Il middleware WS-Federation può essere usato senza identità.</span><span class="sxs-lookup"><span data-stu-id="87f70-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="87f70-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="87f70-167">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
