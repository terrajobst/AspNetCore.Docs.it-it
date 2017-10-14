---
title: Abilitazione dell'autenticazione con Facebook, Google e altri esterni
author: rick-anderson
description: 
keywords: ASP.NET di base, autenticazione, social, provider di autenticazione, google, facebook, twitter, account di microsoft
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 7b72421f703db919daf640c54b725fc1497d2481
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni

<a name="security-authentication-social-logins"></a>

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come compilare un'app ASP.NET Core 2.x che consente agli utenti di accedere tramite OAuth 2.0 con le credenziali dai provider di autenticazione esterni.

I provider di [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), e [Microsoft](microsoft-logins.md) vengono trattati nelle sezioni seguenti. Altri provider sono disponibili nei pacchetti di terze parti, ad esempio [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) e [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

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

## <a name="require-ssl"></a>Richiedere SSL

OAuth 2.0 richiede l'uso di SSL per l'autenticazione tramite il protocollo HTTPS.

Nota: i progetti creati tramite i modelli di progetto **Applicazione Web** o **API Web** per ASP.NET Core 2.x vengono configurati automaticamente per attivare SSL e avviarlo con l'URL https se l'opzione **Account utente singoli** è stata selezionata l'opzione su **Modifica finestra di dialogo di autenticazione** nella procedura guidata del progetto, come illustrato in precedenza.

* Richiedere SSL sul sito seguendo la procedura descritta nell'argomento [Applicazione SSL in un'app ASP.NET Core](xref:security/enforcing-ssl).

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Usare SecretManager per archiviare i token assegnati dai provider di accesso

I provider di accesso ai social assegnano i token **ID applicazione** e **Segreto dell'applicazione** durante il processo di registrazione (la denominazione esatta varia in base al provider).

Questi valori sono di fatto il *nome utente* e la *password* che l'applicazione usa per accedere alle API e costituiscono i "segreti" che possono essere collegati a una configurazione dell'applicazione con l'aiuto di **Manager segreto** invece di archiviarli direttamente nei file di configurazione o di impostarli come hard-coded.

Seguire la procedura nell'argomento [Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core](xref:security/app-secrets) in modo da poter archiviare i token assegnati da ciascun provider di accesso indicato di seguito.

## <a name="setup-login-providers-required-by-your-application"></a>Impostare i provider di accesso necessari per l'applicazione

Usare gli argomenti seguenti per configurare l'applicazione per usare i rispettivi provider:

* Istruzioni di [Facebook](facebook-logins.md)
* Istruzioni di [Twitter](twitter-logins.md)
* Istruzioni di [Google](google-logins.md)
* Istruzioni di [Microsoft](microsoft-logins.md)
* Istruzioni di [altri provider](other-logins.md)

## <a name="optionally-set-password"></a>Impostare facoltativamente la password

Quando ci si registra con un provider di accesso esterno, non si dispone di una password registrata con l'app. Questo evita la creazione e la memorizzazione di una password per il sito, ma crea anche una dipendenza dal provider di accesso esterno. Se il provider di accesso esterno non è disponibile, non si potrà accedere al sito web.

Per creare una password e accedere usando la posta elettronica impostata durante il processo di accesso con provider esterni:

* Toccare il collegamento **Hello <email alias>** nell'angolo superiore destro per passare alla vista **Gestione**.

![Vista Gestione dell'applicazione Web](index/_static/pass1a.png)

* Toccare **Crea**

![Impostare la pagina della password](index/_static/pass2a.png)

* Impostare una password valida in modo da usarla per accedere con la posta elettronica.

## <a name="next-steps"></a>Passaggi successivi

* Questo articolo ha introdotto l'autenticazione esterna e ha illustrato i prerequisiti necessari per aggiungere gli account di accesso esterni all'app di ASP.NET Core.

* Pagine di riferimento specifico del provider per configurare gli account di accesso per i provider richiesti dall'app.
