---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Individuare il supporto per il debug di applicazioni ASP.NET Core durante l'esecuzione di base IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core

Dal [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)

Questo articolo descrive [Visual Studio](https://www.visualstudio.com/vs/) supporto per il debug di App di ASP.NET Core in esecuzione dietro IIS su Windows Server. Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Abilitare IIS

1. Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).
1. Selezionare il **Internet Information Services** casella di controllo.

![Casella di controllo di Internet Information Services con le funzionalità di Windows archiviata come un quadrato bianco (non un segno di spunta) che indica che alcune delle funzionalità di IIS siano abilitate](development-time-iis-support/_static/enable_iis.png)

L'installazione di IIS potrebbe richiedere un riavvio del sistema.

## <a name="configure-iis"></a>Configura IIS

IIS deve avere un sito Web configurato con gli elementi seguenti:

* Nome host che corrisponde al nome host dell'URL profilo avvio dell'app.
* Associazione per la porta 443 con un certificato assegnati.

Ad esempio, il **nome Host** per un sito Web aggiunto è impostato su "localhost" (il profilo di avvio utilizzeranno "localhost" più avanti in questo argomento). La porta è impostata su "443" (HTTPS). Il **IIS Express sviluppo certificato** viene assegnato per il sito Web, ma qualsiasi certificato valido works:

![Aggiungi finestra del sito Web in IIS che mostra il set di binding per localhost sulla porta 443 con il certificato assegnato.](development-time-iis-support/_static/add-website-window.png)

Se l'installazione di IIS già è un **sito Web predefinito** con un nome host che corrisponde al nome host dell'URL profilo avvio dell'app:

* Aggiungere un binding di porta per la porta 443 (HTTPS).
* Assegnare un certificato valido per il sito Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Abilitare il supporto IIS in fase di sviluppo in Visual Studio

1. Avviare il programma di installazione di Visual Studio.
1. Selezionare il **fase di sviluppo IIS supporta** componente. Il componente è elencato come facoltativi nel **riepilogo** pannello per il **sviluppo web ASP.NET e** carico di lavoro. Il componente viene installato il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module), ovvero un modulo nativo di IIS necessario per eseguire applicazioni dietro IIS ASP.NET Core in una configurazione di proxy inverso.

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata. Nella sezione Web e Cloud è selezionato il pannello Sviluppo ASP.NET e Web. A destra nell'area del pannello riepilogo facoltativo è una casella di controllo per la fase di sviluppo che supportano IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configurare il progetto

### <a name="https-redirection"></a>Reindirizzamento HTTPS

Per un nuovo progetto, selezionare la casella di controllo **Configura per HTTPS** nel **nuova applicazione Web di ASP.NET Core** finestra:

![Nuova finestra dell'applicazione Web di ASP.NET Core con la configurazione per la casella di controllo HTTPS.](development-time-iis-support/_static/new-app.png)

In un progetto esistente, utilizzare HTTPS reindirizzamento Middleware `Startup.Configure` chiamando il [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) metodo di estensione:

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

### <a name="iis-launch-profile"></a>IIS avviare profilo

Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:

1. Per **profilo**, selezionare il **New** pulsante. Denominare il profilo "IIS" nella finestra popup. Selezionare **OK** per creare il profilo.
1. Per il **avviare** impostazione, selezionare **IIS** dall'elenco.
1. Selezionare la casella di controllo **avvio browser** e fornire l'URL dell'endpoint. Utilizzare il protocollo HTTPS. In questo esempio viene usato `https://localhost/WebApplication1`.
1. Nel **variabili di ambiente** sezione, selezionare il **Add** pulsante. Fornire una variabile di ambiente con una chiave di `ASPNETCORE_ENVIRONMENT` e il valore `Development`.
1. Nel **impostazioni del Server Web** area, impostare il **URL App**. In questo esempio viene usato `https://localhost/WebApplication1`.
1. Salvare il profilo.

![Finestra delle proprietà del progetto con la scheda Debug selezionata. Le impostazioni Profilo e Avvia sono impostate su IIS. La funzionalità di browser di avvio è abilitata con l'indirizzo https://localhost/WebApplication1. Lo stesso indirizzo viene fornito anche nel campo URL di App dell'area di impostazioni del Server Web.](development-time-iis-support/_static/project_properties.png)

In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:

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

Nell'interfaccia utente di Visual Studio, impostare il pulsante di esecuzione il **IIS** del profilo e selezionare il pulsante per avviare l'app:

![Pulsante di esecuzione sulla barra degli strumenti di Visual Studio impostare il profilo "IIS".](development-time-iis-support/_static/toolbar.png)

Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore. In tal caso riavviare Visual Studio.

Se viene utilizzato un certificato di sviluppo non attendibile, il browser potrebbe essere necessario creare un'eccezione per il certificato non attendibile.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Imporre HTTPS](xref:security/enforcing-ssl)
