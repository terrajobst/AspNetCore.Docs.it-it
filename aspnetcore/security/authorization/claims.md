---
title: Autorizzazione basata sulle attestazioni in ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere i controlli di attestazioni per l'autorizzazione in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336304"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="2dde4-103">Autorizzazione basata sulle attestazioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dde4-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="2dde4-104">Quando viene creata un'identità è possibile assegnare una o più attestazioni emesso da un'entità attendibile.</span><span class="sxs-lookup"><span data-stu-id="2dde4-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="2dde4-105">Un'attestazione è una coppia nome / valore che rappresenta il soggetto è, non quali l'oggetto è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="2dde4-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="2dde4-106">Ad esempio, è possibile di Guida patente, emesso da un'autorità di licenza Guida locale.</span><span class="sxs-lookup"><span data-stu-id="2dde4-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="2dde4-107">Di Guida la patente presenta la data di nascita.</span><span class="sxs-lookup"><span data-stu-id="2dde4-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="2dde4-108">In questo caso il nome di attestazione sarebbe `DateOfBirth`, il valore dell'attestazione sarebbe ad esempio la data di nascita, `8th June 1970` e l'autorità emittente sarebbe l'autorità di licenza determinante.</span><span class="sxs-lookup"><span data-stu-id="2dde4-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="2dde4-109">Nella forma più semplice, l'autorizzazione basata sulle attestazioni, controlla il valore di un'attestazione e consente l'accesso a una risorsa in base al valore.</span><span class="sxs-lookup"><span data-stu-id="2dde4-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="2dde4-110">Per esempio, se si desidera che l'accesso a un'associazione di notte il processo di autorizzazione potrebbe essere:</span><span class="sxs-lookup"><span data-stu-id="2dde4-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="2dde4-111">Responsabile della sicurezza della porta restituirà il valore della tua data di nascita attestazione e se devono considerare attendibili l'autorità emittente (l'autorità licenza determinante) prima che concede che l'accesso.</span><span class="sxs-lookup"><span data-stu-id="2dde4-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="2dde4-112">Un'identità può contenere più attestazioni con più valori e può contenere più attestazioni dello stesso tipo.</span><span class="sxs-lookup"><span data-stu-id="2dde4-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="2dde4-113">Aggiunta di controlli di attestazioni</span><span class="sxs-lookup"><span data-stu-id="2dde4-113">Adding claims checks</span></span>

<span data-ttu-id="2dde4-114">Attestazione controlli di autorizzazione di base sono dichiarativi, lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, specificare le attestazioni che l'utente corrente deve possedere e, facoltativamente, il valore attestazione deve contenere per l'accesso di risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="2dde4-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="2dde4-115">Attestazioni requisiti sono basati su criteri, lo sviluppatore deve creare e registrare un criterio di esprimere i requisiti di attestazioni.</span><span class="sxs-lookup"><span data-stu-id="2dde4-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="2dde4-116">Il tipo più semplice di criteri Cerca la presenza di un'attestazione di attestazione e non verifica il valore.</span><span class="sxs-lookup"><span data-stu-id="2dde4-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="2dde4-117">Innanzitutto è necessario compilare e registrare i criteri.</span><span class="sxs-lookup"><span data-stu-id="2dde4-117">First you need to build and register the policy.</span></span> <span data-ttu-id="2dde4-118">Il processo viene eseguito come parte della configurazione del servizio di autorizzazione, che in genere fa parte di `ConfigureServices()` nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="2dde4-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="2dde4-119">In questo caso il `EmployeeOnly` criteri consentono di controllare la presenza di un `EmployeeNumber` attestazione all'identità corrente.</span><span class="sxs-lookup"><span data-stu-id="2dde4-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="2dde4-120">È quindi applicare il criterio utilizzando il `Policy` proprietà il `AuthorizeAttribute` attributo per specificare il nome dei criteri;</span><span class="sxs-lookup"><span data-stu-id="2dde4-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="2dde4-121">Il `AuthorizeAttribute` attributo può essere applicato a un controller intero, in questa istanza, solo i criteri di corrispondenza di identità potrà accedere a qualsiasi azione nel controller.</span><span class="sxs-lookup"><span data-stu-id="2dde4-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="2dde4-122">Se si dispone di un controller che è protetta dal `AuthorizeAttribute` attributo, ma si desidera consentire l'accesso anonimo a specifiche azioni si applica il `AllowAnonymousAttribute` attributo.</span><span class="sxs-lookup"><span data-stu-id="2dde4-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="2dde4-123">La maggior parte delle attestazioni vengono forniti con un valore.</span><span class="sxs-lookup"><span data-stu-id="2dde4-123">Most claims come with a value.</span></span> <span data-ttu-id="2dde4-124">Quando si crea il criterio, è possibile specificare un elenco di valori consentiti.</span><span class="sxs-lookup"><span data-stu-id="2dde4-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="2dde4-125">Nell'esempio seguente viene completata solo per i dipendenti il cui numero di dipendenti è 1, 2, 3, 4 o 5.</span><span class="sxs-lookup"><span data-stu-id="2dde4-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="2dde4-126">Aggiungere un controllo di attestazione generico</span><span class="sxs-lookup"><span data-stu-id="2dde4-126">Add a generic claim check</span></span>

<span data-ttu-id="2dde4-127">Se il valore dell'attestazione non è un valore singolo o una trasformazione è necessario, utilizzare [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="2dde4-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="2dde4-128">Per altre informazioni, vedere [utilizzando un oggetto func per soddisfare un criterio](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="2dde4-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="2dde4-129">Più la valutazione dei criteri</span><span class="sxs-lookup"><span data-stu-id="2dde4-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="2dde4-130">Se più criteri si applicano a un controller o un'azione, tutti i criteri devono passare prima che venga concesso l'accesso.</span><span class="sxs-lookup"><span data-stu-id="2dde4-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="2dde4-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2dde4-131">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="2dde4-132">Nell'esempio precedente qualsiasi identità che soddisfa la `EmployeeOnly` criteri possono accedere il `Payslip` azione come tale criterio viene imposto per il controller.</span><span class="sxs-lookup"><span data-stu-id="2dde4-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="2dde4-133">Tuttavia per chiamare il `UpdateSalary` azione deve soddisfare l'identità *entrambi* il `EmployeeOnly` criteri e `HumanResources` criteri.</span><span class="sxs-lookup"><span data-stu-id="2dde4-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="2dde4-134">Se si desidera criteri più complessi, ad esempio l'esecuzione di una data di nascita attestazione, il calcolo di un'età da quest'ultimo verifica la validità è 21 o meno recenti quindi è necessario scrivere [gestori di criteri personalizzati](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="2dde4-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
