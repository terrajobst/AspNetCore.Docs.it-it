---
uid: mvc/overview/deployment/docker
title: Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows
description: Informazioni su come eseguire un'applicazione ASP.NET MVC esistente in un contenitore Docker di Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 1343bd100f521326477ecd831aa627b4394bad44
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795353"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows

Per l'esecuzione di un'applicazione basata su .NET Framework esistente in un contenitore di Windows non sono richieste modifiche dell'app. Per eseguire l'app in un contenitore di Windows si crea un'immagine Docker contenente l'app e si avvia il contenitore. Questo argomento illustra come distribuire un'[applicazione ASP.NET MVC](http://www.asp.net/mvc) esistente in un contenitore di Windows.

Si parte da un'app esistente ASP.NET MVC e quindi si compilano gli asset pubblicati usando Visual Studio. Si usa Docker per creare l'immagine che contiene ed esegue l'app. Si passerà poi al sito in esecuzione in un contenitore di Windows per verificare il funzionamento dell'app.

In questo articolo si presuppone che l'utente abbia una conoscenza di base di Docker. Per informazioni su Docker, è possibile leggere la [panoramica su Docker](https://docs.docker.com/engine/understanding-docker/).

L'app che verrà eseguita in un contenitore è un semplice sito Web che risponde alle domande in modo casuale. Questa app è un'applicazione MVC di base senza supporto per l'autenticazione né l'archiviazione in database, che consente di concentrarsi sullo spostamento del livello Web in un contenitore. Gli argomenti successivi spiegheranno come spostare e gestire l'archiviazione permanente nelle applicazioni eseguite nei contenitori.

Per spostare l'applicazione sono necessari i passaggi seguenti:

1. [Creazione di un'attività di pubblicazione per compilare le risorse necessarie per un'immagine.](#publish-script)
1. [Creazione di un'immagine Docker che eseguirà l'applicazione.](#build-the-image)
1. [Avvio di un contenitore Docker che esegue l'immagine.](#start-a-container)
1. [Verifica dell'applicazione con il browser.](#verify-in-the-browser)

L'[applicazione completata](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) è disponibile in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Il computer di sviluppo deve avere il seguente software:

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (o versione successiva) oppure [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (o versioni successive)
- [Docker per Windows](https://docs.docker.com/docker-for-windows/) - versione Stable 1.13.0 o 1.12 Beta 26 (o versioni successive)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Se si usa Windows Server 2016, seguire le istruzioni riportate in [Distribuzione di host contenitore - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Dopo aver installato e avviato Docker, fare doppio clic sull'icona sulla barra delle applicazioni e selezionare **passare ai contenitori Windows**. Ciò è necessario per eseguire le immagini Docker basate su Windows. L'esecuzione del comando richiede alcuni secondi:

![Contenitore di Windows][windows-container]

## <a name="publish-script"></a>Pubblicare lo script

Riunire in un'unica posizione tutti gli asset da caricare in un'immagine Docker. È possibile usare il comando **Pubblica** di Visual Studio per creare un profilo di pubblicazione per l'app. Tale profilo consente di inserire tutti gli asset in un'unica struttura di directory che verrà copiata nell'immagine di destinazione più avanti in questa esercitazione.

**Procedura di pubblicazione**

1. Fare clic con il pulsante destro del mouse sul progetto Web in Visual Studio e selezionare **Pubblica**.
1. Fare clic sul **pulsante del profilo personalizzato** e quindi selezionare **File system** come metodo.
1. Scegliere la directory. Per convenzione, l'esempio scaricato usa `bin\Release\PublishOutput`.

![Connessione di pubblicazione][publish-connection]

Aprire la sezione **Opzioni pubblicazione file** della scheda **Impostazioni**. Selezionare **Precompila durante la pubblicazione**. Questa ottimizzazione significa che compilando le viste nel contenitore Docker, si stanno copiando le viste precompilate.

![Impostazioni di pubblicazione][publish-settings]

Fare clic su **Pubblica** in modo che Visual Studio copi tutti gli asset necessari nella cartella di destinazione.

## <a name="build-the-image"></a>Creare l'immagine

Definire l'immagine Docker in un Dockerfile. Il Dockerfile contiene le istruzioni per l'immagine di base, eventuali componenti aggiuntivi, l'app da eseguire e altre immagini di configurazione.  Il Dockerfile costituisce l'input per il comando `docker build` che crea l'immagine.

Verrà creata un'immagine basata sull'immagine `microsoft/aspnet` situata nell'[hub Docker](https://hub.docker.com/r/microsoft/aspnet/).
L'immagine di base, `microsoft/aspnet`, è un'immagine di Windows Server. Contiene Windows Server Core, IIS e ASP.NET 4.6.2. Quando si esegue questa immagine in un contenitore, verranno avviati automaticamente IIS e i siti Web installati.

Il documento Dockerfile che crea l'immagine è simile al seguente:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Non esiste alcun comando `ENTRYPOINT` in questo Dockerfile. Non è necessario. Quando si esegue Windows Server con IIS, il processo IIS è il punto di ingresso, configurato per l'avvio nell'immagine di base aspnet.

Eseguire il comando di compilazione di Docker per creare l'immagine che esegue l'app ASP.NET. A tale scopo, aprire una finestra di PowerShell nella directory del progetto e digitare il comando seguente nella directory della soluzione:

```console
docker build -t mvcrandomanswers .
```

Questo comando consente di creare la nuova immagine usando le istruzioni nel Dockerfile, denominazione (-t tagging) come mvcrandomanswers un'immagine. L'operazione potrebbe includere il pull dell'immagine di base dall'[hub Docker](http://hub.docker.com) e l'aggiunta dell'app a tale immagine.

Dopo aver completato il comando è possibile eseguire il comando `docker images` per visualizzare informazioni sulla nuova immagine:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

L'ID immagine sarà diverso nel computer in uso. A questo punto è possibile eseguire l'app.

## <a name="start-a-container"></a>Avviare un contenitore

Avviare un contenitore eseguendo il comando `docker run` seguente:

```console
docker run -d --name randomanswers mvcrandomanswers
```

L'argomento `-d` indica a Docker di avviare l'immagine senza collegamento. Ciò significa che l'immagine Docker viene eseguita scollegata dalla shell corrente.

In molti esempi di docker, è possibile riscontrare -p per il mapping delle porte contenitore e host. L'immagine aspnet di predefinita è già configurato il contenitore per l'ascolto sulla porta 80 e lo si espone.

Il valore `--name randomanswers` assegna un nome al contenitore in esecuzione. È possibile usare questo nome anziché l'ID del contenitore nella maggior parte dei comandi.

Il valore `mvcrandomanswers` è il nome dell'immagine da avviare.

## <a name="verify-in-the-browser"></a>Verificare nel browser

> [!NOTE]
> Con la versione corrente di contenitore di Windows, è possibile esplorare `http://localhost`.
> Questo è un comportamento noto in WinNAT e verrà risolto in futuro. Fino a quel momento sarà necessario usare l'indirizzo IP del contenitore.

Dopo l'avvio del contenitore, trovarne l'indirizzo IP in modo da consentire la connessione al contenitore in esecuzione da un browser:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

Connettersi al contenitore in esecuzione usando l'indirizzo IPv4, `http://172.31.194.61` nell'esempio illustrato. Digitando l'URL nel browser viene visualizzato il sito in esecuzione.

> [!NOTE]
> Alcuni software VPN o proxy possono impedire la navigazione nel sito.
> È possibile disabilitarli temporaneamente per verificare che il contenitore funzioni.

La directory di esempio su GitHub contiene un [script di PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) che esegue automaticamente questi comandi. Aprire una finestra di PowerShell, passare alla directory soluzione e digitare:

```console
./run.ps1
```

Il comando precedente crea l'immagine, visualizza l'elenco di immagini presenti nel computer, avvia un contenitore e visualizza l'indirizzo IP per tale contenitore.

Per arrestare il contenitore, eseguire un comando `docker
stop`:

```console
docker stop randomanswers
```

Per rimuovere il contenitore, eseguire un comando `docker rm`:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Passare a un contenitore di Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Pubblicare nel file system"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Impostazioni di pubblicazione"
