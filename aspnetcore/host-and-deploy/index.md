---
title: Hosting e distribuzione di ASP.NET Core
author: guardrex
description: Informazioni sulla configurazione degli ambienti host e sulla distribuzione delle app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: host-and-deploy/index
ms.openlocfilehash: f443a8ee28a859b5075a8bb03016407af9a3ddb1
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284526"
---
# <a name="host-and-deploy-aspnet-core"></a>Hosting e distribuzione di ASP.NET Core

In generale, per distribuire un'app ASP.NET Core in un ambiente host:

* Distribuire l'app pubblicata in una cartella nel server di hosting.
* Configurare un gestore processi che avvia l'app quando arrivano richieste e la riavvia quando si blocca o quando il server viene riavviato.
* Per la configurazione di un proxy inverso, configurare un proxy inverso per inoltrare le richieste all'app.

## <a name="publish-to-a-folder"></a>Pubblicare in una cartella

Il comando [dotnet publish](/dotnet/core/tools/dotnet-publish) compila il codice dell'app e copia il file necessario per eseguire l'app in una cartella *publish*. Quando si esegue la distribuzione da Visual Studio, il passaggio `dotnet publish` viene eseguito automaticamente prima della copia dei file nella destinazione di distribuzione.

### <a name="folder-contents"></a>Contenuto della cartella

La cartella *publish* contiene uno o più file di assembly di app, le dipendenze e facoltativamente il runtime di .NET.

È possibile pubblicare un'app .NET Core come *distribuzione indipendente* o come *distribuzione dipendente dal framework*. Se l'app è indipendente, i file di assembly che contengono il runtime .NET sono inclusi nella cartella *publish*. Se l'app è dipendente dal framework, i file di runtime .NET non sono inclusi, perché l'app contiene un riferimento a una versione di .NET installata nel server. Il modello di distribuzione predefinito è il modello dipendente dal framework. Per altre informazioni, vedere [Distribuzione di applicazioni .NET Core](/dotnet/core/deploying/).

Oltre ai file con estensione *EXE* e *DLL* la cartella *publish* di un'app ASP.NET Core contiene in genere i file di configurazione, gli asset statici e le visualizzazioni MVC. Per ulteriori informazioni, vedere <xref:host-and-deploy/directory-structure>.

## <a name="set-up-a-process-manager"></a>Configurare un gestore processi

Un'app ASP.NET Core è un'app console che deve essere avviata all'avvio di un server e riavviata in caso di arresto anomalo del server stesso. Per automatizzare le operazioni di avvio e riavvio, deve essere presente un gestore processi. I gestori processi più comuni per ASP.NET Core sono:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* WINDOWS
  * [IIS](xref:host-and-deploy/iis/index)
  * [Servizio Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurare un proxy inverso

::: moniker range=">= aspnetcore-2.0"

Se l'app usa il server [Kestrel](xref:fundamentals/servers/kestrel) è possibile usare come server proxy inverso [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index). Il server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel.

Entrambe le configurazioni &mdash;con o senza un server proxy inverso&mdash; sono configurazioni di hosting supportate per le app ASP.NET Core 2.0 o versioni successive. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Se l'app usa il server [Kestrel](xref:fundamentals/servers/kestrel) e sarà esposta a Internet, usare [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) come server proxy inverso. Il server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel. Il motivo principale per l'uso di un proxy inverso è la sicurezza. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico. Senza alcuna configurazione aggiuntiva, un'app potrebbe non avere accesso allo schema (HTTP/HTTPS) e all'indirizzo IP remoto di origine di una richiesta. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="use-visual-studio-and-msbuild-to-automate-deployments"></a>Usare Visual Studio e MSBuild per automatizzare le distribuzioni

In molti casi la distribuzione richiede attività aggiuntive oltre alla copia dell'output da [dotnet publish](/dotnet/core/tools/dotnet-publish) a un server. Ad esempio è possibile che nella cartella *publish* siano necessari file aggiuntivi o vengano esclusi uno o più file. Per la distribuzione Web, Visual Studio usa MSBuild, che può essere personalizzato per eseguire molte altre attività durante la distribuzione. Per altre informazioni, vedere <xref:host-and-deploy/visual-studio-publish-profiles> e il libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Uso di MSBuild e Team Foundation Build).

Mediante la [funzionalità Pubblica sito Web](xref:tutorials/publish-to-azure-webapp-using-vs) o il [supporto di Git incorporato](xref:host-and-deploy/azure-apps/azure-continuous-deployment) è possibile eseguire direttamente la distribuzione di app da Visual Studio al Servizio app di Azure. Azure DevOps Services supporta la [distribuzione continua al Servizio app di Azure](/azure/devops/pipelines/targets/webapp). Per altre informazioni, vedere [DevOps con ASP.NET Core e Azure](xref:azure/devops/index).

## <a name="publish-to-azure"></a>Pubblicare in Azure

Vedere <xref:tutorials/publish-to-azure-webapp-using-vs> per istruzioni su come pubblicare un'app in Azure usando Visual Studio. Un altro esempio è disponibile in [Creare un'app Web ASP.NET Core in Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="publish-with-msdeploy-on-windows"></a>Pubblicare con MSDeploy in Windows

Vedere <xref:host-and-deploy/visual-studio-publish-profiles> per istruzioni su come pubblicare un'app con un profilo di pubblicazione di Visual Studio, ad esempio da un prompt dei comandi di Windows usando il comando [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild).

## <a name="host-in-a-web-farm"></a>Hosting in una Web farm

Per informazioni sulla configurazione per ospitare app ASP.NET Core in un ambiente Web farm (ad esempio, distribuzione di più istanze di un'app per la scalabilità), vedere <xref:host-and-deploy/web-farm>.

::: moniker range=">= aspnetcore-2.2"

## <a name="perform-health-checks"></a>Eseguire controlli di integrità

Usare il middleware di controllo integrità per eseguire controlli di integrità su un'app e le relative dipendenze. Per ulteriori informazioni, vedere <xref:host-and-deploy/health-checks>.

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/docker/index>
* <xref:test/troubleshoot>
