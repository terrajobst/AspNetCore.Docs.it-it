---
title: Ospitare ASP.NET Core in Servizio app di Azure
author: guardrex
description: Informazioni su come ospitare le app ASP.NET Core nel servizio app di Azure con collegamenti a risorse utili.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f53f77d342cc59094a80e8667db6ef345a6e8305
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
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

## <a name="application-configuration"></a>Configurazione dell'applicazione

Con ASP.NET Core 2.0 e versioni successive, tre pacchetti del [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage) offrono funzionalità di registrazione automatica per le app distribuite nel servizio app di Azure:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) per fornire l'integrazione di ASP.NET Core con il servizio app di Azure. La funzionalità di registrazione aggiunte vengono fornite dal pacchetto `Microsoft.AspNetCore.AzureAppServicesIntegration`.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) esegue [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) per aggiungere i provider di registrazione diagnostica del servizio app di Azure nel pacchetto `Microsoft.Extensions.Logging.AzureAppServices`.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) offre implementazioni di logger per il supporto dei registri di diagnostica del servizio app di Azure e delle funzionalità del flusso di registrazione.

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta. Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Monitoraggio e registrazione

Per informazioni sul monitoraggio, la registrazione e la risoluzione dei problemi, vedere gli articoli seguenti:

[Procedura: Eseguire il monitoraggio delle app nel servizio app di Azure](/azure/app-service/web-sites-monitor)  
Informazioni su come esaminare le quote e le metriche per le app e i piani del servizio app.

[Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](/azure/app-service/web-sites-enable-diagnostic-log)  
Informazioni su come abilitare e accedere alla registrazione diagnostica per i codici di stato HTTP, le richieste non riuscite e l'attività del server Web.

[Introduzione alla gestione degli errori in ASP.NET Core](xref:fundamentals/error-handling)  
Informazioni sugli approcci comuni di gestione degli errori nelle app ASP.NET Core.

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
* [Distribuire l'app autonoma](#deploy-the-app-self-contained)
* [Usare Docker con app Web per contenitori](#use-docker-with-web-apps-for-containers)

Se si verifica un problema con l'estensione del sito di anteprima, aprire un problema in [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Installare l'estensione del sito di anteprima

* Dal portale di Azure passare al pannello Servizi App.
* Immettere "est" nella casella di ricerca.
* Selezionare **Estensioni**.
* Seleziona "Aggiungi".

![Pannello dell'app Azure con i passaggi precedenti](index/_static/x1.png)

* Selezionare **ASP.NET Core 2.1 (x86) Runtime** (Runtime ASP.NET Core 2.1 (x86)) o **ASP.NET Core 2.1 (x64) Runtime** (Runtime ASP.NET Core 2.1 (x64)).
* Scegliere **OK**. Selezionare di nuovo **OK**.

Al termine delle operazioni di aggiunta, viene installata l'anteprima più recente di .NET Core 2.1. Verificare l'installazione eseguendo `dotnet --info` nella console. Dal pannello **Servizi app**:

* Immettere "con" nella casella di ricerca.
* Selezionare **Console**.
* Immettere `dotnet --info` nella console.

![Pannello dell'app Azure con i passaggi precedenti](index/_static/cons.png)

L'immagine precedente era aggiornata al momento della redazione di questo articolo. Potrebbe essere visualizzata una versione diversa.

`dotnet --info` consente di visualizzare il percorso dell'estensione del sito in cui è stata installata l'anteprima. Mostra che l'app è in esecuzione dall'estensione del sito invece che dal percorso predefinito *ProgramFiles*. Se viene visualizzato *ProgramFiles*, riavviare il sito ed eseguire `dotnet --info`.

**Usare l'estensione del sito di anteprima con un modello ARM**

Se per creare e distribuire le app si usa un modello ARM, è possibile usare il tipo di risorsa `siteextensions` per aggiungere l'estensione del sito a un'app Web. Ad esempio:

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Distribuire l'app autonoma

È possibile distribuire un'[app autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) che porti il runtime di anteprima nella distribuzione. Per la distribuzione di un'app autonoma:

* Non è necessario preparare il sito.
* L'app deve essere pubblicata in modo diverso rispetto a quando la pubblicazione riguarda una distribuzione dipendente dal framework con il runtime condiviso e l'host nel server.

Le app autonome sono un'opzione per tutte le app ASP.NET Core.

### <a name="use-docker-with-web-apps-for-containers"></a>Usare Docker con app Web per contenitori

L'[hub di Docker](https://hub.docker.com/r/microsoft/aspnetcore/) contiene le immagini di Docker più recenti per la versione di anteprima 2.1. Le immagini possono essere usate come immagini di base. Usare l'immagine e distribuirla alle app Web per i contenitori normalmente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Panoramica di App Web (video di 5 minuti)](/azure/app-service/app-service-web-overview)
* [Azure App Service: The Best Place to Host your .NET Apps ](https://channel9.msdn.com/events/dotnetConf/2017/T222) (Servizio app di Azure: la soluzione migliore per l'hosting delle app .NET) (video di 55 minuti)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience) (Azure Friday: diagnostica e risoluzione dei problemi del servizio app di Azure) (video di 12 minuti)
* [Panoramica della diagnostica del servizio app di Azure](/azure/app-service/app-service-diagnostics)

Il servizio app di Azure in Windows Server usa [Internet Information Services (IIS)](https://www.iis.net/). Gli argomenti seguenti riguardano la tecnologia IIS sottostante:

* [Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index)
* [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Libreria di Microsoft TechNet: Windows Server](/windows-server/windows-server-versions)
