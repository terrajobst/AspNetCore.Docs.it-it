---
title: Ospitare ASP.NET Core in Servizio app di Azure
author: guardrex
description: Informazioni su come ospitare le app ASP.NET Core nel servizio app di Azure con collegamenti a risorse utili.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 42775bf4d3e88893260a5973f6f7bc9d3a006b5a
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927828"
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Ospitare ASP.NET Core in Servizio app di Azure

Il [servizio app di Azure](https://azure.microsoft.com/services/app-service/) è un [servizio di piattaforma di cloud computing Microsoft](https://azure.microsoft.com/) per l'hosting di app Web, inclusa ASP.NET Core.

## <a name="useful-resources"></a>Risorse utili

[Documentazione di App Web](/azure/app-service/) di Azure è la home page della documentazione, delle esercitazioni, degli esempi, delle guide introduttive e di altre risorse per le app di Azure. Due importanti esercitazioni relative all'hosting di app ASP.NET Core sono:

[Guida introduttiva: Creare un'app Web ASP.NET Core in Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Usare Visual Studio per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Windows.

[Guida introduttiva: Creare un'app Web .NET Core nel Servizio app in Linux](/azure/app-service/containers/quickstart-dotnetcore)  
Usare la riga di comando per creare e distribuire un'app Web ASP.NET Core nel servizio app di Azure in Linux.

Gli articoli seguenti sono disponibili nella documentazione di ASP.NET Core:

[Pubblicare in Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.

[Pubblicare in Azure con gli strumenti dell'interfaccia della riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli)  
Informazioni su come pubblicare un'app ASP.NET Core nel servizio app di Azure con il client della riga di comando Git.

[Distribuzione continua in Azure con Visual Studio e Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Informazioni su come creare un'app Web ASP.NET Core tramite Visual Studio e distribuirla nel Servizio app di Azure usando Git per la distribuzione continua.

[Distribuzione continua in Azure con VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Impostare una build CI per un'app ASP.NET Core e quindi creare una versione di distribuzione continua in Servizio App di Azure.

[Azure Web App sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) (Sandbox per app Web di Azure)  
Individuare le limitazioni di esecuzione di runtime di Servizio app di Azure applicate dalla piattaforma per le app Azure.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Configurazione dell'applicazione

In ASP.NET Core 2.0 o versioni successive, i pacchetti NuGet seguenti offrono funzionalità di registrazione automatica per le app distribuite nel Servizio app di Azure:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) per fornire l'integrazione di ASP.NET Core con il Servizio app di Azure. La funzionalità di registrazione aggiunte vengono fornite dal pacchetto `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) esegue [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) per aggiungere i provider di registrazione diagnostica del servizio app di Azure nel pacchetto `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) offre implementazioni di logger per il supporto dei registri di diagnostica del servizio app di Azure e delle funzionalità del flusso di registrazione.

Se destinati a .NET Core e aventi come riferimento il [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage), i pacchetti sono già inclusi. I pacchetti non sono presenti nel nuovo [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Se destinati a .NET Framework o aventi come riferimento il metapacchetto `Microsoft.AspNetCore.App`, fare riferimento ai singoli pacchetti di registrazione.

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>Eseguire l'override della configurazione delle app usando il portale di Azure

L'area **Impostazioni app** del pannello **Impostazioni applicazione** consente di impostare le variabili di ambiente per l'app. Le variabili di ambiente possono essere utilizzate dal [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quando un'app usa l'[host Web](xref:fundamentals/host/web-host) e compila l'host con [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), le variabili di ambiente che consentono di configurare l'host usano il prefisso `ASPNETCORE_`. Per altre informazioni, vedere <xref:fundamentals/host/web-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Quando un'app usa l'[host generico](xref:fundamentals/host/generic-host), le variabili di ambiente non vengono caricate nella configurazione di un'app per impostazione predefinita e il provider di configurazione deve essere aggiunto dallo sviluppatore. Lo sviluppatore determina il prefisso delle variabili di ambiente quando viene aggiunto il provider di configurazione. Per altre informazioni, vedere <xref:fundamentals/host/generic-host> e il [provider di configurazione delle variabili di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta. Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitoraggio e registrazione

Per informazioni sul monitoraggio, la registrazione e la risoluzione dei problemi, vedere gli articoli seguenti:

[Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure](/azure/app-service/web-sites-monitor)  
Informazioni su come esaminare le quote e le metriche per le app e i piani del servizio app.

[Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Informazioni su come abilitare e accedere alla registrazione diagnostica per i codici di stato HTTP, le richieste non riuscite e l'attività del server Web.

[Introduzione alla gestione degli errori in ASP.NET Core](xref:fundamentals/error-handling)  
Riconoscimento degli approcci comuni di gestione degli errori nelle app ASP.NET Core.

[Risolvere i problemi di ASP.NET Core in Servizio app di Azure](xref:host-and-deploy/azure-apps/troubleshoot)  
Informazioni su come diagnosticare i problemi delle distribuzioni del servizio app di Azure con le app ASP.NET Core.

[Errori comuni di Azure App Service e IIS con ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Informazioni sugli errori comuni di configurazione della distribuzione per le app ospitate dal servizio app di Azure o da IIS con suggerimenti per la risoluzione.

## <a name="data-protection-key-ring-and-deployment-slots"></a>KeyRing di protezione dati e slot di distribuzione

Le [chiavi di protezione dati](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sono salvate in modo permanente nella cartella *%HOME%\ASP.NET\DataProtection-Keys*. La cartella è associata all'archiviazione di rete e sincronizzata in tutti i computer che ospitano l'app. Le chiavi non vengono protette quando sono inattive. La cartella offre il KeyRing a tutte le istanze di un'app in un singolo slot di distribuzione. Gli slot di distribuzione separati, ad esempio gli slot di gestione temporanea e di produzione, non condividono un KeyRing.

Nel passaggio da uno slot di distribuzione all'altro, tutti i sistemi che usano la protezione dati non saranno in grado di decrittografare i dati archiviati usando il KeyRing all'interno dello slot precedente. Il middleware dei cookie di ASP.NET usa la protezione dati per proteggere i cookie. Di conseguenza, gli utenti vengono disconnessi da un'app che usa il middleware dei cookie di ASP.NET standard. Per una soluzione di KeyRing indipendente dallo slot, usare un provider di KeyRing esterno, ad esempio:

* Archiviazione BLOB di Azure
* Azure Key Vault
* Archivio SQL
* Cache Redis

Per altre informazioni, vedere [Provider di archiviazione chiavi](xref:security/data-protection/implementation/key-storage-providers).

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Distribuire la versione di anteprima di ASP.NET Core in Servizio app di Azure

Le app in anteprima di ASP.NET Core possono essere distribuite in Servizio app di Azure con gli approcci seguenti:

* [Installare l'estensione del sito di anteprima](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) -->
* [Usare Docker con app Web per contenitori](#use-docker-with-web-apps-for-containers)

Se si verifica un problema con l'estensione del sito di anteprima, aprire un problema in [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Installare l'estensione del sito di anteprima

1. Dal portale di Azure passare al pannello Servizi App.
1. Selezionare l'app Web.
1. Immettere "ex" nella casella di ricerca o scorrere verso il basso l'elenco dei riquadri di gestione fino a **STRUMENTI DI LAVORO**.
1. Selezionare **STRUMENTI DI LAVORO** > **Extensions** (Estensioni).
1. Selezionare **Aggiungi**.

   ![Pannello dell'app Azure con i passaggi precedenti](index/_static/x1.png)

1. Selezionare **ASP.NET Core Runtime Extensions** (Estensioni di ASP.NET Core).
1. Selezionare **OK** per accettare le condizioni legali.
1. Per installare l'estensione, selezionare **OK**.

Al termine delle operazioni di aggiunta, viene installata l'anteprima più recente di .NET Core. Verificare l'installazione eseguendo `dotnet --info` nella console. Dal pannello **Servizi app**:

1. Immettere "con" nella casella di ricerca o scorrere verso il basso l'elenco dei riquadri di gestione fino a **STRUMENTI DI LAVORO**.
1. Selezionare **STRUMENTI DI LAVORO** > **Console**.
1. Immettere `dotnet --info` nella console.

Se la versione `2.1.300-preview1-008174` è la versione di anteprima più recente,viene ottenuto l'output seguente eseguendo `dotnet --info` al prompt dei comandi:

![Pannello dell'app Azure con i passaggi precedenti](index/_static/cons.png)

La versione di ASP.NET Core illustrata nell'immagine precedente `2.1.300-preview1-008174` è un esempio. La versione di anteprima di ASP.NET Core più recente al momento della configurazione dell'estensione del sito viene visualizzata quando si esegue `dotnet --info`.

`dotnet --info` consente di visualizzare il percorso dell'estensione del sito in cui è stata installata l'anteprima. Mostra che l'app è in esecuzione dall'estensione del sito invece che dal percorso predefinito *ProgramFiles*. Se viene visualizzato *ProgramFiles*, riavviare il sito ed eseguire `dotnet --info`.

**Usare l'estensione del sito di anteprima con un modello ARM**

Se per creare e distribuire le app si usa un modello ARM, è possibile usare il tipo di risorsa `siteextensions` per aggiungere l'estensione del sito a un'app Web. Ad esempio:

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a>Usare Docker con app Web per contenitori

L'[hub Docker](https://hub.docker.com/r/microsoft/aspnetcore/) contiene le immagini di Docker più recenti per la versione di anteprima. Le immagini possono essere usate come immagini di base. Usare l'immagine e distribuirla alle app Web per i contenitori normalmente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Panoramica di App Web (video di 5 minuti)](/azure/app-service/app-service-web-overview)
* [Azure App Service: The Best Place to Host your .NET Apps ](https://channel9.msdn.com/events/dotnetConf/2017/T222) (Servizio app di Azure: la soluzione migliore per l'hosting delle app .NET) (video di 55 minuti)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)
* [Panoramica della diagnostica del servizio app di Azure](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Il servizio app di Azure in Windows Server usa [Internet Information Services (IIS)](https://www.iis.net/). Gli argomenti seguenti riguardano la tecnologia IIS sottostante:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Libreria di Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
