---
title: Panoramica dell'hosting e della distribuzione - ASP.NET Core
author: tdykstra
description: Panoramica sulla configurazione degli ambienti host e sulla distribuzione delle app ASP.NET Core in tali ambienti.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: df3c1f0c2768b89c3ea5dc901782170c530a542e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>Panoramica dell'hosting e della distribuzione per le app ASP.NET Core

Ecco i passaggi principali da eseguire per distribuire un'applicazione ASP.NET Core in un ambiente host:

* Pubblicare l'app in una cartella nel server host.
* Configurare un gestore processi che avvia l'app quando arrivano richieste e la riavvia quando si blocca o quando il server viene riavviato.
* Configurare un proxy inverso che inoltra le richieste all'app.

## <a name="publish-to-a-folder"></a>Pubblicare in una cartella 

Il comando CLI [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) compila il codice dell'applicazione e copia i file necessari per eseguire l'applicazione in una cartella *publish*. Quando si esegue la distribuzione da Visual Studio, il passaggio `dotnet publish` viene eseguito automaticamente prima della copia dei file nella destinazione di distribuzione.

### <a name="folder-contents"></a>Contenuto della cartella

La cartella *publish* contiene i file dell'applicazione con estensione *EXE* e *DLL*, le relative dipendenze e facoltativamente il runtime .NET.

È possibile pubblicare un'app .NET Core come app *indipendente* o *dipendente dal framework*. Se l'app è indipendente i file con estensione *DLL* che contengono il runtime .NET sono inclusi nella cartella *publish*.  Se l'app è dipendente dal framework i file di runtime .NET non sono inclusi, perché l'app contiene un riferimento a una versione di .NET installata nel computer. Il modello di distribuzione predefinito è il modello dipendente dal framework. Per altre informazioni, vedere [Distribuzione di applicazioni .NET Core](https://docs.microsoft.com/dotnet/articles/core/deploying/index).

Oltre ai file con estensione *EXE* e *DLL* la cartella *publish* di un'app ASP.NET Core contiene in genere i file di configurazione, gli asset statici e le visualizzazioni MVC.  Per altre informazioni, vedere [Directory structure](xref:hosting/directory-structure) (Struttura della directory).

## <a name="set-up-a-process-manager"></a>Configurare un gestore processi

Un'app ASP.NET Core è un'app console che deve essere avviata all'avvio di un server e riavviata dopo ogni arresto anomalo del sistema. Per automatizzare le operazioni di avvio e riavvio è necessario un gestore processi. I gestori processo più comuni per ASP.NET Core sono [Nginx](xref:publishing/linuxproduction) e [Apache](xref:publishing/apache-proxy) su Linux e [IIS](xref:publishing/iis) e [Windows Service](xref:hosting/windows-service) su Windows.

## <a name="set-up-a-reverse-proxy"></a>Configurare un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se l'app usa il server Web [Kestrel](xref:fundamentals/servers/kestrel) è possibile usare come server proxy inverso [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) o [IIS](xref:publishing/iis). Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Se l'app usa il server Web [Kestrel](xref:fundamentals/servers/kestrel) ed è esposta a Internet, è necessario usare [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy) o [IIS](xref:publishing/iis) come server proxy inverso. Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari. Il motivo principale per l'uso di un proxy inverso è la sicurezza. Per altre informazioni, vedere [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy) (Quando usare Kestrel con un proxy inverso).

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Uso di Visual Studio e MSBuild per automatizzare la distribuzione

In molti casi la distribuzione richiede attività aggiuntive oltre alla copia dell'output da `dotnet publish` a un server. Ad esempio può risultare utile includere file aggiuntivi nella cartella *publish* o escludere file dalla cartella stessa. Per la distribuzione Web Visual Studio usa MSBuild, che può essere personalizzato per l'esecuzione di molte altre attività durante la distribuzione. Per altre informazioni, vedere [Publish profiles in Visual Studio](xref:publishing/web-publishing-vs) (Pubblicare profili in Visual Studio) e il libro [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Uso di MSBuild e Team Foundation Build).

È possibile eseguire direttamente la distribuzione da Visual Studio al Servizio app di Azure usando la [funzionalità Pubblica sito Web](xref:tutorials/publish-to-azure-webapp-using-vs) o il [supporto di Git incorporato](xref:publishing/azure-continuous-deployment). Visual Studio Team Services supporta la [distribuzione continua al Servizio app di Azure](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure).

## <a name="additional-resources"></a>Risorse aggiuntive

Per informazioni sull'uso di Docker come ambiente host, vedere [Host ASP.NET Core apps in Docker](xref:publishing/docker) (Ospitare app ASP.NET Core in Docker).
