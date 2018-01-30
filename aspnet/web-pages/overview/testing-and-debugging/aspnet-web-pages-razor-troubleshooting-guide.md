---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages Guida alla risoluzione dei problemi (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo vengono descritti i problemi che possono verificarsi quando si utilizzano pagine Web ASP.NET (Razor) e alcune soluzioni suggerite. Versioni del software pagina Web ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 2e25e9c7cea937016301e0f11919adb9647ca216
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="d7b5f-104">Pagine Web ASP.NET (Razor) Troubleshooting Guide</span><span class="sxs-lookup"><span data-stu-id="d7b5f-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="d7b5f-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d7b5f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d7b5f-106">In questo articolo vengono descritti i problemi che possono verificarsi quando si utilizzano pagine Web ASP.NET (Razor) e alcune soluzioni suggerite.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="d7b5f-107">Versioni del software</span><span class="sxs-lookup"><span data-stu-id="d7b5f-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d7b5f-108">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d7b5f-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d7b5f-109">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2 e pagine Web ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="d7b5f-110">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d7b5f-111">Problemi con l'esecuzione delle pagine</span><span class="sxs-lookup"><span data-stu-id="d7b5f-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="d7b5f-112">Problemi di codice Razor</span><span class="sxs-lookup"><span data-stu-id="d7b5f-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="d7b5f-113">Problemi relativi alla sicurezza e l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="d7b5f-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="d7b5f-114">Problemi con l'invio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d7b5f-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="d7b5f-115">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d7b5f-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="d7b5f-116">Per domande di carattere generale, vedere [pagine Web ASP.NET (Razor) domande frequenti su](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="d7b5f-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="d7b5f-117">Problemi con l'esecuzione delle pagine</span><span class="sxs-lookup"><span data-stu-id="d7b5f-117">Issues with Running Pages</span></span>

<span data-ttu-id="d7b5f-118">Un'ampia gamma di problemi può impedire *. cshtml* e *. vbhtml* pagine da eseguito correttamente.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="d7b5f-119">In questa sezione elenca i messaggi di errore comuni e cause probabili.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="d7b5f-120">Errore HTTP 403 - accesso negato: Accesso negato</span><span class="sxs-lookup"><span data-stu-id="d7b5f-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="d7b5f-121">*Non si dispone dell'autorizzazione per visualizzare la directory o la pagina utilizzando le credenziali fornite.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="d7b5f-122">Questo errore può verificarsi se il server non è in esecuzione la versione corretta di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="d7b5f-123">Assicurarsi che il computer che esegue il server (locale o remota) disponga almeno .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="d7b5f-124">Assicurarsi inoltre che l'applicazione è configurato per eseguire la versione corretta.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="d7b5f-125">Se si riscontra questo problema in locale mentre si lavora in WebMatrix, fare clic su di **sito** area di lavoro e nella visualizzazione albero fare clic su **impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="d7b5f-126">Nel **selezionare .NET Framework versione** selezionare **.NET 4 (modalità integrata)**.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="d7b5f-127">Se questa versione è già impostata, provare a eseguire WebMatrix come amministratore.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="d7b5f-128">Assicurarsi che la radice del sito Web disponga di almeno uno *. cshtml* file in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="d7b5f-129">Se viene visualizzato questo errore quando il server web in un server remoto, contattare l'amministratore del server.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="d7b5f-130">Assicurarsi che il server dispone di .NET Framework 4 o successiva.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="d7b5f-131">Assicurarsi inoltre che l'applicazione è in esecuzione in un pool di applicazioni configurato per utilizzare tale versione di.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="d7b5f-132">Se si dispone di controllo del server, assicurarsi che la versione corretta di .NET Framework è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="d7b5f-133">È anche possibile provare a ripristinare l'installazione eseguendo il `aspnet_regiis -iru` comando.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="d7b5f-134">(Ad esempio, se si installa IIS dopo l'installazione di .NET Framework, IIS verrà non essere configurato correttamente per eseguire le pagine ASP.NET.) Per ulteriori informazioni, vedere [strumento di registrazione IIS ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="d7b5f-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="d7b5f-135">Errore HTTP 403.14 - accesso negato</span><span class="sxs-lookup"><span data-stu-id="d7b5f-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="d7b5f-136">*Il server Web è configurato per non visualizzare il contenuto di questa directory.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="d7b5f-137">Questo errore può verificarsi se la richiesta di una risorsa protetta (ad esempio il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).</span><span class="sxs-lookup"><span data-stu-id="d7b5f-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="d7b5f-138">Errore HTTP 404.17 - non trovato</span><span class="sxs-lookup"><span data-stu-id="d7b5f-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="d7b5f-139">*Il contenuto richiesto sembra essere uno script e non verrà fornito dal gestore di file statici.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="d7b5f-140">Questo errore può verificarsi se il server non è configurato correttamente per l'utilizzo di .NET Framework 4 o versione successiva e pertanto non riconosce il codice in `@{ }` blocchi.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="d7b5f-141">Vedere la descrizione precedente *HTTP Errore 403 - accesso negato: accesso negato*.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="d7b5f-142">Errore HTTP 404.7 - non trovato</span><span class="sxs-lookup"><span data-stu-id="d7b5f-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="d7b5f-143">*Il modulo filtro delle richieste è configurato per negare l'estensione di file*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="d7b5f-144">Questo errore può verificarsi se *. cshtml* o *. vbhtml* estensioni sono state bloccate in modo esplicito nel server.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="d7b5f-145">Un sintomo del problema è che gli URL lavoro quando non includono l'estensione, ma gli URL che includono *. cshtml* o *. vbhtml* non funzionano.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="d7b5f-146">Una soluzione possibile consiste nell'abilitare nuovamente le estensioni del sito *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="d7b5f-147">Nell'esempio seguente viene illustrato come abilitare il *. cshtml* estensione.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="d7b5f-148">Errore HTTP 404.8 - non trovato</span><span class="sxs-lookup"><span data-stu-id="d7b5f-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="d7b5f-149">*Il modulo filtro delle richieste è configurato per negare un percorso nell'URL che contiene una sezione hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="d7b5f-150">Questo errore può verificarsi se la richiesta di una risorsa protetta (ad esempio il *Web. config* file) o che si trova in una cartella protetta (ad esempio *App\_dati* o *App\_Codice*).</span><span class="sxs-lookup"><span data-stu-id="d7b5f-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="d7b5f-151">Questo tipo di pagina non è disponibile (errore Server nell'applicazione '/')</span><span class="sxs-lookup"><span data-stu-id="d7b5f-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="d7b5f-152">Vedere la descrizione in precedenza per HTTP 404.17 di errore.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="d7b5f-153">Problemi di codice Razor</span><span class="sxs-lookup"><span data-stu-id="d7b5f-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="d7b5f-154">Il nome '*classe*' non esiste nel contesto corrente</span><span class="sxs-lookup"><span data-stu-id="d7b5f-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="d7b5f-155">Un motivo viene visualizzato questo errore è spesso che `class` riferimenti a un file di supporto, ma l'helper non è installato.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="d7b5f-156">Ad esempio, se si tenta di utilizzare un file di supporto, ma non è stato installato il pacchetto da NuGet, verrà visualizzato questo errore.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="d7b5f-157">Utilizzare la raccolta in WebMatrix per trovare e installare il supporto.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="d7b5f-158">Se è installato il supporto, ma la pagina ancora non lo riconosce, provare ad aggiungere aggiungere un `using` istruzione al codice.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="d7b5f-159">Nel `using` istruzione, lo spazio dei nomi che include il supporto di riferimento.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="d7b5f-160">Ad esempio, gli helper di base sono incluse nel package di ASP.NET Web Helpers presenti il `System.Web.Helpers` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="d7b5f-161">Nella parte superiore della pagina in cui si desidera utilizzare l'helper, aggiungere questa riga:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="d7b5f-162">Problemi relativi alla sicurezza e l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="d7b5f-162">Issues with Security and Membership</span></span>

<span data-ttu-id="d7b5f-163">Se si utilizza il sistema di sicurezza incorporati (membership) nelle pagine Web ASP.NET (Razor), possono verificarsi i problemi seguenti.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="d7b5f-164">Per chiamare questo metodo, la proprietà "Membership.Provider" deve essere un'istanza di "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="d7b5f-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="d7b5f-165">Questo errore può indicare che non `AspNetSqlMembershipProvider` classe è configurata.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="d7b5f-166">(Un sintomo è che il sito funziona correttamente in locale, ma genera questo errore quando si pubblica il server del provider di hosting). Una correzione per questo problema consiste nell'abilitare in modo esplicito l'appartenenza semplice aggiungendo quanto segue per il sito *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="d7b5f-167">Problemi con l'invio di posta elettronica</span><span class="sxs-lookup"><span data-stu-id="d7b5f-167">Issues with Sending Email</span></span>

<span data-ttu-id="d7b5f-168">Problemi relativi all'invio di posta elettronica possono essere difficile eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="d7b5f-169">Può essere un problema iniziale che non è possibile connettersi al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="d7b5f-170">Se la connessione ha esito positivo, ASP.NET invia il messaggio al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="d7b5f-171">Tuttavia, si possono verificare problemi con il messaggio stesso che impedisce l'invio al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="d7b5f-172">Se l'applicazione non inviare posta elettronica, procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="d7b5f-173">Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="d7b5f-174">Tuttavia, se si pubblica il sito per un provider di hosting, il nome del server SMTP a questo punto potrebbe essere `localhost`.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="d7b5f-175">Questa situazione si verifica perché dopo avere pubblicato e il sito è in esecuzione nel server del provider, il server SMTP potrebbe essere locale dal punto di vista dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="d7b5f-176">Questa modifica nei nomi di server potrebbe indicare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="d7b5f-177">In genere, il numero di porta è 25.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-177">The port number is usually 25.</span></span> <span data-ttu-id="d7b5f-178">Tuttavia, alcuni provider richiedono l'utilizzo della porta 587 o alcune altre porte.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="d7b5f-179">Contattare il proprietario del server SMTP del numero di porta prevedono di utilizzare.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="d7b5f-180">Assicurarsi di utilizzare le credenziali corrette.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="d7b5f-181">Se il sito è stato pubblicato in un provider di hosting, utilizzare le credenziali che il provider ha specificamente indicato per la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="d7b5f-182">Queste credenziali possono essere diverse dalle credenziali che utilizzare per la pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="d7b5f-183">In alcuni casi non è necessario credenziali affatto.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="d7b5f-184">Se si invia tramite posta elettronica utilizzando l'account personale, provider di posta elettronica potrebbe essere già conoscere le credenziali.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="d7b5f-185">Dopo la pubblicazione, potrebbe essere necessario utilizzare credenziali diverse rispetto a quando si testa il computer locale.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="d7b5f-186">Se il provider di posta elettronica utilizza la crittografia, impostare `WebMail.EnableSsl` a `true`.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="d7b5f-187">Se si verifica un errore di invio di posta elettronica, è visualizzato un messaggio di errore ASP.NET standard, che è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Messaggio di errore ASP.NET quando si è verificato un problema con messaggio di posta elettronica](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="d7b5f-189">È anche possibile eseguire il debug problemi relativi all'invio di posta elettronica utilizzando un `try-catch` blocco, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="d7b5f-190">Quando si utilizza un `try-catch` blocco, ASP.NET non vengono visualizzati i messaggi di errore standard.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="d7b5f-191">In alternativa, è possibile acquisire l'errore nel `catch` parte del blocco.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="d7b5f-192">Sostituire i valori appropriati per `your-SMTP-server-name`e così via.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="d7b5f-193">Alcuni dei messaggi di errore che potrebbe essere visualizzato in questo modo i seguenti:</span><span class="sxs-lookup"><span data-stu-id="d7b5f-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="d7b5f-194">*Errore durante l'invio di posta elettronica.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="d7b5f-195">oppure</span><span class="sxs-lookup"><span data-stu-id="d7b5f-195">-or-</span></span>

    <span data-ttu-id="d7b5f-196">*Un tentativo di connessione non riuscita perché l'entità connesso non ha risposto correttamente dopo un periodo di tempo o stabilire una connessione non è riuscita perché l'host connesso non ha risposto*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="d7b5f-197">Questo errore indica in genere che l'applicazione non riuscito a connettersi al server SMTP.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="d7b5f-198">Controllare il nome del server e il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-198">Check the server name and port number.</span></span>
- <span data-ttu-id="d7b5f-199">*Cassetta postale non disponibile. Risposta del server: 5.1.0 &lt; someuser@invaliddomain &gt; mittente rifiutato: il dominio del mittente non valido*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-199">*Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain*</span></span>

    <span data-ttu-id="d7b5f-200">Questo messaggio può indicare che il `From` indirizzo non è corretto o mancante.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="d7b5f-201">*La stringa specificata non è nel formato richiesto per un indirizzo di posta elettronica.*</span><span class="sxs-lookup"><span data-stu-id="d7b5f-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="d7b5f-202">Questo errore potrebbe indicare che il valore di `To` o `From` proprietà non sono riconosciute come indirizzi di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="d7b5f-203">(ASP.NET non è possibile verificare che l'indirizzo di posta elettronica sia valido, ma solo la 's nel formato corretto, ad esempio  *name@domain.com* .)</span><span class="sxs-lookup"><span data-stu-id="d7b5f-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="d7b5f-204">Rimuovere il markup che viene visualizzato l'errore (`@errorMessage`) prima di pubblicare la pagina in un sito in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="d7b5f-205">Non è consigliabile per consentire agli utenti di vedere i messaggi di errore che si ottiene da un server.</span><span class="sxs-lookup"><span data-stu-id="d7b5f-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d7b5f-206">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d7b5f-206">Additional Resources</span></span>

[<span data-ttu-id="d7b5f-207">Domande frequenti sulle pagine Web ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="d7b5f-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="d7b5f-208">[WebMatrix e ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum sul sito Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d7b5f-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
