---
title: Autenticazione e autorizzazione in ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come usare l'autenticazione e autorizzazione in ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/authn-and-authz
ms.openlocfilehash: d4259e04a0e3bb9ff517a10465323ccb5e2895a5
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095171"
---
# <a name="authentication-and-authorization-in-aspnet-core-signalr"></a>Autenticazione e autorizzazione in ASP.NET Core SignalR

Da [Andrew Stanton-Nurse](https://twitter.com/anurse)

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/authn-and-authz/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="authenticate-users-connecting-to-a-signalr-hub"></a>Eseguire l'autenticazione di utenti che si connettono a un hub SignalR

Può essere utilizzato con SignalR [autenticazione di ASP.NET Core](xref:security/authentication/index) per associare un utente a ogni connessione. In un hub, i dati di autenticazione è possibile accedere dal [ `HubConnectionContext.User` ](/dotnet/api/microsoft.aspnetcore.signalr.hubconnectioncontext.user) proprietà. L'autenticazione consente l'hub chiamare i metodi in tutte le connessioni associate a un utente (vedere [gestire utenti e gruppi in SignalR](xref:signalr/groups) per altre informazioni). Più connessioni possono essere associate a un singolo utente.

### <a name="cookie-authentication"></a>Autenticazione tramite cookie

In un'app basata su browser, l'autenticazione tramite cookie consente le credenziali utente esistenti propagare automaticamente per le connessioni SignalR. Quando si usa il browser client, è necessaria alcuna configurazione aggiuntiva. Se l'utente è connesso all'App, la connessione SignalR eredita automaticamente questa autenticazione.

L'autenticazione dei cookie non è consigliato, a meno che l'app è sufficiente autenticare gli utenti dal browser client. Quando si usa la [Client .NET](xref:signalr/dotnet-client), il `Cookies` proprietà può essere configurata nel `.WithUrl` chiamata per fornire un cookie. Tuttavia, utilizzando l'autenticazione dei cookie del client .NET richiede l'app per fornire un'API per lo scambio di dati di autenticazione per un cookie.

### <a name="bearer-token-authentication"></a>Autenticazione del bearer token

Autenticazione del bearer token è l'approccio consigliato quando si usano client diversi da client browser. Questo approccio, il client fornisce un token di accesso che il server di convalida e utilizza per identificare l'utente. I dettagli dell'autenticazione del bearer token non rientrano nell'ambito di questo documento. Nel server di autenticazione del bearer token viene configurato usando le [middleware del Bearer token JWT](/dotnet/api/microsoft.extensions.dependencyinjection.jwtbearerextensions.addjwtbearer).

Nel client JavaScript, il token può essere specificato con il [accessTokenFactory](xref:signalr/configuration#configure-bearer-authentication) opzione.

[!code-typescript[Configure Access Token](authn-and-authz/sample/wwwroot/js/chat.ts?range=63-65)]

Nel client di .NET, è presente un simile [AccessTokenProvider](xref:signalr/configuration#configure-bearer-authentication) proprietà che può essere usata per configurare il token:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options =>
    { 
        options.AccessTokenProvider = () => _myAccessToken;
    })
    .Build();
```

> [!NOTE]
> Prima viene chiamata la funzione di token di accesso è fornire **ogni** richiesta HTTP effettuata da SignalR. Se è necessario rinnovare il token per mantenere attiva la connessione (in quanto può scadere durante la connessione), all'interno di questa funzione e restituire il token aggiornato.

Nell'API web standard, vengono inviati i token di connessione in un'intestazione HTTP. SignalR è, tuttavia, non è possibile impostare queste intestazioni nel browser quando si usano alcuni trasporti. Quando si usa WebSocket e Server-Sent eventi, il token viene trasmesso come un parametro di stringa di query. Per supportare questa funzionalità nel server, è necessaria un'ulteriore configurazione:

[!code-csharp[Configure Server to accept access token from Query String](authn-and-authz/sample/Startup.cs?name=snippet)]

### <a name="windows-authentication"></a>Autenticazione di Windows

Se [autenticazione di Windows](xref:security/authentication/windowsauth) è configurato nell'app, SignalR può usare tale identità per proteggere gli hub. Tuttavia, per inviare messaggi a singoli utenti, è necessario aggiungere un provider personalizzato di ID utente. Questo avviene perché il sistema di autenticazione di Windows non fornisce l'attestazione "Name Identifier" che usa SignalR per determinare il nome utente.

Aggiungere una nuova classe che implementa `IUserIdProvider` e recuperare una delle attestazioni da parte dell'utente da utilizzare come identificatore. Ad esempio, per usare l'attestazione "Name" (ovvero il nome utente di Windows nel formato `[Domain]\[Username]`), creare la classe seguente:

```csharp
public class NameUserIdProvider : IUserIdProvider
{
    public string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Name)?.Value;
    }
}
```

Anziché `ClaimTypes.Name`, è possibile usare qualsiasi valore compreso il `User` (ad esempio l'identificatore SID di Windows, ecc.).

> [!NOTE]
> Il valore che scelto deve essere univoco tra tutti gli utenti nel sistema. In caso contrario, un messaggio destinato a un utente può arrivare passare a un altro utente.

Registrare il componente nel `Startup.ConfigureServices` metodo **dopo** la chiamata a `.AddSignalR`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ... other services ...

    services.AddSignalR();
    services.AddSingleton<IUserIdProvider, NameUserIdProvider>();
}
```

## <a name="authorize-users-to-access-hubs-and-hub-methods"></a>Autorizzare gli utenti per hub di accesso e i metodi dell'hub

Per impostazione predefinita, tutti i metodi in un hub possono essere chiamati da un utente non autenticato. Per richiedere l'autenticazione, si applicano i [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) attributo all'hub:

[!code-csharp[Restrict a hub to only authorized users](authn-and-authz/sample/Hubs/ChatHub.cs?range=8-10,32)]

È possibile usare le proprietà della e argomenti del costruttore di `[Authorize]` attributo per limitare l'accesso solo agli utenti di corrispondenza specifiche [i criteri di autorizzazione](xref:security/authorization/policies). Ad esempio, se si dispone di un criterio di autorizzazione personalizzato chiamato `MyAuthorizationPolicy` è possibile garantire che solo gli utenti che Criteri di corrispondenza possono accedere all'hub usando il codice seguente:

```csharp
[Authorize("MyAuthorizationPolicy")]
public class ChatHub: Hub
{
}
```

Metodi dell'hub sul singolo possono avere il `[Authorize]` attributo viene applicato anche. Se l'utente corrente non corrisponde i criteri applicati al metodo, viene restituito un errore al chiamante:

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
