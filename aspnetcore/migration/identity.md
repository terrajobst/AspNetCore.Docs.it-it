---
title: Eseguire la migrazione di autenticazione e identità per ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di autenticazione e identità da un progetto ASP.NET MVC per un progetto ASP.NET MVC di base.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275697"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Eseguire la migrazione di autenticazione e identità per ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Nell'articolo precedente, si [eseguita la migrazione di configurazione da un progetto ASP.NET MVC per MVC ASP.NET Core](xref:migration/configuration). In questo articolo è la migrazione delle funzionalità di gestione utente, account di accesso e registrazione.

## <a name="configure-identity-and-membership"></a>Configurare l'identità e l'appartenenza

In ASP.NET MVC, funzionalità di autenticazione e identità vengono configurate mediante ASP.NET Identity nel *Startup.Auth.cs* e *IdentityConfig.cs*, che si trova nel *App_Start* cartella. In ASP.NET MVC di base, queste funzionalità vengono configurate in *Startup.cs*.

Installare il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacchetti NuGet.

Quindi, aprire *Startup.cs* e aggiornare il `Startup.ConfigureServices` metodo per utilizzare i servizi di identità e di Entity Framework:

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

A questo punto, sono disponibili due tipi a cui fa riferimento nel codice precedente è non ancora migrati dal progetto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`. Creare un nuovo *modelli* in ASP.NET Core cartella del progetto e aggiungere due classi corrispondenti a questi tipi. Si noterà ASP.NET MVC versioni di queste classi */Models/IdentityModels.cs*, ma verrà utilizzato un file per ogni classe nel progetto migrato dal momento che è più chiaro.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Il progetto Web ASP.NET Core MVC Starter non include a livello di personalizzazione degli utenti, o `ApplicationDbContext`. Durante la migrazione di una vera app, è inoltre necessario eseguire la migrazione di tutte le proprietà personalizzate e i metodi dell'utente dell'app e `DbContext` classi, nonché altre classi di modello prevede l'utilizzo dell'app. Ad esempio, se il `DbContext` ha un `DbSet<Album>`, è necessario eseguire la migrazione di `Album` (classe).

Con questi file in punto, il *Startup.cs* file può essere resi per compilare aggiornando relativo `using` istruzioni:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

L'app è ora pronto per supportare i servizi di autenticazione e identità. È necessario solo le funzionalità esposte agli utenti.

## <a name="migrate-registration-and-login-logic"></a>Eseguire la migrazione della logica di registrazione e account di accesso

Con servizi di identità configurati per l'app e l'accesso ai dati configurati con Entity Framework e SQL Server, saremo pronti aggiungere il supporto per la registrazione e account di accesso all'app. Si tenga presente che [più indietro nel processo di migrazione](xref:migration/mvc#migrate-the-layout-file) è impostata come commento un riferimento a *loginpartial* in *layout. cshtml*. È ora necessario tornare a tale codice, rimuovere il commento e aggiungere nelle viste per supportare la funzionalità di accesso e controller necessari.

Rimuovere il commento il `@Html.Partial` riga *cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

A questo punto, aggiungere una nuova visualizzazione Razor denominata *loginpartial* per il *Views/Shared* cartella:

Aggiornamento *loginpartial. cshtml* con il codice riportato di seguito (sostituire tutti i relativi contenuti):

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

A questo punto, dovrebbe essere in grado di aggiornare il sito nel browser.

## <a name="summary"></a>Riepilogo

ASP.NET Core sono state introdotte modifiche alle funzionalità di ASP.NET Identity. In questo articolo è stato illustrato come eseguire la migrazione alle funzionalità di ASP.NET Identity Gestione utente e l'autenticazione in ASP.NET Core.
