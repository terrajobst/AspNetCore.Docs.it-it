---
title: "La migrazione di autenticazione e identità"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: ed96266f06eb473fa3c3e1cc81b2b58fcd89f29e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="migrating-authentication-and-identity"></a>La migrazione di autenticazione e identità

<a name=migration-identity></a>

Da [Steve Smith](https://ardalis.com/)

Nella precedente dell'articolo è [eseguita la migrazione di configurazione da un progetto MVC ASP.NET ad ASP.NET MVC di base](configuration.md). In questo articolo è la migrazione delle funzionalità di gestione utente, account di accesso e registrazione.

## <a name="configure-identity-and-membership"></a>Configurare l'identità e l'appartenenza

In ASP.NET MVC, funzionalità di autenticazione e identità sono configurate utilizzando l'identità di ASP.NET in Startup.Auth.cs e IdentityConfig.cs, si trova nella cartella App_Start. In ASP.NET MVC di base, queste funzionalità vengono configurate in *Startup.cs*.

Installare il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` e `Microsoft.AspNetCore.Authentication.Cookies` pacchetti NuGet.

Aprire quindi Startup.cs e aggiornare il `ConfigureServices()` metodo per utilizzare i servizi di identità e di Entity Framework:

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

A questo punto, sono disponibili due tipi a cui fa riferimento nel codice precedente è non ancora migrati dal progetto ASP.NET MVC: `ApplicationDbContext` e `ApplicationUser`. Creare un nuovo *modelli* in ASP.NET Core cartella del progetto e aggiungere due classi corrispondenti a questi tipi. Si noterà ASP.NET MVC versioni di queste classi `/Models/IdentityModels.cs`, ma verrà utilizzato un file per ogni classe nel progetto migrato, dal momento che è più chiaro.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

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

Livello di personalizzazione degli utenti o di ApplicationDbContext, non include il progetto Web ASP.NET Core MVC Starter. Durante la migrazione di un'applicazione reale, è necessario anche eseguire la migrazione di tutte le proprietà personalizzate e i metodi dell'utente dell'applicazione e le classi DbContext, nonché altre classi di modello che prevede l'utilizzo dell'applicazione (ad esempio, se l'elemento DbContext è un elemento DbSet<Album>, naturalmente è necessario eseguire la migrazione della classe di Album).

Con questi file sul posto, è possibile eseguire il file Startup.cs per compilare aggiornando l'usando istruzioni:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

L'applicazione è ora pronto per supportare l'autenticazione e i servizi di identità, ma è sufficiente che hanno le funzionalità esposte agli utenti.

## <a name="migrate-registration-and-login-logic"></a>Eseguire la migrazione di registrazione e la logica di accesso

Con i servizi di identità configurati per l'applicazione e l'accesso ai dati configurati con Entity Framework e SQL Server, ci sono ora pronti per aggiungere il supporto per la registrazione e l'account di accesso all'applicazione. Tenere presente che [precedenti nel processo di migrazione](mvc.md#migrate-layout-file) è impostata come commento un riferimento a loginpartial in layout. cshtml. È ora necessario tornare a tale codice, rimuovere il commento e aggiungere nelle viste per supportare la funzionalità di accesso e controller necessari.

Aggiornamento layout. cshtml; rimuovere il commento di @Html.Partial riga:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

A questo punto, aggiungere una nuova pagina visualizzazione MVC chiamato loginpartial nella cartella Views/Shared:

Aggiornamento loginpartial. cshtml con il codice riportato di seguito (sostituire tutto il contenuto):

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

A questo punto, dovrebbe essere in grado di aggiornare il sito nel browser.

## <a name="summary"></a>Riepilogo

ASP.NET Core sono state introdotte modifiche alle funzionalità di ASP.NET Identity. In questo articolo è stato illustrato come eseguire la migrazione le funzionalità di gestione utente e l'autenticazione di un'identità di ASP.NET per ASP.NET Core.
