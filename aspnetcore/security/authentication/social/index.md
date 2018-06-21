---
title: Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x tramite OAuth 2.0 con provider di autenticazione esterni.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: 58045504ce4588f854428273273d3ea8f181e12e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277998"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Autenticazione dei provider Facebook, Google ed esterni in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali dai provider di autenticazione esterni.

I provider di [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), e [Microsoft](xref:security/authentication/microsoft-logins) vengono trattati nelle sezioni seguenti. Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Icone di social media per Facebook, Twitter, Google + e Windows](index/_static/social.png)

Consentire agli utenti di accedere con le credenziali esistenti è utile per gli utenti e sposta molte delle complessità di gestione del processo di accesso in una terza parte. Per degli esempi di come gli account di accesso ai social possano risultare utili per la conversione del traffico e del cliente, vedere dei case study da [Facebook](https://www.facebook.com/unsupportedbrowser) e [Twitter](https://dev.twitter.com/resources/case-studies).

Nota: i pacchetti qui presentati riassumono una notevole dose di complessità del flusso di autenticazione OAuth, ma comprendere i dettagli potrebbe essere necessario per risolvere il problema. Sono disponibili molte risorse; ad esempio, vedere [Introduzione a OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) o [Comprensione di OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Alcuni problemi possono essere risolti esaminando il [Codice sorgente di ASP.NET Core per i pacchetti di provider](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

* In Visual Studio 2017 creare un nuovo progetto dalla Pagina iniziale o tramite **File > Nuovo > Progetto**.

* Selezionare il modello **Applicazione Web di ASP.NET Core** disponibile nella categoria **Visual C# > .NET Core**:

![Finestra di dialogo Nuovo progetto](index/_static/new-project.png)

* Toccare **Applicazione Web** e verificare che l'**Autenticazione** sia impostata su **Account utente individuali**:

![Finestra di dialogo Nuova Applicazione Web](index/_static/select-project.png)

Nota: questa esercitazione si applica alla versione di ASP.NET Core 2.0 SDK che può essere selezionata nella parte superiore della procedura guidata.

## <a name="apply-migrations"></a>Applicare le migrazioni

* Eseguire l'app e selezionare il collegamento **Accedi**.
* Selezionare il collegamento **Esegui registrazione come nuovo utente**.
* Immettere l'indirizzo di posta elettronica e la password per il nuovo account, quindi selezionare **Registra**.
* Seguire le istruzioni per applicare le migrazioni.

## <a name="require-ssl"></a>Richiedere SSL

OAuth 2.0 richiede l'uso di SSL per l'autenticazione tramite il protocollo HTTPS.

Nota: i progetti creati tramite i modelli di progetto **Applicazione Web** o **API Web** per ASP.NET Core 2.x vengono configurati automaticamente per attivare SSL e avviarlo con l'URL https se l'opzione **Account utente singoli** è stata selezionata l'opzione su **Modifica finestra di dialogo di autenticazione** nella procedura guidata del progetto, come illustrato in precedenza.

* Richiedere SSL per il sito seguendo la procedura descritta nell'argomento [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) (Imporre SSL in un'app ASP.NET Core).

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Usare SecretManager per archiviare i token assegnati dai provider di accesso

I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione (la denominazione esatta varia in base al provider).

Questi valori sono di fatto il *nome utente* e la *password* che l'applicazione usa per accedere alle API e costituiscono i "segreti" che possono essere collegati a una configurazione dell'applicazione con l'aiuto di **Manager segreto** invece di archiviarli direttamente nei file di configurazione o di impostarli come hard-coded.

Seguire la procedura nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) in modo da poter archiviare i token assegnati da ciascun provider di accesso indicato di seguito.

## <a name="setup-login-providers-required-by-your-application"></a>Impostare i provider di accesso necessari per l'applicazione

Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:

* Istruzioni di [Facebook](xref:security/authentication/facebook-logins)
* Istruzioni di [Twitter](xref:security/authentication/twitter-logins)
* Istruzioni di [Google](xref:security/authentication/google-logins)
* Istruzioni di [Microsoft](xref:security/authentication/microsoft-logins)
* Istruzioni di [altri provider](xref:security/authentication/otherlogins)

[!INCLUDE[](~/includes/chain-auth-providers.md)]

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
