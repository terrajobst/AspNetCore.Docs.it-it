---
title: Usare ASP.NET Core SignalR con TypeScript e Webpack
author: ssougnez
description: In questa esercitazione viene configurato Webpack per aggregare e compilare un ASP.NET Core SignalR app Web il cui client è scritto in TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317479"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a>Usare ASP.NET Core SignalR con TypeScript e Webpack

Di [Sébastien Sougnez](https://twitter.com/ssougnez) e [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) consente agli sviluppatori di creare un bundle delle risorse di un'app Web sul lato client-e di compilarle. Questa esercitazione illustra l'uso di Webpack in un ASP.NET Core SignalR app Web il cui client è scritto in [typescript](https://www.typescriptlang.org/).

In questa esercitazione si imparerà a:

> [!div class="checklist"]
> * Impalcatura uno starter ASP.NET Core SignalR app
> * Configurare il client TypeScript di SignalR
> * Configurare una pipeline di compilazione tramite Webpack
> * Configurare il server di SignalR
> * Abilitare la comunicazione tra client e server

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Prerequisiti

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**
* [.NET Core SDK 3.0 o versione successiva](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 3.0 o versione successiva](https://www.microsoft.com/net/download/all)
* [C# per Visual Studio Code 1.17.1 o versione successiva](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Creare l'app Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*. Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione. Seguire queste istruzioni in Visual Studio:

1. Passare a **strumenti** > **Opzioni** > **progetti e soluzioni** > **Gestione pacchetti Web** > **strumenti Web esterni**.
1. Selezionare la voce *$(PATH)* dall'elenco. Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco.

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

La configurazione di Visual Studio è completata. A questo punto è possibile creare il progetto

1. Utilizzare l'opzione di menu **File** > **nuovo** > **progetto** e scegliere il modello **applicazione Web ASP.NET Core** .
1. Assegnare al progetto il nome *SignalRWebPack*e selezionare **Crea**.
1. Selezionare *.NET Core* dall'elenco a discesa Framework di destinazione e selezionare *ASP.NET Core 3,0* dall'elenco a discesa del selettore del Framework. Selezionare il modello **vuoto** e selezionare **Crea**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente nel **Terminale integrato**:

```dotnetcli
dotnet new web -o SignalRWebPack
```

Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.

---

## <a name="configure-webpack-and-typescript"></a>Configurare Webpack e TypeScript

I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.

1. Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:

    ```console
    npm init -y
    ```

1. Aggiungere la proprietà evidenziata al file *package.json*:

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.

1. Installare i pacchetti npm necessari. Eseguire il comando seguente dalla radice del progetto:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Ecco alcuni dettagli del comando da tenere in considerazione:

    * Un numero di versione segue il segno `@` per ogni nome di pacchetto. npm installa queste versioni del pacchetto specifiche.
    * L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*. Ad esempio, viene usato `"webpack": "4.29.3"` invece di `"webpack": "^4.29.3"`. Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.

    Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).

1. Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Ecco una spiegazione degli script:

    * `build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file. Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto. L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione. Usare `build` solo in modalità di sviluppo.
    * `release`: crea un bundle delle risorse sul lato client in modalità di produzione.
    * `publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione. Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.

1. Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    Il file precedente configura la compilazione di Webpack. Ecco alcuni dettagli di configurazione del server da tenere in considerazione:

    * La proprietà `output` esegue l'override del valore predefinito di *dist*. Il bundle viene invece creato nella directory *wwwroot*.
    * La matrice `resolve.extensions` include *. js* per importare il codice JavaScript del client SignalR.

1. Creare una nuova directory *src* nella radice del progetto. La sua funzione è quella di archiviare le risorse lato client del progetto.

1. Creare *src/index.html* con il contenuto seguente.

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    Il codice HTML precedente definisce il markup del boilerplate della home page.

1. Creare una nuova directory *src/css*. La sua funzione è quella di archiviare i file *.css* del progetto.

1. Creare *src/css/main.css* con il contenuto seguente:

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    Il file *main.css* precedente definisce lo stile dell'app.

1. Creare *src/tsconfig.json* con il contenuto seguente:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Creare *src/index.ts* con il contenuto seguente:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:

    * `keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`. La funzione `send` viene chiamata quando l'utente preme **INVIO**.
    * `click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**. Viene chiamata la funzione `send`.

## <a name="configure-the-aspnet-core-app"></a>Configurare l'app ASP.NET Core

1. Nel metodo `Startup.Configure` aggiungere chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.

1. Alla fine del metodo `Startup.Configure`, eseguire il mapping di una route */Hub* all'hub `ChatHub`. Sostituire il codice che visualizza *Hello World!* con la riga seguente: 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. Nel metodo `Startup.ConfigureServices` chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_). Aggiunge i servizi SignalR al progetto.

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. Creare una nuova directory denominata *Hubs* nella radice del progetto. Lo scopo è archiviare l'hub SignalR, che verrà creato nel passaggio successivo.

1. Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Abilitare la comunicazione tra client e server

L'app attualmente visualizza un semplice form per l'invio di messaggi. Quando si prova a eseguire questa operazione, non accade nulla. Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.

1. Eseguire il comando seguente nella radice del progetto:

    ```console
    npm install @microsoft/signalr
    ```

    Il comando precedente installa il [client diSignalR typescript](https://www.npmjs.com/package/@microsoft/signalr), che consente al client di inviare messaggi al server.

1. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Il codice precedente supporta la ricezione di messaggi dal server. La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server. La funzione `withUrl` configura l'URL dell'hub.

    SignalR consente lo scambio di messaggi tra un client e un server. Ogni messaggio ha un nome specifico. Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi. L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`. È possibile restare in ascolto di un numero indefinito di nomi di messaggio. Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto. Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`. Viene aggiunto all'elemento `div` principale che visualizza i messaggi.

1. Ora che il client può ricevere messaggi, configurarlo anche per l'invio. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`. Il primo parametro del metodo è il nome del messaggio. I dati del messaggio sono rappresentati dagli altri parametri. In questo esempio un messaggio identificato come `newMessage` viene inviato al server. Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo. Se l'invio riesce, il valore della casella di testo viene cancellato.

1. Aggiungere il metodo evidenziato alla classe `ChatHub`:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve. Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi. È sufficiente un metodo denominato dopo il nome del messaggio.

    In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`. Il metodo C# `NewMessage` si aspetta i dati inviati dal client. Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.

## <a name="test-the-app"></a>Eseguire il test dell'applicazione

Per verificare che l'app funzioni, eseguire la procedura seguente.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Eseguire Webpack in modalità *versione*. Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto. Se non ci si trova nella directory radice del progetto, immettere `cd SignalRWebPack` prima di immettere il comando.

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger. Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.

   Se vengono visualizzati errori di compilazione, provare a chiudere e riaprire la soluzione. 

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:

    ```dotnetcli
    dotnet run
    ```

    Il server Web avvia l'app e la rende disponibile su localhost.

1. Aprire un browser all'indirizzo `http://localhost:<port_number>`. Il file *wwwroot/index.html* viene servito. Copiare l'URL dalla barra dell'indirizzo.

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**
* [.NET Core SDK 2.2 o versione successiva](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.2 o versione successiva](https://www.microsoft.com/net/download/all)
* [C# per Visual Studio Code 1.17.1 o versione successiva](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Creare l'app Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*. Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione. Seguire queste istruzioni in Visual Studio:

1. Passare a **strumenti** > **Opzioni** > **progetti e soluzioni** > **Gestione pacchetti Web** > **strumenti Web esterni**.
1. Selezionare la voce *$(PATH)* dall'elenco. Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco.

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

La configurazione di Visual Studio è completata. A questo punto è possibile creare il progetto

1. Utilizzare l'opzione di menu **File** > **nuovo** > **progetto** e scegliere il modello **applicazione Web ASP.NET Core** .
1. Assegnare al progetto il nome *SignalRWebPack*e selezionare **Crea**.
1. Selezionare *.NET Core* nell'elenco a discesa del framework di destinazione e quindi selezionare *ASP.NET Core 2.2* nell'elenco a discesa del selettore del framework. Selezionare il modello **vuoto** e selezionare **Crea**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente nel **Terminale integrato**:

```dotnetcli
dotnet new web -o SignalRWebPack
```

Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.

---

## <a name="configure-webpack-and-typescript"></a>Configurare Webpack e TypeScript

I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.

1. Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:

    ```console
    npm init -y
    ```

1. Aggiungere la proprietà evidenziata al file *package.json*:

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.

1. Installare i pacchetti npm necessari. Eseguire il comando seguente dalla radice del progetto:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Ecco alcuni dettagli del comando da tenere in considerazione:

    * Un numero di versione segue il segno `@` per ogni nome di pacchetto. npm installa queste versioni del pacchetto specifiche.
    * L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*. Ad esempio, viene usato `"webpack": "4.29.3"` invece di `"webpack": "^4.29.3"`. Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.

    Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).

1. Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Ecco una spiegazione degli script:

    * `build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file. Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto. L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione. Usare `build` solo in modalità di sviluppo.
    * `release`: crea un bundle delle risorse sul lato client in modalità di produzione.
    * `publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione. Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.

1. Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    Il file precedente configura la compilazione di Webpack. Ecco alcuni dettagli di configurazione del server da tenere in considerazione:

    * La proprietà `output` esegue l'override del valore predefinito di *dist*. Il bundle viene invece creato nella directory *wwwroot*.
    * La matrice `resolve.extensions` include *. js* per importare il codice JavaScript del client SignalR.

1. Creare una nuova directory *src* nella radice del progetto. La sua funzione è quella di archiviare le risorse lato client del progetto.

1. Creare *src/index.html* con il contenuto seguente.

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    Il codice HTML precedente definisce il markup del boilerplate della home page.

1. Creare una nuova directory *src/css*. La sua funzione è quella di archiviare i file *.css* del progetto.

1. Creare *src/css/main.css* con il contenuto seguente:

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    Il file *main.css* precedente definisce lo stile dell'app.

1. Creare *src/tsconfig.json* con il contenuto seguente:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Creare *src/index.ts* con il contenuto seguente:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:

    * `keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`. La funzione `send` viene chiamata quando l'utente preme **INVIO**.
    * `click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**. Viene chiamata la funzione `send`.

## <a name="configure-the-aspnet-core-app"></a>Configurare l'app ASP.NET Core

1. Il codice fornito nel metodo `Startup.Configure` visualizza *Hello World!* . Sostituire la chiamata del metodo `app.Run` con chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.

1. Chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) nel metodo `Startup.ConfigureServices`. Aggiunge i servizi SignalR al progetto.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. Eseguire il mapping di una route */hub* all'hub `ChatHub`. Aggiungere le righe seguenti alla fine del metodo `Startup.Configure`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. Creare una nuova directory denominata *Hubs* nella radice del progetto. Lo scopo è archiviare l'hub SignalR, che verrà creato nel passaggio successivo.

1. Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Abilitare la comunicazione tra client e server

L'app attualmente visualizza un semplice form per l'invio di messaggi. Quando si prova a eseguire questa operazione, non accade nulla. Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.

1. Eseguire il comando seguente nella radice del progetto:

    ```console
    npm install @microsoft/signalr
    ```

    Il comando precedente installa il [client diSignalR typescript](https://www.npmjs.com/package/@microsoft/signalr), che consente al client di inviare messaggi al server.

1. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Il codice precedente supporta la ricezione di messaggi dal server. La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server. La funzione `withUrl` configura l'URL dell'hub.

    SignalR consente lo scambio di messaggi tra un client e un server. Ogni messaggio ha un nome specifico. Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi. L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`. È possibile restare in ascolto di un numero indefinito di nomi di messaggio. Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto. Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`. Viene aggiunto all'elemento `div` principale che visualizza i messaggi.

1. Ora che il client può ricevere messaggi, configurarlo anche per l'invio. Aggiungere il codice evidenziato al file *src/index.ts*:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`. Il primo parametro del metodo è il nome del messaggio. I dati del messaggio sono rappresentati dagli altri parametri. In questo esempio un messaggio identificato come `newMessage` viene inviato al server. Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo. Se l'invio riesce, il valore della casella di testo viene cancellato.

1. Aggiungere il metodo evidenziato alla classe `ChatHub`:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve. Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi. È sufficiente un metodo denominato dopo il nome del messaggio.

    In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`. Il metodo C# `NewMessage` si aspetta i dati inviati dal client. Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.

## <a name="test-the-app"></a>Eseguire il test dell'applicazione

Per verificare che l'app funzioni, eseguire la procedura seguente.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Eseguire Webpack in modalità *versione*. Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto. Se non ci si trova nella directory radice del progetto, immettere `cd SignalRWebPack` prima di immettere il comando.

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger. Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:

    ```dotnetcli
    dotnet run
    ```

    Il server Web avvia l'app e la rende disponibile su localhost.

1. Aprire un browser all'indirizzo `http://localhost:<port_number>`. Il file *wwwroot/index.html* viene servito. Copiare l'URL dalla barra dell'indirizzo.

1. Aprire un'altra istanza del browser (qualsiasi browser). Incollare l'URL nella barra degli indirizzi.

1. Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**. Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
