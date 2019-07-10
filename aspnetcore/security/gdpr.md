---
title: Supporto di General Data Protection Regulation (GDPR) in ASP.NET Core
author: rick-anderson
description: Informazioni su come accedere ai punti di estensione al GDPR in un'app web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724575"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="b08a0-103">Supporto dell'Unione europea protezione il regolamento GDPR (General Data) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b08a0-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="b08a0-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b08a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b08a0-105">ASP.NET Core offre API e i modelli per soddisfare alcune delle [EU protezione il regolamento GDPR (General Data)](https://www.eugdpr.org/) requisiti:</span><span class="sxs-lookup"><span data-stu-id="b08a0-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="b08a0-106">I modelli di progetto includono punti di estensione e sottoposto a stub il markup che è possibile sostituire con la privacy e i criteri di utilizzo di cookie.</span><span class="sxs-lookup"><span data-stu-id="b08a0-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="b08a0-107">Il *Pages/Privacy.cshtml* pagina oppure *Views/Home/Privacy.cshtml* Vista fornisce una pagina di dettagli sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="b08a0-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="b08a0-108">Per abilitare la funzionalità di consenso di cookie predefinito come quello disponibile nei modelli di ASP.NET Core 2.2 in un'app di modello generata di ASP.NET Core 3.0:</span><span class="sxs-lookup"><span data-stu-id="b08a0-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="b08a0-109">Aggiungere [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) troppo `Startup.ConfigureServices` e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) a `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b08a0-109">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) too `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="b08a0-110">Aggiungere cookie consenso parziale per il *layout. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="b08a0-110">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="b08a0-111">Aggiungere il  *\_CookieConsentPartial.cshtml* file al progetto:</span><span class="sxs-lookup"><span data-stu-id="b08a0-111">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="b08a0-112">Selezionare la versione di ASP.NET Core 2.2 di questo articolo per conoscere la funzionalità di consenso di cookie.</span><span class="sxs-lookup"><span data-stu-id="b08a0-112">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="b08a0-113">I modelli di progetto includono punti di estensione e sottoposto a stub il markup che è possibile sostituire con la privacy e i criteri di utilizzo di cookie.</span><span class="sxs-lookup"><span data-stu-id="b08a0-113">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="b08a0-114">Una funzionalità di consenso cookie consente richiedere (e tenere traccia) il consenso da parte degli utenti per l'archiviazione dei dati personali.</span><span class="sxs-lookup"><span data-stu-id="b08a0-114">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="b08a0-115">Se un utente non ha fornito il consenso alla raccolta di dati e l'applicazione dispone [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) impostato su `true`, i cookie non essenziali non vengono inviati al browser.</span><span class="sxs-lookup"><span data-stu-id="b08a0-115">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="b08a0-116">I cookie possono essere contrassegnati come essenziali.</span><span class="sxs-lookup"><span data-stu-id="b08a0-116">Cookies can be marked as essential.</span></span> <span data-ttu-id="b08a0-117">Essenziali e vengono inviati al browser, anche quando l'utente non ha fornito il consenso e la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="b08a0-117">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="b08a0-118">[I cookie TempData e Session](#tempdata) non sono funzionali durante la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="b08a0-118">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="b08a0-119">Il [gestire identità](#pd) pagina fornisce un collegamento per scaricare ed eliminare i dati dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b08a0-119">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="b08a0-120">Il [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) consente testare la maggior parte dei punti di estensione al GDPR e aggiunte le API per i modelli ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="b08a0-120">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="b08a0-121">Vedere le [Leggimi](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file per testare le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="b08a0-121">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="b08a0-122">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b08a0-122">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="b08a0-123">ASP.NET Core GDPR supportano nel codice modello generato</span><span class="sxs-lookup"><span data-stu-id="b08a0-123">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="b08a0-124">Razor Pages ed MVC i progetti creati con i modelli di progetto includono il supporto al GDPR:</span><span class="sxs-lookup"><span data-stu-id="b08a0-124">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="b08a0-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) vengono impostate `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="b08a0-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="b08a0-126">Il  *\_CookieConsentPartial.cshtml* [visualizzazione parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b08a0-126">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="b08a0-127">Un' **Accept** pulsante è incluso in questo file.</span><span class="sxs-lookup"><span data-stu-id="b08a0-127">An **Accept** button is included in this file.</span></span> <span data-ttu-id="b08a0-128">Quando l'utente sceglie il **Accept** pulsante, il consenso per archiviare i cookie viene fornito.</span><span class="sxs-lookup"><span data-stu-id="b08a0-128">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="b08a0-129">Il *Pages/Privacy.cshtml* pagina oppure *Views/Home/Privacy.cshtml* Vista fornisce una pagina di dettagli sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="b08a0-129">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="b08a0-130">Il  *\_CookieConsentPartial.cshtml* file genera un collegamento alla pagina della Privacy.</span><span class="sxs-lookup"><span data-stu-id="b08a0-130">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="b08a0-131">Per le app create con singoli account utente, la pagina di gestione vengono forniti collegamenti per scaricare ed eliminare [i dati personali dell'utente](#pd).</span><span class="sxs-lookup"><span data-stu-id="b08a0-131">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="b08a0-132">CookiePolicyOptions e UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="b08a0-132">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="b08a0-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) vengono inizializzate `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b08a0-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="b08a0-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) viene chiamato `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b08a0-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="b08a0-135">\_Visualizzazione parziale CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="b08a0-135">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="b08a0-136">Il  *\_CookieConsentPartial.cshtml* visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="b08a0-136">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="b08a0-137">Questo parziale:</span><span class="sxs-lookup"><span data-stu-id="b08a0-137">This partial:</span></span>

* <span data-ttu-id="b08a0-138">Ottiene lo stato di rilevamento per l'utente.</span><span class="sxs-lookup"><span data-stu-id="b08a0-138">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="b08a0-139">Se l'app è configurato per richiedere il consenso, l'utente deve acconsentire prima che sia possibile rilevare i cookie.</span><span class="sxs-lookup"><span data-stu-id="b08a0-139">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="b08a0-140">Se è necessario il consenso, il cookie consenso dei pannelli è fissato nella parte superiore della barra di spostamento creata per il  *\_layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="b08a0-140">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="b08a0-141">Fornisce un elemento HTML `<p>` elemento per riepilogare la privacy e cookie utilizzare criteri.</span><span class="sxs-lookup"><span data-stu-id="b08a0-141">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="b08a0-142">Fornisce un collegamento alla pagina Privacy o alla vista in cui è possibile descrivere in dettaglio informativa sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="b08a0-142">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="b08a0-143">Cookie essenziali</span><span class="sxs-lookup"><span data-stu-id="b08a0-143">Essential cookies</span></span>

<span data-ttu-id="b08a0-144">Se il consenso per archiviare i cookie non è stata fornita, solo i cookie contrassegnati essenziali vengono inviati al browser.</span><span class="sxs-lookup"><span data-stu-id="b08a0-144">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="b08a0-145">Il codice seguente effettua un cookie essenziali:</span><span class="sxs-lookup"><span data-stu-id="b08a0-145">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="b08a0-146">Cookie di stato sessione e provider TempData non sono indispensabili</span><span class="sxs-lookup"><span data-stu-id="b08a0-146">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="b08a0-147">Il [provider TempData](xref:fundamentals/app-state#tempdata) cookie non è essenziale.</span><span class="sxs-lookup"><span data-stu-id="b08a0-147">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="b08a0-148">Se la verifica è disabilitata, il provider TempData non è funzionale.</span><span class="sxs-lookup"><span data-stu-id="b08a0-148">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="b08a0-149">Per abilitare il provider TempData quando la verifica è disabilitata, contrassegnare il cookie TempData come essenziale `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b08a0-149">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="b08a0-150">[Lo stato della sessione](xref:fundamentals/app-state) i cookie non sono essenziali.</span><span class="sxs-lookup"><span data-stu-id="b08a0-150">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="b08a0-151">Lo stato della sessione non è attiva quando la verifica è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="b08a0-151">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="b08a0-152">Il codice seguente rende essenziale i cookie di sessione:</span><span class="sxs-lookup"><span data-stu-id="b08a0-152">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="b08a0-153">Dati personali</span><span class="sxs-lookup"><span data-stu-id="b08a0-153">Personal data</span></span>

<span data-ttu-id="b08a0-154">Le app ASP.NET Core create con singoli account utente è incluso codice per scaricare ed eliminare i dati personali.</span><span class="sxs-lookup"><span data-stu-id="b08a0-154">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="b08a0-155">Selezionare il nome utente e quindi selezionare **i dati personali**:</span><span class="sxs-lookup"><span data-stu-id="b08a0-155">Select the user name and then select **Personal data**:</span></span>

![Pagina dati personali di gestione](gdpr/_static/pd.png)

<span data-ttu-id="b08a0-157">Note:</span><span class="sxs-lookup"><span data-stu-id="b08a0-157">Notes:</span></span>

* <span data-ttu-id="b08a0-158">Per generare il `Account/Manage` il codice, vedere [Scaffold identità](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="b08a0-158">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="b08a0-159">Il **eliminare** e **scaricare** collegamenti solo agiscono sui dati di identità predefinito.</span><span class="sxs-lookup"><span data-stu-id="b08a0-159">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="b08a0-160">Le app che creano i dati utente personalizzato devono essere esteso per eliminare e scaricare i dati utente personalizzati.</span><span class="sxs-lookup"><span data-stu-id="b08a0-160">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="b08a0-161">Per altre informazioni, vedere [Add, scaricare ed elimina i dati utente personalizzati per Identity](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="b08a0-161">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="b08a0-162">Salvare i token per l'utente che vengono archiviati nella tabella di database di identità `AspNetUserTokens` vengono eliminati quando l'utente viene eliminato tramite il comportamento di eliminazione a catena a causa dell'errore il [chiave esterna](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="b08a0-162">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="b08a0-163">[Autenticazione basata su provider esterni](xref:security/authentication/social/index), ad esempio Facebook e Google, non è disponibile, prima che venga accettato i criteri per i cookie.</span><span class="sxs-lookup"><span data-stu-id="b08a0-163">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="b08a0-164">Crittografia dei dati inattivi</span><span class="sxs-lookup"><span data-stu-id="b08a0-164">Encryption at rest</span></span>

<span data-ttu-id="b08a0-165">Alcuni database e meccanismi di archiviazione consentono la crittografia dei dati inattivi.</span><span class="sxs-lookup"><span data-stu-id="b08a0-165">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="b08a0-166">Crittografia dei dati inattivi:</span><span class="sxs-lookup"><span data-stu-id="b08a0-166">Encryption at rest:</span></span>

* <span data-ttu-id="b08a0-167">Consente di crittografare automaticamente i dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="b08a0-167">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="b08a0-168">Esegue la crittografia senza configurazione, la programmazione o eseguire altre operazioni relative al software che accede ai dati.</span><span class="sxs-lookup"><span data-stu-id="b08a0-168">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="b08a0-169">È l'opzione più semplice e più sicuro.</span><span class="sxs-lookup"><span data-stu-id="b08a0-169">Is the easiest and safest option.</span></span>
* <span data-ttu-id="b08a0-170">Consente al database di gestione delle chiavi e crittografia.</span><span class="sxs-lookup"><span data-stu-id="b08a0-170">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="b08a0-171">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b08a0-171">For example:</span></span>

* <span data-ttu-id="b08a0-172">Microsoft SQL e SQL di Azure forniscono [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="b08a0-172">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="b08a0-173">SQL Azure consente di crittografare il database per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="b08a0-173">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="b08a0-174">[Azure BLOB, file, tabella e coda di archiviazione vengono crittografati per impostazione predefinita](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="b08a0-174">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="b08a0-175">Per i database che non offrono la crittografia incorporata quando sono inattivi, è possibile usare crittografia dischi per fornire la protezione stessa.</span><span class="sxs-lookup"><span data-stu-id="b08a0-175">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="b08a0-176">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b08a0-176">For example:</span></span>

* [<span data-ttu-id="b08a0-177">BitLocker per Windows Server</span><span class="sxs-lookup"><span data-stu-id="b08a0-177">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="b08a0-178">Linux:</span><span class="sxs-lookup"><span data-stu-id="b08a0-178">Linux:</span></span>
  * [<span data-ttu-id="b08a0-179">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="b08a0-179">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="b08a0-180">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="b08a0-180">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b08a0-181">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b08a0-181">Additional resources</span></span>

* [<span data-ttu-id="b08a0-182">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="b08a0-182">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
