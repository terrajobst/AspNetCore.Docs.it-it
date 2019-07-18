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
# <a name="authentication-and-authorization-in-grpc-for-aspnet-core"></a>Autenticazione e autorizzazione in gRPC per ASP.NET Core

Di [James Newton-King](https://twitter.com/jamesnk)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/grpc/authn-and-authz/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="authenticate-users-calling-a-grpc-service"></a>Autenticare gli utenti che chiamano un servizio gRPC

gRPC può essere usato con [l'autenticazione ASP.NET Core](xref:security/authentication/identity) per associare un utente a ogni chiamata.

Di `Startup.Configure` seguito è riportato un esempio che usa gRPC e l'autenticazione ASP.NET Core:

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
> L'ordine in cui si registra il middleware di autenticazione ASP.NET Core è importante. Chiamare `UseAuthentication` sempre e `UseAuthorization` dopo `UseRouting` e prima`UseEndpoints`di.

Al termine dell'installazione dell'autenticazione, l'utente può accedere ai metodi del servizio gRPC tramite `ServerCallContext`.

```csharp
public override Task<BuyTicketsResponse> BuyTickets(
    BuyTicketsRequest request, ServerCallContext context)
{
    var user = context.GetHttpContext().User;

    // ... access data from ClaimsPrincipal ...
}

```

### <a name="bearer-token-authentication"></a>Autenticazione del token di porta

Il client può fornire un token di accesso per l'autenticazione. Il server convalida il token e lo usa per identificare l'utente.

Nel server bearer token autenticazione viene configurata con il [middleware di JWT Bearer](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

Nel client .NET gRPC, il token può essere inviato con chiamate come intestazione:

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

### <a name="client-certificate-authentication"></a>Autenticazione del certificato client

Un client può in alternativa fornire un certificato client per l'autenticazione. L' [autenticazione del certificato](https://tools.ietf.org/html/rfc5246#section-7.4.4) viene eseguita a livello di TLS, molto prima che venga mai ASP.NET Core. Quando la richiesta entra ASP.NET Core, il [pacchetto di autenticazione del certificato client](xref:security/authentication/certauth) consente di risolvere il certificato in `ClaimsPrincipal`un.

> [!NOTE]
> L'host deve essere configurato per accettare i certificati client. Vedere [configurare l'host per richiedere i certificati](xref:security/authentication/certauth#configure-your-host-to-require-certificates) per informazioni sull'accettazione dei certificati client in gheppio, IIS e Azure.

Nel client .NET gRPC, il certificato client viene aggiunto a `HttpClientHandler` che viene quindi usato per creare il client di gRPC:

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

### <a name="other-authentication-mechanisms"></a>Altri meccanismi di autenticazione

Oltre a bearer token e l'autenticazione del certificato client, tutti ASP.NET Core meccanismi di autenticazione supportati, ad esempio OAuth, OpenID e Negotiate, dovrebbero funzionare con gRPC. Per ulteriori informazioni sulla configurazione dell'autenticazione sul lato server, vedere [ASP.NET Core Authentication](xref:security/authentication/identity) .

La configurazione lato client dipende dal meccanismo di autenticazione utilizzato. Gli esempi precedenti di autenticazione dei certificati client e bearer token illustrano due modi per configurare il client gRPC per l'invio di metadati di autenticazione con chiamate gRPC:

* I client gRPC fortemente tipizzati utilizzano `HttpClient` internamente. È possibile configurare l'autenticazione [`HttpClientHandler`](/dotnet/api/system.net.http.httpclienthandler)in o aggiungere istanze personalizzate [`HttpMessageHandler`](/dotnet/api/system.net.http.httpmessagehandler) a `HttpClient`.
* Ogni chiamata a gRPC ha un `CallOptions` argomento facoltativo. Le intestazioni personalizzate possono essere inviate tramite la raccolta di intestazioni dell'opzione.

## <a name="authorize-users-to-access-services-and-service-methods"></a>Autorizzare gli utenti ad accedere ai servizi e ai metodi del servizio

Per impostazione predefinita, tutti i metodi di un servizio possono essere chiamati da utenti non autenticati. Per richiedere l'autenticazione, applicare l'attributo [[autorizzate]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) al servizio:

```csharp
[Authorize]
public class TicketerService : Ticketer.TicketerBase
{
}
```

È possibile utilizzare gli argomenti del costruttore e le proprietà `[Authorize]` dell'attributo per limitare l'accesso solo agli utenti che corrispondono a [criteri di autorizzazione](xref:security/authorization/policies)specifici. Se, ad esempio, si dispone di un criterio di `MyAuthorizationPolicy`autorizzazione personalizzato denominato, assicurarsi che solo gli utenti che corrispondono a tale criterio possano accedere al servizio usando il codice seguente:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class TicketerService : Ticketer.TicketerBase
{
}
```

È possibile applicare anche l'attributo `[Authorize]` a singoli metodi del servizio. Se l'utente corrente non corrisponde ai criteri applicati **sia** al metodo che alla classe, al chiamante viene restituito un errore:

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

## <a name="additional-resources"></a>Risorse aggiuntive

* [Autenticazione del token di porta in ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2016/10/27/bearer-token-authentication-in-asp-net-core/)
* [Configurare l'autenticazione del certificato client in ASP.NET Core](xref:security/authentication/certauth)
