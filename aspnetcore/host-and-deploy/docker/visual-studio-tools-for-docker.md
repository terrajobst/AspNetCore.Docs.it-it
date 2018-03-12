---
title: Visual Studio Tools per Docker con ASP.NET Core
author: spboyer
description: Informazioni su come usare gli strumenti di Visual Studio 2017 e Docker per Windows per aggiungere un'app ASP.NET Core in un contenitore.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools per Docker con ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) supporta la compilazione, debug e in esecuzione nei contenitori ASP.NET Core App destinate a .NET Core. Sono supportati sia contenitori Windows che contenitori Linux.

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017](https://www.visualstudio.com/) con il carico di lavoro **Sviluppo multipiattaforma .NET Core**
* [Docker per Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Installazione e configurazione

Per l'installazione Docker, vedere le informazioni riportate in [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker per Windows: informazioni da conoscere prima dell'installazione) e installare [Docker per Windows](https://docs.docker.com/docker-for-windows/install/).

**[Le unità condivise](https://docs.docker.com/docker-for-windows/#shared-drives)**  in Docker per Windows devono essere configurate per supportare il mapping e il debug del volume. Fare clic sull'icona di Docker dell'area di notifica, selezionare **impostazioni...** e selezionare **condiviso unità**. Selezionare l'unità in cui Docker vengono archiviati i file. Selezionare **applicare**.

![Shared Drives (Unità condivise)](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 15,6 e versioni successive è richiesto quando **condiviso unità** non sono configurati.

## <a name="add-docker-support-to-an-app"></a>Aggiungere il supporto di Docker a un'app

Per aggiungere il supporto di Docker a un progetto ASP.NET Core, il progetto deve avere come destinazione .NET Core. Contenitori di Linux e Windows sono supportati.

Quando si aggiunge il supporto di Docker a un progetto, scegliere Windows o un contenitore di Linux. L'host Docker deve eseguire lo stesso tipo di contenitore. Per modificare il tipo di contenitore nell'istanza di Docker in esecuzione, fare clic con il pulsante destro del mouse sull'icona di Docker sulla barra delle applicazioni e scegliere **Switch to Windows containers** (Passa ai contenitori Windows) o **Switch to Linux containers** (Passa ai contenitori Linux).

### <a name="new-app"></a>Nuova app

Quando si crea una nuova app con i modelli di progetto **Applicazione Web ASP.NET Core** selezionare la casella di controllo **Abilita supporto Docker**:

![Casella di controllo Abilita supporto Docker](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Se il framework di destinazione è .NET Core, il **OS** elenco a discesa consente la selezione di un tipo di contenitore.

### <a name="existing-app"></a>App esistente

Visual Studio Tools per Docker non supporta l'aggiunta di Docker a un progetto esistente di ASP.NET Core destinato a .NET Framework. Per i progetti ASP.NET Core destinati a .NET Core, esistono due opzioni per l'aggiunta del supporto di Docker tramite gli strumenti. Aprire il progetto in Visual Studio e scegliere una delle opzioni seguenti:

* Scegliere **Supporto Docker** dal menu **Progetto**.
* Fare clic con il pulsante destro del mouse in Esplora soluzioni e scegliere **Aggiungi** > **Supporto Docker**.

## <a name="docker-assets-overview"></a>Panoramica degli asset di Docker

Visual Studio Tools per Docker aggiunge un progetto *docker-compose* alla soluzione, contenente i file seguenti:

* *.dockerignore*: contiene un elenco di modelli di file e directory da escludere durante la generazione di un contesto di compilazione.
* *docker-compose.yml*: file di base [Docker Compose](https://docs.docker.com/compose/overview/) usato per definire la raccolta di immagini da compilare ed eseguire rispettivamente con `docker-compose build` e `docker-compose run`.
* *docker compose.override.yml*: file facoltativo, letto da Docker Compose, che contiene gli override di configurazione per i servizi. Visual Studio esegue `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` per unire questi file.

Alla radice del progetto viene aggiunto un *Dockerfile*, ovvero il file recipe per la creazione di un'immagine Docker finale. Vedere le [informazioni di riferimento su Dockerfile](https://docs.docker.com/engine/reference/builder/) per conoscere i comandi inclusi. Questo particolare *Dockerfile* usa una [compilazione in più fasi](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) contenente quattro fasi di compilazione distinte e denominate:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

Il *Dockerfile* si basa sull'immagine [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore). Questa immagine di base include i pacchetti NuGet ASP.NET Core che sono stati compilati PreJit per migliorare le prestazioni di avvio.

Il *docker compose.yml* file contiene il nome dell'immagine che viene creato quando viene eseguito il progetto:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

Nell'esempio precedente, `image: hellodockertools` genera l'immagine `hellodockertools:dev` quando l'app viene eseguita in modalità **Debug**. L'immagine `hellodockertools:latest` viene generata quando l'app viene eseguita in modalità **Versione**.

Anteporre il nome immagine di [Hub Docker](https://hub.docker.com/) nome utente (ad esempio, `dockerhubusername/hellodockertools`) se l'immagine verrà inserita nel Registro di sistema. In alternativa, modificare il nome dell'immagine per includere l'URL del Registro di sistema privati (ad esempio, `privateregistry.domain.com/hellodockertools`) a seconda della configurazione.

## <a name="debug"></a>Debug

Selezionare **Docker** nell'elenco a discesa Debug nella barra degli strumenti e avviare il debug dell'app. La visualizzazione **Docker** della finestra **Output** mostra le azioni seguenti:

* Il *microsoft/aspnetcore* immagine di runtime viene acquisito (se non è già nella cache).
* Il *aspnetcore/microsoft-compilazione* immagine pubblicazione/creazione di compilazione viene acquisito (se non è già nella cache).
* La variabile di ambiente *ASPNETCORE_ENVIRONMENT* viene impostata su `Development` all'interno del contenitore.
* La porta 80 viene esposta e mappata a una porta assegnata in modo dinamico per localhost. La porta è determinata dall'host Docker ed è possibile eseguirvi query con il comando `docker ps`.
* L'app viene copiato nel contenitore.
* Il browser predefinito viene avviato con il debugger collegato al contenitore utilizzando la porta assegnata dinamicamente. 

L'immagine di Docker risultante è il *dev* immagine dell'app con il *microsoft/aspnetcore* immagini come immagine di base. Eseguire il comando `docker images` nella finestra **Console di Gestione pacchetti**. Vengono visualizzate le immagini del computer:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> L'immagine di sviluppo non dispone del contenuto delle app, come **Debug** utilizzano il montaggio di volumi per garantire un'esperienza iterativa. Per effettuare il push di un'immagine, usare la configurazione **Versione**.

Eseguire il comando `docker ps` nella console di Gestione pacchetti. Si noti che l'app viene eseguita usando il contenitore:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Modifica e continuazione

Le modifiche apportate ai file statici e alle visualizzazioni Razor vengono aggiornate automaticamente senza la necessità di eseguire un passaggio di compilazione. Dopo aver apportato una modifica, salvarla e aggiornare il browser per visualizzare l'aggiornamento.  

Le modifiche ai file del codice richiedono la compilazione e il riavvio di Kestrel all'interno del contenitore. Dopo aver apportato una modifica, premere CTRL+F5 per eseguire il processo e avviare l'app all'interno del contenitore. Il contenitore Docker non viene ricompilato o arrestato. Eseguire il comando `docker ps` nella console di Gestione pacchetti. Si noti che il contenitore originale è ancora in esecuzione da 10 minuti:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Pubblicare immagini Docker

Una volta completato il ciclo di sviluppo e debug dell'app, Visual Studio Tools per Docker d'aiuto nella creazione dell'immagine di produzione dell'app. Selezionare **Versione** nell'elenco a discesa della configurazione ed eseguire l'app. Gli strumenti produce l'immagine con il *più recente* tag, che può essere inserito all'Hub Docker o privata del Registro di sistema. 

Eseguire il comando `docker images` nella console di Gestione pacchetti per visualizzare l'elenco delle immagini:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Il comando `docker images` restituisce le immagini intermedie con i nomi di repository e i tag identificati come *\<none>* (non inclusi nell'elenco precedente). Queste immagini senza nome vengono prodotte dal *Dockerfile* con [compilazione in più fasi](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). L'efficienza della creazione dell'immagine finale risulta migliorata, dato che vengono ricompilati solo i livelli necessari in seguito a modifiche. Quando le immagini intermedi non sono più necessari, eliminarli utilizzando il [rmi docker](https://docs.docker.com/engine/reference/commandline/rmi/) comando.

Ci si potrebbe aspettare che l'immagine di produzione o di versione abbia dimensioni minori rispetto all'immagine *dev*. A causa di mapping del volume, il debugger e app in esecuzione nel computer locale e non all'interno del contenitore. L'immagine *latest* include il codice dell'app necessario per eseguire l'app in un computer host. Pertanto, il delta è la dimensione del codice dell'applicazione.
