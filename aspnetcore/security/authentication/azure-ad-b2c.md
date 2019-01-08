---
title: Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core
author: camsoper
description: Informazioni su come configurare l'autenticazione di Azure Active Directory B2C con ASP.NET Core.
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 2c544475ccd3eb76f2737fec1cf269ac86add372
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098987"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core

Di [Cam Soper](https://twitter.com/camsoper)

[Azure Active B2C di Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) è una soluzione di gestione identità cloud per le App per dispositivi mobili e web. Il servizio fornisce l'autenticazione per le app ospitate nel cloud e locali. Tipi di autenticazione includono account individuali, gli account di social network e account aziendali federati. Inoltre, Azure AD B2C fornisce l'autenticazione a più fattori con la configurazione minima.

> [!TIP]
> Azure Active Directory (Azure AD) e Azure AD B2C vengono offerti come prodotti separati. Un tenant di Azure AD rappresenta un'organizzazione, mentre un tenant di Azure AD B2C rappresenta una raccolta di identità da usare con le applicazioni relying party. Per altre informazioni, vedere [Azure AD B2C: Domande frequenti (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C
> * Registrare un'app in Azure AD B2C
> * Usare Visual Studio per creare un'app web ASP.NET Core configurata per l'utilizzo del tenant di Azure AD B2C per l'autenticazione
> * Configurare i criteri di controllo del comportamento del tenant di Azure AD B2C

## <a name="prerequisites"></a>Prerequisiti

Di seguito sono necessarie per questa procedura dettagliata:

* [Sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (qualsiasi edizione)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Creare il tenant di Azure Active Directory B2C

Creare un tenant di Azure Active Directory B2C [come descritto nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-get-started). Quando richiesto, associare il tenant con una sottoscrizione di Azure è facoltativa per questa esercitazione.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrare l'app in Azure AD B2C

Nel tenant di Azure AD B2C appena creato, registrare l'app usando [i passaggi nella documentazione sullo](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sotto il **registrare un'app web** sezione. Termina la **creare un segreto client dell'app web** sezione. Un segreto client non è necessario per questa esercitazione. 

Usare i valori seguenti:

| Impostazione                       | Value                     | Note                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nome dell'App&gt;*        | Immettere un **nome** per le app che descrive l'app agli utenti.                                                                                                                                 |
| **Includi app web / API web** | Yes                       |                                                                                                                                                                                                    |
| **Consenti flusso implicito**       | Yes                       |                                                                                                                                                                                                    |
| **URL di risposta**                 | `https://localhost:44300/signin-oidc` | Gli URL di risposta sono gli endpoint in cui Azure AD B2C restituisce eventuali token richiesti dall'app. Visual Studio fornisce l'URL di risposta da utilizzare. Per ora immettere `https://localhost:44300/signin-oidc` per completare il modulo. |
| **URI ID App**                | Lasciare vuoto               | Non è obbligatorio per questa esercitazione.                                                                                                                                                                    |
| **Includi client nativo**     | No                        |                                                                                                                                                                                                    |

> [!WARNING]
> Se l'impostazione di un URL di risposta diversi da localhost, tenere presenti le [vincoli su ciò che è consentito nell'elenco URL di risposta](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Dopo aver registrato l'app, viene visualizzato l'elenco di App nel tenant. Selezionare l'app che è stato appena registrato. Selezionare il **copia** a destra dell'icona le **ID applicazione** campo per copiarlo negli Appunti.

Nessun elemento maggiore, può essere configurato nel tenant di Azure AD B2C in questo momento, ma lasciare aperta la finestra del browser. Dopo aver creato l'app ASP.NET Core è altre opzioni di configurazione.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Creare un'app ASP.NET Core in Visual Studio 2017

Il modello di applicazione Web di Visual Studio può essere configurato per usare il tenant di Azure AD B2C per l'autenticazione.

In Visual Studio:

1. Creare una nuova applicazione Web ASP.NET Core. 
2. Selezionare **applicazione Web** dall'elenco dei modelli.
3. Selezionare il **Modifica autenticazione** pulsante.
    
    ![Pulsante Modifica autenticazione](./azure-ad-b2c/_static/changeauth.png)

4. Nel **Modifica autenticazione** finestra di dialogo, seleziona **account utente individuali**e quindi selezionare **Connetti a un archivio utente esistente nel cloud** nell'elenco a discesa. 
    
    ![Finestra di dialogo Modifica autenticazione](./azure-ad-b2c/_static/changeauthdialog.png)

5. Compilare il modulo con i valori seguenti:
    
    | Impostazione                       | Value                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nome di dominio**               | *&lt;il nome di dominio del tenant di B2C&gt;*          |
    | **ID dell'applicazione**            | *&lt;incollare l'ID dell'applicazione dagli Appunti&gt;* |
    | **Percorso di callback**             | *&lt;usare il valore predefinito&gt;*                       |
    | **Criteri di iscrizione o accesso** | `B2C_1_SiUpIn`                                        |
    | **Criteri di reimpostazione della password**     | `B2C_1_SSPR`                                          |
    | **Modifica criteri del profilo**       | *&lt;Lasciare vuoto&gt;*                                 |
    
    Selezionare il **copia** collegamento accanto a **URI di risposta** per copiare l'URI di risposta negli Appunti. Selezionare **OK** per chiudere la **Modifica autenticazione** finestra di dialogo. Selezionare **OK** per creare l'app web.

## <a name="finish-the-b2c-app-registration"></a>Completare la registrazione dell'app B2C

Tornare alla finestra del browser con le proprietà dell'app B2C ancora aperta. Modificare il file temporaneo **URL di risposta** specificato in precedenza per il valore copiato da Visual Studio. Selezionare **salvare** nella parte superiore della finestra.

> [!TIP]
> Se si non copia l'URL di risposta, usare l'indirizzo HTTPS dalla scheda Debug nelle proprietà del progetto web e aggiungere il **CallbackPath** valore dal *appSettings. JSON*.

## <a name="configure-policies"></a>Configurare i criteri

Usare i passaggi descritti nella documentazione di Azure AD B2C per [creare un criterio di iscrizione o accesso](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)e quindi [creare un criterio di reimpostazione della password](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Usare i valori di esempio forniti nella documentazione relativa a **provider di identità**, **attributi di iscrizione**, e **attestazioni dell'applicazione**. Usando il **Esegui adesso** pulsante per testare i criteri, come descritto nella documentazione è facoltativo.

> [!WARNING]
> Verificare che i nomi dei criteri sono esattamente come descritto nella documentazione, come tali criteri sono stati usati durante la **Modifica autenticazione** finestra di dialogo in Visual Studio. I nomi dei criteri può essere verificati nel *appSettings. JSON*.

## <a name="run-the-app"></a>Eseguire l'app

In Visual Studio, premere **F5** per compilare ed eseguire l'app. Dopo aver avviato l'app web, selezionare **Accept** per accettare l'uso dei cookie (se richiesto) e quindi selezionare **Accedi**.

![Accedere all'app](./azure-ad-b2c/_static/signin.png)

Il browser Reindirizza al tenant di Azure AD B2C. Accedere con un account esistente (se ne è stato creato il test dei criteri) oppure selezionare **iscriversi adesso** per creare un nuovo account. Il **password dimenticata?** collegamento viene utilizzato per reimpostare una password dimenticata.

![Account di accesso di Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Dopo aver effettuato l'accesso, il browser reindirizza all'app web.

![Riuscito](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C
> * Registrare un'app in Azure AD B2C
> * Usare Visual Studio per creare un'applicazione Web ASP.NET Core configurati per l'utilizzo del tenant di Azure AD B2C per l'autenticazione
> * Configurare i criteri di controllo del comportamento del tenant di Azure AD B2C

Ora che l'app ASP.NET Core è configurato per usare Azure AD B2C per l'autenticazione, il [attributo Authorize](xref:security/authorization/simple) può essere utilizzato per proteggere l'app. Continuare a sviluppare l'app da learning per:

* [Personalizzare l'interfaccia utente di Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurare i requisiti di complessità delle password](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Abilitare multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurare altri provider di identità, ad esempio [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e così via.
* [Usare l'API Graph di Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) per recuperare informazioni aggiuntive sull'utente, ad esempio l'appartenenza al gruppo, dal tenant Azure AD B2C.
* [Proteggere un ASP.NET Core API web usando Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Chiamare un'API web .NET da un'app web .NET usando Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
