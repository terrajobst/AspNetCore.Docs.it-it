---
title: Autenticazione e autorizzazione in gRPC per ASP.NET Core
author: jamesnk
description: Informazioni su come usare l'autenticazione e l'autorizzazione in gRPC per ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 06/07/2019
uid: grpc/authn-and-authz
ms.openlocfilehash: 49024295e4db7976924397bb24567d92d6298562
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308774"
---
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a><span data-ttu-id="28177-103">Autenticazione e autorizzazione in gRPC per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28177-103">Authentication and authorization in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="28177-104">Di [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="28177-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="28177-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="28177-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="authenticate-users-calling-a-grpc-service"></a><span data-ttu-id="28177-106">Autenticare gli utenti che chiamano un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="28177-106">Authenticate users calling a gRPC service</span></span>

<span data-ttu-id="28177-107">gRPC può essere usato con [l'autenticazione ASP.NET Core](xref:security/authentication/identity) per associare un utente a ogni chiamata.</span><span class="sxs-lookup"><span data-stu-id="28177-107">gRPC can be used with [ASP.NET Core authentication](xref:security/authentication/identity) to associate a user with each call.</span></span>

<span data-ttu-id="28177-108">Di `Startup.Configure` seguito è riportato un esempio che usa gRPC e l'autenticazione ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="28177-108">The following is an example of `Startup.Configure` which uses gRPC and ASP.NET Core authentication:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        routes.MapGrpcService<GreeterService>();
    });
}
```

> [!NOTE]
> <span data-ttu-id="28177-109">L'ordine in cui si registra il middleware di autenticazione ASP.NET Core è importante.</span><span class="sxs-lookup"><span data-stu-id="28177-109">The order in which you register the ASP.NET Core authentication middleware matters.</span></span> <span data-ttu-id="28177-110">Chiamare `UseAuthentication` sempre e `UseAuthorization` dopo `UseRouting` e prima`UseEndpoints`di.</span><span class="sxs-lookup"><span data-stu-id="28177-110">Always call `UseAuthentication` and `UseAuthorization` after `UseRouting` and before `UseEndpoints`.</span></span>

<span data-ttu-id="28177-111">Al termine dell'installazione dell'autenticazione, l'utente può accedere ai metodi del servizio gRPC tramite `ServerCallContext`.</span><span class="sxs-lookup"><span data-stu-id="28177-111">Once authentication has been setup, the user can be accessed in a gRPC service methods via the `ServerCallContext`.</span></span>

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a><span data-ttu-id="28177-112">Autenticazione del token di porta</span><span class="sxs-lookup"><span data-stu-id="28177-112">Bearer token authentication</span></span>

<span data-ttu-id="28177-113">Il client può fornire un token di accesso per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="28177-113">The client can provide an access token for authentication.</span></span> <span data-ttu-id="28177-114">Il server convalida il token e lo usa per identificare l'utente.</span><span class="sxs-lookup"><span data-stu-id="28177-114">The server validates the token and uses it to identify the user.</span></span>

<span data-ttu-id="28177-115">Nel server bearer token autenticazione viene configurata con il [middleware di JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span><span class="sxs-lookup"><span data-stu-id="28177-115">On the server, bearer token authentication is configured using the [JWT Bearer middleware](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).</span></span>

<span data-ttu-id="28177-116">Nel client .NET gRPC, il token può essere inviato con chiamate come intestazione:</span><span class="sxs-lookup"><span data-stu-id="28177-116">In the .NET gRPC client, the token can be sent with calls as a header:</span></span>

```csharp
public bool DoAuthenticatedCall(
    Ticketer.TicketerClient client, string token)
{
    var headers = new Metadata();
    headers.Add("Authorization", $"Bearer {token}");

    var request = new BuyTicketsRequest { Count = 1 };
    var response = await client.BuyTicketsAsync(request, headers);

    return response.Success;
}
```

### <a name="client-certificate-authentication"></a><span data-ttu-id="28177-117">Autenticazione del certificato client</span><span class="sxs-lookup"><span data-stu-id="28177-117">Client certificate authentication</span></span>

<span data-ttu-id="28177-118">Un client può in alternativa fornire un certificato client per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="28177-118">A client could alternatively provide a client certificate for authentication.</span></span> <span data-ttu-id="28177-119">L' [autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) viene eseguita a livello di TLS, molto prima che venga mai ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28177-119">[Certificate authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="28177-120">Quando la richiesta entra ASP.NET Core, il [pacchetto di autenticazione del certificato client](xref:security/authentication/certauth) consente di risolvere il certificato in `ClaimsPrincipal`un.</span><span class="sxs-lookup"><span data-stu-id="28177-120">When the request enters ASP.NET Core, the [client certificate authentication package](xref:security/authentication/certauth) allows you to resolve the certificate to a `ClaimsPrincipal`.</span></span>

> [!NOTE]
> <span data-ttu-id="28177-121">L'host deve essere configurato per accettare i certificati client.</span><span class="sxs-lookup"><span data-stu-id="28177-121">The host needs to be configured to accept client certificates.</span></span> <span data-ttu-id="28177-122">Vedere [configurare l'host per richiedere i certificati](xref:security/authentication/certauth#configure-your-host-to-require-certificates) per informazioni sull'accettazione dei certificati client in gheppio, IIS e Azure.</span><span class="sxs-lookup"><span data-stu-id="28177-122">See [configure your host to require certificates](xref:security/authentication/certauth#configure-your-host-to-require-certificates) for information on accepting client certificates in Kestrel, IIS and Azure.</span></span>

<span data-ttu-id="28177-123">Nel client .NET gRPC, il certificato client viene aggiunto a `HttpClientHandler` che viene quindi usato per creare il client di gRPC:</span><span class="sxs-lookup"><span data-stu-id="28177-123">In the .NET gRPC client, the client certificate is added to `HttpClientHandler` that is then used to create the gRPC client:</span></span>

```csharp
public Ticketer.TicketerClient CreateClientWithCert(
    string baseAddress,
    X509Certificate2 certificate)
{
    // Add client cert to the handler
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(certificate);

    // Create the gRPC client
    var httpClient = new HttpClient(handler);
    httpClient.BaseAddress = new Uri(baseAddress);

    return GrpcClient.Create<Ticketer.TicketerClient>(httpClient);
}
```

### <a name="other-authentication-mechanisms"></a><span data-ttu-id="28177-124">Altri meccanismi di autenticazione</span><span class="sxs-lookup"><span data-stu-id="28177-124">Other authentication mechanisms</span></span>

<span data-ttu-id="28177-125">Oltre a bearer token e l'autenticazione del certificato client, tutti ASP.NET Core meccanismi di autenticazione supportati, ad esempio OAuth, OpenID e Negotiate, dovrebbero funzionare con gRPC.</span><span class="sxs-lookup"><span data-stu-id="28177-125">In addition to bearer token and client certificate authentication, all ASP.NET Core supported authentication mechanisms such as OAuth, OpenID and Negotiate should work with gRPC.</span></span> <span data-ttu-id="28177-126">Per ulteriori informazioni sulla configurazione dell'autenticazione sul lato server, vedere [ASP.NET Core Authentication](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="28177-126">Visit [ASP.NET Core authentication](xref:security/authentication/identity) for more information for configuring authentication on the server side.</span></span>

<span data-ttu-id="28177-127">La configurazione lato client dipende dal meccanismo di autenticazione utilizzato.</span><span class="sxs-lookup"><span data-stu-id="28177-127">Client side configuration will depend on the authentication mechanism you are using.</span></span> <span data-ttu-id="28177-128">Gli esempi precedenti di autenticazione dei certificati client e bearer token illustrano due modi per configurare il client gRPC per l'invio di metadati di autenticazione con chiamate gRPC:</span><span class="sxs-lookup"><span data-stu-id="28177-128">The previous bearer token and client certificate authentication examples show a couple of ways the gRPC client can be configured to send authentication metadata with gRPC calls:</span></span>

* <span data-ttu-id="28177-129">I client gRPC fortemente tipizzati utilizzano `HttpClient` internamente.</span><span class="sxs-lookup"><span data-stu-id="28177-129">Strongly typed gRPC clients use `HttpClient` internally.</span></span> <span data-ttu-id="28177-130">È possibile configurare l'autenticazione [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)in o aggiungere istanze personalizzate [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) a `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="28177-130">Authentication can be configured on [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler), or by adding custom [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) instances to the `HttpClient`.</span></span>
* <span data-ttu-id="28177-131">Ogni chiamata a gRPC ha un `CallOptions` argomento facoltativo.</span><span class="sxs-lookup"><span data-stu-id="28177-131">Each gRPC call has an optional `CallOptions` argument.</span></span> <span data-ttu-id="28177-132">Le intestazioni personalizzate possono essere inviate tramite la raccolta di intestazioni dell'opzione.</span><span class="sxs-lookup"><span data-stu-id="28177-132">Custom headers can be sent using the option's headers collection.</span></span>

## <a name="authorize-users-to-access-services-and-service-methods"></a><span data-ttu-id="28177-133">Autorizzare gli utenti ad accedere ai servizi e ai metodi del servizio</span><span class="sxs-lookup"><span data-stu-id="28177-133">Authorize users to access services and service methods</span></span>

<span data-ttu-id="28177-134">Per impostazione predefinita, tutti i metodi di un servizio possono essere chiamati da utenti non autenticati.</span><span class="sxs-lookup"><span data-stu-id="28177-134">By default, all methods in a service can be called by unauthenticated users.</span></span> <span data-ttu-id="28177-135">Per richiedere l'autenticazione, applicare l'attributo [[autorizzate]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) al servizio:</span><span class="sxs-lookup"><span data-stu-id="28177-135">To require authentication, apply the [[Authorize]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute to the service:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="28177-136">È possibile utilizzare gli argomenti del costruttore e le proprietà `[Authorize]` dell'attributo per limitare l'accesso solo agli utenti che corrispondono a [criteri di autorizzazione](xref:security/authorization/policies)specifici.</span><span class="sxs-lookup"><span data-stu-id="28177-136">You can use the constructor arguments and properties of the `[Authorize]` attribute to restrict access to only users matching specific [authorization policies](xref:security/authorization/policies).</span></span> <span data-ttu-id="28177-137">Se, ad esempio, si dispone di un criterio di `MyAuthorizationPolicy`autorizzazione personalizzato denominato, assicurarsi che solo gli utenti che corrispondono a tale criterio possano accedere al servizio usando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="28177-137">For example, if you have a custom authorization policy called `MyAuthorizationPolicy`, ensure that only users matching that policy can access the service using the following code:</span></span>

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

<span data-ttu-id="28177-138">È possibile applicare anche l'attributo `[Authorize]` a singoli metodi del servizio.</span><span class="sxs-lookup"><span data-stu-id="28177-138">Individual service methods can have the `[Authorize]` attribute applied as well.</span></span> <span data-ttu-id="28177-139">Se l'utente corrente non corrisponde ai criteri applicati **sia** al metodo che alla classe, al chiamante viene restituito un errore:</span><span class="sxs-lookup"><span data-stu-id="28177-139">If the current user doesn't match the policies applied to **both** the method and the class, an error is returned to the caller:</span></span>

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
    public override Task<AvailableTicketsResponse> GetAvailableTickets(
        Empty request, ServerCallContext context)
    {
        // ... buy tickets for the current user ...
    }

    [Authorize("Administrators")]
    public override Task<BuyTicketsResponse> RefundTickets(
        BuyTicketsRequest request, ServerCallContext context)
    {
        // ... refund tickets (something only Administrators can do) ..
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="28177-140">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="28177-140">Additional resources</span></span>

* [<span data-ttu-id="28177-141">Autenticazione del token di porta in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28177-141">Bearer Token authentication in ASP.NET Core</span></span>](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [<span data-ttu-id="28177-142">Configurare l'autenticazione del certificato client in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28177-142">Configure Client Certificate authentication in ASP.NET Core</span></span>](xref:security/authentication/certauth)
