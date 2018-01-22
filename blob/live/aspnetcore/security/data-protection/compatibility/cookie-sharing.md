---
title: La condivisione dei cookie tra applicazioni
author: rick-anderson
description: Questo documento viene illustrato come lo stack di protezione dati supporta la condivisione dei cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 0cbf5a3e9dfe8f99433800ac5c10ed36b4de6527
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="26180-103">La condivisione dei cookie tra applicazioni</span><span class="sxs-lookup"><span data-stu-id="26180-103">Sharing cookies between applications</span></span>

<span data-ttu-id="26180-104">Siti Web in genere costituiti da numerose singole applicazioni web funzionante tutti insieme harmoniously.</span><span class="sxs-lookup"><span data-stu-id="26180-104">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="26180-105">Se uno sviluppatore di applicazioni deve fornire un'esperienza ottimale single sign-on, sarà spesso necessario tutte le applicazioni web diverse all'interno del sito per condividere i ticket di autenticazione reciproca.</span><span class="sxs-lookup"><span data-stu-id="26180-105">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="26180-106">Per supportare questo scenario, lo stack di protezione dati consente la condivisione di autenticazione dei cookie Katana e il ticket di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="26180-106">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="26180-107">Condividere i cookie di autenticazione tra applicazioni</span><span class="sxs-lookup"><span data-stu-id="26180-107">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="26180-108">Per condividere i cookie di autenticazione tra due diverse applicazioni ASP.NET Core, configurare ciascuna applicazione che deve condividere i cookie, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="26180-108">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="26180-109">Nel configurare (metodo), utilizzare il CookieAuthenticationOptions per configurare il servizio di protezione dati per i cookie e di AuthenticationScheme in modo che corrisponda ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="26180-109">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="26180-110">Se si utilizza l'identità:</span><span class="sxs-lookup"><span data-stu-id="26180-110">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="26180-111">Se si utilizza direttamente i cookie:</span><span class="sxs-lookup"><span data-stu-id="26180-111">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="26180-112">Il `DataProtectionProvider` richiede il `Microsoft.AspNetCore.DataProtection.Extensions` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="26180-112">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="26180-113">Quando utilizzato in questo modo, DirectoryInfo deve puntare a un percorso di archiviazione chiavi specificamente riservare per i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="26180-113">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="26180-114">Il middleware di autenticazione cookie userà l'implementazione fornita in modo esplicito di DataProtectionProvider, che ora è isolato dal sistema di protezione di dati utilizzato da altre parti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26180-114">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="26180-115">Il nome dell'applicazione viene ignorato (intenzionalmente in questo caso, poiché si sta tentando di ottenere più applicazioni possono condividere payload).</span><span class="sxs-lookup"><span data-stu-id="26180-115">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="26180-116">È consigliabile configurare il DataProtectionProvider in modo che le chiavi vengono crittografate a riposo, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="26180-116">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="26180-117">Condividere i cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26180-117">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="26180-118">Applicazioni di 4. x ASP.NET che utilizzano Katana cookie middleware di autenticazione possono essere configurate per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="26180-118">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="26180-119">In questo modo l'aggiornamento a fasi di singole applicazioni del sito di grandi dimensioni pur fornendo un unico segno smooth sull'esperienza nell'intero sito.</span><span class="sxs-lookup"><span data-stu-id="26180-119">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="26180-120">È possibile indicare se l'applicazione esistente Usa middleware di autenticazione cookie Katana dall'esistenza di una chiamata a UseCookieAuthentication in Startup.Auth.cs del progetto.</span><span class="sxs-lookup"><span data-stu-id="26180-120">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="26180-121">Progetti applicazione web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive, utilizzare il middleware di autenticazione del cookie Katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="26180-121">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="26180-122">Applicazione ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versioni successive, in caso contrario i pacchetti NuGet necessari installazione avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="26180-122">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="26180-123">Per condividere i cookie di autenticazione tra applicazioni ASP.NET 4. x e le applicazioni ASP.NET Core, configurare l'applicazione ASP.NET di base, come descritto in precedenza, quindi configurare le applicazioni di ASP.NET 4. x seguendo la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="26180-123">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="26180-124">Installare il pacchetto Microsoft.Owin.Security.Interop in ognuna delle applicazioni ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="26180-124">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="26180-125">In Startup.Auth.cs, individuare la chiamata a UseCookieAuthentication, che in genere avrà un aspetto simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="26180-125">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="26180-126">Modificare la chiamata a UseCookieAuthentication come indicato di seguito, modificando il CookieName per corrispondere al nome utilizzato per il middleware di autenticazione ASP.NET Core cookie, fornendo un'istanza di un DataProtectionProvider che è stata inizializzata in un percorso di archiviazione delle chiavi, e impostata CookieManager ChunkingCookieManager interoperabilità il formato di suddivisione in blocchi è compatibile.</span><span class="sxs-lookup"><span data-stu-id="26180-126">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    <span data-ttu-id="26180-127">DirectoryInfo contiene puntare alla stessa posizione di archiviazione a cui fa riferimento l'applicazione ASP.NET di base e deve essere configurato usando le stesse impostazioni.</span><span class="sxs-lookup"><span data-stu-id="26180-127">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="26180-128">In ASP.NET 4. x e le applicazioni ASP.NET Core siano configurate per condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="26180-128">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="26180-129">È necessario assicurarsi che il sistema di identità per ogni applicazione fa riferimento allo stesso database utente.</span><span class="sxs-lookup"><span data-stu-id="26180-129">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="26180-130">In caso contrario il sistema di identità genererà errori in fase di esecuzione quando tenta di far corrispondere le informazioni nel cookie di autenticazione e le informazioni sul proprio database.</span><span class="sxs-lookup"><span data-stu-id="26180-130">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
