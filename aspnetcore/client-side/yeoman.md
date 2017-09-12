---
title: Compilazione di progetti con Yeoman in ASP.NET Core
author: spboyer
description: In questo articolo illustra come generare un ASP.NET Core applicazione web tramite il Yeoman generatore in macOS.
keywords: ASP.NET Core, Yeoman, multipiattaforma, messaggio aspnet
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: 61561b55774faf375090c92b574a64f1a12f9647
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Introduzione alla creazione di progetti con Yeoman in ASP.NET Core

[Yeoman](http://yeoman.io/) è un sistema di scaffolding di progetto per la creazione di diversi tipi di applicazioni. Il Yeoman generatore per ASP.NET Core contiene una varietà di modelli di progetto per l'avvio di un nuovo sito web, MVC o l'applicazione console.

## <a name="install-nodejs-npm-and-yeoman"></a>Installazione di Node.js e npm Yeoman

### <a name="prerequisites"></a>Prerequisiti

Sono necessari per Yeoman Node.js e npm. Scaricare da [Node.js](https://nodejs.org/). Il programma di installazione include [Node.js](https://nodejs.org/) e [npm](https://www.npmjs.com/). È necessario per l'installazione di Framework dell'interfaccia utente come Bootstrap anche bower.

Per installare Yeoman e Bower, eseguire il comando seguente:

```console
npm install -g yo bower
```

>[!Note]
>Se viene visualizzato l'errore `npm ERR! Please try running this command again as root/Administrator.` in macOS, eseguire il comando seguente utilizzando [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Da un prompt dei comandi, installare il generatore ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Se si verifica un errore di autorizzazione, eseguire il comando in `sudo` come descritto in precedenza.

Il `–g` flag installa il generatore a livello globale, in modo che può essere utilizzato da qualsiasi percorso.

## <a name="create-an-aspnet-app"></a>Creare un'applicazione ASP.NET

Eseguire il generatore di ASP.NET basato su Yeoman:

```console
yo aspnet
```

Il generatore di Visualizza un menu. Freccia verso il basso per il **Web Application base [senza l'appartenenza e l'autorizzazione]** del progetto e toccare **invio**:

![Finestra di comando: il tipo di applicazione si desidera creare? Menu di tipi di applicazioni](yeoman/_static/yeoman-yo-aspnet.png)

Selezionare Bootstrap del framework dell'interfaccia utente e toccare **invio**.

Utilizzare "**MyWebApp**" per il nome dell'applicazione e quindi toccare **invio**.

Yeoman verrà lo scaffolding al progetto e i relativi file di supporto. Passaggi successivi suggeriti vengono inoltre forniti sotto forma di comandi.

![Finestra di comando: che cos'è il nome dell'applicazione ASP.NET? Prompt dei comandi](yeoman/_static/yeoman-yo-aspnet-created.png)

Il [generatore ASP.NET](https://www.npmjs.com/package/generator-aspnet) crea ASP.NET Core progetti che possono essere caricato nel codice di Visual Studio, Visual Studio, o eseguire dalla riga di comando.

## <a name="restore-build-and-run"></a>Il ripristino, compilare ed eseguire

Seguire i comandi suggeriti modificando la directory in cui il `MyWebApp` directory. Eseguire quindi `dotnet restore`.

![finestra di comando](yeoman/_static/dotnet-restore.png)

Compilare ed eseguire l'app mediante `dotnet build` e `dotnet run`:

![finestra di comando](yeoman/_static/dotnet-build-run.png)

A questo punto, è possibile passare all'URL indicato per testare l'app di ASP.NET Core appena creato.

## <a name="client-side-packages"></a>Pacchetti sul lato client

Le risorse front-end vengono fornite tramite i modelli dal Yeoman utilizzando Generatore di [Bower](xref:client-side/bower) gestione di pacchetti sul lato client, aggiungere *bower. JSON* e *. bowerrc* file da ripristinare i pacchetti sul lato client tramite Bower.

Il [BundlerMinifier](xref:client-side/bundling-and-minification) componente è inoltre incluso per impostazione predefinita per facilità di concatenazione (aggregazione) e la minimizzazione CSS, JavaScript e HTML.

## <a name="building-and-running-from-visual-studio"></a>Compilazione e l'esecuzione da Visual Studio

È possibile caricare il progetto web ASP.NET Core generato direttamente in Visual Studio, quindi compilare ed eseguire il progetto da qui. Seguire le istruzioni sopra riportate per eseguire lo scaffolding di una nuova app di ASP.NET Core utilizzando Yeoman. Questa volta scegliere **applicazione Web** nel menu e il nome dell'app `MyWebApp`.

Aprire Visual Studio. Dal menu File, selezionare ‣ Apri progetto/soluzione.

Nella finestra di dialogo Apri progetto individuare il *csproj* file, selezionarlo e fare clic su di **aprire** pulsante. In Esplora soluzioni, il progetto dovrebbe essere simile nella schermata seguente.

![File e cartelle di un nuovo progetto in Esplora soluzioni](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds un'applicazione web MVC, completo sia lato client e server di supporto per la compilazione. Dipendenze sul lato server sono elencate sotto la **dipendenze/NuGet** nodo e le dipendenze sul lato client il **dipendenze/Bower** nodo di Esplora soluzioni. Le dipendenze vengono ripristinate automaticamente quando viene caricato il progetto.

![Sotto il nodo di dipendenze in visualizzazione struttura ad albero di Esplora soluzioni, la cartella Bower è aprire l'elenco delle relative dipendenze.](yeoman/_static/yeoman-loading-dependencies.png)

Quando tutte le dipendenze vengono ripristinate, premere **F5** per eseguire il progetto. Consente di visualizzare la home page predefinita nel browser.

![Applicazione Web aperto in Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Il ripristino, la creazione e l'hosting dalla riga di comando

È possibile preparare e ospitare l'applicazione web tramite l'interfaccia CLI di .NET Core.

Al prompt dei comandi, modificare la directory corrente alla cartella contenente il progetto (vale a dire la cartella contenente il *csproj* file):

```console
cd src\MyWebApp
```

Ripristinare le dipendenze di pacchetto NuGet del progetto:

```console
dotnet restore
```

Eseguire l'applicazione:

```console
dotnet run
```

La piattaforma incrociata [Kestrel](xref:fundamentals/servers/kestrel) server web verrà mettersi in ascolto sulla porta 5000.

Aprire un web browser e passare a `http://localhost:5000`.

![Applicazione Web aperto in Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Aggiunta a un progetto con i generatori di sub

Utilizzando Yeoman [sub generatori](https://github.com/omnisharp/generator-aspnet), è possibile aggiungere un `nuget.config` o `web.config` dopo la creazione del progetto. Ad esempio, eseguire il comando seguente dalla directory in cui deve essere creato il file:

```console
yo aspnet:nugetconfig
```

Il risultato è un file di configurazione NuGet denominato `nuget.config` con il seguente contenuto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Server (Kestrel WebListener)](xref:fundamentals/servers/index)
* [Nozioni di base](xref:fundamentals/index)
