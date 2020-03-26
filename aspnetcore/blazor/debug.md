---
title: Debug ASP.NET Core Blazor webassembly
author: guardrex
description: Informazioni su come eseguire il debug di app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 5dbc900ab68682068a7f9e3ffdaabef89a0c7798
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306477"
---
# <a name="debug-aspnet-core-opno-locblazor-webassembly"></a>Debug ASP.NET Core Blazor webassembly

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

è possibile eseguire il debug delle app webassembly Blazor usando gli strumenti di sviluppo del browser nei browser basati su cromo (Edge/Chrome).  In alternativa è possibile eseguire il debug dell'app usando Visual Studio o Visual Studio Code.

Gli scenari disponibili includono:

* Impostare e rimuovere punti di interruzione.
* Eseguire l'app con supporto per il debug in Visual Studio e Visual Studio Code (supporto<kbd>F5</kbd> ).
* Singolo passaggio (<kbd>F10</kbd>) tramite il codice.
* Riprendere l'esecuzione del codice con <kbd>F8</kbd> in un browser o <kbd>F5</kbd> in Visual Studio o Visual Studio Code.
* Nella visualizzazione variabili *locali* osservare i valori delle variabili locali.
* Vedere lo stack di chiamate, incluse le catene di chiamate che passano da JavaScript a .NET e da .NET a JavaScript.

Per il momento *non è possibile*:

* Controllare le matrici.
* Passare il mouse per esaminare i membri.
* Eseguire il debug all'interno o all'esterno del codice gestito.
* Disporre del supporto completo per il controllo dei tipi di valore.
* Interrompi in corrispondenza di eccezioni non gestite.
* Raggiunge i punti di interruzione durante l'avvio dell'app.
* Eseguire il debug di un'app con un ruolo di lavoro del servizio.

Si continuerà a migliorare l'esperienza di debug nelle prossime versioni.

## <a name="prerequisites"></a>Prerequisites

Il debug richiede uno dei seguenti browser:

* Microsoft Edge (versione 80 o successiva)
* Google Chrome (versione 70 o successiva)

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a>Abilitare il debug per Visual Studio e Visual Studio Code

Il debug viene abilitato automaticamente per i nuovi progetti creati usando il modello di progetto ASP.NET Core 3,2 Preview 3 o versioni successive Blazor webassembly.

Per abilitare il debug per un'app webassembly Blazor esistente, aggiornare il file *launchSettings. JSON* nel progetto di avvio per includere la proprietà `inspectUri` seguente in ogni profilo di avvio:

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

Al termine dell'aggiornamento, il file *launchSettings. JSON* dovrebbe avere un aspetto simile all'esempio seguente:

[!code-json[](debug/launchSettings.json?highlight=14,22)]

Proprietà `inspectUri`:

* Consente all'IDE di rilevare che l'app è un'app webassembly Blazor.
* Indica all'infrastruttura di debug degli script di connettersi al browser tramite il proxy di debug di Blazor.

## <a name="visual-studio"></a>Visual Studio

Per eseguire il debug di un'app webassembly Blazor in Visual Studio:

1. Assicurarsi di aver [installato la versione di anteprima più recente di Visual Studio 2019 16,6](https://visualstudio.com/preview) (Preview 2 o versione successiva).
1. Creare una nuova app ASP.NET Core Hosted Blazor webassembly.
1. Premere <kbd>F5</kbd> per eseguire l'app nel debugger.
1. Impostare un punto di interruzione in *Counter. Razor* nel metodo `IncrementCount`.
1. Passare alla scheda **contatore** e selezionare il pulsante per raggiungere il punto di interruzione:

   ![Contatore debug](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. Verificare il valore del campo `currentCount` nella finestra variabili locali:

   ![Visualizza variabili locali](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. Premere <kbd>F5</kbd> per continuare l'esecuzione.

Quando si esegue il debug dell'app webassembly Blazor, è anche possibile eseguire il debug del codice del server:

1. Impostare un punto di interruzione nella pagina *fetchData. Razor* in `OnInitializedAsync`.
1. Impostare un punto di interruzione nel `WeatherForecastController` nel metodo di azione `Get`.
1. Passare alla scheda **Recupera dati** per raggiungere il primo punto di interruzione nel componente `FetchData` appena prima di eseguire una richiesta HTTP al server:

   ![Eseguire il debug dei dati di recupero](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. Premere <kbd>F5</kbd> per continuare l'esecuzione e quindi raggiungere il punto di interruzione sul server nel `WeatherForecastController`:

   ![Server di debug](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. Premere di nuovo <kbd>F5</kbd> per consentire l'esecuzione continua e visualizzare la tabella delle previsioni meteorologiche sottoposta a rendering.

## <a name="visual-studio-code"></a>Visual Studio Code

Per eseguire il debug di un'app webassembly Blazor in Visual Studio Code:
 
1. Installare l' [ C# estensione](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) e il [debugger JavaScript (notturno)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) con `debug.javascript.usePreview` impostato su `true`.

   ![Estensioni](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-extensions.png)

   ![Debugger di anteprima JS](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-js-use-preview.png)

1. Aprire un'app webassembly Blazor esistente con il debug abilitato.

   * Se si riceve la notifica seguente che è necessaria un'installazione aggiuntiva per abilitare il debug, verificare che siano state installate le estensioni corrette e che sia stato abilitato il debug dell'anteprima JavaScript e quindi ricaricare la finestra:

     ![Obbligatorie di installazione aggiuntivi](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)

   * Una notifica consente di aggiungere le risorse necessarie all'app per la compilazione e il debug. Selezionare **Sì**:

     ![Aggiungi asset necessari](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. L'avvio dell'app nel debugger è un processo in due passaggi:

   1 \. Per **prima cosa**, avviare l'app usando la configurazione di avvio di **.net core (Blazor autonoma)** .

   2 \. **Dopo che l'app è stata avviata**, avviare il browser usando il **debug di .NET Core Blazor assembly Web in Chrome** Launch Configuration (richiede Chrome). Per usare Edge anziché Chrome, modificare il `type` della configurazione di avvio in *. VSCODE/Launch. JSON* da `pwa-chrome` a `pwa-edge`.

1. Impostare un punto di interruzione nel metodo `IncrementCount` nel componente `Counter`, quindi selezionare il pulsante per raggiungere il punto di interruzione:

   ![Esegui il debug del contatore in VS Code](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

## <a name="debug-in-the-browser"></a>Debug nel browser

1. Eseguire una build di debug dell'app nell'ambiente di sviluppo.

1. Premere <kbd>maiusc</kbd>+<kbd>ALT</kbd>+<kbd>D</kbd>.

1. Il browser deve essere eseguito con il debug remoto abilitato. Se il debug remoto è disabilitato, viene generata una pagina di errore della **scheda del browser di cui è possibile eseguire il debug** . La pagina di errore contiene le istruzioni per eseguire il browser con la porta di debug aperta, in modo che il proxy di debug Blazor possa connettersi all'app. *Chiudere tutte le istanze del browser* e riavviare il browser come indicato.

Quando il browser è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida di debug apre una nuova scheda del debugger. Dopo qualche istante la scheda **Sources (origini** ) Mostra un elenco degli assembly .NET nell'app. Espandere ogni assembly e trovare i file di origine *. cs*/ *. Razor* disponibili per il debug. Impostare i punti di interruzione, tornare alla scheda dell'app e i punti di interruzione vengono raggiunti quando viene eseguito il codice. Quando viene raggiunto un punto di interruzione, l'esecuzione<kbd>F10</kbd>del codice in un singolo passaggio (F10<kbd>) viene</kbd>eseguito normalmente.

Blazor fornisce un proxy di debug che implementa il [protocollo devtools di Chrome](https://chromedevtools.github.io/devtools-protocol/) e potenzia il protocollo con. Informazioni specifiche del NET. Quando si preme il tasto di scelta rapida per il debug, Blazor punta il DevTools di Chrome sul proxy. Il proxy si connette alla finestra del browser che si sta tentando di eseguire il debug, quindi è necessario abilitare il debug remoto.

## <a name="browser-source-maps"></a>Mappe di origine del browser

Le mappe di origine del browser consentono al browser di eseguire il mapping dei file compilati ai file di origine originali e vengono comunemente usati per il debug sul lato client. Tuttavia, non Blazor attualmente viene C# effettuato il mapping direttamente a JavaScript/WASM. Al contrario, Blazor l'interpretazione del nel browser, quindi le mappe di origine non sono rilevanti.

## <a name="troubleshoot"></a>Risolvere problemi

Se si verificano errori, è possibile che venga utile il suggerimento seguente:

Nella scheda **debugger** aprire gli strumenti di sviluppo nel browser. Nella console di eseguire `localStorage.clear()` per rimuovere tutti i punti di interruzione.
