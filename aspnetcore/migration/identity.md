---
title: Eseguire la migrazione di autenticazione e identità ad ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di autenticazione e identità da un progetto ASP.NET MVC per un progetto ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891878"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="cd398-103">Eseguire la migrazione di autenticazione e identità ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd398-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="cd398-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cd398-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cd398-105">Nell'articolo precedente, abbiamo [migrati configurazione da un progetto ASP.NET MVC ad ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="cd398-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="cd398-106">In questo articolo, la migrazione delle funzionalità di gestione di registrazione, accesso e utente.</span><span class="sxs-lookup"><span data-stu-id="cd398-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="cd398-107">Configurazione di identità e appartenenza</span><span class="sxs-lookup"><span data-stu-id="cd398-107">Configure Identity and Membership</span></span>

<span data-ttu-id="cd398-108">In ASP.NET MVC, funzionalità di autenticazione e identità vengono configurate mediante ASP.NET Identity nelle *Startup.Auth.cs* e *IdentityConfig.cs*, che si trova nel *App_Start* cartella.</span><span class="sxs-lookup"><span data-stu-id="cd398-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="cd398-109">In ASP.NET Core MVC, queste funzionalità sono configurate nel *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="cd398-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="cd398-110">Installare il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd398-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="cd398-111">Quindi, aprire *Startup.cs* e aggiornare il `Startup.ConfigureServices` metodo per utilizzare i servizi di identità e di Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="cd398-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

<span data-ttu-id="cd398-112">A questo punto, sono disponibili due tipi riferimento nel codice precedente che è ancora stato ancora migrati dal progetto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="cd398-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="cd398-113">Creare una nuova *modelli* cartella in ASP.NET Core di progetto, due classi per aggiungervi corrispondenti a questi tipi.</span><span class="sxs-lookup"><span data-stu-id="cd398-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="cd398-114">Si noterà ASP.NET MVC versioni di queste classi in */Models/IdentityModels.cs*, ma si userà un file per ogni classe nel progetto migrato dal momento che è più chiaro.</span><span class="sxs-lookup"><span data-stu-id="cd398-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="cd398-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd398-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="cd398-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="cd398-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

<span data-ttu-id="cd398-117">Il progetto Web ASP.NET Core MVC Starter non include molte personalizzazioni degli utenti, o `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd398-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="cd398-118">Durante la migrazione di una vera app, è anche necessario eseguire la migrazione di tutte le proprietà personalizzate e i metodi dell'utente dell'app e `DbContext` classi, nonché a eventuali altre classi di modello Usa l'app.</span><span class="sxs-lookup"><span data-stu-id="cd398-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="cd398-119">Ad esempio, se il `DbContext` ha un `DbSet<Album>`, è necessario eseguire la migrazione di `Album` classe.</span><span class="sxs-lookup"><span data-stu-id="cd398-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="cd398-120">Questi file sul posto, il *Startup.cs* siano possibili dei file da compilare tramite l'aggiornamento relativo `using` istruzioni:</span><span class="sxs-lookup"><span data-stu-id="cd398-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="cd398-121">L'app è ora pronto per supportare l'autenticazione e i servizi di identità.</span><span class="sxs-lookup"><span data-stu-id="cd398-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="cd398-122">È sufficiente disporre di queste funzionalità esposte agli utenti.</span><span class="sxs-lookup"><span data-stu-id="cd398-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="cd398-123">Eseguire la migrazione di registrazione e account di accesso per la logica</span><span class="sxs-lookup"><span data-stu-id="cd398-123">Migrate registration and login logic</span></span>

<span data-ttu-id="cd398-124">Con servizi di identità configurati per l'app e l'accesso ai dati configurati tramite Entity Framework e SQL Server, siamo pronti per aggiungere il supporto per la registrazione e account di accesso all'app.</span><span class="sxs-lookup"><span data-stu-id="cd398-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="cd398-125">È importante ricordare che [precedenti nel processo di migrazione](xref:migration/mvc#migrate-the-layout-file) è un riferimento a come commento *loginpartial* nelle *layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cd398-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="cd398-126">È ora possibile tornare al codice, rimuovere i commenti e aggiungere necessari sui controller e visualizzazioni per supportare la funzionalità di accesso.</span><span class="sxs-lookup"><span data-stu-id="cd398-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="cd398-127">Rimuovere il commento il `@Html.Partial` linea *layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="cd398-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="cd398-128">A questo punto, aggiungere una nuova visualizzazione Razor chiamata *loginpartial* per il *Views/Shared* cartella:</span><span class="sxs-lookup"><span data-stu-id="cd398-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="cd398-129">Update *loginpartial. cshtml* con il codice seguente (sostituire tutti i relativi contenuti):</span><span class="sxs-lookup"><span data-stu-id="cd398-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="cd398-130">A questo punto, è necessario essere in grado di aggiornare il sito nel browser.</span><span class="sxs-lookup"><span data-stu-id="cd398-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="cd398-131">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="cd398-131">Summary</span></span>

<span data-ttu-id="cd398-132">ASP.NET Core introduce modifiche alle funzionalità di ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="cd398-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="cd398-133">In questo articolo, si hanno visto come eseguire la migrazione le funzionalità di gestione utente e l'autenticazione dell'identità di ASP.NET ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd398-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
