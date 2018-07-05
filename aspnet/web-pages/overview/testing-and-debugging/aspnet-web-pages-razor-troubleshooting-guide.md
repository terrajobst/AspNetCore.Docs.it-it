---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) Troubleshooting Guide | Microsoft Docs
author: tfitzmac
description: Questo articolo vengono descritti i problemi che possono verificarsi quando si lavora con ASP.NET Web Pages (Razor) e alcune soluzioni suggerite. Versioni del software ASP.NET Web Pag...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 48c0380af32038a1d916d1b99f7de0f918d1a74e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373541"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="262f6-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span><span class="sxs-lookup"><span data-stu-id="262f6-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="262f6-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="262f6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="262f6-106">Questo articolo vengono descritti i problemi che possono verificarsi quando si lavora con ASP.NET Web Pages (Razor) e alcune soluzioni suggerite.</span><span class="sxs-lookup"><span data-stu-id="262f6-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="262f6-107">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="262f6-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="262f6-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="262f6-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="262f6-109">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e le pagine Web ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="262f6-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="262f6-110">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="262f6-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="262f6-111">Problemi con l'esecuzione di pagine</span><span class="sxs-lookup"><span data-stu-id="262f6-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="262f6-112">Problemi con il codice Razor</span><span class="sxs-lookup"><span data-stu-id="262f6-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="262f6-113">Problemi relativi alla sicurezza e appartenenza</span><span class="sxs-lookup"><span data-stu-id="262f6-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="262f6-114">Problemi con l'invio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="262f6-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="262f6-115">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="262f6-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="262f6-116">Per domande generali, vedere [ASP.NET Web Pages (Razor) domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="262f6-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="262f6-117">Problemi con l'esecuzione di pagine</span><span class="sxs-lookup"><span data-stu-id="262f6-117">Issues with Running Pages</span></span>

<span data-ttu-id="262f6-118">Un'ampia gamma di problemi possibile impedire *. cshtml* e *vbhtml* pagine funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="262f6-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="262f6-119">In questa sezione elenca i messaggi di errore comuni e cause possibili.</span><span class="sxs-lookup"><span data-stu-id="262f6-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="262f6-120">Errore HTTP 403 - accesso negato: Accesso negato</span><span class="sxs-lookup"><span data-stu-id="262f6-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="262f6-121">*Non è autorizzato a visualizzare la directory o la pagina utilizzando le credenziali fornite.*</span><span class="sxs-lookup"><span data-stu-id="262f6-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="262f6-122">Questo errore può verificarsi se il server non è in esecuzione la versione corretta di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="262f6-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="262f6-123">Assicurarsi che il computer che esegue il server (locale o remota) abbia almeno .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="262f6-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="262f6-124">Assicurarsi anche che l'applicazione stessa è configurato per eseguire la versione corretta.</span><span class="sxs-lookup"><span data-stu-id="262f6-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="262f6-125">Se si riscontra questo problema in locale mentre si lavora in WebMatrix, fare clic sui **Site** dell'area di lavoro e nella visualizzazione albero fare clic su **impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="262f6-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="262f6-126">Nel **Seleziona versione di .NET Framework** , selezionare dalla lista **.NET 4 (Integrated)**.</span><span class="sxs-lookup"><span data-stu-id="262f6-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="262f6-127">Se questa versione è già impostata, provare a eseguire WebMatrix come amministratore.</span><span class="sxs-lookup"><span data-stu-id="262f6-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="262f6-128">Assicurarsi che sia la radice del sito Web ad almeno uno *cshtml* file in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="262f6-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="262f6-129">Se viene visualizzato questo errore quando il server web sia in un server remoto, contattare l'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="262f6-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="262f6-130">Assicurarsi che il server di .NET Framework 4 o versione successiva installato.</span><span class="sxs-lookup"><span data-stu-id="262f6-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="262f6-131">Assicurarsi anche che l'applicazione sia in esecuzione in un pool di applicazioni configurato per usare tale versione di.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="262f6-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="262f6-132">Se si dispone di controllo del server, assicurarsi che è in esecuzione la versione corretta di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="262f6-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="262f6-133">È possibile anche provare a ripristinare l'installazione eseguendo il `aspnet_regiis -iru` comando.</span><span class="sxs-lookup"><span data-stu-id="262f6-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="262f6-134">(Ad esempio, se si installa IIS dopo l'installazione di .NET Framework, IIS sarà non essere configurato correttamente per eseguire le pagine ASP.NET.) Per altre informazioni, vedere [strumento di registrazione ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="262f6-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="262f6-135">Errore HTTP 403.14 - accesso negato</span><span class="sxs-lookup"><span data-stu-id="262f6-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="262f6-136">*Il server Web è configurato per non visualizzare il contenuto di questa directory.*</span><span class="sxs-lookup"><span data-stu-id="262f6-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="262f6-137">Questo errore può verificarsi se si richiede una risorsa protetta (come il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).</span><span class="sxs-lookup"><span data-stu-id="262f6-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="262f6-138">Errore HTTP 404.17 - non trovato</span><span class="sxs-lookup"><span data-stu-id="262f6-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="262f6-139">*Il contenuto richiesto sembra essere script e non verrà servito dal gestore di file statici.*</span><span class="sxs-lookup"><span data-stu-id="262f6-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="262f6-140">Questo errore può verificarsi se il server non è configurato correttamente per l'uso di .NET Framework 4 o versione successiva e pertanto non riconosce il codice in `@{ }` blocchi.</span><span class="sxs-lookup"><span data-stu-id="262f6-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="262f6-141">Vedere la descrizione in precedenza *HTTP Errore 403 - accesso negato: accesso negato*.</span><span class="sxs-lookup"><span data-stu-id="262f6-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="262f6-142">Errore HTTP 404.7 - non trovato</span><span class="sxs-lookup"><span data-stu-id="262f6-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="262f6-143">*Il modulo filtro delle richieste sono configurata per rifiutare l'estensione di file*</span><span class="sxs-lookup"><span data-stu-id="262f6-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="262f6-144">Questo errore può verificarsi se *. cshtml* oppure *vbhtml* estensioni sono state bloccate in modo esplicito nel server.</span><span class="sxs-lookup"><span data-stu-id="262f6-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="262f6-145">Un sintomo di questo problema è che funzionano URL quando non includono l'estensione, ma gli URL che includono *. cshtml* oppure *vbhtml* non funzionano.</span><span class="sxs-lookup"><span data-stu-id="262f6-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="262f6-146">Una soluzione possibile consiste nell'abilitare nuovamente le estensioni del sito *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="262f6-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="262f6-147">Nell'esempio seguente viene illustrato come abilitare il *cshtml* estensione.</span><span class="sxs-lookup"><span data-stu-id="262f6-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="262f6-148">Errore HTTP 404.8 - non trovato</span><span class="sxs-lookup"><span data-stu-id="262f6-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="262f6-149">*Il modulo filtro delle richieste sono configurata per rifiutare un percorso nell'URL che contiene una sezione hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="262f6-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="262f6-150">Questo errore può verificarsi se si richiede una risorsa protetta (come il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).</span><span class="sxs-lookup"><span data-stu-id="262f6-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="262f6-151">Questo tipo di pagina non è servito (errore Server nell'applicazione '/')</span><span class="sxs-lookup"><span data-stu-id="262f6-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="262f6-152">Vedere la descrizione in precedenza per HTTP 404.17 di errore.</span><span class="sxs-lookup"><span data-stu-id="262f6-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="262f6-153">Problemi con il codice Razor</span><span class="sxs-lookup"><span data-stu-id="262f6-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="262f6-154">Il nome '*classe*' non esiste nel contesto corrente</span><span class="sxs-lookup"><span data-stu-id="262f6-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="262f6-155">Un motivo viene visualizzato questo errore è spesso che `class` riferimenti a un file di supporto, ma l'helper non è installato.</span><span class="sxs-lookup"><span data-stu-id="262f6-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="262f6-156">Ad esempio, se si prova a usare un file di supporto, ma se non è stato installato il pacchetto da NuGet, verrà visualizzato questo errore.</span><span class="sxs-lookup"><span data-stu-id="262f6-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="262f6-157">Usa la raccolta di WebMatrix per trovare e installare il supporto.</span><span class="sxs-lookup"><span data-stu-id="262f6-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="262f6-158">Se l'helper è installato, ma la pagina ancora non lo riconosce, provare ad aggiungere aggiungere un `using` istruzione per il codice.</span><span class="sxs-lookup"><span data-stu-id="262f6-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="262f6-159">Nel `using` istruzione, lo spazio dei nomi che include il supporto di riferimento.</span><span class="sxs-lookup"><span data-stu-id="262f6-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="262f6-160">Ad esempio, gli helper di base che sono nel pacchetto di ASP.NET Web Helpers sono nel `System.Web.Helpers` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="262f6-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="262f6-161">Nella parte superiore della pagina in cui si desidera utilizzare l'helper, aggiungere questa riga:</span><span class="sxs-lookup"><span data-stu-id="262f6-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="262f6-162">Problemi relativi alla sicurezza e appartenenza</span><span class="sxs-lookup"><span data-stu-id="262f6-162">Issues with Security and Membership</span></span>

<span data-ttu-id="262f6-163">Se si usa il sistema di sicurezza incorporati (membership) in ASP.NET Web Pages (Razor), si potrebbero verificarsi i problemi seguenti.</span><span class="sxs-lookup"><span data-stu-id="262f6-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="262f6-164">Per chiamare questo metodo, la proprietà "Membership.Provider" deve essere un'istanza di "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="262f6-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="262f6-165">Questo errore può indicare che nessun `AspNetSqlMembershipProvider` classe è configurata.</span><span class="sxs-lookup"><span data-stu-id="262f6-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="262f6-166">(Un sintomo è che il sito funziona correttamente in locale, ma genera questo errore quando lo si pubblica per il server del provider di hosting). Una soluzione al problema consiste nell'abilitare in modo esplicito l'appartenenza semplice aggiungendo il codice seguente del sito *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="262f6-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="262f6-167">Problemi con l'invio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="262f6-167">Issues with Sending Email</span></span>

<span data-ttu-id="262f6-168">Problemi con l'invio di posta elettronica possono essere difficile eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="262f6-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="262f6-169">Un problema iniziale è possibile che non è possibile connettersi al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="262f6-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="262f6-170">Se la connessione ha esito positivo, ASP.NET invia il messaggio al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="262f6-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="262f6-171">Tuttavia, si possono verificare problemi con il messaggio stesso che impedisce l'invio al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="262f6-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="262f6-172">Se l'applicazione non correttamente Invia messaggio di posta elettronica, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="262f6-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="262f6-173">Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="262f6-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="262f6-174">Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a quel punto possibile `localhost`.</span><span class="sxs-lookup"><span data-stu-id="262f6-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="262f6-175">Questa situazione si verifica perché dopo aver pubblicato e il sito è in esecuzione nel server del provider, il server SMTP potrebbe essere locale dal punto di vista dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="262f6-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="262f6-176">Questa modifica nei nomi di server potrebbe indicare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="262f6-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="262f6-177">Il numero di porta in genere è 25.</span><span class="sxs-lookup"><span data-stu-id="262f6-177">The port number is usually 25.</span></span> <span data-ttu-id="262f6-178">Tuttavia, alcuni provider richiedono di usare la porta 587 o alcune porte.</span><span class="sxs-lookup"><span data-stu-id="262f6-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="262f6-179">Contattare il proprietario del server SMTP del numero di porta si aspettano da usare.</span><span class="sxs-lookup"><span data-stu-id="262f6-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="262f6-180">Assicurarsi di usare le credenziali corrette.</span><span class="sxs-lookup"><span data-stu-id="262f6-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="262f6-181">Se il sito è stata pubblicata in un provider di hosting, usare le credenziali che il provider ha segnalato in modo specifico sono per la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="262f6-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="262f6-182">Queste credenziali possono essere diverse da quelle usate per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="262f6-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="262f6-183">In alcuni casi non occorre credenziali affatto.</span><span class="sxs-lookup"><span data-stu-id="262f6-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="262f6-184">Se si invia tramite posta elettronica usando l'account personale, il provider di posta elettronica sappia già le proprie credenziali.</span><span class="sxs-lookup"><span data-stu-id="262f6-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="262f6-185">Dopo aver pubblicato, è necessario usare credenziali diverse da quelle quando si testa nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="262f6-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="262f6-186">Se il provider di posta elettronica utilizza la crittografia, impostare `WebMail.EnableSsl` a `true`.</span><span class="sxs-lookup"><span data-stu-id="262f6-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="262f6-187">Se si verifica un errore l'invio di posta elettronica, si potrebbe visualizzare un messaggio di errore ASP.NET standard, che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="262f6-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Messaggio di errore ASP.NET quando si è verificato un problema con la posta elettronica](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="262f6-189">È anche possibile eseguire il debug dei problemi con l'invio di posta elettronica utilizzando un `try-catch` blocco, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="262f6-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="262f6-190">Quando si usa un `try-catch` blocco, ASP.NET non visualizza messaggi di errore standard.</span><span class="sxs-lookup"><span data-stu-id="262f6-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="262f6-191">In alternativa, è possibile acquisire l'errore nel `catch` parte del blocco.</span><span class="sxs-lookup"><span data-stu-id="262f6-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="262f6-192">Sostituire i valori appropriati per `your-SMTP-server-name`e così via.</span><span class="sxs-lookup"><span data-stu-id="262f6-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="262f6-193">Di seguito alcuni dei messaggi di errore che potrebbe essere visualizzato in questo modo:</span><span class="sxs-lookup"><span data-stu-id="262f6-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="262f6-194">*Errore durante l'invio di posta elettronica.*</span><span class="sxs-lookup"><span data-stu-id="262f6-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="262f6-195">oppure</span><span class="sxs-lookup"><span data-stu-id="262f6-195">-or-</span></span>

    <span data-ttu-id="262f6-196">*Un tentativo di connessione non è riuscito perché la parte connessa non ha risposto correttamente dopo un periodo di tempo o stabilire una connessione non è riuscita perché l'host connesso non ha risposto*</span><span class="sxs-lookup"><span data-stu-id="262f6-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="262f6-197">Questo errore indica generalmente che l'applicazione non è riuscito a connettersi al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="262f6-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="262f6-198">Controllare il nome del server e il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="262f6-198">Check the server name and port number.</span></span>
- <span data-ttu-id="262f6-199"><em>Cassetta postale non disponibile. Risposta del server: 5.1.0 &lt; someuser@invaliddomain &gt; mittente rifiutata: dominio mittente non valido</em></span><span class="sxs-lookup"><span data-stu-id="262f6-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="262f6-200">Questo messaggio può indicare che il `From` indirizzo non è corretto o mancante.</span><span class="sxs-lookup"><span data-stu-id="262f6-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="262f6-201">*La stringa specificata non è nel formato richiesto per un indirizzo di posta elettronica.*</span><span class="sxs-lookup"><span data-stu-id="262f6-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="262f6-202">Questo errore potrebbe indicare che il valore della `To` o `From` proprietà non sono riconosciute come indirizzi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="262f6-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="262f6-203">(ASP.NET non è possibile verificare che l'indirizzo di posta elettronica sia valido, ma solo la 's nel formato corretto, ad esempio *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="262f6-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="262f6-204">Rimuovere il markup che viene visualizzato l'errore (`@errorMessage`) prima di pubblicare la pagina in un sito live.</span><span class="sxs-lookup"><span data-stu-id="262f6-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="262f6-205">Non è una buona idea per consentire agli utenti di visualizzare i messaggi di errore che si ottiene da un server.</span><span class="sxs-lookup"><span data-stu-id="262f6-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="262f6-206">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="262f6-206">Additional Resources</span></span>

[<span data-ttu-id="262f6-207">Domande frequenti sulle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="262f6-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="262f6-208">[ASP.NET Web Pages e WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) forum nel sito Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="262f6-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
