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
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Chiave di crittografia quando sono inattivi in ASP.NET Core

Il sistema di protezione dati [Usa un meccanismo di individuazione per impostazione predefinita](xref:security/data-protection/configuration/default-settings) per determinare le chiavi di crittografia come devono essere crittografati a riposo. Lo sviluppatore può ignorare il meccanismo di individuazione e specificare manualmente come chiavi devono essere crittografate a riposo.

> [!WARNING]
> Se si specifica esplicita [percorso di salvataggio permanente della chiave](xref:security/data-protection/implementation/key-storage-providers), il sistema di protezione dati Annulla la registrazione di crittografia della chiave predefinita al meccanismo di rest. Di conseguenza, le chiavi non vengono crittografate a riposo. È consigliabile che si [specifica un meccanismo di crittografia della chiave esplicite](xref:security/data-protection/implementation/key-encryption-at-rest) per distribuzioni di produzione. In questo argomento sono descritte le opzioni per il meccanismo di crittografia dei dati inattivi.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

Per archiviare le chiavi nel [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurare il sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) nel `Startup` classe:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Per altre informazioni, vedere [configurare protezione dati di ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**Si applica solo alle distribuzioni di Windows.**

Quando viene utilizzato DPAPI di Windows, il materiale della chiave viene crittografato con [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) prima di essere resi persistenti in archiviazione. DPAPI è un meccanismo di crittografia appropriato per i dati che non viene mai letto esterne al computer corrente (tuttavia è possibile eseguire il backup di queste chiavi fino a Active Directory, vedere [DPAPI e i profili](https://support.microsoft.com/kb/309408/#6)). Per configurare la crittografia di chiavi a riposo DPAPI, chiamare uno dei [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) i metodi di estensione:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Se `ProtectKeysWithDpapi` viene chiamata senza parametri, solo l'account utente Windows corrente può decrittografare il Keyring persistente. È facoltativamente possibile specificare che tutti gli account utente nel computer (non solo l'account utente corrente) in grado di decifrare il gruppo di chiavi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>Certificato X.509

Se l'app viene distribuito tra più macchine, potrebbe risultare utile distribuire un certificato X.509 condiviso tra il computer e configurare l'App per usare il certificato per la crittografia delle chiavi a riposo ospitate:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

A causa delle limitazioni di .NET Framework, sono supportati solo i certificati con chiavi private CryptoAPI. Visualizzare il contenuto di sotto delle possibili soluzioni alternative a queste limitazioni.

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**Questo meccanismo è disponibile solo in Windows 8 e Windows Server 2012 o versione successiva.**

A partire da Windows 8, il sistema operativo Windows supporta DPAPI-NG (detto anche CNG DPAPI). Per altre informazioni, vedere [su CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).

L'entità viene codificato come una regola del descrittore di protezione. Nell'esempio seguente che chiama [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), solo l'utente di dominio con il SID specificato è in grado di decrittografare il gruppo di chiavi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

È inoltre disponibile un overload senza parametri di `ProtectKeysWithDpapiNG`. Usare questo metodo per specificare la regola "SID = {CURRENT_ACCOUNT_SID}", dove *CURRENT_ACCOUNT_SID* è il SID dell'account utente Windows corrente:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

In questo scenario, il controller di dominio Active Directory è responsabile della distribuzione di chiavi di crittografia usate dalle operazioni di DPAPI-NG. L'utente di destinazione può decrittografare i payload crittografati da qualsiasi computer aggiunto al dominio (a condizione che il processo è in esecuzione con la loro identità).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Crittografia basata su certificati con Windows DPAPI-NG.

Se l'app è in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive, è possibile usare Windows DPAPI-NG per eseguire la crittografia basata su certificati. Usare la stringa del descrittore regola "certificato = HashId:THUMBPRINT", dove *identificazione personale* è l'identificazione del certificato SHA1 con codifica esadecimale:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Qualsiasi app a questo repository deve essere in esecuzione in Windows 8.1 e Windows Server 2012 R2 o versioni successive alla decrittazione delle chiavi.

## <a name="custom-key-encryption"></a>Chiave di crittografia personalizzato

Se i meccanismi in arrivo non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di crittografia della chiave, fornendo un oggetto personalizzato [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
