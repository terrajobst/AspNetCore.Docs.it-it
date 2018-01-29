---
title: Panoramica sulla sicurezza di ASP.NET Core | Microsoft Docs
author: rachelappel
description: Informazioni sui concetti di base di autenticazione, autorizzazione e sicurezza in ASP.NET Core
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: a4558162158ddb6746aa45a29310b42224d6e7fe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-security-overview"></a>Panoramica sulla sicurezza di ASP.NET Core

ASP.NET Core consente agli sviluppatori di configurare e gestire facilmente la sicurezza per le proprie app. ASP.NET Core contiene funzionalità per gestire l'autenticazione, l'autorizzazione, la protezione dei dati, l'applicazione di SSL, i segreti dell'app, la protezione dalla falsificazione delle richieste e la gestione di CORS. Queste funzionalità di sicurezza consentono di compilare app ASP.NET Core solide e sicure. 

## <a name="aspnet-core-security-features"></a>Funzionalità di sicurezza di ASP.NET Core

ASP.NET Core fornisce numerosi strumenti e librerie per proteggere le app, inclusi i provider di identità predefiniti, ma è anche possibile usare servizi di identità di terze parti, ad esempio Facebook, Twitter e LinkedIn. Con ASP.NET Core, è possibile gestire facilmente i segreti dell'app, che consentono di archiviare e usare informazioni riservate senza la necessità di esporle nel codice. 

## <a name="authentication-vs-authorization"></a>Autenticazione e Autorizzazione

L'autenticazione è un processo in cui un utente fornisce le credenziali che vengono quindi confrontate con quelle archiviate in un sistema operativo, un database, un'applicazione o una risorsa. Se corrispondono, gli utenti vengono autenticati correttamente e quindi possono eseguire azioni per cui sono autorizzati, durante un processo di autorizzazione. L'autorizzazione è il processo che determina quali operazioni può eseguire un utente. 

È possibile considerare l'autenticazione come un modo per entrare in uno spazio, ad esempio un server, un database, un'app o una risorsa, mentre l'autorizzazione definisce le azioni che l'utente può eseguire e su quali oggetti all'interno di tale spazio (server, database o app).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilità comuni nel software

ASP.NET Core ed Entity Framework contengono funzionalità che consentono di proteggere le app e impedire violazioni della sicurezza. L'elenco di collegamenti seguente offre documentazione in cui sono descritte le tecniche per evitare le vulnerabilità della sicurezza più comuni nelle app Web:

* [Attacchi di tipo cross-site scripting (XSS)](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Attacchi SQL injection](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Richieste intersito false](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Attacchi di reindirizzamento aperti](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Esistono altre vulnerabilità di cui è necessario essere consapevoli. Per altre informazioni, vedere la sezione in questo documento sulla *documentazione sulla sicurezza di ASP.NET*. 

## <a name="aspnet-security-documentation"></a>Documentazione sulla sicurezza di ASP.NET

*   [Autenticazione](authentication/index.md)
    *   [Introduzione a Identity](authentication/identity.md)
    *   [Abilitare l'autenticazione tramite Facebook, Google e altri provider esterni](authentication/social/index.md)
    * [Configurare l'autenticazione Windows](authentication/windowsauth.md)
    *   [Conferma account e recupero password](authentication/accconfirm.md)
    *   [Autenticazione a due fattori con SMS](authentication/2fa.md) 
    *   [Usare l'autenticazione tramite cookie senza Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrare Azure AD in un'app Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Chiamare un'API Web ASP.NET Core da un'app WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Chiamare un'API Web in un'app Web ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Un'app Web ASP.NET Core con Azure Active Directory B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Proteggere app ASP.NET Core con IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorizzazione](authorization/index.md)
    *   [Introduzione](authorization/introduction.md)
    *   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
    *   [Autorizzazione semplice](authorization/simple.md)
    *   [Autorizzazione basata sui ruoli](authorization/roles.md)
    *   [Autorizzazione basata sulle attestazioni](authorization/claims.md)
    *   [Autorizzazione basata sui criteri](authorization/policies.md)
    *   [Inserimento di dipendenze nei gestori di requisiti](authorization/dependencyinjection.md)
    *   [Autorizzazione basata su risorse](authorization/resourcebased.md)
    *   [Autorizzazione basata sulla visualizzazione](authorization/views.md)
    *   [Limitare l'identità tramite schema](authorization/limitingidentitybyscheme.md)
*   [Protezione dati](data-protection/index.md)
    *   [Introduzione alla protezione dati](data-protection/introduction.md)
    *   [Introduzione alle Data Protection API](data-protection/using-data-protection.md)
    *   [API utente](data-protection/consumer-apis/index.md)
        *   [Panoramica di API utente](data-protection/consumer-apis/overview.md)
        *   [Stringhe di scopi](data-protection/consumer-apis/purpose-strings.md)
        *   [Gerarchia di scopi e multi-tenancy](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hashing della password](data-protection/consumer-apis/password-hashing.md)
        *   [Limitare la durata di payload protetti](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Payload non protetti le cui chiavi sono state revocate](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Configurazione](data-protection/configuration/index.md)
        *   [Configurare la protezione dati](data-protection/configuration/overview.md)
        *   [Impostazioni predefinite](data-protection/configuration/default-settings.md)
        *   [Criteri a livello di computer](data-protection/configuration/machine-wide-policy.md)
        *   [Scenari non compatibili con l'inserimento di dipendenze](data-protection/configuration/non-di-scenarios.md)
    *   [API di estendibilità](data-protection/extensibility/index.md)
        *   [Estendibilità della crittografia Core](data-protection/extensibility/core-crypto.md)
        *   [Estendibilità della gestione delle chiavi](data-protection/extensibility/key-management.md)
        *   [Varie API](data-protection/extensibility/misc-apis.md)
    *   [Implementazione](data-protection/implementation/index.md)
        *   [Dettagli di crittografia autenticata](data-protection/implementation/authenticated-encryption-details.md)
        *   [Derivazione di sottochiavi e crittografia autenticata](data-protection/implementation/subkeyderivation.md)
        *   [Intestazioni di contesto](data-protection/implementation/context-headers.md)
        *   [Gestione delle chiavi](data-protection/implementation/key-management.md)
        *   [Provider di archiviazione chiavi](data-protection/implementation/key-storage-providers.md)
        *   [Crittografia a chiave inattiva](data-protection/implementation/key-encryption-at-rest.md)
        *   [Immutabilità delle chiavi e modifica delle impostazioni](data-protection/implementation/key-immutability.md)
        *   [Formato di archiviazione chiavi](data-protection/implementation/key-storage-format.md)
        *   [Provider di protezione dati temporanea](data-protection/implementation/key-storage-ephemeral.md)
    *   [Compatibilità](data-protection/compatibility/index.md)
        *   [Condivisione di cookie tra le app](data-protection/compatibility/cookie-sharing.md)
        *   [Sostituire <machineKey> in ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
*   [Archiviazione sicura di segreti dell'app durante lo sviluppo](app-secrets.md)
*   [Azure Key Vault configuration provider](key-vault-configuration.md) (Provider di configurazione di Azure Key Vault)
*   [Imporre SSL](enforcing-ssl.md)
*   [Richiesta intersito falsa](anti-request-forgery.md)
*   [Prevenire attacchi di reindirizzamento aperto](preventing-open-redirects.md)
*   [Evitare il cross-site scripting](cross-site-scripting.md)
*   [Abilitare richieste tra le origini (CORS)](cors.md)
