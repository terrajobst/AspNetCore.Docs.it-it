---
title: Chiave di crittografia quando sono inattivi in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione della crittografia chiave di protezione dei dati di ASP.NET Core inattivi.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892308"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="e799d-103">Chiave di crittografia quando sono inattivi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e799d-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="e799d-104">Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare le chiavi di crittografia come devono essere crittografati a riposo.</span><span class="sxs-lookup"><span data-stu-id="e799d-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="e799d-105">Lo sviluppatore può ignorare il meccanismo di individuazione e specificare manualmente come chiavi devono essere crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="e799d-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="e799d-106">Se si specifica esplicita [percorso di salvataggio permanente della chiave](xref:security/data-protection/implementation/key-storage-providers), il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest.</span><span class="sxs-lookup"><span data-stu-id="e799d-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="e799d-107">Di conseguenza, le chiavi non vengono crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="e799d-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="e799d-108">È consigliabile che si [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="e799d-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="e799d-109">In questo argomento sono descritte le opzioni per il meccanismo di crittografia dei dati inattivi.</span><span class="sxs-lookup"><span data-stu-id="e799d-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="e799d-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e799d-110">Azure Key Vault</span></span>

<span data-ttu-id="e799d-111">Per archiviare le chiavi nel [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="e799d-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="e799d-112">Per altre informazioni, vedere [configurare protezione dati di ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="e799d-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="e799d-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="e799d-113">Windows DPAPI</span></span>

<span data-ttu-id="e799d-114">**Si applica solo alle distribuzioni di Windows.**</span><span class="sxs-lookup"><span data-stu-id="e799d-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="e799d-115">Quando viene utilizzato DPAPI di Windows, il materiale della chiave viene crittografato con [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) prima di essere resi persistenti in archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e799d-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="e799d-116">DPAPI è un meccanismo di crittografia appropriato per i dati che non viene mai letto esterne al computer corrente (tuttavia è possibile eseguire il backup di queste chiavi fino a Active Directory, vedere [DPAPI e i profili](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="e799d-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="e799d-117">Per configurare la crittografia di chiavi a riposo DPAPI, chiamare uno dei [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="e799d-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="e799d-118">Se `ProtectKeysWithDpapi` viene chiamata senza parametri, solo l'account utente Windows corrente può decrittografare il Keyring persistente.</span><span class="sxs-lookup"><span data-stu-id="e799d-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="e799d-119">È facoltativamente possibile specificare che tutti gli account utente nel computer (non solo l'account utente corrente) in grado di decifrare il gruppo di chiavi:</span><span class="sxs-lookup"><span data-stu-id="e799d-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="e799d-120">Certificato X.509</span><span class="sxs-lookup"><span data-stu-id="e799d-120">X.509 certificate</span></span>

<span data-ttu-id="e799d-121">Se l'app viene distribuito tra più macchine, potrebbe risultare utile distribuire un certificato X.509 condiviso tra il computer e configurare l'App per usare il certificato per la crittografia delle chiavi a riposo ospitate:</span><span class="sxs-lookup"><span data-stu-id="e799d-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="e799d-122">A causa delle limitazioni di .NET Framework, sono supportati solo i certificati con chiavi private CryptoAPI.</span><span class="sxs-lookup"><span data-stu-id="e799d-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="e799d-123">Visualizzare il contenuto di sotto delle possibili soluzioni alternative a queste limitazioni.</span><span class="sxs-lookup"><span data-stu-id="e799d-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="e799d-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="e799d-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="e799d-125">**Questo meccanismo è disponibile solo in Windows 8 e Windows Server 2012 o versione successiva.**</span><span class="sxs-lookup"><span data-stu-id="e799d-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="e799d-126">A partire da Windows 8, il sistema operativo Windows supporta DPAPI-NG (detto anche CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="e799d-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="e799d-127">Per altre informazioni, vedere [su CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="e799d-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="e799d-128">L'entità viene codificato come una regola del descrittore di protezione.</span><span class="sxs-lookup"><span data-stu-id="e799d-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="e799d-129">Nell'esempio seguente che chiama [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), solo l'utente di dominio con il SID specificato è in grado di decrittografare il gruppo di chiavi:</span><span class="sxs-lookup"><span data-stu-id="e799d-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="e799d-130">È inoltre disponibile un overload senza parametri di `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="e799d-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="e799d-131">Usare questo metodo per specificare la regola "SID = {CURRENT_ACCOUNT_SID}", dove *CURRENT_ACCOUNT_SID* è il SID dell'account utente Windows corrente:</span><span class="sxs-lookup"><span data-stu-id="e799d-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="e799d-132">In questo scenario, il controller di dominio Active Directory è responsabile della distribuzione di chiavi di crittografia usate dalle operazioni di DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="e799d-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="e799d-133">L'utente di destinazione può decrittografare i payload crittografati da qualsiasi computer aggiunto al dominio (a condizione che il processo è in esecuzione con la loro identità).</span><span class="sxs-lookup"><span data-stu-id="e799d-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="e799d-134">Crittografia basata su certificati con Windows DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="e799d-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="e799d-135">Se l'app è in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive, è possibile usare Windows DPAPI-NG per eseguire la crittografia basata su certificati.</span><span class="sxs-lookup"><span data-stu-id="e799d-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="e799d-136">Usare la stringa del descrittore regola "certificato = HashId:THUMBPRINT", dove *identificazione personale* è l'identificazione del certificato SHA1 con codifica esadecimale:</span><span class="sxs-lookup"><span data-stu-id="e799d-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="e799d-137">Qualsiasi app a questo repository deve essere in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive alla decrittazione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e799d-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="e799d-138">Chiave di crittografia personalizzato</span><span class="sxs-lookup"><span data-stu-id="e799d-138">Custom key encryption</span></span>

<span data-ttu-id="e799d-139">Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di crittografia della chiave, fornendo un oggetto personalizzato [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="e799d-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
