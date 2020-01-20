---
title: Debug ASP.NET Core Blazor
author: guardrex
description: Informazioni su come eseguire il debug di app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159989"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>Debug ASP.NET Core Blazor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Il supporto *anticipato* esiste per il debug Blazor webassembly usando gli strumenti di sviluppo del browser nei browser basati su cromo (Chrome/Edge). Il lavoro è in corso per:

* Abilita completamente il debug in Visual Studio.
* Abilitare il debug in Visual Studio Code.

Le funzionalità del debugger sono limitate. Gli scenari disponibili includono:

* Impostare e rimuovere punti di interruzione.
* Singolo passaggio (`F10`) tramite il codice o la ripresa (`F8`) esecuzione del codice.
* Nella visualizzazione variabili *locali* osservare i valori di tutte le variabili locali di tipo `int`, `string`e `bool`.
* Vedere lo stack di chiamate, incluse le catene di chiamate che passano da JavaScript a .NET e da .NET a JavaScript.

*Non è possibile*:

* Osservare i valori delle variabili locali che non sono `int`, `string`o `bool`.
* Osservare i valori di qualsiasi proprietà o campo di una classe.
* Passare il mouse sulle variabili per visualizzarne i valori.
* Valutare le espressioni nella console.
* Eseguire un passaggio tra le chiamate asincrone.
* Eseguire la maggior parte degli altri scenari di debug comuni.

Lo sviluppo di altri scenari di debug è un'attività in corso del team di progettazione.

## <a name="prerequisites"></a>Prerequisiti

Il debug richiede uno dei seguenti browser:

* Google Chrome (versione 70 o successiva)
* Anteprima di Microsoft Edge ([canale di sviluppo Edge](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Procedura

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> Il supporto del debug in Visual Studio si trova in una fase iniziale dello sviluppo. Il debug di **F5** attualmente non è supportato.

1. Eseguire un'app webassembly Blazor in `Debug` configurazione senza debug (**Ctrl**+**F5** anziché **F5**).
1. Aprire le proprietà di debug dell'app (ultima voce nel menu **debug** ) e copiare l'URL dell' **app**http. Individuare l'indirizzo HTTP (non l'indirizzo HTTPS) dell'app usando un browser basato su cromo (Edge beta o Chrome).
1. Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser, non nel pannello strumenti di sviluppo. È consigliabile lasciare il pannello strumenti di sviluppo chiuso per questa procedura. Dopo l'avvio del debug, è possibile riaprire il pannello strumenti di sviluppo.
1. Selezionare il seguente tasto di scelta rapida specifico per Blazor:

   * `Shift+Alt+D` in Windows
   * `Shift+Cmd+D` in macOS

   Se viene visualizzata la **scheda non è possibile trovare il browser di cui è possibile eseguire il debug**, vedere [abilitare il debug remoto](#enable-remote-debugging).
   
   Dopo l'abilitazione del debug remoto:
   
   1 \. Verrà aperta una nuova finestra del browser Chiudere la finestra precedente.

   2 \. Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser.

   3 \. Selezionare il tasto di scelta rapida specifico per Blazornella nuova finestra del browser: `Shift+Alt+D` in Windows o `Shift+Cmd+D` in macOS.

   4 \. Verrà visualizzata la scheda **devtools** nel browser. **Selezionare nuovamente la scheda dell'app nella finestra del browser.**

   Per aggiungere l'app a Visual Studio, vedere la sezione [Connetti a processo in Visual Studio](#attach-to-process-in-visual-studio) .

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

1. Eseguire un'app webassembly Blazor nella configurazione `Debug` passando l'opzione `--configuration Debug` al comando [DotNet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Passare all'app nell'URL HTTP visualizzato nella finestra della shell.
1. Posizionare lo stato attivo della tastiera sull'app, non sul pannello strumenti di sviluppo. È consigliabile lasciare il pannello strumenti di sviluppo chiuso per questa procedura. Dopo l'avvio del debug, è possibile riaprire il pannello strumenti di sviluppo.
1. Selezionare il seguente tasto di scelta rapida specifico per Blazor:

   * `Shift+Alt+D` in Windows
   * `Shift+Cmd+D` in macOS

   Se viene visualizzata la **scheda non è possibile trovare il browser di cui è possibile eseguire il debug**, vedere [abilitare il debug remoto](#enable-remote-debugging).
   
   Dopo l'abilitazione del debug remoto:
   
   1 \. Verrà aperta una nuova finestra del browser Chiudere la finestra precedente.

   2 \. Posizionare lo stato attivo sulla tastiera nell'app nella finestra del browser, non nel pannello strumenti di sviluppo.

   3 \. Selezionare il tasto di scelta rapida specifico per Blazornella nuova finestra del browser: `Shift+Alt+D` in Windows o `Shift+Cmd+D` in macOS.

---

## <a name="enable-remote-debugging"></a>Abilitare il debug remoto

Se il debug remoto è disabilitato, la pagina di errore **di una scheda del browser di cui è possibile eseguire il debug** viene generata da Chrome. La pagina di errore contiene le istruzioni per eseguire Chrome con la porta di debug aperta, in modo che il proxy di debug Blazor possa connettersi all'app. *Chiudere tutte le istanze di Chrome* e riavviare Chrome come indicato.

## <a name="debug-the-app"></a>Eseguire il debug dell'app

Quando Chrome è in esecuzione con il debug remoto abilitato, il tasto di scelta rapida per il debug apre una nuova scheda del debugger. Dopo qualche istante la scheda **Sources (origini** ) Mostra un elenco degli assembly .NET nell'app. Espandere ogni assembly e trovare i file di origine *. cs*/ *. Razor* disponibili per il debug. Impostare i punti di interruzione, tornare alla scheda dell'app e i punti di interruzione vengono raggiunti quando viene eseguito il codice. Quando viene raggiunto un punto di interruzione, un singolo passaggio (`F10`) tramite il codice o riprende (`F8`) l'esecuzione del codice normalmente.

Blazor fornisce un proxy di debug che implementa il [protocollo devtools di Chrome](https://chromedevtools.github.io/devtools-protocol/) e potenzia il protocollo con. Informazioni specifiche del NET. Quando si preme il tasto di scelta rapida per il debug, Blazor punta il DevTools di Chrome sul proxy. Il proxy si connette alla finestra del browser che si sta tentando di eseguire il debug, quindi è necessario abilitare il debug remoto.

## <a name="attach-to-process-in-visual-studio"></a>Connetti a processo in Visual Studio

La connessione al processo dell'app in Visual Studio è uno scenario di debug *temporaneo* per Blazor webassembly mentre il debug di **F5** è in fase di sviluppo.

Per alleghi il processo dell'app in esecuzione a Visual Studio:

1. In Visual Studio selezionare **Debug** > **Connetti a processo**.
1. Per il **tipo di connessione**, selezionare il **protocollo DevTools per Chrome (nessuna autenticazione)** .
1. Per la **destinazione della connessione**, incollare l'indirizzo http (non l'indirizzo https) dell'app.
1. Selezionare **Aggiorna** per aggiornare le voci in **processi disponibili**.
1. Selezionare il processo del browser di cui eseguire il debug e selezionare **Connetti**.
1. Nella finestra di dialogo **Seleziona tipo di codice** selezionare il tipo di codice per il browser specifico a cui si sta eseguendo la connessione (Edge o Chrome) e quindi fare clic su **OK**.

## <a name="browser-source-maps"></a>Mappe di origine del browser

Le mappe di origine del browser consentono al browser di eseguire il mapping dei file compilati ai file di origine originali e vengono comunemente usati per il debug sul lato client. Tuttavia, non Blazor attualmente viene C# effettuato il mapping direttamente a JavaScript/WASM. Al contrario, Blazor l'interpretazione del nel browser, quindi le mappe di origine non sono rilevanti.

## <a name="troubleshoot"></a>Risolvere i problemi

Se si verificano errori, è possibile che venga utile il suggerimento seguente:

Nella scheda **debugger** aprire gli strumenti di sviluppo nel browser. Nella console di eseguire `localStorage.clear()` per rimuovere tutti i punti di interruzione.
