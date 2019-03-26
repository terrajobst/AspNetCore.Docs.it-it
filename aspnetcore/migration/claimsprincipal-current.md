---
title: Eseguire la migrazione da claimsprincipal. Current
author: mjrousos
description: Informazioni su come eseguire la migrazione da claimsprincipal. Current per recuperare l'identità dell'utente autenticato corrente e le attestazioni in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488642"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Eseguire la migrazione da claimsprincipal. Current

In progetti ASP.NET 4.x, era pratica comune usare [claimsprincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) per recuperare l'oggetto corrente autenticato identità e le attestazioni dell'utente. In ASP.NET Core, questa proprietà non è impostata non è più. Il codice che è stata che dipendono da essa deve essere aggiornato per ottenere l'identità dell'utente autenticato corrente tramite modi diversi.

## <a name="context-specific-data-instead-of-static-data"></a>Dati specifici del contesto anziché i dati statici

Quando si usa ASP.NET Core, i valori di entrambi `ClaimsPrincipal.Current` e `Thread.CurrentPrincipal` non sono impostati. Entrambe le proprietà rappresentano lo stato statico, evitando così di ASP.NET Core a livello generale. È invece architettura ASP.NET Core per recuperare le dipendenze (ad esempio, l'identità dell'utente corrente) da raccolte specifici per il contesto del servizio (con relativo [inserimento delle dipendenze](xref:fundamentals/dependency-injection) modello (inserimento delle dipendenze)). What ' s, ulteriori `Thread.CurrentPrincipal` è statico, thread e pertanto potrebbe non conservare le modifiche in alcuni scenari asincroni (e `ClaimsPrincipal.Current` chiama semplicemente `Thread.CurrentPrincipal` per impostazione predefinita).

Per comprendere gli ordinamenti dei thread dei problemi possono causare i membri statici in scenari asincroni, si consideri il frammento di codice seguente:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

L'esempio di codice precedente imposta `Thread.CurrentPrincipal` e viene controllato il valore prima e dopo l'attesa di una chiamata asincrona. `Thread.CurrentPrincipal` è specifico per il *thread* in cui è impostata e il metodo è probabile che riprendere l'esecuzione in un thread diverso dopo l'attesa. Di conseguenza `Thread.CurrentPrincipal` è presente quando viene dapprima controllata ma è null dopo la chiamata a `await Task.Yield()`.

Introduzione all'identità dell'utente corrente dalla raccolta di servizi di inserimento delle dipendenze dell'app è più testabili, troppo, poiché le identità di test possono essere inserite facilmente.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperare l'utente corrente in un'app ASP.NET Core

Sono disponibili diverse opzioni per il recupero dell'utente autenticato corrente `ClaimsPrincipal` in ASP.NET Core al posto di `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Controller MVC possono accedere l'utente autenticato corrente con loro [utente](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) proprietà.
* **HttpContext.User**. I componenti con accesso all'oggetto corrente `HttpContext` (middleware, ad esempio) è possibile ottenere l'utente corrente `ClaimsPrincipal` dalla [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passato dal chiamante**. Librerie senza accesso all'attuale `HttpContext` sono spesso chiamati dal controller o i componenti middleware e può avere identità dell'utente corrente passato come argomento.
* **IHttpContextAccessor**. Il progetto in fase di migrazione ad ASP.NET Core potrebbe essere troppo grande per essere facilmente passare identità dell'utente corrente per tutte le posizioni necessarie. In questi casi [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) può essere utilizzato come soluzione alternativa. `IHttpContextAccessor` è in grado di accedere a corrente `HttpContext` (se presente). Una soluzione a breve termine per il recupero di identità dell'utente corrente nel codice che non è ancora stato aggiornato per funzionare con architettura basata sull'inserimento delle dipendenze di ASP.NET Core sarebbe:

  * Rendere `IHttpContextAccessor` disponibile nel contenitore di inserimento delle dipendenze chiamando [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.
  * Ottenere un'istanza di `IHttpContextAccessor` durante l'avvio e archiviarlo in una variabile statica. L'istanza viene resa disponibile al codice che in precedenza il recupero dell'utente corrente da una proprietà statica.
  * Recuperare l'utente corrente `ClaimsPrincipal` usando `HttpContextAccessor.HttpContext?.User`. Se questo codice viene usato all'esterno del contesto di una richiesta HTTP, il `HttpContext` è null.

L'elemento finale opzione, tramite un `IHttpContextAccessor` istanza archiviata in una variabile statica, è contraria ai principi di ASP.NET Core (preferendo inserito le dipendenze delle dipendenze statico). Pianificare recuperare eventualmente `IHttpContextAccessor` istanze invece dall'inserimento di dipendenze. Un helper statico può essere un bridge utile, tuttavia, durante la migrazione di grandi dimensioni ASP.NET le app esistenti che in precedenza usava `ClaimsPrincipal.Current`.
