---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di app ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862382"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core

Di [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)

Questo articolo descrive il supporto del debug di app ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server. Questo argomento illustra nel dettaglio l'abilitazione della funzionalità e la configurazione di un progetto.

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio per Windows](https://www.microsoft.com/net/download/windows)
* **ASP.NET e carico di lavoro di sviluppo Web**
* Carico di lavoro di **sviluppo multipiattaforma .NET Core**
* Certificato di protezione X.509

## <a name="enable-iis"></a>Abilitare IIS

1. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).
1. Selezionare la casella di controllo **Internet Information Services**.

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

L'installazione di IIS potrebbe richiedere un riavvio del sistema.

## <a name="configure-iis"></a>Configura IIS

IIS deve disporre di un sito Web configurato con gli elementi seguenti:

* Nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app.
* Binding per la porta 443 con un certificato assegnato.

Ad esempio, il **nome host** per un sito Web aggiunto è impostato su "localhost" (anche il profilo di avvio userà "localhost" più avanti in questo argomento). La porta è impostata su "443" (HTTPS). Al sito Web viene assegnato il **certificato di sviluppo di IIS Express**, ma può essere usato qualsiasi certificato valido:

![Aggiungere la finestra del sito Web in IIS, in cui è visualizzato il binding impostato per localhost sulla porta 443 con un certificato assegnato.](development-time-iis-support/_static/add-website-window.png)

Se l'installazione di IIS dispone già di un **sito Web predefinito** con un nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app:

* Aggiungere un binding per la porta 443 (HTTPS).
* Assegnare un certificato valido al sito Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Abilitare il supporto di IIS in fase di sviluppo in Visual Studio

1. Avviare il programma di installazione di Visual Studio.
1. Selezionare il componente **Supporto IIS in fase di sviluppo**. Il componente è elencato come facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**. Il componente installa il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core con IIS.

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata. Nella sezione Web e Cloud è selezionato il pannello Sviluppo ASP.NET e Web. A destra, nell'area Facoltativi del pannello Riepilogo è presente la casella di controllo Supporto IIS in fase di sviluppo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurare il progetto

### <a name="https-redirection"></a>Reindirizzamento HTTPS

Per un nuovo progetto, selezionare la casella di controllo **Configura per HTTPS** nella finestra **Nuova applicazione Web ASP.NET Core**:

![Finestra Nuova applicazione Web ASP.NET Core con la casella di controllo Configura per HTTPS selezionata.](development-time-iis-support/_static/new-app.png)

In un progetto esistente, usare il middleware di reindirizzamento HTTPS in `Startup.Configure` chiamando il metodo di estensione [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Profilo di avvio IIS

Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:

1. Per **Profilo** selezionare il pulsante **Nuovo**. Denominare il profilo "IIS" nella finestra popup. Selezionare **OK** per creare il profilo.
1. Per l'impostazione **Avvio** selezionare **IIS** dall'elenco.
1. Selezionare la casella di controllo **Avvia browser** e specificare l'URL dell'endpoint. Usare il protocollo HTTPS. In questo esempio viene usato `https://localhost/WebApplication1`.
1. Nella sezione **Variabili di ambiente** selezionare il pulsante **Aggiungi**. Specificare una variabile di ambiente con una chiave `ASPNETCORE_ENVIRONMENT` e il valore `Development`.
1. Nell'area **Impostazioni server Web** impostare **URL App**. In questo esempio viene usato `https://localhost/WebApplication1`.
1. Salvare il profilo.

![Finestra delle proprietà del progetto con la scheda Debug selezionata. Le impostazioni Profilo e Avvia sono impostate su IIS. La funzionalità Avvia browser è abilitata con l'indirizzo https://localhost/WebApplication1. Lo stesso indirizzo viene visualizzato anche nel campo URL app nell'area Impostazioni server Web.](development-time-iis-support/_static/project_properties.png)

In alternativa, aggiungere manualmente un profilo di avvio al file [launchSettings.json](http://json.schemastore.org/launchsettings) nell'app:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Eseguire il progetto

In Visual Studio:

* Verificare che l'elenco di riepilogo a discesa della configurazione della build sia impostato su **Debug**.
* Impostare il pulsante Esegui sul profilo **IIS** e selezionare il pulsante per avviare l'app.

![Il pulsante Esegui sulla barra degli strumenti di Visual Studio viene impostato sul profilo IIS con l'elenco di riepilogo a discesa della configurazione della build impostato su Versione.](development-time-iis-support/_static/toolbar.png)

Visual Studio potrebbe richiedere un riavvio, se non si è in modalità amministratore. In tal caso riavviare Visual Studio.

Se viene usato un certificato di sviluppo non attendibile, il browser potrebbe richiedere di creare un'eccezione per il certificato non attendibile.

> [!NOTE]
> Il debug della configurazione della build Versione con [Just My Code](/visualstudio/debugger/just-my-code) e le ottimizzazioni del compilatore genera un'esperienza con funzionalità ridotta. Ad esempio, i punti di interruzione non vengono raggiunti.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Imporre HTTPS](xref:security/enforcing-ssl)
