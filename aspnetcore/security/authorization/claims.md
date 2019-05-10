---
title: Autorizzazione basata sulle attestazioni in ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere controlli di attestazioni per l'autorizzazione in un'app ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086159"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="32f7c-103">Autorizzazione basata sulle attestazioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32f7c-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="32f7c-104">Quando viene creata un'identità è possibile assegnare uno o più delle attestazioni rilasciate da un'entità attendibile.</span><span class="sxs-lookup"><span data-stu-id="32f7c-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="32f7c-105">Un'attestazione è una coppia nome-valore che rappresenta il soggetto è, non quali il soggetto è possibile farlo.</span><span class="sxs-lookup"><span data-stu-id="32f7c-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="32f7c-106">Ad esempio, potrebbe essere di Guida una patente, emesso da un'autorità di licenza Guida locale.</span><span class="sxs-lookup"><span data-stu-id="32f7c-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="32f7c-107">Di Guida la patente è la data di nascita su di esso.</span><span class="sxs-lookup"><span data-stu-id="32f7c-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="32f7c-108">In questo caso sarebbe il nome dell'attestazione `DateOfBirth`, il valore dell'attestazione sarà la data di nascita, ad esempio `8th June 1970` e l'autorità di certificazione sarebbe l'autorità di licenza determinante.</span><span class="sxs-lookup"><span data-stu-id="32f7c-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="32f7c-109">Autorizzazione basata sulle attestazioni, nella forma più semplice, controlla il valore dell'attestazione e consente l'accesso a una risorsa in base al valore.</span><span class="sxs-lookup"><span data-stu-id="32f7c-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="32f7c-110">Per esempio, se si desidera che l'accesso a un club di notte il processo di autorizzazione potrebbe essere:</span><span class="sxs-lookup"><span data-stu-id="32f7c-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="32f7c-111">Il responsabile della sicurezza sportello restituirà il valore della data di nascita attestazione e se sono attendibili l'autorità emittente (l'autorità licenza determinante) prima che concede che l'accesso.</span><span class="sxs-lookup"><span data-stu-id="32f7c-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="32f7c-112">Un'identità può contenere più attestazioni con più valori e può contenere più attestazioni dello stesso tipo.</span><span class="sxs-lookup"><span data-stu-id="32f7c-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="32f7c-113">Aggiunta di controlli di attestazioni</span><span class="sxs-lookup"><span data-stu-id="32f7c-113">Adding claims checks</span></span>

<span data-ttu-id="32f7c-114">Attestazione controlli di autorizzazione basata su sono dichiarativi, lo sviluppatore li incorpora all'interno del codice, a fronte di un controller o un'azione all'interno di un controller, che specifica le attestazioni che l'utente corrente deve disporre del privilegio e, facoltativamente, il valore di attestazione deve contenere per l'accesso di risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="32f7c-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="32f7c-115">I requisiti sono criteri basati su attestazioni, lo sviluppatore deve creare e registrare un criterio che esprimono i requisiti di attestazioni.</span><span class="sxs-lookup"><span data-stu-id="32f7c-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="32f7c-116">Il tipo più semplice di criteri Cerca la presenza di un'attestazione di attestazione e non controlla il valore.</span><span class="sxs-lookup"><span data-stu-id="32f7c-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="32f7c-117">Prima di tutto è necessario compilare e registrare i criteri.</span><span class="sxs-lookup"><span data-stu-id="32f7c-117">First you need to build and register the policy.</span></span> <span data-ttu-id="32f7c-118">Questa operazione viene eseguita come parte della configurazione del servizio di autorizzazione, che in genere fa parte di `ConfigureServices()` nella *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="32f7c-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="32f7c-119">In questo caso il `EmployeeOnly` criteri consentono di controllare la presenza di un `EmployeeNumber` attestazioni sull'identità corrente.</span><span class="sxs-lookup"><span data-stu-id="32f7c-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="32f7c-120">È quindi applicare il criterio tramite il `Policy` proprietà il `AuthorizeAttribute` attributo per specificare il nome del criterio;</span><span class="sxs-lookup"><span data-stu-id="32f7c-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="32f7c-121">Il `AuthorizeAttribute` attributo può essere applicato a un intero controller, in questo caso solo le identità di criteri di corrispondenza potrà accedere a qualsiasi azione sul controller.</span><span class="sxs-lookup"><span data-stu-id="32f7c-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="32f7c-122">Se si dispone di un controller protetto con il `AuthorizeAttribute` dell'attributo, ma si vuole consentire l'accesso anonimo ad azioni specifiche si applica il `AllowAnonymousAttribute` attributo.</span><span class="sxs-lookup"><span data-stu-id="32f7c-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="32f7c-123">La maggior parte delle attestazioni dotati di un valore.</span><span class="sxs-lookup"><span data-stu-id="32f7c-123">Most claims come with a value.</span></span> <span data-ttu-id="32f7c-124">Quando si crea il criterio, è possibile specificare un elenco di valori consentiti.</span><span class="sxs-lookup"><span data-stu-id="32f7c-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="32f7c-125">Nell'esempio seguente viene completata solo per i dipendenti il cui numero di dipendenti è 1, 2, 3, 4 o 5.</span><span class="sxs-lookup"><span data-stu-id="32f7c-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="32f7c-126">Aggiungere un controllo di attestazione generico</span><span class="sxs-lookup"><span data-stu-id="32f7c-126">Add a generic claim check</span></span>

<span data-ttu-id="32f7c-127">Se il valore dell'attestazione non è un singolo valore o una trasformazione è obbligatorio, usare [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="32f7c-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="32f7c-128">Per altre informazioni, vedere [usando una funzione per soddisfare un criterio](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="32f7c-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="32f7c-129">Valutazione dei criteri più</span><span class="sxs-lookup"><span data-stu-id="32f7c-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="32f7c-130">Se si applicano più criteri in un controller o azione, tutti i criteri devono passare prima che venga concesso l'accesso.</span><span class="sxs-lookup"><span data-stu-id="32f7c-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="32f7c-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="32f7c-131">For example:</span></span>

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

<span data-ttu-id="32f7c-132">Nell'esempio precedente qualsiasi entità che soddisfa la `EmployeeOnly` criteri possono accedere il `Payslip` azione come tale criterio viene applicato nel controller.</span><span class="sxs-lookup"><span data-stu-id="32f7c-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="32f7c-133">Tuttavia affinché la chiamata di `UpdateSalary` azione l'identità deve soddisfare *entrambi* il `EmployeeOnly` criteri e il `HumanResources` criteri.</span><span class="sxs-lookup"><span data-stu-id="32f7c-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="32f7c-134">Se si desidera che i criteri più complicati, ad esempio richiedendo una data di nascita attestazione, calcolando un'età da quest'ultimo, quindi verifica la validità è 21 o versione precedente, è necessario scrivere [gestori di criteri personalizzate](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="32f7c-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
