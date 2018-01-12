---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: ardalis
description: In questo articolo viene descritto come configurare l'autenticazione di Windows in ASP.NET Core, utilizzando IIS Express, IIS, HTTP.sys e WebListener.
keywords: ASP.NET Core, l'autenticazione di Windows, l'attributo Authorize, attributo AllowAnonymous
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: e5ceffe5b7f7e3ef4f6158b6b7b7d571a21ee130
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>Configurare l'autenticazione di Windows in un'applicazione ASP.NET di base

Da [Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)

L'autenticazione di Windows può essere configurata per le applicazioni ASP.NET Core ospitate in IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Che cos'è l'autenticazione di Windows?

L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle applicazioni ASP.NET Core. Quando il server viene eseguito in una rete aziendale usando le identità di dominio Active Directory o altri account di Windows per identificare gli utenti, è possibile utilizzare l'autenticazione di Windows. L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le applicazioni client e server web appartengono allo stesso dominio di Windows.

[Ulteriori informazioni sull'autenticazione di Windows e l'installazione di IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Abilitare l'autenticazione di Windows in un'applicazione ASP.NET di base

Il modello di applicazione Web di Visual Studio può essere configurato per supportare l'autenticazione di Windows.

### <a name="use-the-windows-authentication-app-template"></a>Utilizzare il modello di app di autenticazione di Windows

In Visual Studio:
1. Creare una nuova applicazione Web ASP.NET Core. 
1. Selezionare l'applicazione Web dall'elenco dei modelli.
1. Selezionare il **Modifica autenticazione** e selezionare **l'autenticazione di Windows**. 

Eseguire l'app. Il nome utente viene visualizzato nella parte superiore destra dell'app.

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/browser-screenshot.png)

Per progetti di sviluppo utilizzando IIS Express, il modello fornisce tutte la configurazione necessaria per utilizzare l'autenticazione di Windows. Nella sezione seguente viene illustrato come configurare manualmente un'applicazione ASP.NET Core per l'autenticazione di Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Impostazioni di Visual Studio per Windows e l'autenticazione anonima

Il progetto di Visual Studio **proprietà** della pagina **Debug** scheda fornisce caselle di controllo per l'autenticazione di Windows e l'autenticazione anonima.

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/vs-auth-property-menu.png)

In alternativa, queste due proprietà possono essere configurate nel *launchSettings.json* file:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Abilitare l'autenticazione di Windows con IIS

IIS Usa il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) (ANCM) per ospitare applicazioni ASP.NET Core. Il ANCM flussi l'autenticazione di Windows in IIS per impostazione predefinita. Configurazione dell'autenticazione di Windows viene eseguita in IIS, non il progetto di applicazione. Nelle sezioni seguenti viene illustrato come utilizzare Gestione IIS per configurare un'app di ASP.NET Core per utilizzare l'autenticazione di Windows.

### <a name="create-a-new-iis-site"></a>Creare un nuovo sito IIS

Specificare un nome e una cartella e consentono di creare un nuovo pool di applicazioni.

### <a name="customize-authentication"></a>Personalizzare l'autenticazione

Aprire il menu di autenticazione per il sito.

![Menu di autenticazione IIS](windowsauth/_static/iis-authentication-menu.png)

Disabilitare l'autenticazione anonima e abilitare l'autenticazione di Windows.

![Impostazioni di autenticazione IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Pubblicare il progetto alla cartella del sito IIS

Con Visual Studio o .NET Core CLI, pubblicare l'app nella cartella di destinazione.

![Finestra di dialogo di pubblicazione di Visual Studio](windowsauth/_static/vs-publish-app.png)

Altre informazioni, vedere [la pubblicazione in IIS](xref:host-and-deploy/iis/index).

Avviare l'app per verificare l'autenticazione di Windows sia funzionante.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Abilitare l'autenticazione di Windows con HTTP.sys o WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [HTTP.sys](xref:fundamentals/servers/httpsys) per supportare gli scenari indipendenti che in Windows. Nell'esempio seguente consente di configurare dell'host dell'applicazione web per l'utilizzo di HTTP.sys con l'autenticazione di Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [WebListener](xref:fundamentals/servers/weblistener) per supportare gli scenari indipendenti che in Windows. Nell'esempio seguente consente di configurare dell'host dell'applicazione web per utilizzare WebListener con l'autenticazione di Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Usare l'autenticazione di Windows

Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app. Nelle due sezioni seguenti illustrano come gestire gli stati di configurazione non consentite e sono consentite dell'accesso anonimo.

### <a name="disallow-anonymous-access"></a>Negare l'accesso anonimo

Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` gli attributi non hanno alcun effetto. Se il sito IIS (o server HTTP. sys o WebListener) è configurato per negare l'accesso anonimo, la richiesta raggiunga mai l'app. Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.

### <a name="allow-anonymous-access"></a>Consenti accesso anonimo

Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` gli attributi. Il `[Authorize]` attributo consente di proteggere parti dell'app che richiedono effettivamente l'autenticazione di Windows. Il `[AllowAnonymous]` esegue l'override dell'attributo `[Authorize]` attributo utilizzo all'interno di applicazioni che consentono l'accesso anonimo. Vedere [autorizzazione semplice](xref:security/authorization/simple) per informazioni dettagliate sull'utilizzo di attributi.

In ASP.NET Core 2. x, il `[Authorize]` attributo richiede una configurazione aggiuntiva in *Startup.cs* di incentivare le richieste anonime per l'autenticazione di Windows. La configurazione consigliata varia leggermente in base al server web in uso.

#### <a name="iis"></a>IIS

Se si utilizza IIS, aggiungere il comando seguente per il `ConfigureServices` metodo: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Se si utilizza HTTP.sys, aggiungere il comando seguente per il `ConfigureServices` metodo:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Rappresentazione

ASP.NET Core non implementa la rappresentazione. Le app vengono eseguite con l'identità di applicazione per tutte le richieste, utilizzando l'identità di processo o i pool di app. Se si desidera eseguire in modo esplicito un'azione per conto dell'utente, utilizzare `WindowsIdentity.RunImpersonated`. Eseguire un'unica azione in questo contesto e quindi chiudere il contesto.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Si noti che `RunImpersonated` non supporta le operazioni asincrone e non devono essere usate per scenari complessi. Ad esempio, il wrapping di richieste intere o middleware catene, non è supportato o consigliato.

---
