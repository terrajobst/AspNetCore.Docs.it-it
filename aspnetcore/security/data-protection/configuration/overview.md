---
title: Configurare la protezione dei dati di ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare la protezione dei dati in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0377fe9fbe5a1eeddb384443370751baa3c0ee43
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482996"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="5efde-103">Configurare la protezione dei dati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5efde-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="5efde-104">Quando il sistema di protezione dei dati viene inizializzato, viene applicato [le impostazioni predefinite](xref:security/data-protection/configuration/default-settings) in base all'ambiente operativo.</span><span class="sxs-lookup"><span data-stu-id="5efde-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="5efde-105">Queste impostazioni sono in genere adatte per le App in esecuzione in un singolo computer.</span><span class="sxs-lookup"><span data-stu-id="5efde-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="5efde-106">Vi sono casi in cui uno sviluppatore potrebbe voler modificare le impostazioni predefinite:</span><span class="sxs-lookup"><span data-stu-id="5efde-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="5efde-107">L'app viene distribuita tra più computer.</span><span class="sxs-lookup"><span data-stu-id="5efde-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="5efde-108">Per motivi di conformità.</span><span class="sxs-lookup"><span data-stu-id="5efde-108">For compliance reasons.</span></span>

<span data-ttu-id="5efde-109">Per questi scenari, il sistema di protezione dei dati offre un'API di configurazione avanzate.</span><span class="sxs-lookup"><span data-stu-id="5efde-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="5efde-110">Analogamente ai file di configurazione, il gruppo di chiavi di protezione dati devono essere protetti usando le autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="5efde-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="5efde-111">È possibile scegliere di crittografare le chiavi a riposo, ma ciò non impedisce agli utenti malintenzionati di creazione di nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="5efde-112">Di conseguenza, la sicurezza dell'app è interessata.</span><span class="sxs-lookup"><span data-stu-id="5efde-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="5efde-113">Il percorso di archiviazione configurato con la protezione dei dati deve avere l'accesso limitato all'app stessa, simile al modo in cui che si proteggono i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5efde-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="5efde-114">Ad esempio, se si sceglie di archiviare il gruppo di chiavi su disco, usare le autorizzazioni del file.</span><span class="sxs-lookup"><span data-stu-id="5efde-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="5efde-115">Assicurarsi che solo l'identità con cui eseguire l'app web ha leggere, scrivere e creare l'accesso a tale directory.</span><span class="sxs-lookup"><span data-stu-id="5efde-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="5efde-116">Se si Usa archiviazione tabelle di Azure, solo l'app web deve avere la possibilità di leggere, scrivere o creare nuove voci nell'archivio tabelle di e così via.</span><span class="sxs-lookup"><span data-stu-id="5efde-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="5efde-117">Il metodo di estensione [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) restituisce un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="5efde-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="5efde-118">`IDataProtectionBuilder` espone i metodi di estensione che è possibile concatenare per configurare la protezione dati opzioni.</span><span class="sxs-lookup"><span data-stu-id="5efde-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="5efde-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="5efde-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="5efde-120">Per archiviare le chiavi nel [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="5efde-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="5efde-121">Impostare il percorso di archiviazione chiavi (ad esempio, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="5efde-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="5efde-122">Il percorso deve essere impostato perché la chiamata `ProtectKeysWithAzureKeyVault` implementa un' [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) che disattiva le impostazioni di protezione automatica dei dati, tra cui la posizione di archiviazione del gruppo di chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="5efde-123">L'esempio precedente Usa l'archiviazione Blob di Azure per rendere persistente il gruppo di chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="5efde-124">Per altre informazioni, vedere [provider di archiviazione chiavi: Azure e Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="5efde-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="5efde-125">È anche possibile salvare il gruppo di chiavi in locale con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="5efde-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="5efde-126">Il `keyIdentifier` è l'identificatore di chiave di insieme di credenziali delle chiavi usato per la chiave di crittografia (ad esempio, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="5efde-126">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="5efde-127">`ProtectKeysWithAzureKeyVault` Overload:</span><span class="sxs-lookup"><span data-stu-id="5efde-127">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="5efde-128">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) consente l'uso di un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-128">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="5efde-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) consente l'uso di un `ClientId` e [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="5efde-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) consente l'uso di un `ClientId` e `ClientSecret` per abilitare il sistema di protezione dati usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="5efde-131">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="5efde-131">PersistKeysToFileSystem</span></span>

<span data-ttu-id="5efde-132">Per archiviare le chiavi in una condivisione UNC invece che nel *% LOCALAPPDATA %* percorso predefinito, configurare il sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="5efde-132">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="5efde-133">Se si modifica il percorso di persistenza chiave, il sistema di Crittografa non sono più automaticamente le chiavi a riposo, perché non conoscere se DPAPI è un meccanismo di crittografia appropriato.</span><span class="sxs-lookup"><span data-stu-id="5efde-133">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="5efde-134">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="5efde-134">ProtectKeysWith\*</span></span>

<span data-ttu-id="5efde-135">È possibile configurare il sistema per proteggere le chiavi a riposo mediante la chiamata a uno qualsiasi dei [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5efde-135">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="5efde-136">Si consideri l'esempio seguente, che archivia le chiavi in una condivisione UNC e consente di crittografare tali chiavi a riposo con un certificato X.509 specifico:</span><span class="sxs-lookup"><span data-stu-id="5efde-136">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5efde-137">In ASP.NET Core 2.1 o versioni successive, è possibile fornire un' [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) al [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), ad esempio un certificato caricato da un file:</span><span class="sxs-lookup"><span data-stu-id="5efde-137">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="5efde-138">Visualizzare [chiave di crittografia dei dati](xref:security/data-protection/implementation/key-encryption-at-rest) per altri esempi e una discussione sui meccanismi di crittografia della chiave incorporato.</span><span class="sxs-lookup"><span data-stu-id="5efde-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="5efde-139">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="5efde-139">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="5efde-140">In ASP.NET Core 2.1 o versioni successive, è possibile ruotare i certificati e la decrittografia di chiavi a riposo usando la matrice di [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificati con [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="5efde-140">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="5efde-141">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="5efde-141">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="5efde-142">Per configurare il sistema per l'uso di durata di una chiave di 14 giorni anziché il valore predefinito di 90 giorni, usare [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="5efde-142">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="5efde-143">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="5efde-143">SetApplicationName</span></span>

<span data-ttu-id="5efde-144">Per impostazione predefinita, il sistema di protezione dati consente di isolare le app da uno a altro, anche se si condivide lo stesso repository chiave fisico.</span><span class="sxs-lookup"><span data-stu-id="5efde-144">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="5efde-145">Ciò impedisce che le app la comprensione di payload protetti di altro.</span><span class="sxs-lookup"><span data-stu-id="5efde-145">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="5efde-146">Per condividere payload protetti tra due App, usare [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con lo stesso valore per ogni app:</span><span class="sxs-lookup"><span data-stu-id="5efde-146">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="5efde-147">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="5efde-147">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="5efde-148">È possibile uno scenario in cui non si desidera distribuire automaticamente le chiavi (creazione di nuove chiavi) come approccio alla scadenza di un'app.</span><span class="sxs-lookup"><span data-stu-id="5efde-148">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="5efde-149">Un esempio di questo può essere impostate in una relazione primario/secondario, solo l'app principale è responsabile di competenze di gestione delle chiavi e App secondaria hanno semplicemente una vista di sola lettura del gruppo di chiavi in cui le app.</span><span class="sxs-lookup"><span data-stu-id="5efde-149">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="5efde-150">Le app secondarie possono essere configurate per considerare il gruppo di chiavi di sola lettura tramite la configurazione di sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="5efde-150">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="5efde-151">Isolamento per ogni applicazione</span><span class="sxs-lookup"><span data-stu-id="5efde-151">Per-application isolation</span></span>

<span data-ttu-id="5efde-152">Quando il sistema di protezione dei dati viene fornito da un host ASP.NET Core, automaticamente consente di isolare le app da uno a altro, anche se queste App sono in esecuzione con lo stesso account di processo di lavoro e utilizzano il materiale della chiave master stesso.</span><span class="sxs-lookup"><span data-stu-id="5efde-152">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="5efde-153">Questo è un po' come il modificatore IsolateApps da System. Web  **\<machineKey >** elemento.</span><span class="sxs-lookup"><span data-stu-id="5efde-153">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="5efde-154">Il meccanismo di isolamento funziona da prendere in considerazione ogni app nel computer locale come tenant univoco, pertanto il [oggetto IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted per un'app include automaticamente l'ID dell'app come discriminatore.</span><span class="sxs-lookup"><span data-stu-id="5efde-154">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="5efde-155">ID univoco dell'app deriva da una delle due posizioni:</span><span class="sxs-lookup"><span data-stu-id="5efde-155">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="5efde-156">Se l'app è ospitata in IIS, l'identificatore univoco è il percorso di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="5efde-156">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="5efde-157">Se un'app viene distribuita in un ambiente web farm, questo valore deve essere stabile, presupponendo che gli ambienti di IIS vengono configurati in modo analogo tra tutte le macchine nella web farm.</span><span class="sxs-lookup"><span data-stu-id="5efde-157">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="5efde-158">Se l'app non è ospitato in IIS, l'identificatore univoco è il percorso fisico dell'app.</span><span class="sxs-lookup"><span data-stu-id="5efde-158">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="5efde-159">L'identificatore univoco è progettato per sopravvivere alle reimpostazioni &mdash; dell'app per le singole e della macchina stessa.</span><span class="sxs-lookup"><span data-stu-id="5efde-159">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="5efde-160">Questo meccanismo di isolamento si presuppone che l'App non siano dannose.</span><span class="sxs-lookup"><span data-stu-id="5efde-160">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="5efde-161">Un'applicazione dannosa sempre può influire su qualsiasi altra app in esecuzione con lo stesso account di processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="5efde-161">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="5efde-162">In un ambiente di hosting condiviso in cui le app sono reciprocamente attendibili, provider di hosting deve provvedere a garantire isolamento a livello di sistema operativo tra le app, tra cui la separazione delle App sottostante repository della chiave.</span><span class="sxs-lookup"><span data-stu-id="5efde-162">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="5efde-163">Se il sistema di protezione dei dati non viene fornito da un host ASP.NET Core (ad esempio, se si crea un'istanza di quest'ultima con il `DataProtectionProvider` tipo concreto) isolamento delle app è disabilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5efde-163">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="5efde-164">Quando l'isolamento delle app è disabilitata, tutte le app supportate dal materiale della chiave stesso possono condividere i payload, purché forniscono appropriato [scopi](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="5efde-164">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="5efde-165">Per fornire isolamento delle app in questo ambiente, chiamare il [SetApplicationName](#setapplicationname) metodo sulla configurazione dell'oggetto e specificare un nome univoco per ogni app.</span><span class="sxs-lookup"><span data-stu-id="5efde-165">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="5efde-166">Modifica gli algoritmi con UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="5efde-166">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="5efde-167">Lo stack di protezione dei dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="5efde-167">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="5efde-168">Il modo più semplice per eseguire questa operazione consiste nel chiamare [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) dal callback di configurazione:</span><span class="sxs-lookup"><span data-stu-id="5efde-168">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5efde-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5efde-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5efde-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5efde-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="5efde-171">Il valore predefinito EncryptionAlgorithm è AES-256-CBC e il valore predefinito ValidationAlgorithm è HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="5efde-171">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="5efde-172">Il criterio predefinito può essere impostato dall'amministratore di sistema tramite un [dei criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy), ma una chiamata esplicita a `UseCryptographicAlgorithms` sovrascrivono i criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="5efde-172">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="5efde-173">La chiamata `UseCryptographicAlgorithms` consente di specificare l'algoritmo desiderato da un elenco incorporato predefinito.</span><span class="sxs-lookup"><span data-stu-id="5efde-173">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="5efde-174">Non è necessario preoccuparsi di implementazione dell'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="5efde-174">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="5efde-175">Nello scenario precedente, il sistema di protezione dei dati tenta di usare l'implementazione di CNG di AES se in esecuzione su Windows.</span><span class="sxs-lookup"><span data-stu-id="5efde-175">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="5efde-176">In caso contrario, esegue il fallback a managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) classe.</span><span class="sxs-lookup"><span data-stu-id="5efde-176">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="5efde-177">È possibile specificare manualmente un'implementazione tramite una chiamata verso [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="5efde-177">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="5efde-178">Gli algoritmi di modifica non influenza le chiavi esistenti nel gruppo di chiavi.</span><span class="sxs-lookup"><span data-stu-id="5efde-178">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="5efde-179">Riguarda solo le chiavi appena generato.</span><span class="sxs-lookup"><span data-stu-id="5efde-179">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="5efde-180">Specifica di algoritmi personalizzati gestiti</span><span class="sxs-lookup"><span data-stu-id="5efde-180">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5efde-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5efde-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5efde-182">Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) istanza che fa riferimento ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="5efde-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5efde-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5efde-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5efde-184">Per specificare gli algoritmi gestiti personalizzati, creare un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) istanza che fa riferimento ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="5efde-184">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="5efde-185">In genere il \*proprietà del tipo devono puntare a concreto, implementazioni istanziabile (tramite un costruttore senza parametri pubblico) di [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), anche se la casi di speciali del sistema, ad esempio alcuni valori `typeof(Aes)` per motivi di praticità.</span><span class="sxs-lookup"><span data-stu-id="5efde-185">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="5efde-186">Il SymmetricAlgorithm deve avere una lunghezza della chiave di ≤ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="5efde-186">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="5efde-187">Il KeyedHashAlgorithm deve avere una dimensione del digest di > = 128 bit, e deve supportare le chiavi di lunghezza uguale alla lunghezza di digest dell'algoritmo hash.</span><span class="sxs-lookup"><span data-stu-id="5efde-187">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="5efde-188">Il KeyedHashAlgorithm non è strettamente necessaria essere HMAC.</span><span class="sxs-lookup"><span data-stu-id="5efde-188">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="5efde-189">Specifica gli algoritmi CNG di Windows personalizzati</span><span class="sxs-lookup"><span data-stu-id="5efde-189">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5efde-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5efde-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5efde-191">Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) istanza che contiene le informazioni su algoritmi:</span><span class="sxs-lookup"><span data-stu-id="5efde-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5efde-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5efde-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5efde-193">Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia in modalità CBC con convalida HMAC, creare un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) istanza che contiene le informazioni su algoritmi:</span><span class="sxs-lookup"><span data-stu-id="5efde-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="5efde-194">L'algoritmo di crittografia a blocchi simmetriche debba avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di > = 64 bit, e deve supportare la crittografia in modalità CBC con riempimento PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="5efde-194">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="5efde-195">L'algoritmo di hash deve avere una dimensione del digest di > = 128 bit e devono supportare viene aperto con la BCRYPT\_ALG\_gestire\_HMAC\_FLAG flag.</span><span class="sxs-lookup"><span data-stu-id="5efde-195">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="5efde-196">Il \*proprietà Provider possono essere impostate su null per usare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="5efde-196">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="5efde-197">Vedere le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="5efde-197">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5efde-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5efde-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5efde-199">Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia/contatore Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) istanza che contiene le informazioni su algoritmi:</span><span class="sxs-lookup"><span data-stu-id="5efde-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5efde-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5efde-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5efde-201">Per specificare un algoritmo CNG di Windows personalizzato usando la crittografia/contatore Galois modalità con la convalida, creare un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) istanza che contiene le informazioni su algoritmi:</span><span class="sxs-lookup"><span data-stu-id="5efde-201">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="5efde-202">L'algoritmo di crittografia a blocchi simmetriche debba avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di esattamente a 128 bit, e deve supportare la crittografia di GCM.</span><span class="sxs-lookup"><span data-stu-id="5efde-202">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="5efde-203">È possibile impostare il [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) proprietà su null per usare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="5efde-203">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="5efde-204">Vedere le [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="5efde-204">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="5efde-205">Specifica altri algoritmi personalizzati</span><span class="sxs-lookup"><span data-stu-id="5efde-205">Specifying other custom algorithms</span></span>

<span data-ttu-id="5efde-206">Anche se non è esposta come un'API di alto livello, il sistema di protezione dei dati è sufficientemente estensibile per consentire la specifica di qualsiasi tipo di algoritmo.</span><span class="sxs-lookup"><span data-stu-id="5efde-206">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="5efde-207">Ad esempio, è possibile mantenere tutte le chiavi contenute all'interno di un modulo di protezione Hardware (HSM) e per fornire un'implementazione personalizzata di una core routine di crittografia e decrittografia.</span><span class="sxs-lookup"><span data-stu-id="5efde-207">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="5efde-208">Visualizzare [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) nelle [estendibilità della crittografia di base](xref:security/data-protection/extensibility/core-crypto) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="5efde-208">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="5efde-209">Mantenimento delle chiavi per l'hosting in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="5efde-209">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="5efde-210">Durante l'hosting di un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenitore, le chiavi devono essere mantenute in uno:</span><span class="sxs-lookup"><span data-stu-id="5efde-210">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="5efde-211">Una cartella che è un volume di Docker che persiste oltre la durata del contenitore, ad esempio un volume condiviso o un volume montato host.</span><span class="sxs-lookup"><span data-stu-id="5efde-211">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="5efde-212">Un provider esterno, ad esempio [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oppure [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="5efde-212">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="5efde-213">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="5efde-213">See also</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
