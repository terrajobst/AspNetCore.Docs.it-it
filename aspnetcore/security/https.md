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
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="4aa99-104">Configurazione di HTTPS per lo sviluppo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4aa99-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="4aa99-105">Questo argomento si applica alla versione di anteprima 1 di ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="4aa99-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="4aa99-106">È possibile configurare l'applicazione per utilizzare HTTPS durante lo sviluppo per simulare HTTPS nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="4aa99-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="4aa99-107">Abilitazione di HTTPS potrebbe essere necessario abilitare l'integrazione con diversi provider di identità (ad esempio [AD Azure](https://azure.microsoft.com/services/active-directory) e [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span><span class="sxs-lookup"><span data-stu-id="4aa99-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="4aa99-108">In Windows, se è stato installato Visual Studio o IIS Express, il certificato lo sviluppo di IIS Express verrà nell'archivio dei certificati LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="4aa99-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="4aa99-109">È possibile aggiornare le proprietà del progetto in Visual Studio per usare questo certificato durante l'esecuzione dietro IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4aa99-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="4aa99-110">In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="4aa99-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="4aa99-111">Nel riquadro sinistro, selezionare **Debug**.</span><span class="sxs-lookup"><span data-stu-id="4aa99-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="4aa99-112">Controllare **abilitare SSL**</span><span class="sxs-lookup"><span data-stu-id="4aa99-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="4aa99-113">Copiare l'URL SSL e incollarlo il **URL App**</span><span class="sxs-lookup"><span data-stu-id="4aa99-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![Scheda Debug di proprietà dell'applicazione web](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="4aa99-115">Per lo sviluppo è possibile utilizzare il certificato lo sviluppo di IIS Express, se disponibile, o creare un nuovo certificato per scopi di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4aa99-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="4aa99-116">Il certificato di sviluppo deve essere configurato nel `appsettings.Development.json` file in modo che non viene utilizzato nell'ambiente di produzione:</span><span class="sxs-lookup"><span data-stu-id="4aa99-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

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

<span data-ttu-id="4aa99-117">Un'app con questa configurazione in esecuzione nell'ambiente di produzione genererà un'eccezione che indica "Nessun certificato denominato 'HTTPS', trovato nella configurazione per l'ambiente corrente (produzione)".</span><span class="sxs-lookup"><span data-stu-id="4aa99-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="4aa99-118">Per passare il [ambiente](xref:fundamentals/environments) a `Development`, impostare il `ASPNETCORE_ENVIRONMENT` variabile di ambiente `Development`.</span><span class="sxs-lookup"><span data-stu-id="4aa99-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="4aa99-119">Se non si dispone di IIS Express sviluppo installato il certificato, è possibile creare un certificato di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4aa99-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="4aa99-120">In Windows è possibile creare un certificato di sviluppo e aggiungerlo all'archivio radice attendibile per l'utente corrente eseguendo i comandi di PowerShell seguenti in un prompt dei comandi con privilegi elevati:</span><span class="sxs-lookup"><span data-stu-id="4aa99-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="4aa99-121">Kestrel in macOS e Linux</span><span class="sxs-lookup"><span data-stu-id="4aa99-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="4aa99-122">È possibile configurare Kestrel per restare in attesa su HTTPS tramite la configurazione di un endpoint con l'indirizzo IP desiderato, porta e certificati.</span><span class="sxs-lookup"><span data-stu-id="4aa99-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="4aa99-123">Il certificato può essere configurato inline, o di primo livello `Certificates` sezione e quindi farvi riferimento in base al nome:</span><span class="sxs-lookup"><span data-stu-id="4aa99-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

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

<span data-ttu-id="4aa99-124">In macOS e Linux è possibile creare un certificato autofirmato per l'utilizzo di HTTPS [OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="4aa99-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="4aa99-125">Una volta il `certificate.pfx` file è stato generato, configurare il certificato HTTPS del `appsettings.Development.json` file:</span><span class="sxs-lookup"><span data-stu-id="4aa99-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

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

<span data-ttu-id="4aa99-126">È inoltre necessario specificare la passphrase per il certificato impostando la proprietà di configurazione "Certificati: HTTPS:Password".</span><span class="sxs-lookup"><span data-stu-id="4aa99-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="4aa99-127">Le password non devono essere archiviate in testo normale.</span><span class="sxs-lookup"><span data-stu-id="4aa99-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="4aa99-128">Vedere [provvisoria archiviazione di segreti durante lo sviluppo di App](app-secrets.md) per la corretta gestione della passphrase del certificato.</span><span class="sxs-lookup"><span data-stu-id="4aa99-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="4aa99-129">MacOS puoi [aggiungere il certificato portachiavi](https://support.apple.com/kb/PH20129?locale=en_US) e [modificarne le impostazioni di attendibilità](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) in modo che sia attendibile per HTTPS durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4aa99-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="4aa99-130">Per aggiungere il certificato portachiavi (l'equivalente del `CurrentUser/My` archiviare in Windows) eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4aa99-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="4aa99-131">E quindi considerare attendibile il certificato:</span><span class="sxs-lookup"><span data-stu-id="4aa99-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="4aa99-132">È quindi possibile configurare l'app per usare questo certificato in fase di sviluppo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4aa99-132">You can then configure your app to use this certificate in development like this:</span></span>

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
