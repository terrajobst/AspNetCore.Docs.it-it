---
title: Autenticazione e autorizzazione in ASP.NET Core SignalR
author: rachelappel
description: Informazioni su come usare l'autenticazione e autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: 32e5fcf2fd3f888e0e131fa47bd9a74eede3c26d
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028463"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a><span data-ttu-id="d18dd-103">Autenticazione e autorizzazione in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d18dd-103">Authentication and authorization in ASP.NET Core SignalR</span></span>

<span data-ttu-id="d18dd-104">Da [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d18dd-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="d18dd-105">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d18dd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a><span data-ttu-id="d18dd-106">Eseguire l'autenticazione di utenti che si connettono a un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="d18dd-106">Authenticate users connecting to a SignalR hub</span></span>

<span data-ttu-id="d18dd-107">Può essere utilizzato con SignalR [autenticazione di ASP.NET Core](xref:security/authentication/index) per associare un utente a ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="d18dd-107">SignalR can be used with [ASP.NET Core Authentication](xref:security/authentication/index) to associate a user with each connection.</span></span> <span data-ttu-id="d18dd-108">In un hub, i dati di autenticazione è possibile accedere dal [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) proprietà.</span><span class="sxs-lookup"><span data-stu-id="d18dd-108">In a hub, authentication data can be accessed from the [`HubConnectionContext.User`](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) property.</span></span> <span data-ttu-id="d18dd-109">L'autenticazione consente l'hub chiamare i metodi in tutte le connessioni associate a un utente (vedere [gestire utenti e gruppi in SignalR](xref:signalr/groups) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="d18dd-109">Authentication allows the hub to call methods on all connections associated with a user (See [Manage users and groups in SignalR](xref:signalr/groups) for more information).</span></span> <span data-ttu-id="d18dd-110">Più connessioni possono essere associate a un singolo utente.</span><span class="sxs-lookup"><span data-stu-id="d18dd-110">Multiple connections may be associated with a single user.</span></span>

### <a name="cookie-authentication"></a><span data-ttu-id="d18dd-111">Autenticazione tramite cookie</span><span class="sxs-lookup"><span data-stu-id="d18dd-111">Cookie authentication</span></span>

<span data-ttu-id="d18dd-112">In un'app basata su browser, l'autenticazione tramite cookie consente le credenziali utente esistenti propagare automaticamente per le connessioni SignalR.</span><span class="sxs-lookup"><span data-stu-id="d18dd-112">In a browser-based app, cookie authentication allows your existing user credentials to automatically flow to SignalR connections.</span></span> <span data-ttu-id="d18dd-113">Quando si usa il browser client, è necessaria alcuna configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="d18dd-113">When using the browser client, no additional configuration is needed.</span></span> <span data-ttu-id="d18dd-114">Se l'utente è connesso all'App, la connessione SignalR eredita automaticamente questa autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d18dd-114">If the user is logged in to your app, the SignalR connection automatically inherits this authentication.</span></span>

<span data-ttu-id="d18dd-115">L'autenticazione dei cookie non è consigliato, a meno che l'app è sufficiente autenticare gli utenti dal browser client.</span><span class="sxs-lookup"><span data-stu-id="d18dd-115">Cookie authentication isn't recommended unless the app only needs to authenticate users from the browser client.</span></span> <span data-ttu-id="d18dd-116">Quando si usa la [Client .NET](xref:signalr/dotnet-client), il `Cookies` proprietà può essere configurata nel `.WithUrl` chiamata per fornire un cookie.</span><span class="sxs-lookup"><span data-stu-id="d18dd-116">When using the [.NET Client](xref:signalr/dotnet-client), the `Cookies` property can be configured in the `.WithUrl` call in order to provide a cookie.</span></span> <span data-ttu-id="d18dd-117">Tuttavia, utilizzando l'autenticazione dei cookie del client .NET richiede l'app per fornire un'API per lo scambio di dati di autenticazione per un cookie.</span><span class="sxs-lookup"><span data-stu-id="d18dd-117">However, using cookie authentication from the .NET Client requires the app to provide an API to exchange authentication data for a cookie.</span></span>

### <a name="bearer-token-authentication"></a><span data-ttu-id="d18dd-118">Autenticazione del bearer token</span><span class="sxs-lookup"><span data-stu-id="d18dd-118">Bearer token authentication</span></span>

<span data-ttu-id="d18dd-119">Autenticazione del bearer token è l'approccio consigliato quando si usano client diversi da client browser.</span><span class="sxs-lookup"><span data-stu-id="d18dd-119">Bearer token authentication is the recommended approach when using clients other than the browser client.</span></span> <span data-ttu-id="d18dd-120">Questo approccio, il client fornisce un token di accesso che il server di convalida e utilizza per identificare l'utente.</span><span class="sxs-lookup"><span data-stu-id="d18dd-120">In this approach, the client provides an access token that the server validates and uses to identify the user.</span></span> <span data-ttu-id="d18dd-121">I dettagli dell'autenticazione del bearer token non rientrano nell'ambito di questo documento.</span><span class="sxs-lookup"><span data-stu-id="d18dd-121">The details of bearer token authentication are beyond the scope of this document.</span></span> <span data-ttu-id="d18dd-122">Nel server di autenticazione del bearer token viene configurato usando le [middleware del Bearer token JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="d18dd-122">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="d18dd-123">Nel client JavaScript, il token può essere specificato con il [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opzione.</span><span class="sxs-lookup"><span data-stu-id="d18dd-123">In the JavaScript client, the token can be provided using the [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) option.</span></span>

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

<span data-ttu-id="d18dd-124">Nel client di .NET, è presente un simile [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) proprietà che può essere usata per configurare il token:</span><span class="sxs-lookup"><span data-stu-id="d18dd-124">In the .NET client, there is a similar [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) property that can be used to configure the token:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => _myAccessToken;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="d18dd-125">Prima viene chiamata la funzione di token di accesso è fornire **ogni** richiesta HTTP effettuata da SignalR.</span><span class="sxs-lookup"><span data-stu-id="d18dd-125">The access token function you provide is called before **every** HTTP request made by SignalR.</span></span> <span data-ttu-id="d18dd-126">Se è necessario rinnovare il token per mantenere attiva la connessione (in quanto può scadere durante la connessione), all'interno di questa funzione e restituire il token aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d18dd-126">If you need to renew the token in order to keep the connection active (because it may expire during the connection), do so from within this function and return the updated token.</span></span>

<span data-ttu-id="d18dd-127">Nell'API web standard, vengono inviati i token di connessione in un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="d18dd-127">In standard web APIs, bearer tokens are sent in an HTTP header.</span></span> <span data-ttu-id="d18dd-128">SignalR è, tuttavia, non è possibile impostare queste intestazioni nel browser quando si usano alcuni trasporti.</span><span class="sxs-lookup"><span data-stu-id="d18dd-128">However, SignalR is unable to set these headers in browsers when using some transports.</span></span> <span data-ttu-id="d18dd-129">Quando si usa WebSocket e Server-Sent eventi, il token viene trasmesso come un parametro di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="d18dd-129">When using WebSockets and Server-Sent Events, the token is transmitted as a query string parameter.</span></span> <span data-ttu-id="d18dd-130">Per supportare questa funzionalità nel server, è necessaria un'ulteriore configurazione:</span><span class="sxs-lookup"><span data-stu-id="d18dd-130">In order to support this on the server, additional configuration is required:</span></span>

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?range=33-34,42-80,90)]

### <a name="windows-authentication"></a><span data-ttu-id="d18dd-131">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="d18dd-131">Windows authentication</span></span>

<span data-ttu-id="d18dd-132">Se [autenticazione di Windows](xref:security/authentication/windowsauth) è configurato nell'app, SignalR può usare tale identità per proteggere gli hub.</span><span class="sxs-lookup"><span data-stu-id="d18dd-132">If [Windows authentication](xref:security/authentication/windowsauth) is configured in your app, SignalR can use that identity to secure hubs.</span></span> <span data-ttu-id="d18dd-133">Tuttavia, per inviare messaggi a singoli utenti, è necessario aggiungere un provider personalizzato di ID utente.</span><span class="sxs-lookup"><span data-stu-id="d18dd-133">However, in order to send messages to individual users, you need to add a custom User ID provider.</span></span> <span data-ttu-id="d18dd-134">Questo avviene perché il sistema di autenticazione di Windows non fornisce l'attestazione "Name Identifier" che usa SignalR per determinare il nome utente.</span><span class="sxs-lookup"><span data-stu-id="d18dd-134">This is because the Windows authentication system doesn't provide the "Name Identifier" claim that SignalR uses to determine the user name.</span></span>

<span data-ttu-id="d18dd-135">Aggiungere una nuova classe che implementa `IUserIdProvider` e recuperare una delle attestazioni da parte dell'utente da utilizzare come identificatore.</span><span class="sxs-lookup"><span data-stu-id="d18dd-135">Add a new class that implements `IUserIdProvider` and retrieve one of the claims from the user to use as the identifier.</span></span> <span data-ttu-id="d18dd-136">Ad esempio, per usare l'attestazione "Name" (ovvero il nome utente di Windows nel formato `[Domain]\[Username]`), creare la classe seguente:</span><span class="sxs-lookup"><span data-stu-id="d18dd-136">For example, to use the "Name" claim (which is the Windows username in the form `[Domain]\[Username]`), create the following class:</span></span>

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

<span data-ttu-id="d18dd-137">Anziché `ClaimTypes.Name`, è possibile usare qualsiasi valore compreso il `User` (ad esempio l'identificatore SID di Windows, ecc.).</span><span class="sxs-lookup"><span data-stu-id="d18dd-137">Rather than `ClaimTypes.Name`, you can use any value from the `User` (such as the Windows SID identifier, etc.).</span></span>

> [!NOTE]
> <span data-ttu-id="d18dd-138">Il valore che scelto deve essere univoco tra tutti gli utenti nel sistema.</span><span class="sxs-lookup"><span data-stu-id="d18dd-138">The value you choose must be unique among all the users in your system.</span></span> <span data-ttu-id="d18dd-139">In caso contrario, un messaggio destinato a un utente può arrivare passare a un altro utente.</span><span class="sxs-lookup"><span data-stu-id="d18dd-139">Otherwise, a message intended for one user could end up going to a different user.</span></span>

<span data-ttu-id="d18dd-140">Registrare il componente nel `Startup.ConfigureServices` metodo **dopo** la chiamata a `.AddSignalR`</span><span class="sxs-lookup"><span data-stu-id="d18dd-140">Register this component in your `Startup.ConfigureServices` method **after** the call to `.AddSignalR`</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a><span data-ttu-id="d18dd-141">Autorizzare gli utenti per hub di accesso e i metodi dell'hub</span><span class="sxs-lookup"><span data-stu-id="d18dd-141">Authorize users to access hubs and hub methods</span></span>

<span data-ttu-id="d18dd-142">Per impostazione predefinita, tutti i metodi in un hub possono essere chiamati da un utente non autenticato.</span><span class="sxs-lookup"><span data-stu-id="d18dd-142">By default, all methods in a hub can be called by an unauthenticated user.</span></span> <span data-ttu-id="d18dd-143">Per richiedere l'autenticazione, si applicano i [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attributo all'hub:</span><span class="sxs-lookup"><span data-stu-id="d18dd-143">In order to require authentication, apply the [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attribute to the hub:</span></span>

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

<span data-ttu-id="d18dd-144">È possibile usare le proprietà della e argomenti del costruttore di `[Authorize]` attributo per limitare l'accesso solo agli utenti di corrispondenza specifiche [i criteri di autorizzazione](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="d18dd-144">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="d18dd-145">Ad esempio, se si dispone di un criterio di autorizzazione personalizzato chiamato `MyAuthorizationPolicy` è possibile garantire che solo gli utenti che Criteri di corrispondenza possono accedere all'hub usando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="d18dd-145">For example, if you have a custom authorization policy called `MyAuthorizationPolicy` you can ensure that only users matching that policy can access the hub using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

<span data-ttu-id="d18dd-146">Metodi dell'hub sul singolo possono avere il `[Authorize]` attributo viene applicato anche.</span><span class="sxs-lookup"><span data-stu-id="d18dd-146">Individual hub methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="d18dd-147">Se l'utente corrente non corrisponde i criteri applicati al metodo, viene restituito un errore al chiamante:</span><span class="sxs-lookup"><span data-stu-id="d18dd-147">If the current user doesn't match the policy applied to the method, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class ChatHub: Hub
{
    public async Task Send(string message)
    {
        // ... send a message to all users ...
    }

    [Authorize("Administrators")]
    public void BanUser(string userName)
    {
        // ... ban a user from the chat room (something only Administrators can do) ...
    }
}
```
