---
uid: signalr/overview/older-versions/hub-authorization
title: Autenticazione e autorizzazione per gli hub SignalR (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="e99de-103">Autenticazione e autorizzazione per gli hub SignalR (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="e99de-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e99de-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e99de-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e99de-105">In questo argomento viene descritto come limitare gli utenti o i ruoli possono accedere i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e99de-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="e99de-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e99de-106">Overview</span></span>

<span data-ttu-id="e99de-107">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="e99de-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e99de-108">Autorizzare l'attributo</span><span class="sxs-lookup"><span data-stu-id="e99de-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="e99de-109">Richiedere l'autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="e99de-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="e99de-110">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="e99de-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="e99de-111">Passare le informazioni di autenticazione client</span><span class="sxs-lookup"><span data-stu-id="e99de-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="e99de-112">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="e99de-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="e99de-113">Cookie di autenticazione basata su form</span><span class="sxs-lookup"><span data-stu-id="e99de-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="e99de-114">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="e99de-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="e99de-115">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="e99de-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="e99de-116">Certificato</span><span class="sxs-lookup"><span data-stu-id="e99de-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="e99de-117">Autorizzare l'attributo</span><span class="sxs-lookup"><span data-stu-id="e99de-117">Authorize attribute</span></span>

<span data-ttu-id="e99de-118">SignalR fornisce il [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attributo per specificare quali utenti o i ruoli hanno accesso a un hub o un metodo.</span><span class="sxs-lookup"><span data-stu-id="e99de-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="e99de-119">Questo attributo si trova nel `Microsoft.AspNet.SignalR` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e99de-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="e99de-120">Si applica il `Authorize` attributo a un hub o metodi particolari in un hub.</span><span class="sxs-lookup"><span data-stu-id="e99de-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="e99de-121">Quando si applica il `Authorize` attributo a una classe di hub, i requisiti di autorizzazione specificato viene applicato a tutti i metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e99de-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="e99de-122">Seguito sono riportati i diversi tipi di requisiti di autorizzazione che è possibile applicare.</span><span class="sxs-lookup"><span data-stu-id="e99de-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="e99de-123">Senza il `Authorize` attributo, tutti i metodi pubblici dell'hub sono disponibili per un client che si è connessi all'hub.</span><span class="sxs-lookup"><span data-stu-id="e99de-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="e99de-124">Se è stato definito un ruolo denominato "Admin" nell'applicazione web, è possibile specificare che solo gli utenti in tale ruolo possono accedere a un hub con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="e99de-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="e99de-125">In alternativa, è possibile specificare che un hub contenga un metodo disponibile per tutti gli utenti e un secondo metodo è disponibile solo per gli utenti autenticati, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e99de-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="e99de-126">Negli esempi seguenti scenari di autorizzazione diversi:</span><span class="sxs-lookup"><span data-stu-id="e99de-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="e99de-127">`[Authorize]`: solo gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="e99de-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="e99de-128">`[Authorize(Roles = "Admin,Manager")]`: solo gli utenti nei ruoli specificati autenticati</span><span class="sxs-lookup"><span data-stu-id="e99de-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="e99de-129">`[Authorize(Users = "user1,user2")]`: solo gli utenti con i nomi utente specificati autenticati</span><span class="sxs-lookup"><span data-stu-id="e99de-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="e99de-130">`[Authorize(RequireOutgoing=false)]`: solo gli utenti autenticati possono richiamare l'hub, ma le chiamate dal server ai client non sono limitate per l'autorizzazione, ad esempio, quando solo determinati utenti possono inviare un messaggio ma non tutti gli altri utenti possono ricevere il messaggio.</span><span class="sxs-lookup"><span data-stu-id="e99de-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="e99de-131">La proprietà RequireOutgoing può essere applicata solo all'intero hub, non sui metodi di persone all'interno dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e99de-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="e99de-132">Quando RequireOutgoing non è impostata su false, solo gli utenti che soddisfano il requisito di autorizzazione vengono chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="e99de-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="e99de-133">Richiedere l'autenticazione per tutti gli hub</span><span class="sxs-lookup"><span data-stu-id="e99de-133">Require authentication for all hubs</span></span>

<span data-ttu-id="e99de-134">È possibile richiedere l'autenticazione per tutti gli hub e i metodi dell'hub nell'applicazione chiamando il [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e99de-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="e99de-135">È possibile utilizzare questo metodo quando si dispone di più hub e imporre un requisito di autenticazione per tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="e99de-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="e99de-136">Con questo metodo, è possibile specificare l'autorizzazione in uscita, un utente o ruolo.</span><span class="sxs-lookup"><span data-stu-id="e99de-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="e99de-137">È possibile specificare solo che l'accesso ai metodi hub è limitato agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="e99de-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="e99de-138">Tuttavia, è comunque possibile applicare l'attributo di Authorize hub o i metodi per specificare i requisiti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="e99de-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="e99de-139">Oltre il requisito di autenticazione di base, viene applicato ai requisiti specificati negli attributi.</span><span class="sxs-lookup"><span data-stu-id="e99de-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="e99de-140">Nell'esempio seguente viene illustrato un file Global. asax che limita tutti i metodi dell'hub per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="e99de-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="e99de-141">Se si chiama il `RequireAuthentication()` metodo dopo l'elaborazione di una richiesta SignalR, SignalR genererà un `InvalidOperationException` eccezione.</span><span class="sxs-lookup"><span data-stu-id="e99de-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="e99de-142">Questa eccezione viene generata perché non è possibile aggiungere un modulo a HubPipeline dopo che la pipeline è stata richiamata.</span><span class="sxs-lookup"><span data-stu-id="e99de-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="e99de-143">Nell'esempio precedente viene chiamata la `RequireAuthentication` metodo nel `Application_Start` metodo che viene eseguito una volta prima di gestire la prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="e99de-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="e99de-144">Autorizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="e99de-144">Customized authorization</span></span>

<span data-ttu-id="e99de-145">Se si desidera personalizzare la modalità di determinazione di autorizzazione, è possibile creare una classe che deriva da `AuthorizeAttribute` ed eseguire l'override di [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) metodo.</span><span class="sxs-lookup"><span data-stu-id="e99de-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="e99de-146">Questo metodo viene chiamato per ogni richiesta per determinare se l'utente è autorizzato a completare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="e99de-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="e99de-147">Nel metodo sottoposto a override, fornire la logica necessaria per lo scenario di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="e99de-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="e99de-148">Nell'esempio seguente viene illustrato come applicare l'autorizzazione tramite l'identità basata sulle attestazioni.</span><span class="sxs-lookup"><span data-stu-id="e99de-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="e99de-149">Passare le informazioni di autenticazione client</span><span class="sxs-lookup"><span data-stu-id="e99de-149">Pass authentication information to clients</span></span>

<span data-ttu-id="e99de-150">Potrebbe essere necessario utilizzare le informazioni di autenticazione nel codice che viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="e99de-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="e99de-151">Passare le informazioni necessarie quando si chiama i metodi nel client.</span><span class="sxs-lookup"><span data-stu-id="e99de-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="e99de-152">Ad esempio, un metodo di applicazione di chat può passare come parametro il nome utente della persona che la registrazione di un messaggio, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e99de-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="e99de-153">In alternativa, è possibile creare un oggetto per rappresentare le informazioni di autenticazione e passare tale oggetto come un parametro, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e99de-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="e99de-154">Non passare mai id connessione del client per altri client, come un utente malintenzionato potrebbe usare per simulare una richiesta dal client.</span><span class="sxs-lookup"><span data-stu-id="e99de-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="e99de-155">Opzioni di autenticazione per i client .NET</span><span class="sxs-lookup"><span data-stu-id="e99de-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="e99de-156">Quando si dispone di un client .NET, ad esempio un'applicazione console, che interagisce con un hub di cui è limitato agli utenti autenticati, è possibile passare le credenziali di autenticazione in un cookie, l'intestazione di connessione o un certificato.</span><span class="sxs-lookup"><span data-stu-id="e99de-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="e99de-157">Negli esempi di questa sezione viene illustrato come utilizzare tali metodi diversi per autenticare un utente.</span><span class="sxs-lookup"><span data-stu-id="e99de-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="e99de-158">Non sono completamente funzionali App SignalR.</span><span class="sxs-lookup"><span data-stu-id="e99de-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="e99de-159">Per ulteriori informazioni sui client .NET con SignalR, vedere [hub API Guida - Client .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="e99de-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="e99de-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="e99de-160">Cookie</span></span>

<span data-ttu-id="e99de-161">Quando il client .NET interagisce con un hub che utilizza l'autenticazione basata su form ASP.NET, è necessario impostare manualmente il cookie di autenticazione per la connessione.</span><span class="sxs-lookup"><span data-stu-id="e99de-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="e99de-162">Il cookie viene aggiunto il `CookieContainer` proprietà il [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) oggetto.</span><span class="sxs-lookup"><span data-stu-id="e99de-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="e99de-163">Nell'esempio seguente mostra un'app console che recupera un cookie di autenticazione da una pagina web e aggiunge tale cookie per la connessione.</span><span class="sxs-lookup"><span data-stu-id="e99de-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="e99de-164">L'URL `https://www.contoso.com/RemoteLogin` in esempio punta a una pagina web che è necessario creare.</span><span class="sxs-lookup"><span data-stu-id="e99de-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="e99de-165">La pagina potrebbe recuperare il nome utente registrato e la password e tentare di accedere con le credenziali utente.</span><span class="sxs-lookup"><span data-stu-id="e99de-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="e99de-166">L'applicazione console invia le credenziali da www.contoso.com/RemoteLogin che potrebbe fare riferimento a una pagina vuota che contiene il file code-behind seguente.</span><span class="sxs-lookup"><span data-stu-id="e99de-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="e99de-167">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="e99de-167">Windows authentication</span></span>

<span data-ttu-id="e99de-168">Quando si utilizza l'autenticazione di Windows, è possibile passare le credenziali dell'utente corrente utilizzando il [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) proprietà.</span><span class="sxs-lookup"><span data-stu-id="e99de-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="e99de-169">Le credenziali per la connessione viene impostato il valore di DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="e99de-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="e99de-170">Intestazione di connessione</span><span class="sxs-lookup"><span data-stu-id="e99de-170">Connection header</span></span>

<span data-ttu-id="e99de-171">Se l'applicazione non utilizza i cookie, è possibile passare le informazioni utente nell'intestazione di connessione.</span><span class="sxs-lookup"><span data-stu-id="e99de-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="e99de-172">Ad esempio, è possibile passare un token nell'intestazione di connessione.</span><span class="sxs-lookup"><span data-stu-id="e99de-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="e99de-173">Quindi, nell'hub, verificare il token dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e99de-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="e99de-174">Certificato</span><span class="sxs-lookup"><span data-stu-id="e99de-174">Certificate</span></span>

<span data-ttu-id="e99de-175">È possibile passare un certificato client per verificare che l'utente.</span><span class="sxs-lookup"><span data-stu-id="e99de-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="e99de-176">Aggiungere il certificato quando si crea la connessione.</span><span class="sxs-lookup"><span data-stu-id="e99de-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="e99de-177">Nell'esempio seguente viene illustrato come aggiungere un certificato client per la connessione; non è visibile l'applicazione console completa.</span><span class="sxs-lookup"><span data-stu-id="e99de-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="e99de-178">Usa il [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) classe che fornisce diversi modi per creare il certificato.</span><span class="sxs-lookup"><span data-stu-id="e99de-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
