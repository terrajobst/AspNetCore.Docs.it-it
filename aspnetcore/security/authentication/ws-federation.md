---
title: Autenticare gli utenti con WS-Federation in ASP.NET Core
author: chlowell
description: In questa esercitazione viene illustrato come utilizzare WS-Federation in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898804"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="679d8-103">Autenticare gli utenti con WS-Federation in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="679d8-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="679d8-104">In questa esercitazione viene illustrato come consentire agli utenti di accedere con un provider di autenticazione come Active Directory Federation Services (ADFS) di WS-Federation o [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="679d8-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="679d8-105">Usa l'app di esempio ASP.NET 2.0 Core descritto in [Facebook, Google, autenticazione basata su provider esterni e](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="679d8-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="679d8-106">Per le applicazioni ASP.NET Core 2.0, il supporto di WS-Federation viene fornito da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="679d8-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="679d8-107">Questo componente è stato trasferito da [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) e condivide molti dei meccanismi di tale componente.</span><span class="sxs-lookup"><span data-stu-id="679d8-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="679d8-108">Tuttavia, i componenti diversi in un paio di modi importanti.</span><span class="sxs-lookup"><span data-stu-id="679d8-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="679d8-109">Per impostazione predefinita, il middleware di nuovo:</span><span class="sxs-lookup"><span data-stu-id="679d8-109">By default, the new middleware:</span></span>

* <span data-ttu-id="679d8-110">Non consentire accessi indesiderati.</span><span class="sxs-lookup"><span data-stu-id="679d8-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="679d8-111">Questa funzionalità del protocollo WS-Federation è vulnerabile agli attacchi XSRF.</span><span class="sxs-lookup"><span data-stu-id="679d8-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="679d8-112">Tuttavia, può essere abilitata con la `AllowUnsolicitedLogins` opzione.</span><span class="sxs-lookup"><span data-stu-id="679d8-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="679d8-113">Non controllare ogni post del form per i messaggi di accesso.</span><span class="sxs-lookup"><span data-stu-id="679d8-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="679d8-114">Solo le richieste per il `CallbackPath` vengono verificate per l'accesso a componenti `CallbackPath` per impostazione predefinita `/signin-wsfed` ma possono essere modificate.</span><span class="sxs-lookup"><span data-stu-id="679d8-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="679d8-115">Questo percorso può essere condivisa con altri provider di autenticazione abilitando il `SkipUnrecognizedRequests` opzione.</span><span class="sxs-lookup"><span data-stu-id="679d8-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="679d8-116">Registrare l'app in Active Directory</span><span class="sxs-lookup"><span data-stu-id="679d8-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="679d8-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="679d8-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="679d8-118">Aprire il server **Aggiunta guidata attendibilità componente** dalla console di gestione di ADFS:</span><span class="sxs-lookup"><span data-stu-id="679d8-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Aggiungere guidata Trust della Relying Party: introduzione](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="679d8-120">Scegliere di immettere manualmente i dati:</span><span class="sxs-lookup"><span data-stu-id="679d8-120">Choose to enter data manually:</span></span>

![Aggiunta guidata attendibilità componente: Selezionare l'origine dati](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="679d8-122">Immettere un nome visualizzato per la relying party.</span><span class="sxs-lookup"><span data-stu-id="679d8-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="679d8-123">Il nome non è importante per l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="679d8-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="679d8-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) non offre il supporto per la crittografia dei token, in modo non si configura un certificato di crittografia del token:</span><span class="sxs-lookup"><span data-stu-id="679d8-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Aggiunta guidata attendibilità componente: Configurazione di certificati](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="679d8-126">Abilitare il supporto per il protocollo passivo WS-Federation, utilizzando l'URL dell'app.</span><span class="sxs-lookup"><span data-stu-id="679d8-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="679d8-127">Verificare che la porta sia corretta per l'app:</span><span class="sxs-lookup"><span data-stu-id="679d8-127">Verify the port is correct for the app:</span></span>

![Aggiunta guidata attendibilità componente: Configurazione di URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="679d8-129">Deve essere un URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="679d8-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="679d8-130">IIS Express, è possibile fornire un certificato autofirmato quando si ospita l'app durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="679d8-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="679d8-131">Kestrel richiede la configurazione manuale del certificato.</span><span class="sxs-lookup"><span data-stu-id="679d8-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="679d8-132">Vedere il [documentazione Kestrel](xref:fundamentals/servers/kestrel) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="679d8-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="679d8-133">Fare clic su **Avanti** attraverso il resto della procedura guidata e **Chiudi** alla fine.</span><span class="sxs-lookup"><span data-stu-id="679d8-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="679d8-134">ASP.NET Identity Core richiede un **ID nome** attestazione.</span><span class="sxs-lookup"><span data-stu-id="679d8-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="679d8-135">Aggiungere uno di **Modifica regole attestazione** finestra di dialogo:</span><span class="sxs-lookup"><span data-stu-id="679d8-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Modifica regole attestazione](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="679d8-137">Nel **Aggiunta guidata regole attestazione trasformare**, lasciare l'impostazione predefinita **inviare attributi LDAP come attestazioni** modello selezionato e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="679d8-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="679d8-138">Aggiungere un regola mapping di **nome di Account SAM** attributo LDAP per la **ID nome** attestazione in uscita:</span><span class="sxs-lookup"><span data-stu-id="679d8-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Aggiunta guidata regole attestazione di trasformazione: Configura regola attestazione](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="679d8-140">Fare clic su **fine** > **OK** nel **Modifica regole attestazione** finestra.</span><span class="sxs-lookup"><span data-stu-id="679d8-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="679d8-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="679d8-141">Azure Active Directory</span></span>

* <span data-ttu-id="679d8-142">Passare al pannello di registrazioni di app del tenant AAD.</span><span class="sxs-lookup"><span data-stu-id="679d8-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="679d8-143">Fare clic su **nuova registrazione applicazione**:</span><span class="sxs-lookup"><span data-stu-id="679d8-143">Click **New application registration**:</span></span>

![Azure Active Directory: Registrazioni di App](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="679d8-145">Immettere un nome per la registrazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="679d8-145">Enter a name for the app registration.</span></span> <span data-ttu-id="679d8-146">Non è importante per l'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="679d8-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="679d8-147">Immettere l'URL dell'app è in ascolto su come il **Sign-on URL**:</span><span class="sxs-lookup"><span data-stu-id="679d8-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Creazione registrazione dell'app](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="679d8-149">Fare clic su **endpoint** e prendere nota di **documento metadati federazione** URL.</span><span class="sxs-lookup"><span data-stu-id="679d8-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="679d8-150">Questo è il middleware di WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="679d8-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: endpoint](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="679d8-152">Passare alla nuova registrazione di app.</span><span class="sxs-lookup"><span data-stu-id="679d8-152">Navigate to the new app registration.</span></span> <span data-ttu-id="679d8-153">Fare clic su **impostazioni** > **proprietà** e annotare il **URI ID App**.</span><span class="sxs-lookup"><span data-stu-id="679d8-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="679d8-154">Questo è il middleware di WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="679d8-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Le proprietà di registrazione dell'App](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="679d8-156">Aggiungere WS-Federation come provider di accesso esterno per ASP.NET Identity Core</span><span class="sxs-lookup"><span data-stu-id="679d8-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="679d8-157">Aggiungere una dipendenza [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al progetto.</span><span class="sxs-lookup"><span data-stu-id="679d8-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="679d8-158">WS-Federation per aggiungere il `Configure` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="679d8-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="679d8-159">Accedi con WS-Federation</span><span class="sxs-lookup"><span data-stu-id="679d8-159">Log in with WS-Federation</span></span>

<span data-ttu-id="679d8-160">Passare all'app e fare clic su di **Accedi** collegamento nell'intestazione della barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="679d8-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="679d8-161">È disponibile un'opzione di accesso con WsFederation: ![pagina di accesso](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="679d8-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="679d8-162">Con ADFS come provider, il pulsante reindirizza a una pagina di accesso ADFS: ![ADFS nella pagina di accesso](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="679d8-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="679d8-163">Con Azure Active Directory come provider, il pulsante reindirizza a una pagina di accesso AAD: ![AAD nella pagina di accesso](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="679d8-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="679d8-164">Una corretta Accedi per un nuovo utente reindirizza alla pagina di registrazione utente dell'app: ![pagina di registrazione](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="679d8-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="679d8-165">Utilizzare WS-Federation senza identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="679d8-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="679d8-166">Il middleware WS-Federation può essere utilizzato senza identità.</span><span class="sxs-lookup"><span data-stu-id="679d8-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="679d8-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="679d8-167">For example:</span></span>

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
