---
title: Supporto di protezione regolamento (PILR) dei dati generali in ASP.NET Core
author: rick-anderson
description: Viene illustrato come accedere ai punti di estensione PILR in un ASP.NET Core app web.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688627"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="bebf1-103">Supporto in Europa generale Data Protection regolamento (PILR) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bebf1-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="bebf1-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bebf1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bebf1-105">ASP.NET Core fornisce le API e modelli per soddisfare alcuni del [Europa generale Data Protection regolamento (PILR)](https://www.eugdpr.org/) requisiti:</span><span class="sxs-lookup"><span data-stu-id="bebf1-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="bebf1-106">I modelli di progetto includono punti di estensione e markup sottoposto a stub, che è possibile sostituire con la privacy e criteri di utilizzo di cookie.</span><span class="sxs-lookup"><span data-stu-id="bebf1-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="bebf1-107">Una funzionalità di consenso cookie consente di chiedere (e tenere traccia) consenso dagli utenti per l'archiviazione di informazioni personali.</span><span class="sxs-lookup"><span data-stu-id="bebf1-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="bebf1-108">Se un utente non ha fornito il consenso alla raccolta di dati e l'app è impostato con [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) a `true`, i cookie non essenziali non vengono ritrasmesse al browser.</span><span class="sxs-lookup"><span data-stu-id="bebf1-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="bebf1-109">I cookie possono essere contrassegnati come essenziali.</span><span class="sxs-lookup"><span data-stu-id="bebf1-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="bebf1-110">I cookie essenziali vengono inviati al browser anche quando l'utente non ha accettato le condizioni e la registrazione è disabilitata.</span><span class="sxs-lookup"><span data-stu-id="bebf1-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="bebf1-111">[I cookie di sessione e TempData](#tempdata) non sono attivi quando viene disabilitato il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="bebf1-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="bebf1-112">Il [gestire identità](#pd) pagina fornisce un collegamento per scaricare ed eliminare i dati utente.</span><span class="sxs-lookup"><span data-stu-id="bebf1-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="bebf1-113">Il [app di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) consente di testare la maggior parte delle API aggiunte ai modelli ASP.NET Core 2.1 e punti di estensione PILR.</span><span class="sxs-lookup"><span data-stu-id="bebf1-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="bebf1-114">Vedere la [Leggimi](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file per testare le istruzioni.</span><span class="sxs-lookup"><span data-stu-id="bebf1-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="bebf1-115">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bebf1-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="bebf1-116">Supporto di ASP.NET Core PILR nel codice modello generato</span><span class="sxs-lookup"><span data-stu-id="bebf1-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="bebf1-117">Pagine Razor e MVC i progetti creati con i modelli di progetto includono il supporto PILR seguente:</span><span class="sxs-lookup"><span data-stu-id="bebf1-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="bebf1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sono impostate `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bebf1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="bebf1-119">Il *_CookieConsentPartial.cshtml* [visualizzazione parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="bebf1-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="bebf1-120">Il *Pages/Privacy.cshtml* oppure *Home/Privacy.cshtml* visualizzazione fornisce una pagina dettagli sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="bebf1-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="bebf1-121">Il *_CookieConsentPartial.cshtml* file genera un collegamento alla pagina sulla privacy.</span><span class="sxs-lookup"><span data-stu-id="bebf1-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="bebf1-122">Per le applicazioni create con singoli account utente, la pagina di gestione vengono forniti i collegamenti per scaricare ed eliminare [dati personali dell'utente](#pd).</span><span class="sxs-lookup"><span data-stu-id="bebf1-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="bebf1-123">CookiePolicyOptions e UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="bebf1-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="bebf1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) vengono inizializzate le `Startup` classe `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="bebf1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="bebf1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) viene chiamato il `Startup` classe `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="bebf1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="bebf1-126">Visualizzazione parziale _CookieConsentPartial.cshtml</span><span class="sxs-lookup"><span data-stu-id="bebf1-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="bebf1-127">Il *_CookieConsentPartial.cshtml* visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="bebf1-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="bebf1-128">Questo parziale:</span><span class="sxs-lookup"><span data-stu-id="bebf1-128">This partial:</span></span>

* <span data-ttu-id="bebf1-129">Ottiene lo stato di rilevamento per l'utente.</span><span class="sxs-lookup"><span data-stu-id="bebf1-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="bebf1-130">Se l'applicazione è configurato per richiedere il consenso che l'utente deve acconsentire prima che sia possibile rilevare i cookie.</span><span class="sxs-lookup"><span data-stu-id="bebf1-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="bebf1-131">Se necessario, il colore di consenso cookie è fisso sopra la barra di navigazione creata nel *Pages/Shared/_Layout.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="bebf1-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="bebf1-132">Fornisce un elemento HTML `<p>` elemento per riepilogare la privacy e cookie utilizzare criteri.</span><span class="sxs-lookup"><span data-stu-id="bebf1-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="bebf1-133">Fornisce un collegamento al *Pages/Privacy.cshtml* in cui è possibile dettaglio informativa sulla privacy del sito.</span><span class="sxs-lookup"><span data-stu-id="bebf1-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="bebf1-134">Cookie essenziali</span><span class="sxs-lookup"><span data-stu-id="bebf1-134">Essential cookies</span></span>

<span data-ttu-id="bebf1-135">Se non è stato assegnato il consenso, solo i cookie contrassegnati essenziali vengono inviati al browser.</span><span class="sxs-lookup"><span data-stu-id="bebf1-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="bebf1-136">Il codice seguente effettua un cookie essenziali:</span><span class="sxs-lookup"><span data-stu-id="bebf1-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="bebf1-137">I cookie di stato sessione e provider TempData non sono essenziali</span><span class="sxs-lookup"><span data-stu-id="bebf1-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="bebf1-138">Il [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie non è essenziale.</span><span class="sxs-lookup"><span data-stu-id="bebf1-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="bebf1-139">Se il rilevamento è disabilitato, il provider Tempdata non funziona.</span><span class="sxs-lookup"><span data-stu-id="bebf1-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="bebf1-140">Per abilitare il provider Tempdata quando rilevamento è disattivato, contrassegnare il cookie TempData come essenziale `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bebf1-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="bebf1-141">[Lo stato della sessione](xref:fundamentals/app-state) i cookie non sono essenziali.</span><span class="sxs-lookup"><span data-stu-id="bebf1-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="bebf1-142">Lo stato della sessione non funziona quando è disabilitato il rilevamento.</span><span class="sxs-lookup"><span data-stu-id="bebf1-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="bebf1-143">Dati personali</span><span class="sxs-lookup"><span data-stu-id="bebf1-143">Personal data</span></span>

<span data-ttu-id="bebf1-144">Le applicazioni ASP.NET Core create con singoli account utente è incluso codice per scaricare ed eliminare i dati personali.</span><span class="sxs-lookup"><span data-stu-id="bebf1-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="bebf1-145">Selezionare il nome utente e quindi selezionare **i dati personali**:</span><span class="sxs-lookup"><span data-stu-id="bebf1-145">Select the user name and then select **Personal data**:</span></span>

![Gestire pagina dati personali](gdpr/_static/pd.png)

<span data-ttu-id="bebf1-147">Note:</span><span class="sxs-lookup"><span data-stu-id="bebf1-147">Notes:</span></span>

* <span data-ttu-id="bebf1-148">Per generare il `Account/Manage` codice, vedere [identità Scaffold](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="bebf1-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="bebf1-149">Eliminare e scaricare solo l'impatto i dati di identità predefinito.</span><span class="sxs-lookup"><span data-stu-id="bebf1-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="bebf1-150">App i dati utente personalizzate create deve essere esteso per delete e scaricare i dati utente personalizzata.</span><span class="sxs-lookup"><span data-stu-id="bebf1-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="bebf1-151">Problema di GitHub [come l'aggiunta/eliminazione di dati dell'utente personalizzati per identità](https://github.com/aspnet/Docs/issues/6226) tiene traccia di un articolo proposto sulla creazione personalizzata e l'eliminazione o il download di dati dell'utente personalizzati.</span><span class="sxs-lookup"><span data-stu-id="bebf1-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="bebf1-152">Se si desidera vedere tale argomento priorità, lasciare un miniature la reazione nel problema.</span><span class="sxs-lookup"><span data-stu-id="bebf1-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="bebf1-153">Salvare i token per l'utente che vengono archiviati nella tabella di database di identità `AspNetUserTokens` vengono eliminati quando l'utente viene eliminato tramite il comportamento di eliminazione a catena a causa di [chiave esterna](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="bebf1-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="bebf1-154">Crittografia inattivi</span><span class="sxs-lookup"><span data-stu-id="bebf1-154">Encryption at rest</span></span>

<span data-ttu-id="bebf1-155">Alcuni database e i meccanismi di archiviazione consentano la crittografia quando sono inattivi.</span><span class="sxs-lookup"><span data-stu-id="bebf1-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="bebf1-156">Crittografia:</span><span class="sxs-lookup"><span data-stu-id="bebf1-156">Encryption at rest:</span></span>

* <span data-ttu-id="bebf1-157">Consente di crittografare automaticamente i dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="bebf1-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="bebf1-158">Esegue la crittografia senza configurazione, la programmazione o eseguire altre operazioni relative al software che accede ai dati.</span><span class="sxs-lookup"><span data-stu-id="bebf1-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="bebf1-159">È l'opzione più semplice e sicuro.</span><span class="sxs-lookup"><span data-stu-id="bebf1-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="bebf1-160">Consente il database di gestire le chiavi e la crittografia.</span><span class="sxs-lookup"><span data-stu-id="bebf1-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="bebf1-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bebf1-161">For example:</span></span>

* <span data-ttu-id="bebf1-162">Microsoft SQL e SQL di Azure forniscono [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="bebf1-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="bebf1-163">SQL Azure consente di crittografare il database per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="bebf1-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="bebf1-164">[Azure BLOB, file, tabella e coda di archiviazione sono crittografati per impostazione predefinita](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="bebf1-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="bebf1-165">Per i database che non forniscono la crittografia incorporata nell'area di archiviazione è possibile usare crittografia del disco per fornire la stessa protezione.</span><span class="sxs-lookup"><span data-stu-id="bebf1-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="bebf1-166">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bebf1-166">For example:</span></span>

* [<span data-ttu-id="bebf1-167">BitLocker per windows server</span><span class="sxs-lookup"><span data-stu-id="bebf1-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="bebf1-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="bebf1-168">Linux:</span></span>
  * [<span data-ttu-id="bebf1-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="bebf1-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="bebf1-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="bebf1-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bebf1-171">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bebf1-171">Additional Resources</span></span>

* [<span data-ttu-id="bebf1-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="bebf1-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)