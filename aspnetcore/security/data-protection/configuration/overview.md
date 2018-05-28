---
title: Configurare la protezione dei dati di ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare la protezione dei dati in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 803b81f5f69496900791ca1d1976f70f8c266f29
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/27/2018
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="b3891-103">Configurare la protezione dei dati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3891-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="b3891-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b3891-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b3891-105">Quando viene inizializzato il sistema di protezione dei dati, si applica [impostazioni predefinite](xref:security/data-protection/configuration/default-settings) in base all'ambiente operativo.</span><span class="sxs-lookup"><span data-stu-id="b3891-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="b3891-106">Queste impostazioni sono in genere adatte per le applicazioni in esecuzione in un singolo computer.</span><span class="sxs-lookup"><span data-stu-id="b3891-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="b3891-107">Esistono casi in cui è possibile che uno sviluppatore desidera modificare le impostazioni predefinite:</span><span class="sxs-lookup"><span data-stu-id="b3891-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="b3891-108">L'app viene distribuito tra più computer.</span><span class="sxs-lookup"><span data-stu-id="b3891-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="b3891-109">Per motivi di conformità.</span><span class="sxs-lookup"><span data-stu-id="b3891-109">For compliance reasons.</span></span>

<span data-ttu-id="b3891-110">Per questi scenari, il sistema di protezione dei dati offre un'API di configurazione avanzate.</span><span class="sxs-lookup"><span data-stu-id="b3891-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="b3891-111">Analogamente ai file di configurazione, l'anello di chiave di protezione dati devono essere protetti utilizzando le autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="b3891-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="b3891-112">È possibile scegliere di crittografare le chiavi inattivi, ma ciò non impedisce agli utenti malintenzionati di creazione di nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="b3891-113">Di conseguenza, la sicurezza dell'app è stata interessata.</span><span class="sxs-lookup"><span data-stu-id="b3891-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="b3891-114">Il percorso di archiviazione configurato con la protezione dei dati deve avere l'accesso limitato alle app stessa, simile a quello che si proteggono i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b3891-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="b3891-115">Ad esempio, se si sceglie di archiviare le chiavi nel disco, utilizzare le autorizzazioni del file.</span><span class="sxs-lookup"><span data-stu-id="b3891-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="b3891-116">Verificare che solo l'identità con cui viene eseguita l'app web è leggere, scrivere e creare l'accesso alla directory.</span><span class="sxs-lookup"><span data-stu-id="b3891-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="b3891-117">Se si utilizza l'archiviazione tabelle di Azure, solo l'app web deve avere la possibilità di leggere, scrivere o creare nuove voci dall'archivio delle tabelle, e così via.</span><span class="sxs-lookup"><span data-stu-id="b3891-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="b3891-118">Il metodo di estensione [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) restituisce un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b3891-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="b3891-119">`IDataProtectionBuilder` espone i metodi di estensione che è possibile concatenare per configurare la protezione dati opzioni.</span><span class="sxs-lookup"><span data-stu-id="b3891-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="b3891-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="b3891-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="b3891-121">Per archiviare le chiavi in [insieme credenziali chiavi Azure](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="b3891-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="b3891-122">Impostare il percorso di archiviazione chiavi (ad esempio [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="b3891-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="b3891-123">Il percorso deve essere impostato poiché se si chiama `ProtectKeysWithAzureKeyVault` implementa un' [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) che disabilita le impostazioni di protezione automatica dei dati, incluso il percorso di archiviazione chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="b3891-124">L'esempio precedente Usa archiviazione Blob di Azure per rendere persistenti la gestione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="b3891-125">Per altre informazioni, vedere [provider di archiviazione chiavi: Azure e Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="b3891-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="b3891-126">È inoltre possibile mantenere l'anello chiave in locale con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="b3891-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="b3891-127">Il `keyIdentifier` è l'identificatore di chiave dell'insieme di credenziali chiave utilizzata per la crittografia chiave (ad esempio, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="b3891-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="b3891-128">`ProtectKeysWithAzureKeyVault` Overload:</span><span class="sxs-lookup"><span data-stu-id="b3891-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="b3891-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) consente di utilizzare un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) per abilitare il sistema di protezione dati utilizzare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b3891-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) consente di utilizzare un `ClientId` e [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) per abilitare il sistema di protezione dati utilizzare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b3891-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) consente di utilizzare un `ClientId` e `ClientSecret` per abilitare il sistema di protezione dati utilizzare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3891-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="b3891-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="b3891-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="b3891-133">Per archiviare le chiavi in una condivisione UNC invece che nel *% LOCALAPPDATA %* percorso predefinito, configurare il sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="b3891-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="b3891-134">Se si modifica il percorso della chiave di persistenza, il sistema di Crittografa non viene più chiavi inattivi, poiché non è chiaro se DPAPI è un meccanismo di crittografia appropriati.</span><span class="sxs-lookup"><span data-stu-id="b3891-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="b3891-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="b3891-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="b3891-136">È possibile configurare il sistema per proteggere le chiavi inattivi chiamando uno del [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) le API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b3891-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="b3891-137">Si consideri l'esempio riportato di seguito, in cui le chiavi vengono archiviate in una condivisione UNC e consente di crittografare le chiavi inattivi con un certificato x. 509 specifico:</span><span class="sxs-lookup"><span data-stu-id="b3891-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="b3891-138">Vedere [chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) per ulteriori esempi e una discussione sui meccanismi di crittografia con chiave incorporata.</span><span class="sxs-lookup"><span data-stu-id="b3891-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="b3891-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="b3891-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="b3891-140">Per configurare il sistema per l'utilizzo di una durata di 14 giorni anziché il valore predefinito di 90 giorni, utilizzare [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="b3891-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="b3891-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="b3891-141">SetApplicationName</span></span>

<span data-ttu-id="b3891-142">Per impostazione predefinita, il sistema di protezione dei dati consente di isolare le applicazioni da un altro, anche se essi condividono lo stesso repository chiave fisico.</span><span class="sxs-lookup"><span data-stu-id="b3891-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="b3891-143">Ciò impedisce che le app la comprensione di altro payload protetto.</span><span class="sxs-lookup"><span data-stu-id="b3891-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="b3891-144">Per condividere un payload protetto tra due applicazioni, utilizzare [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con lo stesso valore per ogni app:</span><span class="sxs-lookup"><span data-stu-id="b3891-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="b3891-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="b3891-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="b3891-146">È possibile uno scenario in cui si desidera distribuire automaticamente le chiavi (creazione di nuove chiavi) come l'approccio della scadenza di un'app.</span><span class="sxs-lookup"><span data-stu-id="b3891-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="b3891-147">Un esempio potrebbe essere le app configurate in una relazione primario o secondario, in cui solo l'app principale è responsabile per motivi di gestione delle chiavi e App secondari hanno semplicemente una visualizzazione di sola lettura dell'anello chiave.</span><span class="sxs-lookup"><span data-stu-id="b3891-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="b3891-148">Le app secondarie possono essere configurate per considerare l'anello chiave come sola lettura tramite la configurazione di sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="b3891-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="b3891-149">Isolamento per ogni applicazione</span><span class="sxs-lookup"><span data-stu-id="b3891-149">Per-application isolation</span></span>

<span data-ttu-id="b3891-150">Quando il sistema di protezione dei dati viene fornito da un host ASP.NET Core, automaticamente consente di isolare le app da un altro, anche se tali applicazioni sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso.</span><span class="sxs-lookup"><span data-stu-id="b3891-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="b3891-151">Ciò è simile al modificatore IsolateApps da System. Web  **\<machineKey >** elemento.</span><span class="sxs-lookup"><span data-stu-id="b3891-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="b3891-152">Il meccanismo di isolamento funziona considerando ogni app nel computer locale come tenant univoco, pertanto il [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted per qualsiasi applicazione specificata include automaticamente l'ID dell'app come discriminatore.</span><span class="sxs-lookup"><span data-stu-id="b3891-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="b3891-153">ID univoco dell'applicazione proviene da una delle due posizioni:</span><span class="sxs-lookup"><span data-stu-id="b3891-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="b3891-154">Se l'applicazione è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b3891-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="b3891-155">Se un'applicazione viene distribuita in un ambiente web farm, questo valore deve essere stabile, supponendo che gli ambienti di IIS vengono configurati in modo analogo in tutti i computer nella web farm.</span><span class="sxs-lookup"><span data-stu-id="b3891-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="b3891-156">Se l'applicazione non è ospitata in IIS, l'identificatore univoco è il percorso fisico dell'app.</span><span class="sxs-lookup"><span data-stu-id="b3891-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="b3891-157">L'identificatore univoco è progettato per sopravvivere Reimposta &mdash; di singole app e della macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="b3891-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="b3891-158">Questo meccanismo di isolamento si presuppone che le app non siano dannose.</span><span class="sxs-lookup"><span data-stu-id="b3891-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="b3891-159">Un'app dannoso sempre può influire su qualsiasi altra app in esecuzione con lo stesso account di processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="b3891-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="b3891-160">In un ambiente di hosting condiviso in cui le app sono reciprocamente attendibili, il provider di hosting deve eseguire i passaggi per garantire l'isolamento a livello del sistema operativo tra App, tra cui la separazione delle App sottostante repository chiave.</span><span class="sxs-lookup"><span data-stu-id="b3891-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="b3891-161">Se il sistema di protezione dei dati non è specificato da un host ASP.NET Core (ad esempio, se si crea un'istanza di tramite il `DataProtectionProvider` tipo concreto) è disabilitato l'isolamento di app per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="b3891-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="b3891-162">Quando viene disabilitato l'isolamento di app, tutte le applicazioni supportate dal materiale della chiave stesso possono condividere i payload come forniscono appropriata [scopi](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="b3891-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="b3891-163">Per garantire l'isolamento di app in questo ambiente, chiamare il [SetApplicationName](#setapplicationname) metodo sulla configurazione dell'oggetto e specificare un nome univoco per ogni app.</span><span class="sxs-lookup"><span data-stu-id="b3891-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="b3891-164">Algoritmi di modifica con UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="b3891-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="b3891-165">Lo stack di protezione dei dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="b3891-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="b3891-166">Il modo più semplice per eseguire questa operazione consiste nel chiamare [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) dal callback di configurazione:</span><span class="sxs-lookup"><span data-stu-id="b3891-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3891-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3891-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3891-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3891-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="b3891-169">Il valore predefinito EncryptionAlgorithm è AES-256-CBC e il valore predefinito ValidationAlgorithm è HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="b3891-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="b3891-170">Il criterio predefinito può essere impostato dall'amministratore di sistema tramite un [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy), ma una chiamata esplicita a `UseCryptographicAlgorithms` sostituisce il criterio predefinito.</span><span class="sxs-lookup"><span data-stu-id="b3891-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="b3891-171">La chiamata `UseCryptographicAlgorithms` consente di specificare l'algoritmo desiderato da un elenco incorporato predefinito.</span><span class="sxs-lookup"><span data-stu-id="b3891-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="b3891-172">Non è necessario preoccuparsi di implementazione dell'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="b3891-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="b3891-173">Nello scenario precedente, il sistema di protezione dei dati tenta di utilizzare l'implementazione di CNG di AES, se in esecuzione su Windows.</span><span class="sxs-lookup"><span data-stu-id="b3891-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="b3891-174">In caso contrario, viene utilizzata la cartella gestito [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.</span><span class="sxs-lookup"><span data-stu-id="b3891-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="b3891-175">È possibile specificare manualmente un'implementazione tramite una chiamata a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="b3891-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="b3891-176">Gli algoritmi di modifica non influisce sulla chiavi esistenti dell'anello di chiave.</span><span class="sxs-lookup"><span data-stu-id="b3891-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="b3891-177">Riguarda solo le chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="b3891-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="b3891-178">Specifica gli algoritmi gestiti personalizzati</span><span class="sxs-lookup"><span data-stu-id="b3891-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3891-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3891-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3891-180">Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) istanza che fa riferimento ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="b3891-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3891-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3891-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3891-182">Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) istanza che fa riferimento ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="b3891-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="b3891-183">In genere il \*le proprietà del tipo devono puntare a concreto, istanziabili (tramite un costruttore senza parametri pubblico) implementazioni di [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), anche se il speciale di sistema-case alcuni valori come `typeof(Aes)` per motivi di praticità.</span><span class="sxs-lookup"><span data-stu-id="b3891-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="b3891-184">Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b3891-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b3891-185">Il KeyedHashAlgorithm deve avere una dimensione di digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash.</span><span class="sxs-lookup"><span data-stu-id="b3891-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="b3891-186">Il KeyedHashAlgorithm non è strettamente necessaria per essere HMAC.</span><span class="sxs-lookup"><span data-stu-id="b3891-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="b3891-187">Specifica gli algoritmi CNG di Windows personalizzati</span><span class="sxs-lookup"><span data-stu-id="b3891-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3891-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3891-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3891-189">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) istanza che contiene le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="b3891-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3891-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3891-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3891-191">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) istanza che contiene le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="b3891-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="b3891-192">L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di > = 64 bit, e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b3891-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b3891-193">L'algoritmo hash deve avere una dimensione di digest di > = 128 bit e deve supportare viene aperto con il BCRYPT\_ALG\_gestire\_HMAC\_FLAG flag.</span><span class="sxs-lookup"><span data-stu-id="b3891-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="b3891-194">Il \*le proprietà del Provider possono essere impostate su null per utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="b3891-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b3891-195">Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="b3891-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b3891-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b3891-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b3891-197">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia o dei contatori Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) istanza che contiene le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="b3891-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b3891-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b3891-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b3891-199">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia o dei contatori Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) istanza che contiene le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="b3891-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="b3891-200">L'algoritmo di crittografia simmetrica blocco deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di esattamente a 128 bit, e deve supportare la crittografia GCM.</span><span class="sxs-lookup"><span data-stu-id="b3891-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="b3891-201">È possibile impostare il [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) proprietà null per utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="b3891-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b3891-202">Vedere il [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentazione per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="b3891-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="b3891-203">Specifica altri algoritmi personalizzati</span><span class="sxs-lookup"><span data-stu-id="b3891-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="b3891-204">Se non è esposta come un'API di prima classe, il sistema di protezione dei dati è estendibile per specificare qualsiasi tipo di algoritmo.</span><span class="sxs-lookup"><span data-stu-id="b3891-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="b3891-205">Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo di protezione Hardware (HSM) e per fornire un'implementazione personalizzata di base di routine di crittografia e decrittografia.</span><span class="sxs-lookup"><span data-stu-id="b3891-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="b3891-206">Vedere [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [dell'estendibilità della crittografia di base](xref:security/data-protection/extensibility/core-crypto) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="b3891-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="b3891-207">Mantenimento delle chiavi durante l'hosting in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="b3891-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="b3891-208">Durante l'hosting in un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenitore chiavi devono essere gestite in uno:</span><span class="sxs-lookup"><span data-stu-id="b3891-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="b3891-209">Una cartella che è un volume di Docker che viene mantenuto oltre la durata del contenitore, ad esempio un volume condiviso o un volume montato host.</span><span class="sxs-lookup"><span data-stu-id="b3891-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="b3891-210">Un provider esterno, ad esempio [insieme credenziali chiavi Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="b3891-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="b3891-211">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="b3891-211">See also</span></span>

* [<span data-ttu-id="b3891-212">Scenari non compatibili con DI</span><span class="sxs-lookup"><span data-stu-id="b3891-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="b3891-213">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="b3891-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
