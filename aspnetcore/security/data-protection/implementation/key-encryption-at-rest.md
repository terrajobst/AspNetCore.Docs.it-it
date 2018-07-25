---
title: Chiave di crittografia quando sono inattivi in ASP.NET Core
author: rick-anderson
description: Scopri i dettagli di implementazione della crittografia chiave di protezione dei dati di ASP.NET Core inattivi.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219290"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="0fe19-103">Chiave di crittografia quando sono inattivi in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0fe19-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="0fe19-104">Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare le chiavi di crittografia come devono essere crittografati a riposo.</span><span class="sxs-lookup"><span data-stu-id="0fe19-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="0fe19-105">Lo sviluppatore può ignorare il meccanismo di individuazione e specificare manualmente come chiavi devono essere crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="0fe19-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="0fe19-106">Se si specifica esplicita [percorso di salvataggio permanente della chiave](xref:security/data-protection/implementation/key-storage-providers), il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest.</span><span class="sxs-lookup"><span data-stu-id="0fe19-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="0fe19-107">Di conseguenza, le chiavi non vengono crittografate a riposo.</span><span class="sxs-lookup"><span data-stu-id="0fe19-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="0fe19-108">È consigliabile che si [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione.</span><span class="sxs-lookup"><span data-stu-id="0fe19-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="0fe19-109">In questo argomento sono descritte le opzioni per il meccanismo di crittografia dei dati inattivi.</span><span class="sxs-lookup"><span data-stu-id="0fe19-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="0fe19-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0fe19-110">Azure Key Vault</span></span>

<span data-ttu-id="0fe19-111">Per archiviare le chiavi nel [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="0fe19-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="0fe19-112">Per altre informazioni, vedere [configurare protezione dati di ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="0fe19-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="0fe19-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="0fe19-113">Windows DPAPI</span></span>

<span data-ttu-id="0fe19-114">**Si applica solo alle distribuzioni di Windows.**</span><span class="sxs-lookup"><span data-stu-id="0fe19-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="0fe19-115">Quando viene utilizzato DPAPI di Windows, il materiale della chiave viene crittografato con [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) prima di essere resi persistenti in archiviazione.</span><span class="sxs-lookup"><span data-stu-id="0fe19-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="0fe19-116">DPAPI è un meccanismo di crittografia appropriato per i dati che non viene mai letto esterne al computer corrente (tuttavia è possibile eseguire il backup di queste chiavi fino a Active Directory, vedere [DPAPI e i profili](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="0fe19-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="0fe19-117">Per configurare la crittografia di chiavi a riposo DPAPI, chiamare uno dei [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) i metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="0fe19-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="0fe19-118">Se `ProtectKeysWithDpapi` viene chiamata senza parametri, solo l'account utente Windows corrente può decrittografare il Keyring persistente.</span><span class="sxs-lookup"><span data-stu-id="0fe19-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="0fe19-119">È facoltativamente possibile specificare che tutti gli account utente nel computer (non solo l'account utente corrente) in grado di decifrare il gruppo di chiavi:</span><span class="sxs-lookup"><span data-stu-id="0fe19-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="0fe19-120">Certificato X.509</span><span class="sxs-lookup"><span data-stu-id="0fe19-120">X.509 certificate</span></span>

<span data-ttu-id="0fe19-121">Se l'app viene distribuito tra più macchine, potrebbe risultare utile distribuire un certificato X.509 condiviso tra il computer e configurare l'App per usare il certificato per la crittografia delle chiavi a riposo ospitate:</span><span class="sxs-lookup"><span data-stu-id="0fe19-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="0fe19-122">A causa delle limitazioni di .NET Framework, sono supportati solo i certificati con chiavi private CryptoAPI.</span><span class="sxs-lookup"><span data-stu-id="0fe19-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="0fe19-123">Visualizzare il contenuto di sotto delle possibili soluzioni alternative a queste limitazioni.</span><span class="sxs-lookup"><span data-stu-id="0fe19-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="0fe19-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="0fe19-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="0fe19-125">**Questo meccanismo è disponibile solo in Windows 8 e Windows Server 2012 o versione successiva.**</span><span class="sxs-lookup"><span data-stu-id="0fe19-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="0fe19-126">A partire da Windows 8, il sistema operativo Windows supporta DPAPI-NG (detto anche CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="0fe19-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="0fe19-127">Per altre informazioni, vedere [su CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="0fe19-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="0fe19-128">L'entità viene codificato come una regola del descrittore di protezione.</span><span class="sxs-lookup"><span data-stu-id="0fe19-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="0fe19-129">Nell'esempio seguente che chiama [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), solo l'utente di dominio con il SID specificato è in grado di decrittografare il gruppo di chiavi:</span><span class="sxs-lookup"><span data-stu-id="0fe19-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="0fe19-130">È inoltre disponibile un overload senza parametri di `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="0fe19-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="0fe19-131">Usare questo metodo per specificare la regola "SID = {CURRENT_ACCOUNT_SID}", dove *CURRENT_ACCOUNT_SID* è il SID dell'account utente Windows corrente:</span><span class="sxs-lookup"><span data-stu-id="0fe19-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="0fe19-132">In questo scenario, il controller di dominio Active Directory è responsabile della distribuzione di chiavi di crittografia usate dalle operazioni di DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="0fe19-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="0fe19-133">L'utente di destinazione può decrittografare i payload crittografati da qualsiasi computer aggiunto al dominio (a condizione che il processo è in esecuzione con la loro identità).</span><span class="sxs-lookup"><span data-stu-id="0fe19-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="0fe19-134">Crittografia basata su certificati con Windows DPAPI-NG.</span><span class="sxs-lookup"><span data-stu-id="0fe19-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="0fe19-135">Se l'app è in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive, è possibile usare Windows DPAPI-NG per eseguire la crittografia basata su certificati.</span><span class="sxs-lookup"><span data-stu-id="0fe19-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="0fe19-136">Usare la stringa del descrittore regola "certificato = HashId:THUMBPRINT", dove *identificazione personale* è l'identificazione del certificato SHA1 con codifica esadecimale:</span><span class="sxs-lookup"><span data-stu-id="0fe19-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="0fe19-137">Qualsiasi app a questo repository deve essere in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive alla decrittazione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="0fe19-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="0fe19-138">Chiave di crittografia personalizzato</span><span class="sxs-lookup"><span data-stu-id="0fe19-138">Custom key encryption</span></span>

<span data-ttu-id="0fe19-139">Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di crittografia della chiave, fornendo un oggetto personalizzato [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="0fe19-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
