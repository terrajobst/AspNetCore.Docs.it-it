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
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Supporto in Europa generale Data Protection regolamento (PILR) in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core fornisce le API e modelli per soddisfare alcuni del [Europa generale Data Protection regolamento (PILR)](https://www.eugdpr.org/) requisiti:

* I modelli di progetto includono punti di estensione e markup sottoposto a stub, che è possibile sostituire con la privacy e criteri di utilizzo di cookie.
* Una funzionalità di consenso cookie consente di chiedere (e tenere traccia) consenso dagli utenti per l'archiviazione di informazioni personali. Se un utente non ha fornito il consenso alla raccolta di dati e l'app è impostato con [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) a `true`, i cookie non essenziali non vengono ritrasmesse al browser.
* I cookie possono essere contrassegnati come essenziali. I cookie essenziali vengono inviati al browser anche quando l'utente non ha accettato le condizioni e la registrazione è disabilitata.
* [I cookie di sessione e TempData](#tempdata) non sono attivi quando viene disabilitato il rilevamento.
* Il [gestire identità](#pd) pagina fornisce un collegamento per scaricare ed eliminare i dati utente.

Il [app di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) consente di testare la maggior parte delle API aggiunte ai modelli ASP.NET Core 2.1 e punti di estensione PILR. Vedere la [Leggimi](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file per testare le istruzioni.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Supporto di ASP.NET Core PILR nel codice modello generato

Pagine Razor e MVC i progetti creati con i modelli di progetto includono il supporto PILR seguente:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sono impostate `Startup`.
* Il *_CookieConsentPartial.cshtml* [visualizzazione parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* Il *Pages/Privacy.cshtml* oppure *Home/Privacy.cshtml* visualizzazione fornisce una pagina dettagli sulla privacy del sito. Il *_CookieConsentPartial.cshtml* file genera un collegamento alla pagina sulla privacy.
* Per le applicazioni create con singoli account utente, la pagina di gestione vengono forniti i collegamenti per scaricare ed eliminare [dati personali dell'utente](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions e UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) vengono inizializzate le `Startup` classe `ConfigureServices` metodo:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) viene chiamato il `Startup` classe `Configure` metodo:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>Visualizzazione parziale _CookieConsentPartial.cshtml

Il *_CookieConsentPartial.cshtml* visualizzazione parziale:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Questo parziale:

* Ottiene lo stato di rilevamento per l'utente. Se l'applicazione è configurato per richiedere il consenso che l'utente deve acconsentire prima che sia possibile rilevare i cookie. Se necessario, il colore di consenso cookie è fisso sopra la barra di navigazione creata nel *Pages/Shared/_Layout.cshtml* file.
* Fornisce un elemento HTML `<p>` elemento per riepilogare la privacy e cookie utilizzare criteri.
* Fornisce un collegamento al *Pages/Privacy.cshtml* in cui è possibile dettaglio informativa sulla privacy del sito.

## <a name="essential-cookies"></a>Cookie essenziali

Se non è stato assegnato il consenso, solo i cookie contrassegnati essenziali vengono inviati al browser. Il codice seguente effettua un cookie essenziali:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>I cookie di stato sessione e provider TempData non sono essenziali

Il [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie non è essenziale. Se il rilevamento è disabilitato, il provider Tempdata non funziona. Per abilitare il provider Tempdata quando rilevamento è disattivato, contrassegnare il cookie TempData come essenziale `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Lo stato della sessione](xref:fundamentals/app-state) i cookie non sono essenziali. Lo stato della sessione non funziona quando è disabilitato il rilevamento.

<a name="pd"></a>

## <a name="personal-data"></a>Dati personali

Le applicazioni ASP.NET Core create con singoli account utente è incluso codice per scaricare ed eliminare i dati personali.

Selezionare il nome utente e quindi selezionare **i dati personali**:

![Gestire pagina dati personali](gdpr/_static/pd.png)

Note:

* Per generare il `Account/Manage` codice, vedere [identità Scaffold](xref:security/authentication/scaffold-identity).
* Eliminare e scaricare solo l'impatto i dati di identità predefinito. App i dati utente personalizzate create deve essere esteso per delete e scaricare i dati utente personalizzata. Problema di GitHub [come l'aggiunta/eliminazione di dati dell'utente personalizzati per identità](https://github.com/aspnet/Docs/issues/6226) tiene traccia di un articolo proposto sulla creazione personalizzata e l'eliminazione o il download di dati dell'utente personalizzati. Se si desidera vedere tale argomento priorità, lasciare un miniature la reazione nel problema.
* Salvare i token per l'utente che vengono archiviati nella tabella di database di identità `AspNetUserTokens` vengono eliminati quando l'utente viene eliminato tramite il comportamento di eliminazione a catena a causa di [chiave esterna](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Crittografia inattivi

Alcuni database e i meccanismi di archiviazione consentano la crittografia quando sono inattivi. Crittografia:

* Consente di crittografare automaticamente i dati archiviati.
* Esegue la crittografia senza configurazione, la programmazione o eseguire altre operazioni relative al software che accede ai dati.
* È l'opzione più semplice e sicuro.
* Consente il database di gestire le chiavi e la crittografia.

Ad esempio:

* Microsoft SQL e SQL di Azure forniscono [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).
* [SQL Azure consente di crittografare il database per impostazione predefinita](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure BLOB, file, tabella e coda di archiviazione sono crittografati per impostazione predefinita](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Per i database che non forniscono la crittografia incorporata nell'area di archiviazione è possibile usare crittografia del disco per fornire la stessa protezione. Ad esempio:

* [BitLocker per windows server](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
