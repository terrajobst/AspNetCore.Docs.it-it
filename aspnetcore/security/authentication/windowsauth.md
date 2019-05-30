---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core per HTTP. sys e IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/29/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9dfff5dcba409ddca7e05c771b864ab121e0ea85
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395934"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurare l'autenticazione di Windows in ASP.NET Core

Dal [Scott Addie](https://twitter.com/Scott_Addie) e [Luke Latham](https://github.com/guardrex)

[L'autenticazione di Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).

L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core. È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o account di Windows per identificare gli utenti. L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le app client e server web appartengono allo stesso dominio di Windows.

## <a name="launch-settings-debugger"></a>Avviare le impostazioni (debugger)

Configurazione per le impostazioni di avvio interessa solo il *Properties/launchSettings.json* file e non configurare il server IIS o HTTP. sys per l'autenticazione di Windows. Configurazione del server viene illustrata nel [abilitare i servizi di autenticazione per IIS o HTTP. sys](#authentication-services-for-iis-or-httpsys) sezione.

Il **applicazione Web** modello disponibile tramite la CLI di .NET Core o Visual Studio possono essere configurato per supportare l'autenticazione di Windows, che aggiorna il *Properties/launchSettings.json* file automaticamente.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**nuovo progetto**

1. Creare un nuovo progetto.
1. Selezionare **Applicazione Web ASP.NET Core**. Scegliere **Avanti**.
1. Specificare un nome nella **nome progetto** campo. Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto. Scegliere **Crea**.
1. Selezionare **Change** sotto **autenticazione**.
1. Nel **Modifica autenticazione** finestra, seleziona **l'autenticazione di Windows**. Scegliere **OK**.
1. Selezionare **Applicazione Web**.
1. Scegliere **Crea**.

Eseguire l'app. Il nome utente viene visualizzato nell'interfaccia utente dell'app sottoposto a rendering.

**Progetto esistente**

Le proprietà del progetto abilita l'autenticazione di Windows e disattivano l'autenticazione anonima:

1. Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e scegliere **Proprietà**.
1. Selezionare la scheda **Debug**.
1. Deselezionare la casella di controllo **abilitare l'autenticazione anonima**.
1. Selezionare la casella di controllo **abilitare l'autenticazione di Windows**.
1. Salvare e chiudere la pagina delle proprietà.

In alternativa, è possibile configurare le proprietà nel `iisSettings` nodo il *launchsettings. JSON* file:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code/Interfaccia della riga di comando di .NET Core](#tab/visual-studio-code+netcore-cli)

**nuovo progetto**

Eseguire la [dotnet nuove](/dotnet/core/tools/dotnet-new) con il `webapp` argomento (App Web di ASP.NET Core) e `--auth Windows` passare:

```console
dotnet new webapp --auth Windows
```

**Progetto esistente**

Aggiornamento il `iisSettings` nodo il *launchsettings. JSON* file:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Quando si modifica un progetto esistente, verificare che il file di progetto include un riferimento al pacchetto per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **oppure** il [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pacchetto NuGet.

## <a name="authentication-services-for-iis-or-httpsys"></a>Servizi di autenticazione per IIS o HTTP. sys

A seconda dello scenario di hosting, seguire le indicazioni fornite in **entrambe** le [IIS](#iis) sezione **oppure** [HTTP. sys](#httpsys) sezione.

### <a name="iis"></a>IIS

Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

IIS Usa il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare App ASP.NET Core. L'autenticazione di Windows è configurato per IIS tramite il *Web. config* file. Le sezioni seguenti mostrano come:

* Specificare una variabile locale *Web. config* file che attiva l'autenticazione di Windows nel server quando l'app viene distribuita.
* Usare Gestione IIS per configurare il *Web. config* file di un'app ASP.NET Core che è già stata distribuita nel server.

Se non già stato fatto, abilitare IIS per ospitare App ASP.NET Core. Per altre informazioni, vedere <xref:host-and-deploy/iis/index>.

Abilitare il servizio ruolo IIS per l'autenticazione di Windows. Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).

[Middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) è configurato per autenticare automaticamente le richieste per impostazione predefinita. Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: Le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app. Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Uso **entrambi** degli approcci seguenti:

* **Prima della pubblicazione e la distribuzione del progetto** aggiungere il codice seguente *Web. config* file alla radice del progetto:

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Quando il progetto viene pubblicato per .NET Core SDK (senza il `<IsTransformWebConfigDisabled>` impostata su `true` nel file di progetto), pubblicato *Web. config* file include il `<location><system.webServer><security><authentication>` sezione. Per altre informazioni sul `<IsTransformWebConfigDisabled>` proprietà, vedere <xref:host-and-deploy/iis/index#webconfig-file>.

* **Dopo la pubblicazione e la distribuzione del progetto,** eseguire la configurazione lato server con Gestione IIS:

  1. In Gestione IIS selezionare il sito IIS sotto il **siti** nodo del **connessioni** nella barra laterale.
  1. Fare doppio clic su **Authentication** nel **IIS** area.
  1. Selezionare **l'autenticazione anonima**. Selezionare **disabilitare** nel **azioni** nella barra laterale.
  1. Selezionare **Windows autenticazione**. Selezionare **abilitare** nel **azioni** nella barra laterale.

  Quando vengono eseguite queste operazioni, Gestione IIS modifica dell'app *Web. config* file. Oggetto `<system.webServer><security><authentication>` nodo viene aggiunto con impostazioni aggiornate per `anonymousAuthentication` e `windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  Il `<system.webServer>` aggiunto alla sezione di *Web. config* file da Gestione IIS è di fuori dell'app `<location>` sezione aggiunte per .NET Core SDK quando la pubblicazione dell'app. Poiché la sezione viene aggiunta di fuori del `<location>` nodo, le impostazioni vengono ereditate da qualsiasi [App secondarie](xref:host-and-deploy/iis/index#sub-applications) all'app corrente. Per impedire l'ereditarietà, spostare l'aggiunta `<security>` all'interno della sezione di `<location><system.webServer>` sezione che .NET Core SDK fornito.

  Quando Gestione IIS consente di aggiungere la configurazione di IIS, questo interessa solo l'app *Web. config* file sul server. Una distribuzione dell'app per le successive possa sovrascrivere le impostazioni nel server se la copia del server del *Web. config* viene sostituita da del progetto *Web. config* file. Uso **entrambi** degli approcci seguenti per gestire le impostazioni:

  * Utilizzare Gestione IIS per reimpostare le impostazioni nel *Web. config* file dopo che il file viene sovrascritto nella distribuzione.
  * Aggiungere un *file Web. config* all'app in locale con le impostazioni.

### <a name="httpsys"></a>HTTP.sys

Sebbene [Kestrel](xref:fundamentals/servers/kestrel) non supporta l'autenticazione di Windows, è possibile usare [HTTP. sys](xref:fundamentals/servers/httpsys) per supportare gli scenari self-hosted in Windows.

Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Configurare l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> le novità di <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos. L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys. È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente. Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.

> [!NOTE]
> Http. sys non è supportata in Nano Server versione 1709 o successiva. Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Autorizzare gli utenti

Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app. Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.

### <a name="disallow-anonymous-access"></a>Non consentire l'accesso anonimo

Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto. Se un sito IIS è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app. Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.

### <a name="allow-anonymous-access"></a>Consenti accesso anonimo

Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi. Il `[Authorize]` attributo consente di proteggere gli endpoint dell'app che richiedono l'autenticazione. Il `[AllowAnonymous]` esegue l'override dell'attributo di `[Authorize]` attributo nelle App che consente l'accesso anonimo. Per informazioni dettagliate sull'utilizzo di attributi, vedere <xref:security/authorization/simple>.

> [!NOTE]
> Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota. Il [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".

## <a name="impersonation"></a>Rappresentazione

ASP.NET Core non implementa la rappresentazione. Le app vengono eseguite con l'identità dell'app per tutte le richieste, usando l'identità del pool o un processo dell'app. Se l'app deve eseguire un'azione per conto di un utente, usare [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in un [middleware inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`. Eseguire una singola azione in questo contesto e quindi chiudere il contesto.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi. Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.

## <a name="claims-transformations"></a>Trasformazioni di attestazioni

Quando si ospitano con la modalità in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente. Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita. Per altre informazioni e un esempio di codice che attiva le trasformazioni di attestazioni per l'hosting in-process, vedere <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

## <a name="additional-resources"></a>Risorse aggiuntive

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
