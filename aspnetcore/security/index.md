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
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="0d320-103">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="0d320-103">Security</span></span>

*   [<span data-ttu-id="0d320-104">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="0d320-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="0d320-105">Introduzione a Identity</span><span class="sxs-lookup"><span data-stu-id="0d320-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="0d320-106">Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni</span><span class="sxs-lookup"><span data-stu-id="0d320-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="0d320-107">Configurare l'autenticazione Windows</span><span class="sxs-lookup"><span data-stu-id="0d320-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="0d320-108">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="0d320-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="0d320-109">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="0d320-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="0d320-110">Uso dell'autenticazione tramite cookie senza ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0d320-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="0d320-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d320-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="0d320-112">Integrazione di Azure AD in un'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d320-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="0d320-113">Chiamata di un'API Web ASP.NET Core da un'applicazione WPF con Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d320-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="0d320-114">Chiamata di un'API Web in un'applicazione Web ASP.NET Core con Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d320-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="0d320-115">Un'app Web ASP.NET Core con Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="0d320-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="0d320-116">Protezione di app ASP.NET Core con IdentityServer4</span><span class="sxs-lookup"><span data-stu-id="0d320-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="0d320-117">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="0d320-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="0d320-118">Introduzione</span><span class="sxs-lookup"><span data-stu-id="0d320-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="0d320-119">Creare un'app con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="0d320-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="0d320-120">Autorizzazione semplice</span><span class="sxs-lookup"><span data-stu-id="0d320-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="0d320-121">Autorizzazione basata sui ruoli</span><span class="sxs-lookup"><span data-stu-id="0d320-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="0d320-122">Autorizzazione basata sulle attestazioni</span><span class="sxs-lookup"><span data-stu-id="0d320-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="0d320-123">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="0d320-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="0d320-124">Inserimento di dipendenze nei gestori di requisiti</span><span class="sxs-lookup"><span data-stu-id="0d320-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="0d320-125">Autorizzazione basata sulle risorse</span><span class="sxs-lookup"><span data-stu-id="0d320-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="0d320-126">Autorizzazione basata sulle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="0d320-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="0d320-127">Limitazione dell'identità tramite schema</span><span class="sxs-lookup"><span data-stu-id="0d320-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="0d320-128">Protezione dati</span><span class="sxs-lookup"><span data-stu-id="0d320-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="0d320-129">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="0d320-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="0d320-130">Introduzione alle API di protezione dati</span><span class="sxs-lookup"><span data-stu-id="0d320-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="0d320-131">API utente</span><span class="sxs-lookup"><span data-stu-id="0d320-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="0d320-132">Panoramica di API utente</span><span class="sxs-lookup"><span data-stu-id="0d320-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="0d320-133">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="0d320-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="0d320-134">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="0d320-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="0d320-135">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="0d320-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="0d320-136">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="0d320-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="0d320-137">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="0d320-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="0d320-138">Configurazione</span><span class="sxs-lookup"><span data-stu-id="0d320-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="0d320-139">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="0d320-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="0d320-140">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="0d320-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="0d320-141">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="0d320-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="0d320-142">Scenari non compatibili con DI</span><span class="sxs-lookup"><span data-stu-id="0d320-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="0d320-143">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="0d320-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="0d320-144">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="0d320-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="0d320-145">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="0d320-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="0d320-146">Varie API</span><span class="sxs-lookup"><span data-stu-id="0d320-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="0d320-147">Implementazione</span><span class="sxs-lookup"><span data-stu-id="0d320-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="0d320-148">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="0d320-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="0d320-149">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="0d320-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="0d320-150">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="0d320-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="0d320-151">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="0d320-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="0d320-152">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="0d320-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="0d320-153">Crittografia chiavi inattive</span><span class="sxs-lookup"><span data-stu-id="0d320-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="0d320-154">Immutabilità della chiave e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="0d320-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="0d320-155">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="0d320-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="0d320-156">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="0d320-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="0d320-157">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="0d320-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="0d320-158">Condivisione dei cookie tra applicazioni</span><span class="sxs-lookup"><span data-stu-id="0d320-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="0d320-159">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0d320-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="0d320-160">Creare un'app con i dati utente protetti da autorizzazione</span><span class="sxs-lookup"><span data-stu-id="0d320-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="0d320-161">Archiviazione sicura di segreti dell'app durante lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="0d320-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   <span data-ttu-id="0d320-162">[Azure Key Vault configuration provider](key-vault-configuration.md) (Provider di configurazione di Azure Key Vault)</span><span class="sxs-lookup"><span data-stu-id="0d320-162">[Azure Key Vault configuration provider](key-vault-configuration.md)</span></span>
*   [<span data-ttu-id="0d320-163">Applicazione di SSL</span><span class="sxs-lookup"><span data-stu-id="0d320-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="0d320-164">Impostazione di HTTPS per lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="0d320-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="0d320-165">Richiesta intersito falsa</span><span class="sxs-lookup"><span data-stu-id="0d320-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="0d320-166">Prevenzione di attacchi di reindirizzamento aperti</span><span class="sxs-lookup"><span data-stu-id="0d320-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="0d320-167">Prevenzione di XSS</span><span class="sxs-lookup"><span data-stu-id="0d320-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="0d320-168">Abilitazione di richieste multiorigine (CORS)</span><span class="sxs-lookup"><span data-stu-id="0d320-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
