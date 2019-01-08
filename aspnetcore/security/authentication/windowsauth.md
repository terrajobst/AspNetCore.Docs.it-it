---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core, usando HTTP. sys, IIS e IIS Express.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/23/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 64178c8fce71445fc6a728a236d811484b21e3e0
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099260"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurare l'autenticazione di Windows in ASP.NET Core

Dal [Scott Addie](https://twitter.com/Scott_Addie) e [Luke Latham](https://github.com/guardrex)

[L'autenticazione di Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).

L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core. È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o account di Windows per identificare gli utenti. L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le app client e server web appartengono allo stesso dominio di Windows.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Abilitare l'autenticazione di Windows in un'app ASP.NET Core

Il **applicazione Web** modello disponibile tramite la CLI di .NET Core o Visual Studio possono essere configurato per supportare l'autenticazione di Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Usare il modello di app di autenticazione di Windows per un nuovo progetto

In Visual Studio:

1. Creare una nuova **applicazione Web ASP.NET Core**.
1. Selezionare **applicazione Web** dall'elenco dei modelli.
1. Selezionare il **Modifica autenticazione** e selezionare **l'autenticazione di Windows**.

Eseguire l'app. Il nome utente viene visualizzato nell'interfaccia utente dell'app sottoposto a rendering.

### <a name="manual-configuration-for-an-existing-project"></a>Configurazione manuale per un progetto esistente

Le proprietà del progetto consentono di abilitare l'autenticazione di Windows e disabilitare l'autenticazione anonima:

1. Fare clic sul progetto in Visual Studio **Esplora soluzioni** e selezionare **proprietà**.
1. Selezionare la scheda **Debug**.
1. Deselezionare la casella di controllo **abilitare l'autenticazione anonima**.
1. Selezionare la casella di controllo **abilitare l'autenticazione di Windows**.

In alternativa, è possibile configurare le proprietà nel `iisSettings` nodo il *launchsettings. JSON* file:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Usare la **Windows autenticazione** modello di app.

Eseguire la [dotnet nuove](/dotnet/core/tools/dotnet-new) con il `webapp` argomento (App Web di ASP.NET Core) e `--auth Windows` passare:

```console
dotnet new webapp --auth Windows
```

---

## <a name="enable-windows-authentication-with-iis"></a>Abilitare l'autenticazione di Windows con IIS

IIS Usa il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare App ASP.NET Core. L'autenticazione di Windows è configurato per IIS tramite il *Web. config* file. Le sezioni seguenti mostrano come:

* Specificare una variabile locale *Web. config* file che attiva l'autenticazione di Windows nel server quando l'app viene distribuita.
* Usare Gestione IIS per configurare il *Web. config* file di un'app ASP.NET Core che è già stata distribuita nel server.

### <a name="iis-configuration"></a>Configurazione di IIS

Se non già stato fatto, abilitare IIS per ospitare App ASP.NET Core. Per altre informazioni, vedere <xref:host-and-deploy/iis/index>.

Abilitare il servizio ruolo IIS per l'autenticazione di Windows. Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).

Per impostazione predefinita, il Middleware di integrazione IIS è configurato per autenticare automaticamente le richieste. Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: Le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app. Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Creare un nuovo sito IIS

Specificare un nome e una cartella e consentirgli di creare un nuovo pool di applicazioni.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>Abilitare l'autenticazione di Windows per le app in IIS.

Uso **entrambi** degli approcci seguenti:

* [Configurazione lato sviluppo prima di pubblicare l'app](#development-side-configuration-with-a-local-webconfig-file) (*consigliato*)
* [Configurazione lato server dopo aver pubblicato l'app](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Configurazione di sviluppo-side con un file Web. config locale

Eseguire la procedura seguente **prima** si [pubblicare e distribuire il progetto](#publish-and-deploy-your-project-to-the-iis-site-folder).

Aggiungere il codice seguente *Web. config* file alla radice del progetto:

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

Quando il progetto viene pubblicato dal SDK (senza il `<IsTransformWebConfigDisabled>` impostata su `true` nel file di progetto), pubblicato *Web. config* file include il `<location><system.webServer><security><authentication>` sezione. Per altre informazioni sul `<IsTransformWebConfigDisabled>` proprietà, vedere <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>Configurazione lato server con Gestione IIS

Eseguire la procedura seguente **dopo aver** si [pubblicare e distribuire il progetto](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. In Gestione IIS selezionare il sito IIS sotto il **siti** nodo del **connessioni** nella barra laterale.
1. Fare doppio clic su **Authentication** nel **IIS** area.
1. Selezionare **l'autenticazione anonima**. Selezionare **disabilitare** nel **azioni** nella barra laterale.
1. Selezionare **Windows autenticazione**. Selezionare **abilitare** nel **azioni** nella barra laterale.

Quando vengono eseguite queste operazioni, Gestione IIS modifica dell'app *Web. config* file. Oggetto `<system.webServer><security><authentication>` nodo viene aggiunto con impostazioni aggiornate per `anonymousAuthentication` e `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

Il `<system.webServer>` aggiunto alla sezione di *Web. config* file da Gestione IIS è di fuori dell'app `<location>` sezione aggiunte per .NET Core SDK quando la pubblicazione dell'app. Poiché la sezione viene aggiunta di fuori del `<location>` nodo, le impostazioni vengono ereditate da qualsiasi [App secondarie](xref:host-and-deploy/iis/index#sub-applications) all'app corrente. Per impedire l'ereditarietà, spostare l'aggiunta `<security>` all'interno della sezione di `<location><system.webServer>` sezione in cui il SDK fornito.

Quando Gestione IIS consente di aggiungere la configurazione di IIS, questo interessa solo l'app *Web. config* file sul server. Una distribuzione dell'app per le successive possa sovrascrivere le impostazioni nel server se la copia del server del *Web. config* viene sostituita da del progetto *Web. config* file. Uso **entrambi** degli approcci seguenti per gestire le impostazioni:

* Utilizzare Gestione IIS per reimpostare le impostazioni nel *Web. config* file dopo che il file viene sovrascritto nella distribuzione.
* Aggiungere un *file Web. config* all'app in locale con le impostazioni. Per altre informazioni, vedere la [configurazione lato sviluppo](#development-side-configuration-with-a-local-webconfig-file) sezione.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Pubblicare e distribuire il progetto alla cartella del sito IIS

Usa Visual Studio o .NET Core CLI, pubblicare e distribuire l'app per la cartella di destinazione.

Per altre informazioni sull'hosting con IIS, pubblicazione e distribuzione, vedere gli argomenti seguenti:

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Avviare l'app per verificare l'autenticazione di Windows.

## <a name="enable-windows-authentication-with-httpsys"></a>Abilitare l'autenticazione di Windows con HTTP. sys

Anche se Kestrel non supporta l'autenticazione di Windows, è possibile usare [HTTP. sys](xref:fundamentals/servers/httpsys) per supportare gli scenari self-hosted in Windows. L'esempio seguente configura l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows:

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos. L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys. È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente. Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.

> [!NOTE]
> Http. sys non è supportata in Nano Server versione 1709 o successiva. Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Utilizzo con l'autenticazione di Windows

Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app. Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.

### <a name="disallow-anonymous-access"></a>Non consentire l'accesso anonimo

Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto. Se il sito IIS (o HTTP. sys) è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app. Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.

### <a name="allow-anonymous-access"></a>Consenti accesso anonimo

Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi. Il `[Authorize]` attributo consente di proteggere parti dell'app che realmente richiedono l'autenticazione di Windows. Il `[AllowAnonymous]` esegue l'override dell'attributo `[Authorize]` utilizzo all'interno delle App che consentono l'accesso anonimo degli attributi. Visualizzare [autorizzazione semplice](xref:security/authorization/simple) per informazioni dettagliate sull'utilizzo di attributi.

In ASP.NET Core 2.x, il `[Authorize]` attributo richiede una configurazione aggiuntiva nel *Startup.cs* per stimolare le richieste anonime per l'autenticazione di Windows. La configurazione consigliata varia leggermente in base sul server web in uso.

> [!NOTE]
> Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota. Il [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".

#### <a name="iis"></a>IIS

Se si usa IIS, aggiungere il codice seguente il `ConfigureServices` metodo:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Se si Usa HTTP. sys, aggiungere il codice seguente il `ConfigureServices` metodo:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Rappresentazione

ASP.NET Core non implementa la rappresentazione. Le app vengono eseguite con l'identità dell'app per tutte le richieste, usando l'identità del pool o un processo dell'app. Se è necessario eseguire in modo esplicito un'azione per conto di un utente, usare [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in un [middleware inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`. Eseguire una singola azione in questo contesto e quindi chiudere il contesto.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi. Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.
