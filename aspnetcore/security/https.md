---
title: Configurazione di HTTPS per lo sviluppo di ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come impostare HTTPS per lo sviluppo di componenti di base di ASP.NET 2.0.
keywords: ASP.NET Core, SSL, HTTPS
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7913758ffa045dcf884d73eed9bab223b8d2c3fe
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>Configurazione di HTTPS per lo sviluppo di ASP.NET Core

> [!NOTE] 
> Questo argomento si applica alla versione di anteprima 1 di ASP.NET Core 2.0

È possibile configurare l'applicazione per utilizzare HTTPS durante lo sviluppo per simulare HTTPS nell'ambiente di produzione. Abilitazione di HTTPS potrebbe essere necessario abilitare l'integrazione con diversi provider di identità (ad esempio [AD Azure](https://azure.microsoft.com/services/active-directory) e [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)).

<a name="iisxpress"></a>

In Windows, se è stato installato Visual Studio o IIS Express, il certificato lo sviluppo di IIS Express verrà nell'archivio dei certificati LocalMachine. È possibile aggiornare le proprietà del progetto in Visual Studio per usare questo certificato durante l'esecuzione dietro IIS Express.

   * In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**.
   * Nel riquadro sinistro, selezionare **Debug**.
   * Controllare **abilitare SSL**
   * Copiare l'URL SSL e incollarlo il **URL App**

![Scheda Debug di proprietà dell'applicazione web](enforcing-ssl/_static/ssl.png)

Per lo sviluppo è possibile utilizzare il certificato lo sviluppo di IIS Express, se disponibile, o creare un nuovo certificato per scopi di sviluppo. Il certificato di sviluppo deve essere configurato nel `appsettings.Development.json` file in modo che non viene utilizzato nell'ambiente di produzione:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

Un'app con questa configurazione in esecuzione nell'ambiente di produzione genererà un'eccezione che indica "Nessun certificato denominato 'HTTPS', trovato nella configurazione per l'ambiente corrente (produzione)". Per passare il [ambiente](xref:fundamentals/environments) a `Development`, impostare il `ASPNETCORE_ENVIRONMENT` variabile di ambiente `Development`.

Se non si dispone di IIS Express sviluppo installato il certificato, è possibile creare un certificato di sviluppo. In Windows è possibile creare un certificato di sviluppo e aggiungerlo all'archivio radice attendibile per l'utente corrente eseguendo i comandi di PowerShell seguenti in un prompt dei comandi con privilegi elevati:

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel in macOS e Linux

È possibile configurare Kestrel per restare in attesa su HTTPS tramite la configurazione di un endpoint con l'indirizzo IP desiderato, porta e certificati. Il certificato può essere configurato inline, o di primo livello `Certificates` sezione e quindi farvi riferimento in base al nome:

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

In macOS e Linux è possibile creare un certificato autofirmato per l'utilizzo di HTTPS [OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

Una volta il `certificate.pfx` file è stato generato, configurare il certificato HTTPS del `appsettings.Development.json` file:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

È inoltre necessario specificare la passphrase per il certificato impostando la proprietà di configurazione "Certificati: HTTPS:Password". Le password non devono essere archiviate in testo normale. Vedere [provvisoria archiviazione di segreti durante lo sviluppo di App](app-secrets.md) per la corretta gestione della passphrase del certificato.

MacOS puoi [aggiungere il certificato portachiavi](https://support.apple.com/kb/PH20129?locale=en_US) e [modificarne le impostazioni di attendibilità](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) in modo che sia attendibile per HTTPS durante lo sviluppo. Per aggiungere il certificato portachiavi (l'equivalente del `CurrentUser/My` archiviare in Windows) eseguire il comando seguente:

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

E quindi considerare attendibile il certificato:

```bash
security add-trusted-cert localhost.cer
```

È quindi possibile configurare l'app per usare questo certificato in fase di sviluppo simile al seguente:

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
