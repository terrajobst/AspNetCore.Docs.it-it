---
title: Autenticare gli utenti con WS-Federation in ASP.NET Core
author: chlowell
description: Questa esercitazione viene illustrato come utilizzare WS-Federation in un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 6b568928aaf6c958d66279af9fef80ac0c968c8b
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396155"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Autenticare gli utenti con WS-Federation in ASP.NET Core

Questa esercitazione illustra come consentire agli utenti di accedere con un provider di autenticazione WS-Federation, ad esempio Active Directory Federation Services (ADFS) o [Azure Active Directory](/azure/active-directory/) (AAD). Usa l'app di esempio ASP.NET Core 2.0 descritto in [Facebook, Google e l'autenticazione esterna provider](xref:security/authentication/social/index).

Per le app ASP.NET Core 2.0, WS-Federation supporto viene fornito da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Questo componente viene trasferito dal [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) e condivide molti dei meccanismi del componente. Tuttavia, i componenti sono diversi in un paio di modi importanti.

Per impostazione predefinita, il middleware di nuovo:

* Non consentire l'accesso non richiesto. Questa funzionalità del protocollo WS-Federation è vulnerabile agli attacchi XSRF. Tuttavia, può essere abilitata con la `AllowUnsolicitedLogins` opzione.
* Non controlla ogni post del form per messaggi di accesso. Solo le richieste per il `CallbackPath` vengono verificate per l'opzione sign `CallbackPath` per impostazione predefinita `/signin-wsfed` ma può essere modificato tramite ereditato [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) proprietà del [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) classe. Questo percorso può essere condivisa con altri provider di autenticazione, consentendo il [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opzione.

## <a name="register-the-app-with-active-directory"></a>Registrare l'app con Active Directory

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* Aprire il server **Aggiunta guidata attendibilità componente** dalla console di gestione di ad FS:

![Aggiunta guidata attendibilità Relying Party: Introduzione](ws-federation/_static/AdfsAddTrust.png)

* Scegliere di immettere manualmente i dati:

![Aggiunta guidata attendibilità Relying Party: Seleziona l'origine dati](ws-federation/_static/AdfsSelectDataSource.png)

* Immettere un nome visualizzato per la relying party. Il nome non è importante per l'app ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) manca il supporto per la crittografia dei token, in modo che non si configura un certificato di crittografia di token:

![Aggiunta guidata attendibilità Relying Party: Configurare il certificato](ws-federation/_static/AdfsConfigureCert.png)

* Abilitare il supporto per il protocollo passivo WS-Federation, usando l'URL dell'app. Verificare che la porta sia corretta per l'app:

![Aggiunta guidata attendibilità Relying Party: Configura URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Deve trattarsi di un URL HTTPS. IIS Express, è possibile fornire un certificato autofirmato per l'hosting dell'app durante lo sviluppo. Kestrel richiede la configurazione manuale del certificato. Vedere le [documentazione di Kestrel](xref:fundamentals/servers/kestrel) per altri dettagli.

* Fare clic su **successivo** attraverso il resto della procedura guidata e **Chiudi** alla fine.

* ASP.NET Core Identity richiede un **ID nome** di attestazione. Aggiungere uno dal **Modifica regole attestazione** finestra di dialogo:

![Modifica regole attestazione](ws-federation/_static/EditClaimRules.png)

* Nel **Nell'Aggiunta guidata regole attestazione di trasformazione**, lasciare l'impostazione predefinita **inviare attributi LDAP come attestazioni** modello selezionato e fare clic su **Next**. Aggiungere un regola mapping il **SAM-Account-Name** attributo LDAP per il **ID nome** attestazione in uscita:

![Aggiunta guidata regole attestazione di trasformazione: Configura regola attestazione](ws-federation/_static/AddTransformClaimRule.png)

* Fare clic su **Finish** > **OK** nel **Edit Claim Rules** finestra.

### <a name="azure-active-directory"></a>Azure Active Directory

* Passare al pannello registrazioni per l'app del tenant AAD. Fare clic su **registrazione nuova applicazione**:

![Azure Active Directory: Registrazioni per l'App](ws-federation/_static/AadNewAppRegistration.png)

* Immettere un nome per la registrazione dell'app. Non è importante per l'app ASP.NET Core.
* Immettere l'URL dell'app è in ascolto su come le **Sign-on di URL**:

![Azure Active Directory: Creare una registrazione di app](ws-federation/_static/AadCreateAppRegistration.png)

* Fare clic su **endpoint** e notare il **documento metadati federazione** URL. Si tratta del middleware WS-Federation `MetadataAddress`:

![Azure Active Directory: Endpoint](ws-federation/_static/AadFederationMetadataDocument.png)

* Passare alla nuova registrazione di app. Fare clic su **impostazioni** > **proprietà** e assicurarsi di annotare il **URI ID App**. Si tratta del middleware WS-Federation `Wtrealm`:

![Azure Active Directory: Proprietà registrazione dell'App](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Aggiungere WS-Federation come provider di accesso esterno per ASP.NET Core Identity

* Aggiungere una dipendenza da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al progetto.
* Aggiungere WS-Federation per `Startup.ConfigureServices`:

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

### <a name="log-in-with-ws-federation"></a>Accedi con WS-Federation

Passare all'app e fare clic sui **Accedi** collegamento nell'intestazione della barra di spostamento. È disponibile un'opzione per accedere con WsFederation: ![Pagina di accesso](ws-federation/_static/WsFederationButton.png)

Con ad FS come provider, il pulsante reindirizza a una pagina di accesso di ad FS: ![Pagina di accesso ad FS](ws-federation/_static/AdfsLoginPage.png)

Con Azure Active Directory come provider, il pulsante reindirizza a una pagina di accesso di AAD: ![Pagina di accesso di AAD](ws-federation/_static/AadSignIn.png)

Un accesso aggiuntivo ha esito positivo per un nuovo utente reindirizza alla pagina di registrazione utente dell'app: ![Pagina di registrazione](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Uso di WS-Federation senza ASP.NET Core Identity

Il middleware WS-Federation può essere usato senza identità. Ad esempio:

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
