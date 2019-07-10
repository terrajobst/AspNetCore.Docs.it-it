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
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Supporto dell'Unione europea protezione il regolamento GDPR (General Data) in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core offre API e i modelli per soddisfare alcune delle [EU protezione il regolamento GDPR (General Data)](https://www.eugdpr.org/) requisiti:

::: moniker range=">= aspnetcore-3.0"

* I modelli di progetto includono punti di estensione e sottoposto a stub il markup che è possibile sostituire con la privacy e i criteri di utilizzo di cookie.
* Il *Pages/Privacy.cshtml* pagina oppure *Views/Home/Privacy.cshtml* Vista fornisce una pagina di dettagli sulla privacy del sito.

Per abilitare la funzionalità di consenso di cookie predefinito come quello disponibile nei modelli di ASP.NET Core 2.2 in un'app di modello generata di ASP.NET Core 3.0:

* Aggiungere [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) troppo `Startup.ConfigureServices` e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) a `Startup.Configure`:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Aggiungere cookie consenso parziale per il *layout. cshtml* file:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Aggiungere il  *\_CookieConsentPartial.cshtml* file al progetto:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Selezionare la versione di ASP.NET Core 2.2 di questo articolo per conoscere la funzionalità di consenso di cookie.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* I modelli di progetto includono punti di estensione e sottoposto a stub il markup che è possibile sostituire con la privacy e i criteri di utilizzo di cookie.
* Una funzionalità di consenso cookie consente richiedere (e tenere traccia) il consenso da parte degli utenti per l'archiviazione dei dati personali. Se un utente non ha fornito il consenso alla raccolta di dati e l'applicazione dispone [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) impostato su `true`, i cookie non essenziali non vengono inviati al browser.
* I cookie possono essere contrassegnati come essenziali. Essenziali e vengono inviati al browser, anche quando l'utente non ha fornito il consenso e la verifica è disabilitata.
* [I cookie TempData e Session](#tempdata) non sono funzionali durante la verifica è disabilitata.
* Il [gestire identità](#pd) pagina fornisce un collegamento per scaricare ed eliminare i dati dell'utente.

Il [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) consente testare la maggior parte dei punti di estensione al GDPR e aggiunte le API per i modelli ASP.NET Core 2.1. Vedere le [Leggimi](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file per testare le istruzioni.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core GDPR supportano nel codice modello generato

Razor Pages ed MVC i progetti creati con i modelli di progetto includono il supporto al GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) e [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) vengono impostate `Startup` classe.
* Il  *\_CookieConsentPartial.cshtml* [visualizzazione parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Un' **Accept** pulsante è incluso in questo file. Quando l'utente sceglie il **Accept** pulsante, il consenso per archiviare i cookie viene fornito.
* Il *Pages/Privacy.cshtml* pagina oppure *Views/Home/Privacy.cshtml* Vista fornisce una pagina di dettagli sulla privacy del sito. Il  *\_CookieConsentPartial.cshtml* file genera un collegamento alla pagina della Privacy.
* Per le app create con singoli account utente, la pagina di gestione vengono forniti collegamenti per scaricare ed eliminare [i dati personali dell'utente](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions e UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) vengono inizializzate `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) viene chiamato `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_Visualizzazione parziale CookieConsentPartial.cshtml

Il  *\_CookieConsentPartial.cshtml* visualizzazione parziale:

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Questo parziale:

* Ottiene lo stato di rilevamento per l'utente. Se l'app è configurato per richiedere il consenso, l'utente deve acconsentire prima che sia possibile rilevare i cookie. Se è necessario il consenso, il cookie consenso dei pannelli è fissato nella parte superiore della barra di spostamento creata per il  *\_layout. cshtml* file.
* Fornisce un elemento HTML `<p>` elemento per riepilogare la privacy e cookie utilizzare criteri.
* Fornisce un collegamento alla pagina Privacy o alla vista in cui è possibile descrivere in dettaglio informativa sulla privacy del sito.

## <a name="essential-cookies"></a>Cookie essenziali

Se il consenso per archiviare i cookie non è stata fornita, solo i cookie contrassegnati essenziali vengono inviati al browser. Il codice seguente effettua un cookie essenziali:

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>Cookie di stato sessione e provider TempData non sono indispensabili

Il [provider TempData](xref:fundamentals/app-state#tempdata) cookie non è essenziale. Se la verifica è disabilitata, il provider TempData non è funzionale. Per abilitare il provider TempData quando la verifica è disabilitata, contrassegnare il cookie TempData come essenziale `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[Lo stato della sessione](xref:fundamentals/app-state) i cookie non sono essenziali. Lo stato della sessione non è attiva quando la verifica è disabilitata. Il codice seguente rende essenziale i cookie di sessione:

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Dati personali

Le app ASP.NET Core create con singoli account utente è incluso codice per scaricare ed eliminare i dati personali.

Selezionare il nome utente e quindi selezionare **i dati personali**:

![Pagina dati personali di gestione](gdpr/_static/pd.png)

Note:

* Per generare il `Account/Manage` il codice, vedere [Scaffold identità](xref:security/authentication/scaffold-identity).
* Il **eliminare** e **scaricare** collegamenti solo agiscono sui dati di identità predefinito. Le app che creano i dati utente personalizzato devono essere esteso per eliminare e scaricare i dati utente personalizzati. Per altre informazioni, vedere [Add, scaricare ed elimina i dati utente personalizzati per Identity](xref:security/authentication/add-user-data).
* Salvare i token per l'utente che vengono archiviati nella tabella di database di identità `AspNetUserTokens` vengono eliminati quando l'utente viene eliminato tramite il comportamento di eliminazione a catena a causa dell'errore il [chiave esterna](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Autenticazione basata su provider esterni](xref:security/authentication/social/index), ad esempio Facebook e Google, non è disponibile, prima che venga accettato i criteri per i cookie.

::: moniker-end

## <a name="encryption-at-rest"></a>Crittografia dei dati inattivi

Alcuni database e meccanismi di archiviazione consentono la crittografia dei dati inattivi. Crittografia dei dati inattivi:

* Consente di crittografare automaticamente i dati archiviati.
* Esegue la crittografia senza configurazione, la programmazione o eseguire altre operazioni relative al software che accede ai dati.
* È l'opzione più semplice e più sicuro.
* Consente al database di gestione delle chiavi e crittografia.

Ad esempio:

* Microsoft SQL e SQL di Azure forniscono [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure consente di crittografare il database per impostazione predefinita](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure BLOB, file, tabella e coda di archiviazione vengono crittografati per impostazione predefinita](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Per i database che non offrono la crittografia incorporata quando sono inattivi, è possibile usare crittografia dischi per fornire la protezione stessa. Ad esempio:

* [BitLocker per Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
