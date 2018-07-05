---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso di SSL in API Web | Microsoft Docs
author: MikeWasson
description: Viene illustrato come usare SSL con l'API Web ASP.NET, incluso l'uso di certificati client SSL.
ms.author: aspnetcontent
ms.date: 12/12/2012
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0e0ca75c6ff1af397fce91079bcd8e9304b025ef
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829810"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="dc6de-103">Uso di SSL in API Web</span><span class="sxs-lookup"><span data-stu-id="dc6de-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="dc6de-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dc6de-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dc6de-105">Numerosi schemi di autenticazione comuni non sono sicuri sul protocollo HTTP normale.</span><span class="sxs-lookup"><span data-stu-id="dc6de-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="dc6de-106">In particolare, l'autenticazione di base e autenticazione basata su form inviano credenziali non crittografate.</span><span class="sxs-lookup"><span data-stu-id="dc6de-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="dc6de-107">Per essere sicuri, questi schemi di autenticazione *necessario* utilizzare SSL.</span><span class="sxs-lookup"><span data-stu-id="dc6de-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="dc6de-108">Inoltre, i certificati client SSL utilizzabile per autenticare i client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="dc6de-109">Abilitazione di SSL nel Server</span><span class="sxs-lookup"><span data-stu-id="dc6de-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="dc6de-110">Per configurare SSL in IIS 7 o versioni successive:</span><span class="sxs-lookup"><span data-stu-id="dc6de-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="dc6de-111">Creare o ottenere un certificato.</span><span class="sxs-lookup"><span data-stu-id="dc6de-111">Create or get a certificate.</span></span> <span data-ttu-id="dc6de-112">Per i test, è possibile creare un certificato autofirmato.</span><span class="sxs-lookup"><span data-stu-id="dc6de-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="dc6de-113">Aggiungere un binding HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dc6de-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="dc6de-114">Per informazioni dettagliate, vedere [come impostazione di SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="dc6de-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="dc6de-115">Per il test locale, è possibile abilitare SSL in IIS Express da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dc6de-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="dc6de-116">Nella finestra Proprietà impostare **SSL abilitato** al **True**.</span><span class="sxs-lookup"><span data-stu-id="dc6de-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="dc6de-117">Prendere nota del valore di **URL SSL**; usare questo URL per testare le connessioni HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dc6de-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="dc6de-118">Imporre SSL in un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="dc6de-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="dc6de-119">Se hai un HTTPS e un binding HTTP, i client possono comunque usare HTTP per accedere al sito.</span><span class="sxs-lookup"><span data-stu-id="dc6de-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="dc6de-120">È possibile consentire alcune risorse necessarie per essere disponibile tramite HTTP, mentre altre risorse richiedono SSL.</span><span class="sxs-lookup"><span data-stu-id="dc6de-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="dc6de-121">In tal caso, usare un filtro azione per richiedere SSL per le risorse protette.</span><span class="sxs-lookup"><span data-stu-id="dc6de-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="dc6de-122">Il codice seguente mostra un filtro di autenticazione di API Web che verifica la presenza di SSL:</span><span class="sxs-lookup"><span data-stu-id="dc6de-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="dc6de-123">Aggiungere il filtro selezionato per eventuali azioni di API Web che richiedono SSL:</span><span class="sxs-lookup"><span data-stu-id="dc6de-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="dc6de-124">Certificati Client SSL</span><span class="sxs-lookup"><span data-stu-id="dc6de-124">SSL Client Certificates</span></span>

<span data-ttu-id="dc6de-125">SSL consente l'autenticazione tramite certificati di infrastruttura a chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="dc6de-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="dc6de-126">Il server deve fornire un certificato che autentica il server al client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="dc6de-127">È meno comune per il client di fornire un certificato al server, ma si tratta di un'opzione per l'autenticazione dei client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="dc6de-128">Per usare i certificati client con SSL, è necessario un modo per distribuire i certificati firmati agli utenti.</span><span class="sxs-lookup"><span data-stu-id="dc6de-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="dc6de-129">Per molti tipi di applicazioni, questo non costituisce una buona esperienza utente, ma in alcuni ambienti (ad esempio, aziendale) può risultare appropriato.</span><span class="sxs-lookup"><span data-stu-id="dc6de-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="dc6de-130">Vantaggi</span><span class="sxs-lookup"><span data-stu-id="dc6de-130">Advantages</span></span> | <span data-ttu-id="dc6de-131">Svantaggi</span><span class="sxs-lookup"><span data-stu-id="dc6de-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="dc6de-132">-Credenziali certificate sono più avanzate rispetto a nome utente/password.</span><span class="sxs-lookup"><span data-stu-id="dc6de-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="dc6de-133">-SSL fornisce un canale sicuro completo, con l'autenticazione, l'integrità del messaggio e la crittografia dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="dc6de-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="dc6de-134">-È necessario ottenere e gestire i certificati PKI.</span><span class="sxs-lookup"><span data-stu-id="dc6de-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="dc6de-135">-La piattaforma client deve supportare i certificati client SSL.</span><span class="sxs-lookup"><span data-stu-id="dc6de-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="dc6de-136">Per configurare IIS per accettare certificati client, aprire Gestione IIS e procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="dc6de-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="dc6de-137">Fare clic sul nodo del sito nella visualizzazione albero.</span><span class="sxs-lookup"><span data-stu-id="dc6de-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="dc6de-138">Fare doppio clic il **impostazioni SSL** funzionalità nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="dc6de-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="dc6de-139">Sotto **certificati Client**, selezionare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc6de-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="dc6de-140">**Accettare**: IIS accetterà un certificato dal client, ma non ne richiede.</span><span class="sxs-lookup"><span data-stu-id="dc6de-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="dc6de-141">**Richiedi**: è necessario un certificato client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="dc6de-142">(Per abilitare questa opzione, è necessario selezionare anche "Richiedi SSL")</span><span class="sxs-lookup"><span data-stu-id="dc6de-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="dc6de-143">È anche possibile impostare queste opzioni nel file ApplicationHost. config:</span><span class="sxs-lookup"><span data-stu-id="dc6de-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="dc6de-144">Il **SslNegotiateCert** flag significa IIS accetterà un certificato dal client, ma non ne richiede (equivalente all'opzione "Accept" in Gestione IIS).</span><span class="sxs-lookup"><span data-stu-id="dc6de-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="dc6de-145">Per richiedere un certificato, impostare il **SslRequireCert** flag.</span><span class="sxs-lookup"><span data-stu-id="dc6de-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="dc6de-146">Per i test, è anche possibile impostare queste opzioni in IIS Express, il applicationhost locale. File di configurazione che si trova in "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="dc6de-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="dc6de-147">Creazione di un certificato Client per i test</span><span class="sxs-lookup"><span data-stu-id="dc6de-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="dc6de-148">A scopo di test, è possibile usare [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) per creare un certificato client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="dc6de-149">Creare innanzitutto un'autorità radice di test:</span><span class="sxs-lookup"><span data-stu-id="dc6de-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="dc6de-150">Makecert verrà chiesto di immettere una password per la chiave privata.</span><span class="sxs-lookup"><span data-stu-id="dc6de-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="dc6de-151">Successivamente, aggiungere il certificato per il test del server "Trusted Root archivio Autorità di certificazione", come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="dc6de-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="dc6de-152">Aprire MMC.</span><span class="sxs-lookup"><span data-stu-id="dc6de-152">Open MMC.</span></span>
2. <span data-ttu-id="dc6de-153">Sotto **File**, selezionare **Aggiungi/Rimuovi Snap-In**.</span><span class="sxs-lookup"><span data-stu-id="dc6de-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="dc6de-154">Selezionare **Account del Computer**.</span><span class="sxs-lookup"><span data-stu-id="dc6de-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="dc6de-155">Selezionare **computer locale** e completare la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="dc6de-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="dc6de-156">Nel riquadro di spostamento, espandere il nodo "Radice autorità di certificazione".</span><span class="sxs-lookup"><span data-stu-id="dc6de-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="dc6de-157">Nel **azione** dal menu **tutte le attività**, quindi fare clic su **importazione** per avviare l'importazione guidata certificati.</span><span class="sxs-lookup"><span data-stu-id="dc6de-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="dc6de-158">Individuare il file di certificato, Tempca.</span><span class="sxs-lookup"><span data-stu-id="dc6de-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="dc6de-159">Fare clic su **aperto**, quindi fare clic su **successivo** e completare la procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="dc6de-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="dc6de-160">(Verrà richiesto di immettere nuovamente la password.)</span><span class="sxs-lookup"><span data-stu-id="dc6de-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="dc6de-161">A questo punto creare un certificato client sia firmata dal primo certificato:</span><span class="sxs-lookup"><span data-stu-id="dc6de-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="dc6de-162">Utilizzo dei certificati Client in API Web</span><span class="sxs-lookup"><span data-stu-id="dc6de-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="dc6de-163">Sul lato server, è possibile ottenere il certificato client chiamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) nel messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="dc6de-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="dc6de-164">Il metodo restituisce null se nessun certificato client.</span><span class="sxs-lookup"><span data-stu-id="dc6de-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="dc6de-165">In caso contrario, restituisce un **X509Certificate2** istanza.</span><span class="sxs-lookup"><span data-stu-id="dc6de-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="dc6de-166">Usare questo oggetto per ottenere informazioni dal certificato, ad esempio l'autorità emittente e il soggetto.</span><span class="sxs-lookup"><span data-stu-id="dc6de-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="dc6de-167">È quindi possibile usare queste informazioni per l'autenticazione e/o autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="dc6de-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
