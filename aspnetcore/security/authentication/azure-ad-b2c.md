---
title: Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core
author: camsoper
description: Scopri come configurare l'autenticazione Azure Active Directory B2C con ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727280"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="12991-103">Autenticazione cloud con Azure Active Directory B2C in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12991-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="12991-104">Di [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="12991-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="12991-105">[Azure Active B2C di Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) è una soluzione di gestione identità cloud per le App per dispositivi mobili e web.</span><span class="sxs-lookup"><span data-stu-id="12991-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="12991-106">Il servizio fornisce l'autenticazione per le app ospitate nel cloud e locali.</span><span class="sxs-lookup"><span data-stu-id="12991-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="12991-107">Tipi di autenticazione includono account individuali, gli account di social network e account aziendali federati.</span><span class="sxs-lookup"><span data-stu-id="12991-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="12991-108">Inoltre, Azure AD B2C possibile fornire l'autenticazione a più fattori con la configurazione minima.</span><span class="sxs-lookup"><span data-stu-id="12991-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="12991-109">Azure Active Directory (Azure AD) e Azure AD B2C vengono offerti come prodotti separati.</span><span class="sxs-lookup"><span data-stu-id="12991-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="12991-110">Un tenant di Azure AD rappresenta un'organizzazione, mentre un tenant di Azure AD B2C rappresenta una raccolta di identità da usare con le applicazioni relying party.</span><span class="sxs-lookup"><span data-stu-id="12991-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="12991-111">Per altre informazioni, vedere [Azure Active Directory B2C: domande frequenti (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="12991-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="12991-112">In questa esercitazione si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="12991-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12991-113">Creare un tenant di Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="12991-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="12991-114">Registrare un'app in Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="12991-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="12991-115">Usare Visual Studio per creare un'app Web ASP.NET Core configurata per l'uso del tenant Azure AD B2C per l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="12991-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="12991-116">Configurare i criteri che controllano il comportamento del tenant Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="12991-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12991-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="12991-117">Prerequisites</span></span>

<span data-ttu-id="12991-118">Di seguito sono necessarie per questa procedura dettagliata:</span><span class="sxs-lookup"><span data-stu-id="12991-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="12991-119">Sottoscrizione di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="12991-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="12991-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="12991-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="12991-121">Creare il tenant di Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="12991-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="12991-122">Creare un tenant [di Azure Active Directory B2C come descritto nella documentazione](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="12991-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="12991-123">Quando richiesto, associare il tenant con una sottoscrizione di Azure è facoltativa per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="12991-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="12991-124">Registrare l'app in Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="12991-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="12991-125">Nel tenant di Azure AD B2C appena creato registrare l'app seguendo [la procedura descritta nella documentazione](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) nella sezione **registrare un'app Web** .</span><span class="sxs-lookup"><span data-stu-id="12991-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) under the **Register a web app** section.</span></span> <span data-ttu-id="12991-126">Termina la **creare un segreto client dell'app web** sezione.</span><span class="sxs-lookup"><span data-stu-id="12991-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="12991-127">Un segreto client non è necessario per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="12991-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="12991-128">Utilizzare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="12991-128">Use the following values:</span></span>

| <span data-ttu-id="12991-129">Impostazione di</span><span class="sxs-lookup"><span data-stu-id="12991-129">Setting</span></span>                       | <span data-ttu-id="12991-130">Valore</span><span class="sxs-lookup"><span data-stu-id="12991-130">Value</span></span>                     | <span data-ttu-id="12991-131">Note</span><span class="sxs-lookup"><span data-stu-id="12991-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="12991-132">**Name**</span><span class="sxs-lookup"><span data-stu-id="12991-132">**Name**</span></span>                      | <span data-ttu-id="12991-133">*nome dell'app &lt;&gt;*</span><span class="sxs-lookup"><span data-stu-id="12991-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="12991-134">Immettere un **nome** per le app che descrive l'app agli utenti.</span><span class="sxs-lookup"><span data-stu-id="12991-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="12991-135">**Includi app web / API web**</span><span class="sxs-lookup"><span data-stu-id="12991-135">**Include web app / web API**</span></span> | <span data-ttu-id="12991-136">Sì</span><span class="sxs-lookup"><span data-stu-id="12991-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="12991-137">**Consenti flusso implicito**</span><span class="sxs-lookup"><span data-stu-id="12991-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="12991-138">Sì</span><span class="sxs-lookup"><span data-stu-id="12991-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="12991-139">**URL di risposta**</span><span class="sxs-lookup"><span data-stu-id="12991-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="12991-140">Gli URL di risposta sono gli endpoint in cui Azure AD B2C restituisce eventuali token richiesti dall'app.</span><span class="sxs-lookup"><span data-stu-id="12991-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="12991-141">Visual Studio fornisce l'URL di risposta da usare.</span><span class="sxs-lookup"><span data-stu-id="12991-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="12991-142">Per il momento, immettere `https://localhost:44300/signin-oidc` per completare il modulo.</span><span class="sxs-lookup"><span data-stu-id="12991-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="12991-143">**URI ID App**</span><span class="sxs-lookup"><span data-stu-id="12991-143">**App ID URI**</span></span>                | <span data-ttu-id="12991-144">Lascia vuoto</span><span class="sxs-lookup"><span data-stu-id="12991-144">Leave blank</span></span>               | <span data-ttu-id="12991-145">Non è obbligatorio per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="12991-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="12991-146">**Includi client nativo**</span><span class="sxs-lookup"><span data-stu-id="12991-146">**Include native client**</span></span>     | <span data-ttu-id="12991-147">No</span><span class="sxs-lookup"><span data-stu-id="12991-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="12991-148">Se si configura un URL di risposta non localhost, tenere presente i [vincoli sugli elementi consentiti nell'elenco URL di risposta](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span><span class="sxs-lookup"><span data-stu-id="12991-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span></span> 

<span data-ttu-id="12991-149">Dopo la registrazione dell'app, viene visualizzato l'elenco delle app nel tenant.</span><span class="sxs-lookup"><span data-stu-id="12991-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="12991-150">Selezionare l'app appena registrata.</span><span class="sxs-lookup"><span data-stu-id="12991-150">Select the app that was just registered.</span></span> <span data-ttu-id="12991-151">Selezionare il **copia** a destra dell'icona le **ID applicazione** campo per copiarlo negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="12991-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="12991-152">Al momento non è possibile configurare altre informazioni nel tenant di Azure AD B2C, ma lasciare aperta la finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="12991-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="12991-153">Dopo la creazione dell'app ASP.NET Core è presente una maggiore configurazione.</span><span class="sxs-lookup"><span data-stu-id="12991-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="12991-154">Creare un'app ASP.NET Core in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12991-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="12991-155">Il modello di applicazione Web di Visual Studio può essere configurato per usare il tenant di Azure AD B2C per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="12991-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="12991-156">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="12991-156">In Visual Studio:</span></span>

1. <span data-ttu-id="12991-157">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12991-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="12991-158">Selezionare **applicazione Web** dall'elenco di modelli.</span><span class="sxs-lookup"><span data-stu-id="12991-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="12991-159">Selezionare il **Modifica autenticazione** pulsante.</span><span class="sxs-lookup"><span data-stu-id="12991-159">Select the **Change Authentication** button.</span></span>
    
    ![Pulsante Modifica autenticazione](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="12991-161">Nella finestra di dialogo **Cambia autenticazione** selezionare **account utente singoli**, quindi selezionare **Connetti a un archivio utente esistente nel cloud** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="12991-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Finestra di dialogo Modifica autenticazione](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="12991-163">Compilare il modulo con i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="12991-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="12991-164">Impostazione di</span><span class="sxs-lookup"><span data-stu-id="12991-164">Setting</span></span>                       | <span data-ttu-id="12991-165">Valore</span><span class="sxs-lookup"><span data-stu-id="12991-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="12991-166">**Nome di dominio**</span><span class="sxs-lookup"><span data-stu-id="12991-166">**Domain Name**</span></span>               | <span data-ttu-id="12991-167">*&lt;il nome di dominio del tenant B2C&gt;*</span><span class="sxs-lookup"><span data-stu-id="12991-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="12991-168">**ID dell'applicazione**</span><span class="sxs-lookup"><span data-stu-id="12991-168">**Application ID**</span></span>            | <span data-ttu-id="12991-169">*&lt;incollare l'ID applicazione dagli Appunti&gt;*</span><span class="sxs-lookup"><span data-stu-id="12991-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="12991-170">**Percorso di callback**</span><span class="sxs-lookup"><span data-stu-id="12991-170">**Callback Path**</span></span>             | <span data-ttu-id="12991-171">*&lt;usare il valore predefinito&gt;*</span><span class="sxs-lookup"><span data-stu-id="12991-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="12991-172">**Criteri di iscrizione o accesso**</span><span class="sxs-lookup"><span data-stu-id="12991-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="12991-173">**Reimposta criteri password**</span><span class="sxs-lookup"><span data-stu-id="12991-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="12991-174">**Modificare i criteri del profilo**</span><span class="sxs-lookup"><span data-stu-id="12991-174">**Edit profile policy**</span></span>       | <span data-ttu-id="12991-175">*&lt;lasciare vuoto&gt;*</span><span class="sxs-lookup"><span data-stu-id="12991-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="12991-176">Selezionare il collegamento **copia** accanto a **URI di risposta** per copiare l'URI di risposta negli Appunti.</span><span class="sxs-lookup"><span data-stu-id="12991-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="12991-177">Selezionare **OK** per chiudere la **Modifica autenticazione** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="12991-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="12991-178">Selezionare **OK** per creare l'app web.</span><span class="sxs-lookup"><span data-stu-id="12991-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="12991-179">Completare la registrazione dell'app B2C</span><span class="sxs-lookup"><span data-stu-id="12991-179">Finish the B2C app registration</span></span>

<span data-ttu-id="12991-180">Tornare alla finestra del browser con le proprietà dell'app B2C ancora aperte.</span><span class="sxs-lookup"><span data-stu-id="12991-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="12991-181">Modificare l' **URL di risposta** temporaneo specificato in precedenza per il valore copiato da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12991-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="12991-182">Selezionare **Save (Salva** ) nella parte superiore della finestra.</span><span class="sxs-lookup"><span data-stu-id="12991-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="12991-183">Se l'URL di risposta non è stato copiato, usare l'indirizzo HTTPS dalla scheda debug delle proprietà del progetto Web e aggiungere il valore **CallbackPath** da *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="12991-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="12991-184">Configurare i criteri</span><span class="sxs-lookup"><span data-stu-id="12991-184">Configure policies</span></span>

<span data-ttu-id="12991-185">Usare la procedura descritta nella documentazione di Azure AD B2C per [creare un criterio di iscrizione o di accesso](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), quindi [creare un criterio di reimpostazione della password](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span><span class="sxs-lookup"><span data-stu-id="12991-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span></span> <span data-ttu-id="12991-186">Usare i valori di esempio forniti nella documentazione relativa a **provider di identità**, **attributi di iscrizione**, e **attestazioni dell'applicazione**.</span><span class="sxs-lookup"><span data-stu-id="12991-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="12991-187">Usare il pulsante **Esegui adesso** per testare i criteri come descritto nella documentazione è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="12991-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="12991-188">Verificare che i nomi dei criteri siano esattamente come descritto nella documentazione, perché tali criteri sono stati usati nella finestra di dialogo **Cambia autenticazione** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12991-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="12991-189">I nomi dei criteri possono essere verificati in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="12991-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="12991-190">Configurare le opzioni di OpenIdConnectOptions/JwtBearer/cookie sottostanti</span><span class="sxs-lookup"><span data-stu-id="12991-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="12991-191">Per configurare direttamente le opzioni sottostanti, utilizzare la costante dello schema appropriato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12991-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="12991-192">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="12991-192">Run the app</span></span>

<span data-ttu-id="12991-193">In Visual Studio premere **F5** per compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="12991-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="12991-194">Dopo l'avvio dell'app Web, selezionare **Accept (accetta** ) per accettare l'uso dei cookie (se richiesto) e quindi selezionare **Sign in (accedi**).</span><span class="sxs-lookup"><span data-stu-id="12991-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Accedi all'app](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="12991-196">Il browser reindirizza al tenant Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="12991-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="12991-197">Accedere con un account esistente (se ne è stato creato il test dei criteri) oppure selezionare **iscriversi adesso** per creare un nuovo account.</span><span class="sxs-lookup"><span data-stu-id="12991-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="12991-198">Il **password dimenticata?** collegamento viene utilizzato per reimpostare una password dimenticata.</span><span class="sxs-lookup"><span data-stu-id="12991-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Accesso Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="12991-200">Dopo aver eseguito l'accesso, il browser reindirizza all'app Web.</span><span class="sxs-lookup"><span data-stu-id="12991-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Success](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="12991-202">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="12991-202">Next steps</span></span>

<span data-ttu-id="12991-203">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="12991-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12991-204">Creare un tenant di Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="12991-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="12991-205">Registrare un'app in Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="12991-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="12991-206">Usare Visual Studio per creare un'applicazione Web ASP.NET Core configurata per l'uso del tenant Azure AD B2C per l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="12991-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="12991-207">Configurare i criteri che controllano il comportamento del tenant Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="12991-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="12991-208">Ora che l'app ASP.NET Core è configurata in modo da usare Azure AD B2C per l'autenticazione, è possibile usare l' [attributo di autorizzazione](xref:security/authorization/simple) per proteggere l'app.</span><span class="sxs-lookup"><span data-stu-id="12991-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="12991-209">Continua a sviluppare l'app imparando a:</span><span class="sxs-lookup"><span data-stu-id="12991-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="12991-210">[Personalizzare l'interfaccia utente di Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="12991-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="12991-211">[Configurare i requisiti di complessità delle password](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="12991-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="12991-212">[Abilitare multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="12991-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="12991-213">Configurare altri provider di identità, ad esempio [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)e così via.</span><span class="sxs-lookup"><span data-stu-id="12991-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="12991-214">[Usare l'API Graph di Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) per recuperare informazioni aggiuntive sull'utente, ad esempio l'appartenenza al gruppo, dal tenant Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="12991-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="12991-215">[Proteggere un'API web ASP.NET Core usando Azure ad B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).</span><span class="sxs-lookup"><span data-stu-id="12991-215">[Secure an ASP.NET Core web API using Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).</span></span>
* <span data-ttu-id="12991-216">[Chiamare un'API web .NET da un'app web .NET usando Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="12991-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>