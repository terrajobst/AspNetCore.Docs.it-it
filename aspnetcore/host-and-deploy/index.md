---
title: Hosting e distribuzione di ASP.NET Core
author: rick-anderson
description: Informazioni sulla configurazione degli ambienti host e sulla distribuzione delle app ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 1ffc7f9f2dc2a06dddb629d2d2553964b56cec05
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2018
---
# <a name="host-and-deploy-aspnet-core"></a>Hosting e distribuzione di ASP.NET Core

In generale, per distribuire un'app ASP.NET Core in un ambiente host:

* Pubblicare l'app in una cartella nel server host.
* Configurare un gestore processi che avvia l'app quando arrivano richieste e la riavvia quando si blocca o quando il server viene riavviato.
* Se è necessario eseguire la configurazione di un proxy inverso, configurare un proxy inverso che inoltra le richieste all'app.

## <a name="publish-to-a-folder"></a>Pubblicare in una cartella

Il comando CLI [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) compila il codice dell'app e copia i file necessari per eseguire l'app in una cartella *publish*. Quando si esegue la distribuzione da Visual Studio, il passaggio [dotnet publish](/dotnet/core/tools/dotnet-publish) viene eseguito automaticamente prima della copia dei file nella destinazione di distribuzione.

### <a name="folder-contents"></a>Contenuto della cartella

La cartella *publish* contiene i file dell'app con estensione *exe* e *dll*, le relative dipendenze e facoltativamente il runtime .NET.

È possibile pubblicare un'app .NET Core come app *indipendente* o come app *dipendente dal framework*. Se l'app è indipendente i file con estensione *DLL* che contengono il runtime .NET sono inclusi nella cartella *publish*. Se l'app è dipendente dal framework, i file di runtime .NET non sono inclusi, perché l'app contiene un riferimento a una versione di .NET installata nel server. Il modello di distribuzione predefinito è il modello dipendente dal framework. Per altre informazioni, vedere [Distribuzione di applicazioni .NET Core](/dotnet/articles/core/deploying/index).

Oltre ai file con estensione *EXE* e *DLL* la cartella *publish* di un'app ASP.NET Core contiene in genere i file di configurazione, gli asset statici e le visualizzazioni MVC. Per altre informazioni, vedere [Directory structure](xref:host-and-deploy/directory-structure) (Struttura della directory).

## <a name="set-up-a-process-manager"></a>Configurare un gestore processi

Un'app ASP.NET Core è un'app console che deve essere avviata all'avvio di un server e riavviata in caso di arresto anomalo del server stesso. Per automatizzare le operazioni di avvio e riavvio, deve essere presente un gestore processi. I gestori processi più comuni per ASP.NET Core sono:

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* WINDOWS
  * [IIS](xref:host-and-deploy/iis/index)
  * [Servizio Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurare un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se l'app usa il servizio Web [Kestrel](xref:fundamentals/servers/kestrel) è possibile usare come server proxy inverso [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index). Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.

Entrambe le configurazioni (con o senza un server proxy inverso) sono configurazioni di hosting valide e supportate per le app ASP.NET Core 2.0 o versioni successive. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se l'app usa il server Web [Kestrel](xref:fundamentals/servers/kestrel) ed è esposta a Internet, usare [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) o [IIS](xref:host-and-deploy/iis/index) come server proxy inverso. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari. Il motivo principale per l'uso di un proxy inverso è la sicurezza. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenari con server proxy e servizi di bilanciamento del carico

Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro a server proxy e a servizi di bilanciamento del carico. Senza alcuna configurazione aggiuntiva, un'app potrebbe non avere accesso allo schema (HTTP/HTTPS) e all'indirizzo IP remoto di origine di una richiesta. Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Uso di Visual Studio e MSBuild per automatizzare la distribuzione

In molti casi la distribuzione richiede attività aggiuntive oltre alla copia dell'output da [dotnet publish](/dotnet/core/tools/dotnet-publish) a un server. Ad esempio è possibile che nella cartella *publish* siano necessari file aggiuntivi o vengano esclusi uno o più file. Per la distribuzione Web, Visual Studio usa MSBuild, che può essere personalizzato per eseguire molte altre attività durante la distribuzione. Per altre informazioni, vedere [Publish profiles in Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) (Pubblicare profili in Visual Studio) e il libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Uso di MSBuild e Team Foundation Build).

Mediante la [funzionalità Pubblica sito Web](xref:tutorials/publish-to-azure-webapp-using-vs) o il [supporto di Git incorporato](xref:host-and-deploy/azure-apps/azure-continuous-deployment) è possibile eseguire direttamente la distribuzione di app da Visual Studio al Servizio app di Azure. Visual Studio Team Services supporta la [distribuzione continua al Servizio app di Azure](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Pubblicazione in Azure

Per istruzioni su come pubblicare l'app in Azure usando Visual Studio, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs). L'app può essere pubblicata in Azure anche dalla [riga di comando](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Risorse aggiuntive

Per informazioni sull'uso di Docker come ambiente host, vedere [Host ASP.NET Core apps in Docker](xref:host-and-deploy/docker/index) (Ospitare app ASP.NET Core in Docker).
