---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: ardalis
description: Come configurare l'autenticazione di Windows in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: f724584b43eb2be105cc8a207d5c7b6fec558881
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/19/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurare l'autenticazione di Windows in ASP.NET Core

Da [Steve Smith](https://ardalis.com)

Per le applicazioni ASP.NET Core ospitate in IIS o WebListener, è possibile configurare l'autenticazione di Windows.

## <a name="what-is-windows-authentication"></a>Che cos'è l'autenticazione di Windows

L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle applicazioni ASP.NET Core. Quando il server viene eseguito in una rete aziendale usando le identità di dominio Active Directory o altri account di Windows per identificare gli utenti, è possibile utilizzare l'autenticazione di Windows. L'autenticazione di Windows è un sistema sicuro di autenticazione migliore adattato agli ambienti intranet in cui gli utenti, le applicazioni client e server web appartengono allo stesso dominio di Windows.

[Ulteriori informazioni sull'autenticazione di Windows e l'installazione di IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>Abilitazione dell'autenticazione di Windows in un'applicazione ASP.NET di base

Il modello di applicazione Web di Visual Studio può essere configurato per supportare l'autenticazione di Windows.

### <a name="using-the-windows-authentication-app-template"></a>Usando il modello di applicazione di autenticazione di Windows

In Visual Studio:
* Creare una nuova applicazione Web ASP.NET Core. 
* Selezionare l'applicazione Web dall'elenco dei modelli.
* Selezionare il pulsante Modifica autenticazione e selezionare **l'autenticazione di Windows**. 

Eseguire l'app. Il nome utente viene visualizzato nella parte superiore destra dell'app.

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/browser-screenshot.png)

Per progetti di sviluppo utilizzando IIS Express, il modello fornisce tutte la configurazione necessaria per utilizzare l'autenticazione di Windows. La sezione successiva viene illustrato come configurare un'app di ASP.NET Core manualmente per l'autenticazione di Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Impostazioni di Visual Studio per Windows e l'autenticazione anonima

La pagina delle proprietà di Visual Studio, scheda debug fornisce caselle di controllo per l'autenticazione di Windows e l'autenticazione anonima.

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/vs-auth-property-menu.png)

È inoltre possibile configurare queste proprietà nel `launchSettings.json` file:

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>Abilitazione dell'autenticazione di Windows con IIS

IIS Usa il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) per ospitare applicazioni ASP.NET Core. Il ANCM flussi l'autenticazione di Windows in IIS per impostazione predefinita. Configurazione dell'autenticazione di Windows viene eseguita in IIS, non il progetto di applicazione. Nelle sezioni seguenti viene illustrato come utilizzare Gestione IIS per configurare un'app di ASP.NET Core per utilizzare l'autenticazione di Windows:

### <a name="create-a-new-iis-site"></a>Creare un nuovo sito IIS

Specificare un nome e una cartella e consentono di creare un nuovo pool di applicazioni.

### <a name="customize-authentication"></a>Personalizzare l'autenticazione

Aprire il menu di autenticazione per il sito.

![Menu di autenticazione IIS](windowsauth/_static/iis-authentication-menu.png)

Disabilitare l'autenticazione anonima e abilitare l'autenticazione di Windows.

![Impostazioni di autenticazione IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Pubblicare il progetto alla cartella del sito IIS

Utilizzando Visual Studio o .NET Core CLI, *pubblicare* l'app nella cartella di destinazione.

![Finestra di dialogo di pubblicazione di Visual Studio](windowsauth/_static/vs-publish-app.png)

Altre informazioni, vedere [la pubblicazione in IIS](xref:publishing/iis).

Avviare l'app per verificare l'autenticazione di Windows sia funzionante.

## <a name="enabling-windows-authentication-with-weblistener"></a>Abilitazione dell'autenticazione di Windows con WebListener

Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [WebListener](xref:fundamentals/servers/weblistener) per supportare gli scenari indipendenti che in Windows. Nell'esempio seguente consente di configurare dell'host dell'applicazione web per utilizzare WebListener con l'autenticazione di Windows:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Utilizzo con l'autenticazione di Windows

Se l'app Usa l'autenticazione di Windows e l'accesso anonimo, è possibile utilizzare il ``[Authorize]`` e ``[AllowAnonymous]`` gli attributi. Le applicazioni che non sono anonimo abilitato non richiedono ``[Authorize]``; l'app viene considerata come richiesta di autenticazione, le richieste anonime vengono rifiutate. Si noti che se il sito IIS è configurato **non** per consentire l'accesso anonimo, il ``[AllowAnonymous]`` l'attributo non **non** Consenti anche richieste anonime. Il ``[AllowAnonymous]`` esegue l'override dell'attributo ``[Authorize]`` attributo utilizzo all'interno di applicazioni che consentono l'accesso anonimo.

### <a name="impersonation"></a>Rappresentazione

Non implementa la rappresentazione ASP.NET Core. Le app vengono eseguite con l'identità di applicazione per tutte le richieste, utilizzando l'identità di processo o i pool di app. Se si desidera eseguire in modo esplicito un'azione per conto dell'utente, utilizzare ``WindowsIdentity.RunImpersonated``. Eseguire un'unica azione in questo contesto e quindi chiudere il contesto. Si noti che ``RunImpersonated`` non supporta asincrono e non deve essere utilizzato per scenari complessi. Ad esempio, il wrapping di richieste intere o catene di middleware non è supportato o consigliato.
