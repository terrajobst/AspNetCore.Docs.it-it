---
title: Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 063d452fb6ab91b712ade7f7b7ed99823dbdc657
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098818"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali dai provider di autenticazione esterni.

I provider di [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), e [Microsoft](xref:security/authentication/microsoft-logins) vengono trattati nelle sezioni seguenti. Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

Consentire agli utenti di accedere con le credenziali esistenti è utile per gli utenti e sposta molte delle complessità di gestione del processo di accesso in una terza parte. Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

* In Visual Studio 2017 creare un nuovo progetto dalla pagina iniziale o scegliendo **File** > **Nuovo** > **Progetto**.

* Selezionare il modello **Applicazione Web ASP.NET Core** disponibile nella categoria **Visual C#** > **.NET Core**:

![Finestra di dialogo Nuovo progetto](index/_static/new-project.png)

* Toccare **Applicazione Web** e verificare che l'**Autenticazione** sia impostata su **Account utente individuali**:

![Finestra di dialogo Nuova Applicazione Web](index/_static/select-project.png)

Nota: questa esercitazione si applica alla versione di ASP.NET Core 2.0 SDK che può essere selezionata nella parte superiore della procedura guidata.

## <a name="apply-migrations"></a>Applicare le migrazioni

* Eseguire l'app e selezionare il collegamento **Accedi**.
* Selezionare il collegamento **Esegui registrazione come nuovo utente**.
* Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.
* Seguire le istruzioni per applicare le migrazioni.

## <a name="require-https"></a>Richiedere HTTPS

OAuth 2.0 richiede l'uso di SSL/TLS per l'autenticazione tramite il protocollo HTTPS.

I progetti creati usando i modelli di progetto **Applicazione Web** o **API Web** con ASP.NET Core 2.1 o versioni successive vengono automaticamente configurati per l'abilitazione di HTTPS. L'app viene avviata con un endpoint predefinito protetto se è stata selezionata l'opzione **Account utente individuali** nella finestra di dialogo **Modifica autenticazione** della procedura guidata per il progetto.

Per ulteriori informazioni, vedere <xref:security/enforcing-ssl>.

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

* Toccare il collegamento **Hello &lt;alias di posta elettronica&gt;** nell'angolo superiore destro per passare alla vista **Gestione**.

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* Toccare **Crea**

![Impostare la pagina della password](index/_static/pass2a.png)

* Impostare una password valida in modo da usarla per accedere con la posta elettronica.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.

* Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.

* Può essere opportuno salvare in modo permanente dati aggiuntivi sull'utente e il relativo accesso e i token di aggiornamento. Per ulteriori informazioni, vedere <xref:security/authentication/social/additional-claims>.
