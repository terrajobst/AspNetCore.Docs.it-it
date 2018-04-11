---
title: Panoramica sulla sicurezza di ASP.NET Core
author: rachelappel
description: Informazioni sui concetti di base di autenticazione, autorizzazione e sicurezza in ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>Panoramica sulla sicurezza di ASP.NET Core

ASP.NET Core consente agli sviluppatori di configurare e gestire facilmente la sicurezza per le proprie app. ASP.NET Core contiene funzionalità per gestire l'autenticazione, l'autorizzazione, la protezione dei dati, l'applicazione di SSL, i segreti dell'app, la protezione dalla falsificazione delle richieste e la gestione di CORS. Queste funzionalità di sicurezza consentono di compilare app ASP.NET Core solide e sicure.

## <a name="aspnet-core-security-features"></a>Funzionalità di sicurezza di ASP.NET Core

ASP.NET Core fornisce numerosi strumenti e librerie per proteggere le app, inclusi i provider di identità predefiniti, ma è anche possibile usare servizi di identità di terze parti, ad esempio Facebook, Twitter e LinkedIn. Con ASP.NET Core, è possibile gestire facilmente i segreti dell'app, che consentono di archiviare e usare informazioni riservate senza la necessità di esporle nel codice.

## <a name="authentication-vs-authorization"></a>Autenticazione e Autorizzazione

L'autenticazione è un processo in cui un utente fornisce le credenziali che vengono quindi confrontate con quelle archiviate in un sistema operativo, un database, un'applicazione o una risorsa. Se corrispondono, gli utenti vengono autenticati correttamente e quindi possono eseguire azioni per cui sono autorizzati, durante un processo di autorizzazione. L'autorizzazione è il processo che determina quali operazioni può eseguire un utente.

È possibile considerare l'autenticazione come un modo per entrare in uno spazio, ad esempio un server, un database, un'app o una risorsa, mentre l'autorizzazione definisce le azioni che l'utente può eseguire e su quali oggetti all'interno di tale spazio (server, database o app).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilità comuni nel software

ASP.NET Core ed Entity Framework contengono funzionalità che consentono di proteggere le app e impedire violazioni della sicurezza. L'elenco di collegamenti seguente offre documentazione in cui sono descritte le tecniche per evitare le vulnerabilità della sicurezza più comuni nelle app Web:

* [Attacchi di tipo cross-site scripting (XSS)](xref:security/cross-site-scripting)
* [Attacchi SQL injection](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Richieste intersito false](xref:security/anti-request-forgery)
* [Attacchi di reindirizzamento aperti](xref:security/preventing-open-redirects)

Esistono altre vulnerabilità di cui è necessario essere consapevoli. Per altre informazioni, vedere la sezione in questo documento sulla *documentazione sulla sicurezza di ASP.NET*.

## <a name="aspnet-security-documentation"></a>Documentazione sulla sicurezza di ASP.NET

*   [Autenticazione](xref:security/authentication/index)
    *   [Introduzione a Identity](xref:security/authentication/identity)
    *   [Abilitare l'autenticazione tramite Facebook, Google e altri provider esterni](xref:security/authentication/social/index)
    *   [Abilitare l'autenticazione con WS-Federation](xref:security/authentication/ws-federation)
    * [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth)
    *   [Conferma account e recupero password](xref:security/authentication/accconfirm)
    *   [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
    *   [Usare l'autenticazione tramite cookie senza Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrare Azure AD in un'app Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Chiamare un'API Web ASP.NET Core da un'app WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Chiamare un'API Web in un'app Web ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Un'app Web ASP.NET Core con Azure Active Directory B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Proteggere app ASP.NET Core con IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorizzazione](xref:security/authorization/index)
    *   [Introduzione](xref:security/authorization/introduction)
    *   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
    *   [Autorizzazione semplice](xref:security/authorization/simple)
    *   [Autorizzazione basata sui ruoli](xref:security/authorization/roles)
    *   [Autorizzazione basata sulle attestazioni](xref:security/authorization/claims)
    *   [Autorizzazione basata sui criteri](xref:security/authorization/policies)
    *   [Inserimento di dipendenze nei gestori di requisiti](xref:security/authorization/dependencyinjection)
    *   [Autorizzazione basata su risorse](xref:security/authorization/resourcebased)
    *   [Autorizzazione basata sulla visualizzazione](xref:security/authorization/views)
    *   [Limitare l'identità tramite schema](xref:security/authorization/limitingidentitybyscheme)
*   [Protezione dati](xref:security/data-protection/index)
    *   [Introduzione alla protezione dati](xref:security/data-protection/introduction)
    *   [Introduzione alle Data Protection API](xref:security/data-protection/using-data-protection)
    *   [API utente](xref:security/data-protection/consumer-apis/index)
        *   [Panoramica di API utente](xref:security/data-protection/consumer-apis/overview)
        *   [Stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Gerarchia di scopi e multi-tenancy](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Password con hash](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Limitare la durata di payload protetti](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Payload non protetti le cui chiavi sono state revocate](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Configurazione](xref:security/data-protection/configuration/index)
        *   [Configurare la protezione dati](xref:security/data-protection/configuration/overview)
        *   [Impostazioni predefinite](xref:security/data-protection/configuration/default-settings)
        *   [Criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Scenari non compatibili con l'inserimento di dipendenze](xref:security/data-protection/configuration/non-di-scenarios)
    *   [API di estendibilità](xref:security/data-protection/extensibility/index)
        *   [Estendibilità della crittografia Core](xref:security/data-protection/extensibility/core-crypto)
        *   [Estendibilità della gestione delle chiavi](xref:security/data-protection/extensibility/key-management)
        *   [Varie API](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementazione](xref:security/data-protection/implementation/index)
        *   [Dettagli di crittografia autenticata](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Derivazione di sottochiavi e crittografia autenticata](xref:security/data-protection/implementation/subkeyderivation)
        *   [Intestazioni di contesto](xref:security/data-protection/implementation/context-headers)
        *   [Gestione delle chiavi](xref:security/data-protection/implementation/key-management)
        *   [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers)
        *   [Crittografia a chiave inattiva](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Immutabilità delle chiavi e impostazioni](xref:security/data-protection/implementation/key-immutability)
        *   [Formato di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-format)
        *   [Provider di protezione dati temporanea](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Compatibilità](xref:security/data-protection/compatibility/index)
        *   [Sostituire <machineKey> in ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
*   [Archiviazione sicura di segreti dell'app durante lo sviluppo](xref:security/app-secrets)
*   [Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)
*   [Imporre SSL](xref:security/enforcing-ssl)
*   [Richiesta intersito falsa](xref:security/anti-request-forgery)
*   [Prevenire attacchi di reindirizzamento aperto](xref:security/preventing-open-redirects)
*   [Evitare il cross-site scripting](xref:security/cross-site-scripting)
*   [Abilitare richieste tra le origini (CORS)](xref:security/cors)
*   [Condivisione di cookie tra le app](xref:security/cookie-sharing)
