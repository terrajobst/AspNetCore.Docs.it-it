---
title: Crittografia chiave inattivi in ASP.NET Core
author: rick-anderson
description: Ulteriori dettagli sull'implementazione della crittografia a chiave di protezione dei dati di ASP.NET Core inattivi.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 9247b141a44c958f34529e5a42a0ddc8c8893cb0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Crittografia chiave inattivi in ASP.NET Core

<a name="data-protection-implementation-key-encryption-at-rest"></a>

Per impostazione predefinita, il sistema di protezione dati [utilizza un approccio euristico](xref:security/data-protection/configuration/default-settings) per determinare la modalità crittografia materiale della chiave devono essere crittografati a riposo. Lo sviluppatore può eseguire l'override l'euristica e specificare manualmente la modalità di crittografia chiavi inattivi.

> [!NOTE]
> Se si specifica una crittografia a chiave esplicita al meccanismo di rest, il sistema di protezione dati verrà annullare la registrazione il meccanismo di archiviazione chiavi predefinito che ha fornito l'euristica indicata. È necessario [specifica un meccanismo di archiviazione chiavi esplicita](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), in caso contrario il sistema di protezione dati non verrà avviati.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

Il sistema di protezione dati viene fornito con tre meccanismi di crittografia della chiave nella casella.

## <a name="windows-dpapi"></a>Windows DPAPI

*Questo meccanismo è disponibile solo in Windows.*

Quando viene utilizzato DPAPI di Windows, il materiale della chiave viene crittografato tramite [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) prima di essere resa persistente nell'archiviazione. DPAPI è un meccanismo di crittografia appropriati per i dati non verranno letti mai di fuori di computer in uso (tuttavia è possibile eseguire il backup di queste chiavi fino a Active Directory, vedere [DPAPI e i profili](https://support.microsoft.com/kb/309408/#6)). Ad esempio per configurare la crittografia della chiave a riposo DPAPI.

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Se `ProtectKeysWithDpapi` viene chiamato senza parametri, solo l'account utente Windows corrente può decrittografare il materiale della chiave permanente. È facoltativamente possibile specificare che tutti gli account utente del computer (non solo l'account utente corrente) deve essere in grado di decifrare il materiale della chiave, come illustrato nell'esempio seguente.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>Certificato x. 509

*Questo meccanismo non è disponibile nel `.NET Core 1.0` o `1.1`.*

Se l'applicazione è suddiviso in più computer, potrebbe essere opportuno distribuire un certificato x. 509 condiviso tra il computer e configurare le applicazioni di utilizzare il certificato per la crittografia delle chiavi inattivi. Per un esempio, vedere di seguito.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

A causa di limitazioni di .NET Framework sono supportati solo i certificati con chiavi private CryptoAPI. Vedere [crittografia basata su certificati con Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) di sotto delle possibili soluzioni alternative per queste limitazioni.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Questo meccanismo è disponibile solo in Windows 8 o Windows Server 2012 e versioni successive.*

A partire da Windows 8, il sistema operativo supporta DPAPI-NG (detto anche DPAPI CNG). Microsoft disposto lo scenario di utilizzo come indicato di seguito.

   Il cloud computing, tuttavia, richiede spesso che essere decrittografato in un altro contenuto crittografato in un solo computer. Pertanto, a partire da Windows 8, esteso il concetto di utilizza un'API relativamente semplice da contenere scenari basati su cloud di Microsoft. Questa nuova API, chiamata DPAPI-NG, consente di condividere in modo sicuro i segreti (chiavi, le password, il materiale della chiave) e i messaggi da di protezione a un set di entità che può essere usato per rimuovere la protezione di essi in computer diversi, dopo la corretta autenticazione e autorizzazione.

   Da [su DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

L'entità viene codificato come una regola del descrittore di protezione. Si consideri l'esempio seguente che crittografa il materiale della chiave in modo che solo l'utente aggiunto al dominio con il SID specificato è in grado di decrittografare il materiale della chiave.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

È inoltre disponibile un overload senza parametri di `ProtectKeysWithDpapiNG`. Si tratta di un metodo pratico per specificare la regola "SID = miei", dove il mio è il SID dell'account utente Windows corrente.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

In questo scenario, il controller di dominio Active Directory è responsabile per la distribuzione di chiavi di crittografia utilizzate dalle operazioni di DPAPI NG. L'utente di destinazione sarà in grado di decifrare il payload crittografato da qualsiasi computer appartenenti a un dominio (a condizione che il processo viene eseguito con la propria identità).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Crittografia basata su certificati con Windows DPAPI-NG.

Se si eseguono in Windows 8.1 o Windows Server 2012 R2 o versioni successive, è possibile utilizzare Windows DPAPI-NG per eseguire la crittografia basata sui certificati, anche se l'applicazione viene eseguita su .NET Core. Per sfruttare i vantaggi di questo, utilizzare la stringa di descrizione regola "certificato = HashId:thumbprint", dove identificazione personale è l'identificazione digitale SHA1 con codifica esadecimale del certificato da utilizzare. Per un esempio, vedere di seguito.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Qualsiasi applicazione che fa riferimento a questo repository deve essere in esecuzione in Windows 8.1 / Windows Server 2012 R2 o versioni successive in grado di decifrare la chiave.

## <a name="custom-key-encryption"></a>Chiave di crittografia personalizzato

Se i meccanismi di casella in non sono appropriati, lo sviluppatore può specificare il proprio meccanismo di crittografia della chiave, fornendo un oggetto personalizzato `IXmlEncryptor`.
