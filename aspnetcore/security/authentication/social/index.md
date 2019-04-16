---
title: Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 61482481358256dc9ddd1a0a894541040a8a452f
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516326"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come compilare un'app ASP.NET Core 2.2 che consente agli utenti di eseguire l'accesso tramite OAuth 2.0 con credenziali di provider di autenticazione esterni.

I provider di [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), e [Microsoft](xref:security/authentication/microsoft-logins) vengono trattati nelle sezioni seguenti. Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

Il fatto di consentire agli utenti l'accesso con le proprie credenziali esistenti:
* È pratico per gli utenti.
* Trasferisce a terzi molti aspetti complicati del processo di accesso. 

Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core.
* Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.
* Selezionare **Modifica autenticazione** e impostare l'autenticazione su **Account utente individuali**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.

* Eseguire i comandi seguenti:

  ```console
  dotnet new webapp -o WebApp1
  code -r WebApp1
  ```

  * Il comando `dotnet new` crea un nuovo progetto Razor Pages nella cartella *WebApp1*.
  * Il comando `code` apre la cartella *WebApp1* in una nuova istanza di Visual Studio Code.

  Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'WebApp1'. Add them?** (Risorse di compilazione e debug mancanti da "WebApp1". Aggiungerle?)

* Selezionare **Sì**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Da un terminale eseguire il comando seguente:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1
```

I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un progetto Razor Pages.

## <a name="open-the-project"></a>Aprire il progetto

Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *WebApp1.csproj*.

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a>Applicare le migrazioni

* Eseguire l'app e selezionare il collegamento **Registra**.
* Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.
* Seguire le istruzioni per applicare le migrazioni.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Usare SecretManager per archiviare i token assegnati dai provider di accesso

I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione. La denominazione esatta dei token varia a seconda del provider. Questi token rappresentano le credenziali usate dall'app per accedere alle API. I token costituiscono i "segreti" che è possibile collegare alla configurazione dell'app usando [Secret Manager](xref:security/app-secrets#secret-manager). Secret Manager è un'alternativa che offre maggior sicurezza rispetto alla memorizzazione dei token in un file di configurazione, ad esempio *appsettings.json*.

> [!IMPORTANT]
> Secret Manager è progettato esclusivamente per lo sviluppo. È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).

Seguire la procedura descritta nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) per archiviare i token assegnati dai singoli provider di accesso elencati di seguito.

## <a name="setup-login-providers-required-by-your-application"></a>Impostare i provider di accesso necessari per l'applicazione

Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:

* Istruzioni di [Facebook](xref:security/authentication/facebook-logins)
* Istruzioni di [Twitter](xref:security/authentication/twitter-logins)
* Istruzioni di [Google](xref:security/authentication/google-logins)
* Istruzioni di [Microsoft](xref:security/authentication/microsoft-logins)
* Istruzioni di [altri provider](xref:security/authentication/otherlogins)

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>Impostare facoltativamente la password

Quando ci si registra con un provider di accesso esterno, non si dispone di una password registrata con l'app. Questo evita la creazione e la memorizzazione di una password per il sito, ma crea anche una dipendenza dal provider di accesso esterno. Se il provider di accesso esterno non è disponibile, non si potrà accedere al sito web.

Per creare una password e accedere usando la posta elettronica impostata durante il processo di accesso con provider esterni:

* Selezionare il collegamento **Salve &lt;alias di posta elettronica&gt;** nell'angolo superiore destro per passare alla vista **Gestione**.

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* Selezionare **Crea**.

![Impostare la pagina della password](index/_static/pass2a.png)

* Impostare una password valida in modo da usarla per accedere con la posta elettronica.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.

* Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.

* Può essere opportuno salvare in modo permanente dati aggiuntivi sull'utente e il relativo accesso e i token di aggiornamento. Per ulteriori informazioni, vedere <xref:security/authentication/social/additional-claims>.
