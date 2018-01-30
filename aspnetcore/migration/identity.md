---
title: "La migrazione di autenticazione e identità"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="81125-102">La migrazione di autenticazione e identità</span><span class="sxs-lookup"><span data-stu-id="81125-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="81125-103">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="81125-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="81125-104">Nella precedente dell'articolo è [eseguita la migrazione di configurazione da un progetto MVC ASP.NET ad ASP.NET MVC di base](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="81125-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="81125-105">In questo articolo è la migrazione delle funzionalità di gestione utente, account di accesso e registrazione.</span><span class="sxs-lookup"><span data-stu-id="81125-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="81125-106">Configurare l'identità e l'appartenenza</span><span class="sxs-lookup"><span data-stu-id="81125-106">Configure Identity and Membership</span></span>

<span data-ttu-id="81125-107">In ASP.NET MVC, funzionalità di autenticazione e identità sono configurate utilizzando l'identità di ASP.NET in Startup.Auth.cs e IdentityConfig.cs, si trova nella cartella App_Start.</span><span class="sxs-lookup"><span data-stu-id="81125-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="81125-108">In ASP.NET MVC di base, queste funzionalità vengono configurate in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="81125-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="81125-109">Installare il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="81125-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="81125-110">Aprire quindi Startup.cs e aggiornare il `ConfigureServices()` metodo per utilizzare i servizi di identità e di Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="81125-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="81125-111">A questo punto, sono disponibili due tipi a cui fa riferimento nel codice precedente è non ancora migrati dal progetto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="81125-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="81125-112">Creare un nuovo *modelli* in ASP.NET Core cartella del progetto e aggiungere due classi corrispondenti a questi tipi.</span><span class="sxs-lookup"><span data-stu-id="81125-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="81125-113">Si noterà ASP.NET MVC versioni di queste classi `/Models/IdentityModels.cs`, ma verrà utilizzato un file per ogni classe nel progetto migrato, dal momento che è più chiaro.</span><span class="sxs-lookup"><span data-stu-id="81125-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="81125-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="81125-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="81125-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="81125-115">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="81125-116">Livello di personalizzazione degli utenti o di ApplicationDbContext, non include il progetto Web ASP.NET Core MVC Starter.</span><span class="sxs-lookup"><span data-stu-id="81125-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="81125-117">Durante la migrazione di un'applicazione reale, è necessario anche eseguire la migrazione di tutte le proprietà personalizzate e i metodi dell'utente dell'applicazione e le classi DbContext, nonché altre classi di modello che prevede l'utilizzo dell'applicazione (ad esempio, se l'elemento DbContext è un elemento DbSet<Album>, naturalmente è necessario eseguire la migrazione della classe di Album).</span><span class="sxs-lookup"><span data-stu-id="81125-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="81125-118">Con questi file sul posto, è possibile eseguire il file Startup.cs per compilare aggiornando l'usando istruzioni:</span><span class="sxs-lookup"><span data-stu-id="81125-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="81125-119">L'applicazione è ora pronto per supportare l'autenticazione e i servizi di identità, ma è sufficiente che hanno le funzionalità esposte agli utenti.</span><span class="sxs-lookup"><span data-stu-id="81125-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="81125-120">Eseguire la migrazione di registrazione e la logica di accesso</span><span class="sxs-lookup"><span data-stu-id="81125-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="81125-121">Con i servizi di identità configurati per l'applicazione e l'accesso ai dati configurati con Entity Framework e SQL Server, ci sono ora pronti per aggiungere il supporto per la registrazione e l'account di accesso all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="81125-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="81125-122">Tenere presente che [precedenti nel processo di migrazione](mvc.md#migrate-layout-file) è impostata come commento un riferimento a loginpartial in layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="81125-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="81125-123">È ora necessario tornare a tale codice, rimuovere il commento e aggiungere nelle viste per supportare la funzionalità di accesso e controller necessari.</span><span class="sxs-lookup"><span data-stu-id="81125-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="81125-124">Aggiornamento layout. cshtml; rimuovere il commento di @Html.Partial riga:</span><span class="sxs-lookup"><span data-stu-id="81125-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="81125-125">A questo punto, aggiungere una nuova pagina visualizzazione MVC chiamato loginpartial nella cartella Views/Shared:</span><span class="sxs-lookup"><span data-stu-id="81125-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="81125-126">Aggiornamento loginpartial. cshtml con il codice riportato di seguito (sostituire tutto il contenuto):</span><span class="sxs-lookup"><span data-stu-id="81125-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

<span data-ttu-id="81125-127">A questo punto, dovrebbe essere in grado di aggiornare il sito nel browser.</span><span class="sxs-lookup"><span data-stu-id="81125-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="81125-128">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="81125-128">Summary</span></span>

<span data-ttu-id="81125-129">ASP.NET Core sono state introdotte modifiche alle funzionalità di ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="81125-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="81125-130">In questo articolo è stato illustrato come eseguire la migrazione le funzionalità di gestione utente e l'autenticazione di un'identità di ASP.NET per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81125-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
