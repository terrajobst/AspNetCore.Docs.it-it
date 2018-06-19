---
title: Autenticazione cloud in web API con Azure Active Directory B2C in ASP.NET Core
author: camsoper
description: Per scoprire come configurare l'autenticazione di Azure Active Directory B2C con API Web di ASP.NET Core. Eseguire il test web API con Postman autenticato.
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 621290f7e303f9157577b5c1b32646b750ed5159
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897804"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticazione cloud in web API con Azure Active Directory B2C in ASP.NET Core

Di [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Directory attiva](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) è una soluzione di gestione di identità cloud per App per dispositivi mobili e web. Il servizio fornisce l'autenticazione per le applicazioni ospitate nel cloud e locali. Tipi di autenticazione includono singoli account, account di social network e federated account dell'organizzazione. Inoltre, Azure AD B2C può fornire l'autenticazione a più fattori con la configurazione minima.

> [!TIP]
> Azure Active Directory (Azure AD) e Azure Active Directory B2C sono offerte prodotto distinto. Un tenant di Azure AD rappresenta un'organizzazione, mentre un tenant di Azure Active Directory B2C rappresenta una raccolta delle identità da utilizzare con le applicazioni relying party. Per ulteriori informazioni, vedere [Azure Active Directory B2C: domande frequenti (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Poiché l'API web non dispone di alcuna interfaccia utente, vengono infatti non è possibile reindirizzare l'utente a un servizio token di sicurezza come Azure Active Directory B2C. Al contrario, l'API viene passato un token di connessione da app chiamante, che è già autenticato l'utente con Azure Active Directory B2C. L'API convalida quindi il token senza interazione diretta dell'utente.

In questa esercitazione, informazioni su come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C.
> * Registrare un'API Web in Azure Active Directory B2C.
> * Utilizzare Visual Studio per creare un'API Web configurato per usare il tenant di Azure Active Directory B2C per l'autenticazione.
> * Configurare i criteri che controllano il comportamento del tenant di Azure Active Directory B2C.
> * Utilizzare Postman per simulare un'app web che presenta una finestra di dialogo account di accesso, recupera un token e viene utilizzato per effettuare una richiesta con l'API web.

## <a name="prerequisites"></a>Prerequisiti

Di seguito è necessari per questa procedura dettagliata:

* [Sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (qualsiasi edizione)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Creare il tenant di Azure Active Directory B2C

Creare un tenant di Azure Active Directory B2C [come descritto nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-get-started). Quando richiesto, associare il tenant a una sottoscrizione di Azure è facoltativa per questa esercitazione.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Configurare un criterio di iscrizione o accesso

Utilizzare i passaggi nella documentazione di Azure Active Directory B2C per [creare un criterio di iscrizione o accesso](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Assegnare il nome criterio **SiUpIn**.  Usare i valori di esempio forniti nella documentazione relativa a **provider di identità**, **attributi iscrizione**, e **attestazioni applicazione**. Utilizzando il **Esegui ora** pulsante per testare il criterio, come descritto nella documentazione è facoltativo.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrare l'API in Azure Active Directory B2C

Nel tenant di Azure Active Directory B2C appena creato, registrare l'utilizzo di API [i passaggi nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) sotto il **registrare un'API web** sezione.

Utilizzare i valori seguenti:

| Impostazione                       | Valore               | Note                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nome dell'API&gt;*  | Immettere un **nome** per l'applicazione che descritta l'app agli utenti.                     |
| **Esempio di app web o web API** | Yes                 |                                                                                        |
| **Consenti flusso implicito**       | Yes                 |                                                                                        |
| **URL di risposta**                 | `https://localhost` | URL di risposta sono endpoint in Azure Active Directory B2C restituisce tutti i token che richiede l'app. |
| **URI ID App**                | *api*               | L'URI non deve necessariamente essere risolto in un indirizzo fisico. Deve essere univoco.     |
| **Includere native client**     | No                  |                                                                                        |

Dopo aver registrato l'API, viene visualizzato l'elenco di App e le API nel tenant. Selezionare l'API che è stato appena registrato. Selezionare il **copia** a destra dell'icona del **ID applicazione** campo per copiarlo negli Appunti. Selezionare **pubblicati ambiti** e verificare il valore predefinito *user_impersonation* ambito è presente.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Creare un'app di ASP.NET Core in Visual Studio 2017

Il modello di applicazione Web di Visual Studio può essere configurato per usare il tenant di Azure Active Directory B2C per l'autenticazione.

In Visual Studio:

1. Creare una nuova applicazione Web ASP.NET Core. 
2. Selezionare **API Web** dall'elenco dei modelli.
3. Selezionare il **Modifica autenticazione** pulsante.

    ![Pulsante di modifica autenticazione](./azure-ad-b2c-webapi/change-auth-button.png)

4. Nel **Modifica autenticazione** finestra di dialogo Seleziona **singoli account utente di**, quindi selezionare **connettersi a un archivio utente esistente nel cloud** nell'elenco a discesa. 

    ![Finestra di dialogo Modifica autenticazione](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Completare il modulo con i valori seguenti:

    | Impostazione                       | Valore                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nome di dominio**               | *&lt;il nome di dominio del tenant B2C&gt;*          |
    | **ID dell'applicazione**            | *&lt;incollare l'ID dell'applicazione dagli Appunti&gt;* |
    | **Criteri di iscrizione o accesso** | `B2C_1_SiUpIn`                                        |

    Selezionare **OK** per chiudere la **Modifica autenticazione** finestra di dialogo. Selezionare **OK** per creare l'app web.

Visual Studio crea l'API web con un controller denominato *ValuesController.cs* che restituisce i valori hardcoded per le richieste GET. La classe decorata con il [attributo Authorize](xref:security/authorization/simple), pertanto tutte le richieste di autenticazione.

## <a name="run-the-web-api"></a>Eseguire l'API web

In Visual Studio, eseguire l'API. Visual Studio avvia un browser a cui fa riferimento all'URL radice dell'API. Prendere nota dell'URL nella barra degli indirizzi e lasciare l'API in esecuzione in background.

> [!NOTE]
> Poiché non è presente alcun controller definito per l'URL radice, il browser visualizza errore 404 (pagina non trovata). Si tratta di un comportamento previsto.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Utilizzare Postman per ottenere un token e l'API di test

[Postman](https://getpostman.com/postman) è uno strumento per il test web API. Per questa esercitazione, Postman Simula un'app web che consente di accedere all'API web per conto dell'utente.

### <a name="register-postman-as-a-web-app"></a>Registrare Postman come un'app web

Poiché Postman Simula un'app web che può ottenere i token dal tenant Azure Active Directory B2C, deve essere registrato nel tenant di un'applicazione web. Registrare Postman con [i passaggi nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sotto il **registrare un'app web** sezione. Arresto di **creare un segreto client dell'applicazione web** sezione. Un segreto client non è necessario per questa esercitazione. 

Utilizzare i valori seguenti:

| Impostazione                       | Valore                            | Note                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Esempio di app web o web API** | Yes                              |                                 |
| **Consenti flusso implicito**       | Yes                              |                                 |
| **URL di risposta**                 | `https://getpostman.com/postman` |                                 |
| **URI ID App**                | *&lt;Lasciare vuoto&gt;*            | Non è obbligatorio per questa esercitazione. |
| **Includere native client**     | No                               |                                 |

L'app web appena registrato richiede l'autorizzazione per accedere all'API web per conto dell'utente.  

1. Selezionare **Postman** nell'elenco di App e quindi selezionare **l'accesso all'API** dal menu a sinistra.
2. Selezionare **+ Aggiungi**.
3. Nel **selezionare API** elenco a discesa selezionare il nome dell'API Web.
4. Nel **selezionare gli ambiti** elenco a discesa, assicurarsi che siano selezionati tutti gli ambiti.
5. Selezionare **Ok**.

Si noti ID applicazione dell'app Postman, perché è necessaria per ottenere un token di connessione.

### <a name="create-a-postman-request"></a>Creare una richiesta di Postman

Avviare Postman. Per impostazione predefinita, viene visualizzato Postman il **Crea nuovo** finestra di dialogo all'avvio. Se non viene visualizzata la finestra di dialogo, selezionare il **+ nuovo** pulsante in alto a sinistra.

Dal **Crea nuovo** finestra di dialogo:

1. Selezionare **richiesta**.

    ![Pulsante di richiesta](./azure-ad-b2c-webapi/postman-create-new.png)

2. Immettere *ottenere valori* nel **nome richiesta** casella.
3. Selezionare **+ Crea raccolta** per creare una nuova raccolta per archiviare la richiesta. Nome insieme *esercitazioni ASP.NET Core* e quindi selezionare il segno di spunta.

    ![Creare una nuova raccolta](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Selezionare il **salvare alle esercitazioni di ASP.NET Core** pulsante.

### <a name="test-the-web-api-without-authentication"></a>L'API web senza l'autenticazione di test

Per verificare che l'API web richiede l'autenticazione, effettuare una richiesta senza autenticazione.

1. Nel **immettere l'URL della richiesta** , immettere l'URL per `ValuesController`. L'URL è identico a quello visualizzato nel browser con **api/valori** aggiunto. Ad esempio `https://localhost:44375/api/values`.
2. Selezionare il **inviare** pulsante.
3. Si noti lo stato della risposta è *401 non autorizzato*.

    ![401 non autorizzato di risposta](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Ottenere un token di connessione

Per rendere una richiesta autenticata all'API web, è necessario un token di connessione. Postman consente di accedere al tenant di Azure Active Directory B2C e ottenere un token.

1. Nel **autorizzazione** nella scheda il **tipo** elenco a discesa Seleziona **OAuth 2.0**. Nel **aggiungere dati di autorizzazione per** elenco a discesa Seleziona **intestazioni di richiesta**. Selezionare **ottenere Token di accesso nuovo**.

    ![Scheda con le impostazioni di autorizzazione](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Completare il **ottenere nuovi TOKEN di accesso** finestra di dialogo come segue:


   |                Impostazione                 |                                             Valore                                             |                                                                                                                                    Note                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <strong>Nome token</strong>       |                                  <em>&lt;Nome token&gt;</em>                                  |                                                                                                                   Immettere un nome descrittivo per il token.                                                                                                                    |
   |      <strong>Tipo di concessione</strong>       |                                           implicito                                            |                                                                                                                                                                                                                                                                              |
   |     <strong>URL callback</strong>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <strong>Autorizzazione URL</strong>        | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |                                                                                                  Sostituire <em>&lt;nome dominio del tenant&gt;</em> con nome di dominio del tenant.                                                                                                  |
   |       <strong>ID client</strong>       |                <em>&lt;Immettere la password di Postman <b>ID applicazione</b>&gt;</em>                 |                                                                                                                                                                                                                                                                              |
   |     <strong>Segreto client</strong>     |                                 <em>&lt;Lasciare vuoto&gt;</em>                                  |                                                                                                                                                                                                                                                                              |
   |         <strong>Ambito</strong>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | Sostituire <em>&lt;nome dominio del tenant&gt;</em> con nome di dominio del tenant. Sostituire <em>&lt;api&gt;</em> con il nome del progetto API Web. È anche possibile usare l'ID dell'applicazione. Il criterio per l'URL è: <em>https://{tenant}.onmicrosoft.com/{app_name_or_id}/{scope nome}</em>. |
   | <strong>Autenticazione client</strong> |                                Inviare le credenziali del client nel corpo                                |                                                                                                                                                                                                                                                                              |


3. Selezionare il **richiesta Token** pulsante.

4. Postman apre una nuova finestra contenente una finestra di dialogo di accesso del tenant Azure Active Directory B2C. Accedere con un account esistente (se ne è stato creato il test dei criteri) oppure selezionare **Iscriviti ora** per creare un nuovo account. Il **password dimenticata?** collegamento viene utilizzato per reimpostare una password dimenticata.

5. Dopo l'accesso correttamente, la finestra viene chiusa e **gestire i token di accesso** viene visualizzata la finestra. Scorrere fino a inferiore e selezionare il **utilizzare Token** pulsante.

    ![Dove trovare il pulsante "Utilizzare Token"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>L'API web con autenticazione di test

Selezionare il **inviare** pulsante a inviare nuovamente la richiesta. In questo caso, lo stato della risposta è *200 OK* e il payload JSON è visibile sulla risposta **corpo** scheda.

![Stato di esito positivo e di payload](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C.
> * Registrare un'API Web in Azure Active Directory B2C.
> * Utilizzare Visual Studio per creare un'API Web configurato per usare il tenant di Azure Active Directory B2C per l'autenticazione.
> * Configurare i criteri che controllano il comportamento del tenant di Azure Active Directory B2C.
> * Utilizzare Postman per simulare un'app web che presenta una finestra di dialogo account di accesso, recupera un token e viene utilizzato per effettuare una richiesta con l'API web.

Continuare a sviluppare l'API per imparare a:

* [Proteggere un Core di ASP.NET web app con Azure Active Directory B2C](xref:security/authentication/azure-ad-b2c).
* [Chiamare un'API web di .NET da un'app web .NET usando Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Personalizzare l'interfaccia utente di Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurare i requisiti di complessità delle password](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Abilitare multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurare i provider di identità aggiuntive, ad esempio [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e altri.
* [Usare l'API di Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) per recuperare le informazioni utente aggiuntive, ad esempio l'appartenenza al gruppo, il tenant di Azure Active Directory B2C.
