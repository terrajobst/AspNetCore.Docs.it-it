---
title: Eseguire la migrazione da ClaimsPrincipal.Current
author: mjrousos
description: Informazioni sulla migrazione per evitare ClaimsPrincipal.Current per recuperare l'identità dell'utente autenticato corrente e le attestazioni in ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851533"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Eseguire la migrazione da ClaimsPrincipal.Current

Nei progetti ASP.NET, era pratica comune usare [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) per recuperare l'oggetto corrente autenticate identità e le attestazioni dell'utente. In ASP.NET Core, questa proprietà non è impostata. Codice che è stata che dipendono da essa deve essere aggiornato per ottenere l'identità dell'utente autenticato corrente in un modo diverso.

## <a name="context-specific-data-instead-of-static-data"></a>Dati specifici del contesto anziché i dati statici

Quando si utilizza ASP.NET Core, i valori di entrambi `ClaimsPrincipal.Current` e `Thread.CurrentPrincipal` non sono impostati. Queste proprietà rappresentano lo stato statico, si evita in genere ASP.NET Core. Invece, architettura del ASP.NET di base consiste nel recuperare le dipendenze (ad esempio, l'identità dell'utente corrente) dalle raccolte specifiche per il contesto del servizio (tramite il relativo [inserimento di dipendenze](xref:fundamentals/dependency-injection) modello (DI)). Che cos'è più `Thread.CurrentPrincipal` è statico, thread, in modo che potrebbe non conservare le modifiche in alcuni scenari asincroni (e `ClaimsPrincipal.Current` chiama semplicemente `Thread.CurrentPrincipal` per impostazione predefinita).

Per comprendere gli ordinamenti di thread problemi membri statici possono causare in scenari asincroni, si consideri il frammento di codice seguente:

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

L'esempio di codice di esempio precedente imposta `Thread.CurrentPrincipal` e controlla il valore prima e dopo l'attesa di una chiamata asincrona. `Thread.CurrentPrincipal` Specifica per il *thread* in cui è impostato e il metodo è probabile che riprendere l'esecuzione su un thread diverso dopo await. Di conseguenza `Thread.CurrentPrincipal` viene visualizzato quando viene dapprima controllata ma è null dopo la chiamata a `await Task.Yield()`.

Recupero di identità dell'utente corrente dalla raccolta dell'applicazione DI servizio è più testabile, troppo, poiché le identità di test possono essere inserite facilmente.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperare l'utente corrente in un'applicazione ASP.NET Core

Sono disponibili diverse opzioni per il recupero dell'utente autenticato corrente `ClaimsPrincipal` in ASP.NET Core anziché `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Controller MVC può accedere l'utente autenticato corrente con i relativi [utente](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) proprietà.
* **HttpContext**. I componenti con l'accesso all'oggetto corrente `HttpContext` (ad esempio middleware) può ottenere l'utente corrente `ClaimsPrincipal` da [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Passato dal chiamante**. Librerie senza accesso all'oggetto corrente `HttpContext` vengono spesso denominati dal controller o i componenti middleware e può avere identità dell'utente corrente passata come argomento.
* **IHttpContextAccessor**. Il progetto ASP.NET in fase di migrazione per ASP.NET Core può essere troppo grande per essere facilmente passare identità dell'utente corrente per tutte le posizioni necessarie. In questi casi [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) può essere utilizzato come soluzione alternativa. `IHttpContextAccessor` è in grado di accedere all'oggetto `HttpContext` (se presente). Una soluzione a breve termine per ottenere l'identità dell'utente corrente nel codice che non è ancora stato aggiornato per funzionare con architettura basata su del ASP.NET Core sarebbe:

  * Rendere `IHttpContextAccessor` disponibile nel contenitore DI chiamando [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.
  * Ottenere un'istanza di `IHttpContextAccessor` durante l'avvio e archiviarlo in una variabile statica. L'istanza è resa disponibile per il codice che è stato in precedenza il recupero dell'utente corrente da una proprietà statica.
  * Recuperare l'utente corrente `ClaimsPrincipal` utilizzando `HttpContextAccessor.HttpContext?.User`. Se questo codice viene utilizzato all'esterno del contesto di una richiesta HTTP, il `HttpContext` è null.

Finale opzione usando `IHttpContextAccessor`, si applichino principi fondamentali di ASP.NET (preferendo dipendenze inserite dipendenze statiche). Prevede di rimuovere, eventualmente, la dipendenza statica `IHttpContextAccessor` helper. Può trattarsi di un bridge utile, tuttavia, durante la migrazione di grandi dimensioni App ASP.NET esistenti che in precedenza utilizzavano `ClaimsPrincipal.Current`.
