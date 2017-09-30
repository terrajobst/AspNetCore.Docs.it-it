---
title: Sicurezza
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: f173d03f55a1ce52222a75c023f9e8a20d5c60dc
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="security"></a>Sicurezza

*   [Autenticazione](authentication/index.md)
    *   [Introduzione a Identity](authentication/identity.md)
    *   [Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni](authentication/social/index.md)
    * [Configurare l'autenticazione Windows](authentication/windowsauth.md)
    *   [Conferma account e recupero password](authentication/accconfirm.md)
    *   [Autenticazione a due fattori con SMS](authentication/2fa.md) 
    *   [Uso dell'autenticazione tramite cookie senza ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrazione di Azure AD in un'app Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Chiamata di un'API Web ASP.NET Core da un'applicazione WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Chiamata di un'API Web in un'applicazione Web ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Un'app Web ASP.NET Core con Azure Active Directory B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Protezione di app ASP.NET Core con IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorizzazione](authorization/index.md)
    *   [Introduzione](authorization/introduction.md)
    *   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
    *   [Autorizzazione semplice](authorization/simple.md)
    *   [Autorizzazione basata sui ruoli](authorization/roles.md)
    *   [Autorizzazione basata sulle attestazioni](authorization/claims.md)
    *   [Autorizzazione personalizzata basata su criteri](authorization/policies.md)
    *   [Inserimento di dipendenze nei gestori di requisiti](authorization/dependencyinjection.md)
    *   [Autorizzazione basata sulle risorse](authorization/resourcebased.md)
    *   [Autorizzazione basata sulle visualizzazioni](authorization/views.md)
    *   [Limitazione dell'identità tramite schema](authorization/limitingidentitybyscheme.md)
*   [Protezione dati](data-protection/index.md)
    *   [Introduzione alla protezione dati](data-protection/introduction.md)
    *   [Introduzione alle API di protezione dati](data-protection/using-data-protection.md)
    *   [API utente](data-protection/consumer-apis/index.md)
        *   [Panoramica di API utente](data-protection/consumer-apis/overview.md)
        *   [Stringhe di scopi](data-protection/consumer-apis/purpose-strings.md)
        *   [Gerarchia di scopi e multi-tenancy](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hashing della password](data-protection/consumer-apis/password-hashing.md)
        *   [Limitazione della durata di payload protetti](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Rimozione della protezione di payload le cui chiavi sono state revocate](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Configurazione](data-protection/configuration/index.md)
        *   [Configurazione della protezione dati](data-protection/configuration/overview.md)
        *   [Impostazioni predefinite](data-protection/configuration/default-settings.md)
        *   [Criteri a livello di computer](data-protection/configuration/machine-wide-policy.md)
        *   [Scenari non compatibili con DI](data-protection/configuration/non-di-scenarios.md)
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
        *   [Crittografia chiavi inattive](data-protection/implementation/key-encryption-at-rest.md)
        *   [Immutabilità della chiave e modifica delle impostazioni](data-protection/implementation/key-immutability.md)
        *   [Formato di archiviazione chiavi](data-protection/implementation/key-storage-format.md)
        *   [Provider di protezione dati temporanea](data-protection/implementation/key-storage-ephemeral.md)
    *   [Compatibilità](data-protection/compatibility/index.md)
        *   [Condivisione dei cookie tra applicazioni](data-protection/compatibility/cookie-sharing.md)
        *   [Sostituzione di <machineKey> in ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Creare un'app con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
*   [Archiviazione sicura di segreti dell'app durante lo sviluppo](app-secrets.md)
*   [Azure Key Vault configuration provider](key-vault-configuration.md) (Provider di configurazione di Azure Key Vault)
*   [Applicazione di SSL](enforcing-ssl.md)
*   [Richiesta intersito falsa](anti-request-forgery.md)
*   [Prevenzione di attacchi di reindirizzamento aperti](preventing-open-redirects.md)
*   [Prevenzione di XSS](cross-site-scripting.md)
*   [Abilitazione di richieste multiorigine (CORS)](cors.md)
