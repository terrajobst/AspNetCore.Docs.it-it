---
title: Eseguire la migrazione di autenticazione e identità ad ASP.NET Core
author: ardalis
description: Informazioni su come eseguire la migrazione di autenticazione e identità da un progetto ASP.NET MVC per un progetto ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828330"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Eseguire la migrazione di autenticazione e identità ad ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Nell'articolo precedente, abbiamo [migrati configurazione da un progetto ASP.NET MVC ad ASP.NET Core MVC](xref:migration/configuration). In questo articolo, la migrazione delle funzionalità di gestione di registrazione, accesso e utente.

## <a name="configure-identity-and-membership"></a>Configurazione di identità e appartenenza

In ASP.NET MVC, funzionalità di autenticazione e identità vengono configurate mediante ASP.NET Identity nelle *Startup.Auth.cs* e *IdentityConfig.cs*, che si trova nel *App_Start* cartella. In ASP.NET Core MVC, queste funzionalità sono configurate nel *Startup.cs*.

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

A questo punto, sono disponibili due tipi riferimento nel codice precedente che è ancora stato ancora migrati dal progetto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`. Creare una nuova *modelli* cartella in ASP.NET Core di progetto, due classi per aggiungervi corrispondenti a questi tipi. Si noterà ASP.NET MVC versioni di queste classi in */Models/IdentityModels.cs*, ma si userà un file per ogni classe nel progetto migrato dal momento che è più chiaro.

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
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Il progetto Web ASP.NET Core MVC Starter non include molte personalizzazioni degli utenti, o `ApplicationDbContext`. Durante la migrazione di una vera app, è anche necessario eseguire la migrazione di tutte le proprietà personalizzate e i metodi dell'utente dell'app e `DbContext` classi, nonché a eventuali altre classi di modello Usa l'app. Ad esempio, se il `DbContext` ha un `DbSet<Album>`, è necessario eseguire la migrazione di `Album` classe.

Questi file sul posto, il *Startup.cs* siano possibili dei file da compilare tramite l'aggiornamento relativo `using` istruzioni:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

L'app è ora pronto per supportare l'autenticazione e i servizi di identità. È sufficiente disporre di queste funzionalità esposte agli utenti.

## <a name="migrate-registration-and-login-logic"></a>Eseguire la migrazione di registrazione e account di accesso per la logica

Con servizi di identità configurati per l'app e l'accesso ai dati configurati tramite Entity Framework e SQL Server, siamo pronti per aggiungere il supporto per la registrazione e account di accesso all'app. È importante ricordare che [precedenti nel processo di migrazione](xref:migration/mvc#migrate-the-layout-file) è un riferimento a come commento *loginpartial* nelle *layout. cshtml*. È ora possibile tornare al codice, rimuovere i commenti e aggiungere necessari sui controller e visualizzazioni per supportare la funzionalità di accesso.

Rimuovere il commento il `@Html.Partial` linea *layout. cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

A questo punto, aggiungere una nuova visualizzazione Razor chiamata *loginpartial* per il *Views/Shared* cartella:

Update *loginpartial. cshtml* con il codice seguente (sostituire tutti i relativi contenuti):

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

A questo punto, è necessario essere in grado di aggiornare il sito nel browser.

## <a name="summary"></a>Riepilogo

ASP.NET Core introduce modifiche alle funzionalità di ASP.NET Identity. In questo articolo, si hanno visto come eseguire la migrazione le funzionalità di gestione utente e l'autenticazione dell'identità di ASP.NET ad ASP.NET Core.
