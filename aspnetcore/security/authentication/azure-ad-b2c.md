---
title: Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core
author: camsoper
description: Per scoprire come configurare l'autenticazione di Azure Active Directory B2C con ASP.NET Core.
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core

Di [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Directory attiva](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) è una soluzione di gestione di identità cloud per App per dispositivi mobili e web. Il servizio fornisce l'autenticazione per le applicazioni ospitate nel cloud e locali. Tipi di autenticazione includono singoli account, account di social network e federated account dell'organizzazione. Inoltre, Azure AD B2C può fornire l'autenticazione a più fattori con la configurazione minima.

> [!TIP]
> Azure Active Directory (Azure AD) Azure Active Directory B2C sono offerte di prodotti separato. Un tenant di Azure AD rappresenta un'organizzazione, mentre un tenant di Azure Active Directory B2C rappresenta una raccolta delle identità da utilizzare con le applicazioni relying party. Per ulteriori informazioni, vedere [Azure Active Directory B2C: domande frequenti (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).

In questa esercitazione, informazioni su come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C
> * Registrare un'app in Azure Active Directory B2C
> * Utilizzare Visual Studio per creare un'app web ASP.NET Core configurata per usare il tenant di Azure Active Directory B2C per l'autenticazione
> * Configurare i criteri che controllano il comportamento del tenant di Azure Active Directory B2C

## <a name="prerequisites"></a>Prerequisiti

Di seguito è necessari per questa procedura dettagliata:

* [Sottoscrizione di Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (qualsiasi edizione)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Creare il tenant di Azure Active Directory B2C

Creare un tenant di Azure Active Directory B2C [come descritto nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-get-started). Quando richiesto, associare il tenant a una sottoscrizione di Azure è facoltativa per questa esercitazione.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrare l'app in Azure Active Directory B2C

Nel tenant di Azure Active Directory B2C appena creato, registrare l'app utilizzando [i passaggi nella documentazione di](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) sotto il **registrare un'app web** sezione. Arresto di **creare un segreto client dell'applicazione web** sezione. Un segreto client non è necessario per questa esercitazione. 

Utilizzare i valori seguenti:

| Impostazione                       | Valore                     | Note                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nome dell'App&gt;*        | Immettere un **nome** per l'applicazione che descritta l'app agli utenti.                                                                                                                                 |
| **Esempio di app web o web API** | Yes                       |                                                                                                                                                                                                    |
| **Consenti flusso implicito**       | Yes                       |                                                                                                                                                                                                    |
| **URL di risposta**                 | `https://localhost:44300` | URL di risposta sono endpoint in Azure Active Directory B2C restituisce tutti i token che richiede l'app. Visual Studio fornisce l'URL di risposta da utilizzare. Per il momento, immettere `https://localhost:44300` per completare il modulo. |
| **URI ID App**                | Lasciare vuoto               | Non è obbligatorio per questa esercitazione.                                                                                                                                                                    |
| **Includere native client**     | No                        |                                                                                                                                                                                                    |

> [!WARNING]
> Se l'impostazione di un URL di risposta non localhost, tenere il [vincoli su ciò che è consentito nell'elenco URL di risposta](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Dopo aver registrato l'app, viene visualizzato l'elenco di App nel tenant. Selezionare l'app che è stato appena registrato. Selezionare il **copia** a destra dell'icona del **ID applicazione** campo per copiarlo negli Appunti.

Nessun elemento maggiore, può essere configurato nel tenant di Azure Active Directory B2C in questo momento, ma lasciare aperta la finestra del browser. Dopo aver creato l'applicazione ASP.NET di base non è più configurazione.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Creare un'app di ASP.NET Core in Visual Studio 2017

Il modello di applicazione Web di Visual Studio può essere configurato per usare il tenant di Azure Active Directory B2C per l'autenticazione.

In Visual Studio:

1. Creare una nuova applicazione Web ASP.NET Core. 
2. Selezionare **applicazione Web** dall'elenco dei modelli.
3. Selezionare il **Modifica autenticazione** pulsante.
    
    ![Pulsante di modifica autenticazione](./azure-ad-b2c/_static/changeauth.png)

4. Nel **Modifica autenticazione** finestra di dialogo Seleziona **singoli account utente di**, quindi selezionare **connettersi a un archivio utente esistente nel cloud** nell'elenco a discesa. 
    
    ![Finestra di dialogo Modifica autenticazione](./azure-ad-b2c/_static/changeauthdialog.png)

5. Completare il modulo con i valori seguenti:
    
    | Impostazione                       | Valore                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nome di dominio**               | *&lt;il nome di dominio del tenant B2C&gt;*          |
    | **ID dell'applicazione**            | *&lt;incollare l'ID dell'applicazione dagli Appunti&gt;* |
    | **Percorso di callback**             | *&lt;Utilizzare il valore predefinito&gt;*                       |
    | **Criteri di iscrizione o accesso** | `B2C_1_SiUpIn`                                        |
    | **Criteri di reimpostazione della password**     | `B2C_1_SSPR`                                          |
    | **Modificare i criteri del profilo**       | *&lt;Lasciare vuoto&gt;*                                 |
    
    Selezionare il **copia** collegamento accanto a **URI di Reply** per copiare l'URI di risposta negli Appunti. Selezionare **OK** per chiudere la **Modifica autenticazione** finestra di dialogo. Selezionare **OK** per creare l'app web.

## <a name="finish-the-b2c-app-registration"></a>Completare la registrazione di app B2C

Tornare alla finestra del browser con le proprietà di app B2C ancora aperte. Modificare il file temporaneo **URL di risposta** specificato precedentemente per il valore copiato da Visual Studio. Selezionare **salvare** nella parte superiore della finestra.

> [!TIP]
> Se si non copia l'URL di risposta, utilizzare l'indirizzo SSL nella scheda Debug nelle proprietà del progetto web e aggiungere il **CallbackPath** valore *appSettings. JSON*.

## <a name="configure-policies"></a>Configurare i criteri

Utilizzare i passaggi nella documentazione di Azure Active Directory B2C per [creare un criterio di iscrizione o accesso](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)e quindi [creare un criterio di reimpostazione password](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Usare i valori di esempio forniti nella documentazione relativa a **provider di identità**, **attributi iscrizione**, e **attestazioni applicazione**. Utilizzando il **Esegui ora** per testare i criteri, come descritto nella documentazione è facoltativo.

> [!WARNING]
> Verificare i nomi dei criteri sono esattamente come descritto nella documentazione di tali criteri sono stati usati durante la **Modifica autenticazione** finestra di dialogo in Visual Studio. I nomi dei criteri può essere verificati nel *appSettings. JSON*.

## <a name="run-the-app"></a>Eseguire l'app

In Visual Studio, premere **F5** per compilare ed eseguire l'app. Dopo avere avviato l'applicazione web, selezionare **Accedi**.

![Accedere all'App](./azure-ad-b2c/_static/signin.png)

Reindirizza il browser al tenant di Azure Active Directory B2C. Accedere con un account esistente (se ne è stato creato il test dei criteri) oppure selezionare **Iscriviti ora** per creare un nuovo account. Il **password dimenticata?** collegamento viene utilizzato per reimpostare una password dimenticata.

![Account di accesso di Azure Active Directory B2C](./azure-ad-b2c/_static/b2csts.png)

Dopo l'accesso correttamente, il browser reindirizza all'app web.

![Riuscito](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un tenant di Azure Active Directory B2C
> * Registrare un'app in Azure Active Directory B2C
> * Utilizzare Visual Studio per creare un'applicazione Web di base ASP.NET configurato per usare il tenant di Azure Active Directory B2C per l'autenticazione
> * Configurare i criteri che controllano il comportamento del tenant di Azure Active Directory B2C

Ora che l'applicazione ASP.NET di base è configurato per usare Azure Active Directory B2C per l'autenticazione, il [attributo Authorize](xref:security/authorization/simple) può essere utilizzato per proteggere l'app. Continuare a sviluppare l'app per imparare a:

* [Personalizzare l'interfaccia utente di Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurare i requisiti di complessità delle password](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Abilitare multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurare i provider di identità aggiuntive, ad esempio [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e altri.
* [Usare l'API di Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) per recuperare le informazioni utente aggiuntive, ad esempio l'appartenenza al gruppo, il tenant di Azure Active Directory B2C.
* [Proteggere un Core di ASP.NET web API con Azure Active Directory B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Chiamare un'API web di .NET da un'app web .NET usando Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
