---
title: Configurare la protezione dei dati ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare la protezione dei dati in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 380293f650c9548c286f98c0447c7ed08b918f2a
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007372"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="9aabd-103">Configurare la protezione dei dati ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9aabd-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="9aabd-104">Quando il sistema di protezione dei dati viene inizializzato, applica [le impostazioni predefinite](xref:security/data-protection/configuration/default-settings) in base all'ambiente operativo.</span><span class="sxs-lookup"><span data-stu-id="9aabd-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="9aabd-105">Queste impostazioni sono generalmente appropriate per le app in esecuzione in un singolo computer.</span><span class="sxs-lookup"><span data-stu-id="9aabd-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="9aabd-106">In alcuni casi uno sviluppatore potrebbe voler modificare le impostazioni predefinite:</span><span class="sxs-lookup"><span data-stu-id="9aabd-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="9aabd-107">L'app viene distribuita tra più computer.</span><span class="sxs-lookup"><span data-stu-id="9aabd-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="9aabd-108">Per motivi di conformità.</span><span class="sxs-lookup"><span data-stu-id="9aabd-108">For compliance reasons.</span></span>

<span data-ttu-id="9aabd-109">Per questi scenari, il sistema di protezione dei dati offre un'API di configurazione avanzata.</span><span class="sxs-lookup"><span data-stu-id="9aabd-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="9aabd-110">Analogamente ai file di configurazione, l'anello della chiave di protezione dei dati deve essere protetto con le autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="9aabd-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="9aabd-111">È possibile scegliere di crittografare le chiavi inattive, ma ciò non impedisce agli utenti malintenzionati di creare nuove chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="9aabd-112">Di conseguenza, si ripercuote sulla sicurezza dell'app.</span><span class="sxs-lookup"><span data-stu-id="9aabd-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="9aabd-113">Il percorso di archiviazione configurato con la protezione dati dovrebbe avere accesso limitato all'app stessa, in modo analogo al modo in cui si proteggono i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="9aabd-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="9aabd-114">Ad esempio, se si sceglie di archiviare l'anello chiave su disco, usare file system autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="9aabd-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="9aabd-115">Verificare solo l'identità con cui viene eseguita l'app Web ha accesso in lettura, scrittura e creazione a tale directory.</span><span class="sxs-lookup"><span data-stu-id="9aabd-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="9aabd-116">Se si usa l'archiviazione BLOB di Azure, solo l'app Web deve essere in grado di leggere, scrivere o creare nuove voci nell'archivio BLOB e così via.</span><span class="sxs-lookup"><span data-stu-id="9aabd-116">If you use Azure Blob Storage, only the web app should have the ability to read, write, or create new entries in the blob store, etc.</span></span>
>
> <span data-ttu-id="9aabd-117">Il metodo di estensione [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) restituisce un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="9aabd-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="9aabd-118">`IDataProtectionBuilder` espone i metodi di estensione che è possibile concatenare per configurare le opzioni di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="9aabd-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="9aabd-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="9aabd-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="9aabd-120">Per archiviare le chiavi in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nella classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9aabd-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="9aabd-121">Impostare la posizione di archiviazione dell'anello di chiave (ad esempio, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="9aabd-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="9aabd-122">Il percorso deve essere impostato perché la chiamata di `ProtectKeysWithAzureKeyVault` implementa un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) che disabilita le impostazioni di protezione dati automatiche, inclusa la posizione di archiviazione dell'anello di chiave.</span><span class="sxs-lookup"><span data-stu-id="9aabd-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="9aabd-123">L'esempio precedente usa l'archivio BLOB di Azure per salvare in modo permanente l'anello di chiave.</span><span class="sxs-lookup"><span data-stu-id="9aabd-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="9aabd-124">Per altre informazioni, vedere [provider di archiviazione chiavi: archiviazione di Azure](xref:security/data-protection/implementation/key-storage-providers#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="9aabd-124">For more information, see [Key storage providers: Azure Storage](xref:security/data-protection/implementation/key-storage-providers#azure-storage).</span></span> <span data-ttu-id="9aabd-125">È anche possibile salvare l'anello chiave localmente con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="9aabd-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="9aabd-126">Il `keyIdentifier` è l'identificatore di chiave dell'insieme di credenziali delle chiavi usato per la crittografia delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-126">The `keyIdentifier` is the key vault key identifier used for key encryption.</span></span> <span data-ttu-id="9aabd-127">Ad esempio, una chiave creata nell'insieme di credenziali delle chiavi denominata `dataprotection` nel `contosokeyvault` ha l'identificatore di chiave `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span><span class="sxs-lookup"><span data-stu-id="9aabd-127">For example, a key created in key vault named `dataprotection` in the `contosokeyvault` has the key identifier `https://contosokeyvault.vault.azure.net/keys/dataprotection/`.</span></span> <span data-ttu-id="9aabd-128">Fornire all'app le autorizzazioni **Unwrap Key** e **Wrap Key** per l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-128">Provide the app with **Unwrap Key** and **Wrap Key** permissions to the key vault.</span></span>

<span data-ttu-id="9aabd-129">Overload `ProtectKeysWithAzureKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="9aabd-129">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="9aabd-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) consente l'uso di un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) per consentire al sistema di protezione dei dati di usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="9aabd-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) consente l'uso di un `ClientId` e di [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) per consentire al sistema di protezione dei dati di usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="9aabd-132">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) consente l'uso di un `ClientId` e `ClientSecret` per consentire al sistema di protezione dei dati di usare l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-132">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="9aabd-133">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="9aabd-133">PersistKeysToFileSystem</span></span>

<span data-ttu-id="9aabd-134">Per archiviare le chiavi in una condivisione UNC anziché nel percorso predefinito *% LocalAppData%* , configurare il sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="9aabd-134">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="9aabd-135">Se si modifica il percorso di persistenza della chiave, il sistema non crittografa più automaticamente le chiavi inattive, perché non è in grado di stabilire se DPAPI è un meccanismo di crittografia appropriato.</span><span class="sxs-lookup"><span data-stu-id="9aabd-135">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="9aabd-136">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="9aabd-136">ProtectKeysWith\*</span></span>

<span data-ttu-id="9aabd-137">È possibile configurare il sistema in modo da proteggere le chiavi inattive chiamando una delle API di configurazione di [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-137">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="9aabd-138">Si consideri l'esempio seguente, in cui le chiavi vengono archiviate in una condivisione UNC e le chiavi inattive vengono crittografate con un certificato X. 509 specifico:</span><span class="sxs-lookup"><span data-stu-id="9aabd-138">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9aabd-139">In ASP.NET Core 2,1 o versioni successive, è possibile fornire un oggetto [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), ad esempio un certificato caricato da un file:</span><span class="sxs-lookup"><span data-stu-id="9aabd-139">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

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

<span data-ttu-id="9aabd-140">Per ulteriori esempi e discussioni sui meccanismi di crittografia chiavi incorporati, vedere la pagina relativa [alla crittografia dei tasti](xref:security/data-protection/implementation/key-encryption-at-rest) inattivi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-140">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="9aabd-141">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="9aabd-141">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="9aabd-142">In ASP.NET Core 2,1 o versioni successive è possibile ruotare i certificati e decrittografare le chiavi inattive usando una matrice di certificati [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) con [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="9aabd-142">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

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

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="9aabd-143">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="9aabd-143">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="9aabd-144">Per configurare il sistema in modo da usare una durata della chiave di 14 giorni anziché i 90 giorni predefiniti, usare [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="9aabd-144">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="9aabd-145">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="9aabd-145">SetApplicationName</span></span>

<span data-ttu-id="9aabd-146">Per impostazione predefinita, il sistema di protezione dei dati isola le app l'una dall'altra in base ai percorsi [radice del contenuto](xref:fundamentals/index#content-root) , anche se condividono lo stesso repository di chiavi fisiche.</span><span class="sxs-lookup"><span data-stu-id="9aabd-146">By default, the Data Protection system isolates apps from one another based on their [content root](xref:fundamentals/index#content-root) paths, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="9aabd-147">In questo modo si impedisce alle app di comprendere i payload protetti degli altri.</span><span class="sxs-lookup"><span data-stu-id="9aabd-147">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="9aabd-148">Per condividere i payload protetti tra le app:</span><span class="sxs-lookup"><span data-stu-id="9aabd-148">To share protected payloads among apps:</span></span>

* <span data-ttu-id="9aabd-149">Configurare <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in ogni app con lo stesso valore.</span><span class="sxs-lookup"><span data-stu-id="9aabd-149">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="9aabd-150">Usare la stessa versione dello stack di Data Protection API tra le app.</span><span class="sxs-lookup"><span data-stu-id="9aabd-150">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="9aabd-151">Eseguire **una** delle operazioni seguenti nei file di progetto delle app:</span><span class="sxs-lookup"><span data-stu-id="9aabd-151">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="9aabd-152">Fare riferimento alla stessa versione del Framework condiviso tramite il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9aabd-152">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="9aabd-153">Fare riferimento alla stessa versione del [pacchetto di protezione dati](xref:security/data-protection/introduction#package-layout) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-153">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="9aabd-154">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="9aabd-154">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="9aabd-155">Si potrebbe avere uno scenario in cui non si vuole che un'app richieda automaticamente le chiavi (creare nuove chiavi) Man mano che si avvicinano alla scadenza.</span><span class="sxs-lookup"><span data-stu-id="9aabd-155">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="9aabd-156">Un esempio potrebbe essere costituito da app configurate in una relazione primaria/secondaria, in cui solo l'app principale è responsabile per i problemi di gestione delle chiavi e le app secondarie dispongono semplicemente di una visualizzazione di sola lettura dell'anello chiave.</span><span class="sxs-lookup"><span data-stu-id="9aabd-156">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="9aabd-157">Le app secondarie possono essere configurate per trattare l'anello chiave come di sola lettura configurando il sistema con <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span><span class="sxs-lookup"><span data-stu-id="9aabd-157">The secondary apps can be configured to treat the key ring as read-only by configuring the system with <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="9aabd-158">Isolamento per applicazione</span><span class="sxs-lookup"><span data-stu-id="9aabd-158">Per-application isolation</span></span>

<span data-ttu-id="9aabd-159">Quando il sistema di protezione dei dati viene fornito da un host ASP.NET Core, isola automaticamente le app l'una dall'altra, anche se tali app sono in esecuzione con lo stesso account del processo di lavoro e usano lo stesso materiale per le chiavi master.</span><span class="sxs-lookup"><span data-stu-id="9aabd-159">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="9aabd-160">Si tratta di un tipo simile al modificatore IsolateApps dell'elemento `<machineKey>` di System. Web.</span><span class="sxs-lookup"><span data-stu-id="9aabd-160">This is somewhat similar to the IsolateApps modifier from System.Web's `<machineKey>` element.</span></span>

<span data-ttu-id="9aabd-161">Il meccanismo di isolamento funziona tenendo conto di ogni app nel computer locale come tenant univoco, in modo che il <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> radice per una determinata app includa automaticamente l'ID app come discriminatore.</span><span class="sxs-lookup"><span data-stu-id="9aabd-161">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="9aabd-162">L'ID univoco dell'app è il percorso fisico dell'app:</span><span class="sxs-lookup"><span data-stu-id="9aabd-162">The app's unique ID is the app's physical path:</span></span>

* <span data-ttu-id="9aabd-163">Per le app ospitate in IIS, l'ID univoco è il percorso fisico IIS dell'app.</span><span class="sxs-lookup"><span data-stu-id="9aabd-163">For apps hosted in IIS, the unique ID is the IIS physical path of the app.</span></span> <span data-ttu-id="9aabd-164">Se un'app viene distribuita in un ambiente di Web farm, questo valore è stabile supponendo che gli ambienti IIS siano configurati in modo analogo in tutti i computer del Web farm.</span><span class="sxs-lookup"><span data-stu-id="9aabd-164">If an app is deployed in a web farm environment, this value is stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>
* <span data-ttu-id="9aabd-165">Per le app indipendenti in esecuzione nel [Server gheppio](xref:fundamentals/servers/index#kestrel), l'ID univoco è il percorso fisico dell'app su disco.</span><span class="sxs-lookup"><span data-stu-id="9aabd-165">For self-hosted apps running on the [Kestrel server](xref:fundamentals/servers/index#kestrel), the unique ID is the physical path to the app on disk.</span></span>

<span data-ttu-id="9aabd-166">L'identificatore univoco è progettato per sopravvivere alle reimpostazioni&mdash;sia della singola app che del computer stesso.</span><span class="sxs-lookup"><span data-stu-id="9aabd-166">The unique identifier is designed to survive resets&mdash;both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="9aabd-167">Questo meccanismo di isolamento presuppone che le app non siano dannose.</span><span class="sxs-lookup"><span data-stu-id="9aabd-167">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="9aabd-168">Un'app dannosa può sempre influenzare qualsiasi altra app in esecuzione con lo stesso account del processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="9aabd-168">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="9aabd-169">In un ambiente host condiviso in cui le app non sono attendibili reciprocamente, il provider di hosting deve eseguire le operazioni necessarie per garantire l'isolamento a livello di sistema operativo tra le app, inclusa la separazione dei repository di chiavi sottostanti delle app.</span><span class="sxs-lookup"><span data-stu-id="9aabd-169">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="9aabd-170">Se il sistema di protezione dei dati non è fornito da un host ASP.NET Core (ad esempio, se si crea un'istanza tramite il tipo concreto `DataProtectionProvider`) l'isolamento delle app è disabilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9aabd-170">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="9aabd-171">Quando l'isolamento delle app è disabilitato, tutte le app supportate dallo stesso materiale di chiave possono condividere i payload purché forniscano gli [scopi](xref:security/data-protection/consumer-apis/purpose-strings)appropriati.</span><span class="sxs-lookup"><span data-stu-id="9aabd-171">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="9aabd-172">Per garantire l'isolamento delle app in questo ambiente, chiamare il metodo [Seapplicationname](#setapplicationname) nell'oggetto Configuration e specificare un nome univoco per ogni app.</span><span class="sxs-lookup"><span data-stu-id="9aabd-172">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="9aabd-173">Modifica degli algoritmi con UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="9aabd-173">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="9aabd-174">Lo stack di protezione dei dati consente di modificare l'algoritmo predefinito usato dalle chiavi appena generate.</span><span class="sxs-lookup"><span data-stu-id="9aabd-174">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="9aabd-175">Il modo più semplice per eseguire questa operazione consiste nel chiamare [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) dal callback di configurazione:</span><span class="sxs-lookup"><span data-stu-id="9aabd-175">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="9aabd-176">Il valore predefinito di EncryptionAlgorithm è AES-256-CBC e il valore predefinito di ValidationAlgorithm è HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="9aabd-176">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="9aabd-177">I criteri predefiniti possono essere impostati da un amministratore di sistema tramite [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy), ma una chiamata esplicita a `UseCryptographicAlgorithms` esegue l'override dei criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="9aabd-177">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="9aabd-178">La chiamata di `UseCryptographicAlgorithms` consente di specificare l'algoritmo desiderato da un elenco predefinito predefinito.</span><span class="sxs-lookup"><span data-stu-id="9aabd-178">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="9aabd-179">Non è necessario preoccuparsi dell'implementazione dell'algoritmo.</span><span class="sxs-lookup"><span data-stu-id="9aabd-179">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="9aabd-180">Nello scenario precedente, il sistema di protezione dei dati tenta di usare l'implementazione CNG di AES se eseguita in Windows.</span><span class="sxs-lookup"><span data-stu-id="9aabd-180">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="9aabd-181">In caso contrario, viene eseguito il fallback alla classe gestita [System. Security. Cryptography. AES](/dotnet/api/system.security.cryptography.aes) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-181">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="9aabd-182">È possibile specificare manualmente un'implementazione tramite una chiamata a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="9aabd-182">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="9aabd-183">La modifica degli algoritmi non influisce sulle chiavi esistenti nell'anello di chiave.</span><span class="sxs-lookup"><span data-stu-id="9aabd-183">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="9aabd-184">Influiscono solo sulle chiavi appena generate.</span><span class="sxs-lookup"><span data-stu-id="9aabd-184">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="9aabd-185">Specifica di algoritmi gestiti personalizzati</span><span class="sxs-lookup"><span data-stu-id="9aabd-185">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9aabd-186">Per specificare algoritmi gestiti personalizzati, creare un'istanza di [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) che punti ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="9aabd-186">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9aabd-187">Per specificare algoritmi gestiti personalizzati, creare un'istanza di [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) che punti ai tipi di implementazione:</span><span class="sxs-lookup"><span data-stu-id="9aabd-187">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

::: moniker-end

<span data-ttu-id="9aabd-188">In genere, le proprietà del tipo di \*devono puntare alle implementazioni concrete, istanziabile (tramite un ctor pubblico senza parametri) di [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) e [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), anche se il sistema specifica alcuni valori come `typeof(Aes)` per praticità.</span><span class="sxs-lookup"><span data-stu-id="9aabd-188">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="9aabd-189">SymmetricAlgorithm deve avere una lunghezza della chiave di ≥ 128 bit e una dimensione del blocco di ≥ 64 bit e deve supportare la crittografia in modalità CBC con la spaziatura interna PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="9aabd-189">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9aabd-190">Il valore di KeyedHashAlgorithm deve avere una dimensione del digest di > = 128 bit e deve supportare chiavi di lunghezza uguale alla lunghezza del digest dell'algoritmo hash.</span><span class="sxs-lookup"><span data-stu-id="9aabd-190">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="9aabd-191">Il KeyedHashAlgorithm non deve necessariamente essere HMAC.</span><span class="sxs-lookup"><span data-stu-id="9aabd-191">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="9aabd-192">Specifica di algoritmi CNG personalizzati di Windows</span><span class="sxs-lookup"><span data-stu-id="9aabd-192">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9aabd-193">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con la convalida HMAC, creare un'istanza di [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) contenente le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="9aabd-193">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9aabd-194">Per specificare un algoritmo CNG di Windows personalizzato utilizzando la crittografia in modalità CBC con la convalida HMAC, creare un'istanza di [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) contenente le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="9aabd-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="9aabd-195">L'algoritmo di crittografia a blocchi simmetrico deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di > = 64 bit e deve supportare la crittografia in modalità CBC con la spaziatura interna PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="9aabd-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9aabd-196">L'algoritmo hash deve avere una dimensione del digest di > = 128 bit e deve supportare l'apertura con il flag BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG.</span><span class="sxs-lookup"><span data-stu-id="9aabd-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="9aabd-197">Le proprietà del provider \*possono essere impostate su null per utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="9aabd-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9aabd-198">Per ulteriori informazioni, vedere la documentazione di [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="9aabd-199">Per specificare un algoritmo CNG di Windows personalizzato con la crittografia della modalità di crittografia/contatore con la convalida, creare un'istanza di [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) contenente le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="9aabd-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="9aabd-200">Per specificare un algoritmo CNG di Windows personalizzato con la crittografia della modalità di crittografia/contatore con la convalida, creare un'istanza di [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) contenente le informazioni algoritmiche:</span><span class="sxs-lookup"><span data-stu-id="9aabd-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="9aabd-201">L'algoritmo di crittografia a blocchi simmetrico deve avere una lunghezza della chiave di > = 128 bit, una dimensione del blocco di esattamente 128 bit e deve supportare la crittografia GCM.</span><span class="sxs-lookup"><span data-stu-id="9aabd-201">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="9aabd-202">È possibile impostare la proprietà [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) su null per utilizzare il provider predefinito per l'algoritmo specificato.</span><span class="sxs-lookup"><span data-stu-id="9aabd-202">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9aabd-203">Per ulteriori informazioni, vedere la documentazione di [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-203">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="9aabd-204">Specifica di altri algoritmi personalizzati</span><span class="sxs-lookup"><span data-stu-id="9aabd-204">Specifying other custom algorithms</span></span>

<span data-ttu-id="9aabd-205">Sebbene non venga esposto come API di prima classe, il sistema di protezione dei dati è sufficientemente estensibile per consentire l'impostazione di quasi tutti i tipi di algoritmo.</span><span class="sxs-lookup"><span data-stu-id="9aabd-205">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="9aabd-206">Ad esempio, è possibile salvare tutte le chiavi contenute in un modulo di protezione hardware (HSM) e fornire un'implementazione personalizzata delle routine di crittografia e decrittografia di base.</span><span class="sxs-lookup"><span data-stu-id="9aabd-206">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="9aabd-207">Per altre informazioni, vedere [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) nell' [estensibilità di crittografia di base](xref:security/data-protection/extensibility/core-crypto) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-207">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="9aabd-208">Salvataggio permanente delle chiavi durante l'hosting in un contenitore Docker</span><span class="sxs-lookup"><span data-stu-id="9aabd-208">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="9aabd-209">Quando si esegue l'hosting in un contenitore [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) , le chiavi devono essere mantenute in uno degli:</span><span class="sxs-lookup"><span data-stu-id="9aabd-209">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="9aabd-210">Una cartella che è un volume Docker che è permanente oltre la durata del contenitore, ad esempio un volume condiviso o un volume montato dall'host.</span><span class="sxs-lookup"><span data-stu-id="9aabd-210">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="9aabd-211">Provider esterno, ad esempio [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="9aabd-211">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="persisting-keys-with-redis"></a><span data-ttu-id="9aabd-212">Salvataggio permanente delle chiavi con Redis</span><span class="sxs-lookup"><span data-stu-id="9aabd-212">Persisting keys with Redis</span></span>

<span data-ttu-id="9aabd-213">Per archiviare le chiavi è necessario usare solo le versioni di redis che supportano la [persistenza dei dati Redis](/azure/azure-cache-for-redis/cache-how-to-premium-persistence) .</span><span class="sxs-lookup"><span data-stu-id="9aabd-213">Only Redis versions supporting [Redis Data Persistence](/azure/azure-cache-for-redis/cache-how-to-premium-persistence) should be used to store keys.</span></span> <span data-ttu-id="9aabd-214">L' [archiviazione BLOB di Azure](/azure/storage/blobs/storage-blobs-introduction) è persistente e può essere usata per archiviare le chiavi.</span><span class="sxs-lookup"><span data-stu-id="9aabd-214">[Azure Blob storage](/azure/storage/blobs/storage-blobs-introduction) is persistent and can be used to store keys.</span></span> <span data-ttu-id="9aabd-215">Per altre informazioni, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/13476).</span><span class="sxs-lookup"><span data-stu-id="9aabd-215">For more information, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/13476).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9aabd-216">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9aabd-216">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
