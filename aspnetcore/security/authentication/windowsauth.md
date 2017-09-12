---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: ardalis
description: Come configurare l'autenticazione di Windows in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="31b2a-104">Configurare l'autenticazione di Windows in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31b2a-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="31b2a-105">Da [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="31b2a-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="31b2a-106">Per le applicazioni ASP.NET Core ospitate in IIS o WebListener, è possibile configurare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="31b2a-107">Che cos'è l'autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="31b2a-107">What is Windows authentication</span></span>

<span data-ttu-id="31b2a-108">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31b2a-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="31b2a-109">Quando il server viene eseguito in una rete aziendale usando le identità di dominio Active Directory o altri account di Windows per identificare gli utenti, è possibile utilizzare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="31b2a-110">L'autenticazione di Windows è un sistema sicuro di autenticazione migliore adattato agli ambienti intranet in cui gli utenti, le applicazioni client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="31b2a-111">[Ulteriori informazioni sull'autenticazione di Windows e l'installazione di IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="31b2a-111">[Learn more about Windows Authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="31b2a-112">Abilitazione dell'autenticazione di Windows in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="31b2a-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="31b2a-113">Il modello di applicazione Web di Visual Studio può essere configurato per supportare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="31b2a-114">Usando il modello di applicazione di autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="31b2a-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="31b2a-115">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="31b2a-115">In Visual Studio:</span></span>
* <span data-ttu-id="31b2a-116">Creare una nuova applicazione Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31b2a-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="31b2a-117">Selezionare l'applicazione Web dall'elenco dei modelli.</span><span class="sxs-lookup"><span data-stu-id="31b2a-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="31b2a-118">Selezionare il pulsante Modifica autenticazione e selezionare **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="31b2a-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="31b2a-119">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="31b2a-119">Run the app.</span></span> <span data-ttu-id="31b2a-120">Il nome utente viene visualizzato nella parte superiore destra dell'app.</span><span class="sxs-lookup"><span data-stu-id="31b2a-120">The username appears in the top right of the app.</span></span>

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="31b2a-122">Per progetti di sviluppo utilizzando IIS Express, il modello fornisce tutte la configurazione necessaria per utilizzare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="31b2a-123">La sezione successiva viene illustrato come configurare un'app di ASP.NET Core manualmente per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="31b2a-124">Impostazioni di Visual Studio per Windows e l'autenticazione anonima</span><span class="sxs-lookup"><span data-stu-id="31b2a-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="31b2a-125">La pagina delle proprietà di Visual Studio, scheda debug fornisce caselle di controllo per l'autenticazione di Windows e l'autenticazione anonima.</span><span class="sxs-lookup"><span data-stu-id="31b2a-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="31b2a-127">È inoltre possibile configurare queste proprietà nel `launchSettings.json` file:</span><span class="sxs-lookup"><span data-stu-id="31b2a-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

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

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="31b2a-128">Abilitazione dell'autenticazione di Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="31b2a-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="31b2a-129">IIS Usa il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) per ospitare applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31b2a-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="31b2a-130">Il ANCM flussi l'autenticazione di Windows in IIS per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="31b2a-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="31b2a-131">Configurazione dell'autenticazione di Windows viene eseguita in IIS, non il progetto di applicazione.</span><span class="sxs-lookup"><span data-stu-id="31b2a-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="31b2a-132">Nelle sezioni seguenti viene illustrato come utilizzare Gestione IIS per configurare un'app di ASP.NET Core per utilizzare l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="31b2a-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="31b2a-133">Creare un nuovo sito IIS</span><span class="sxs-lookup"><span data-stu-id="31b2a-133">Create a new IIS site</span></span>

<span data-ttu-id="31b2a-134">Specificare un nome e una cartella e consentono di creare un nuovo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="31b2a-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="31b2a-135">Personalizzare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="31b2a-135">Customize Authentication</span></span>

<span data-ttu-id="31b2a-136">Aprire il menu di autenticazione per il sito.</span><span class="sxs-lookup"><span data-stu-id="31b2a-136">Open the Authentication menu for the site.</span></span>

![Menu di autenticazione IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="31b2a-138">Disabilitare l'autenticazione anonima e abilitare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Impostazioni di autenticazione IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="31b2a-140">Pubblicare il progetto alla cartella del sito IIS</span><span class="sxs-lookup"><span data-stu-id="31b2a-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="31b2a-141">Utilizzando Visual Studio o .NET Core CLI, *pubblicare* l'app nella cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="31b2a-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Finestra di dialogo di pubblicazione di Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="31b2a-143">Altre informazioni, vedere [la pubblicazione in IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="31b2a-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="31b2a-144">Avviare l'app per verificare l'autenticazione di Windows sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="31b2a-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="31b2a-145">Abilitazione dell'autenticazione di Windows con WebListener</span><span class="sxs-lookup"><span data-stu-id="31b2a-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="31b2a-146">Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [WebListener](xref:fundamentals/servers/weblistener) per supportare gli scenari indipendenti che in Windows.</span><span class="sxs-lookup"><span data-stu-id="31b2a-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="31b2a-147">Nell'esempio seguente consente di configurare dell'host dell'applicazione web per utilizzare WebListener con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="31b2a-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

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

## <a name="working-with-windows-authentication"></a><span data-ttu-id="31b2a-148">Utilizzo con l'autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="31b2a-148">Working with Windows authentication</span></span>

<span data-ttu-id="31b2a-149">Se l'app Usa l'autenticazione di Windows e l'accesso anonimo, è possibile utilizzare il ``[Authorize]`` e ``[AllowAnonymous]`` gli attributi.</span><span class="sxs-lookup"><span data-stu-id="31b2a-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="31b2a-150">Le applicazioni che non sono anonimo abilitato non richiedono ``[Authorize]``; l'app viene considerata come richiesta di autenticazione, le richieste anonime vengono rifiutate.</span><span class="sxs-lookup"><span data-stu-id="31b2a-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="31b2a-151">Si noti che se il sito IIS è configurato **non** per consentire l'accesso anonimo, il ``[AllowAnonymous]`` l'attributo non **non** Consenti anche richieste anonime.</span><span class="sxs-lookup"><span data-stu-id="31b2a-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="31b2a-152">Il ``[AllowAnonymous]`` esegue l'override dell'attributo ``[Authorize]`` attributo utilizzo all'interno di applicazioni che consentono l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="31b2a-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="31b2a-153">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="31b2a-153">Impersonation</span></span>

<span data-ttu-id="31b2a-154">Non implementa la rappresentazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31b2a-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="31b2a-155">Le app vengono eseguite con l'identità di applicazione per tutte le richieste, utilizzando l'identità di processo o i pool di app.</span><span class="sxs-lookup"><span data-stu-id="31b2a-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="31b2a-156">Se si desidera eseguire in modo esplicito un'azione per conto dell'utente, utilizzare ``WindowsIdentity.RunImpersonated``.</span><span class="sxs-lookup"><span data-stu-id="31b2a-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="31b2a-157">Eseguire un'unica azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="31b2a-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="31b2a-158">Si noti che ``RunImpersonated`` non supporta asincrono e non deve essere utilizzato per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="31b2a-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="31b2a-159">Ad esempio, il wrapping di richieste intere o catene di middleware non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="31b2a-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
