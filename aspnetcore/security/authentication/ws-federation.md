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
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Autenticare gli utenti con WS-Federation in ASP.NET Core

In questa esercitazione viene illustrato come consentire agli utenti di accedere con un provider di autenticazione come Active Directory Federation Services (ADFS) di WS-Federation o [Azure Active Directory](/azure/active-directory/) (AAD). Usa l'app di esempio ASP.NET 2.0 Core descritto in [Facebook, Google, autenticazione basata su provider esterni e](xref:security/authentication/social/index).

Per le applicazioni ASP.NET Core 2.0, il supporto di WS-Federation viene fornito da [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Questo componente è stato trasferito da [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) e condivide molti dei meccanismi di tale componente. Tuttavia, i componenti diversi in un paio di modi importanti.

Per impostazione predefinita, il middleware di nuovo:

* Non consentire accessi indesiderati. Questa funzionalità del protocollo WS-Federation è vulnerabile agli attacchi XSRF. Tuttavia, può essere abilitata con la `AllowUnsolicitedLogins` opzione.
* Non controllare ogni post del form per i messaggi di accesso. Solo le richieste per il `CallbackPath` vengono verificate per l'accesso a componenti `CallbackPath` per impostazione predefinita `/signin-wsfed` ma possono essere modificate. Questo percorso può essere condivisa con altri provider di autenticazione abilitando il `SkipUnrecognizedRequests` opzione.

## <a name="register-the-app-with-active-directory"></a>Registrare l'app in Active Directory

### <a name="active-directory-federation-services"></a>Active Directory Federation Services

* Aprire il server **Aggiunta guidata attendibilità componente** dalla console di gestione di ADFS:

![Aggiungere guidata Trust della Relying Party: introduzione](ws-federation/_static/AdfsAddTrust.png)

* Scegliere di immettere manualmente i dati:

![Aggiunta guidata attendibilità componente: Selezionare l'origine dati](ws-federation/_static/AdfsSelectDataSource.png)

* Immettere un nome visualizzato per la relying party. Il nome non è importante per l'applicazione ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) non offre il supporto per la crittografia dei token, in modo non si configura un certificato di crittografia del token:

![Aggiunta guidata attendibilità componente: Configurazione di certificati](ws-federation/_static/AdfsConfigureCert.png)

* Abilitare il supporto per il protocollo passivo WS-Federation, utilizzando l'URL dell'app. Verificare che la porta sia corretta per l'app:

![Aggiunta guidata attendibilità componente: Configurazione di URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Deve essere un URL HTTPS. IIS Express, è possibile fornire un certificato autofirmato quando si ospita l'app durante lo sviluppo. Kestrel richiede la configurazione manuale del certificato. Vedere il [documentazione Kestrel](xref:fundamentals/servers/kestrel) per altri dettagli.

* Fare clic su **Avanti** attraverso il resto della procedura guidata e **Chiudi** alla fine.

* ASP.NET Identity Core richiede un **ID nome** attestazione. Aggiungere uno di **Modifica regole attestazione** finestra di dialogo:

![Modifica regole attestazione](ws-federation/_static/EditClaimRules.png)

* Nel **Aggiunta guidata regole attestazione trasformare**, lasciare l'impostazione predefinita **inviare attributi LDAP come attestazioni** modello selezionato e fare clic su **Avanti**. Aggiungere un regola mapping di **nome di Account SAM** attributo LDAP per la **ID nome** attestazione in uscita:

![Aggiunta guidata regole attestazione di trasformazione: Configura regola attestazione](ws-federation/_static/AddTransformClaimRule.png)

* Fare clic su **fine** > **OK** nel **Modifica regole attestazione** finestra.

### <a name="azure-active-directory"></a>Azure Active Directory

* Passare al pannello di registrazioni di app del tenant AAD. Fare clic su **nuova registrazione applicazione**:

![Azure Active Directory: Registrazioni di App](ws-federation/_static/AadNewAppRegistration.png)

* Immettere un nome per la registrazione dell'app. Non è importante per l'applicazione ASP.NET Core.
* Immettere l'URL dell'app è in ascolto su come il **Sign-on URL**:

![Azure Active Directory: Creazione registrazione dell'app](ws-federation/_static/AadCreateAppRegistration.png)

* Fare clic su **endpoint** e prendere nota di **documento metadati federazione** URL. Questo è il middleware di WS-Federation `MetadataAddress`:

![Azure Active Directory: endpoint](ws-federation/_static/AadFederationMetadataDocument.png)

* Passare alla nuova registrazione di app. Fare clic su **impostazioni** > **proprietà** e annotare il **URI ID App**. Questo è il middleware di WS-Federation `Wtrealm`:

![Azure Active Directory: Le proprietà di registrazione dell'App](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Aggiungere WS-Federation come provider di accesso esterno per ASP.NET Identity Core

* Aggiungere una dipendenza [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) al progetto.
* WS-Federation per aggiungere il `Configure` metodo *Startup.cs*:

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

Passare all'app e fare clic su di **Accedi** collegamento nell'intestazione della barra di spostamento. È disponibile un'opzione di accesso con WsFederation: ![pagina di accesso](ws-federation/_static/WsFederationButton.png)

Con ADFS come provider, il pulsante reindirizza a una pagina di accesso ADFS: ![ADFS nella pagina di accesso](ws-federation/_static/AdfsLoginPage.png)

Con Azure Active Directory come provider, il pulsante reindirizza a una pagina di accesso AAD: ![AAD nella pagina di accesso](ws-federation/_static/AadSignIn.png)

Una corretta Accedi per un nuovo utente reindirizza alla pagina di registrazione utente dell'app: ![pagina di registrazione](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Utilizzare WS-Federation senza identità ASP.NET Core

Il middleware WS-Federation può essere utilizzato senza identità. Ad esempio:

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
