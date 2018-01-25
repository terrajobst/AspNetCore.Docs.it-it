---
title: Utilizzare il modello di progetto Angular
author: SteveSandersonMS
description: Informazioni su come iniziare con il modello di progetto ASP.NET Core a pagina singola applicazione (SPA) versione finale candidata per angolare e CLI angolare.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 4162b1c26e9d278c811f691c4277d4de25adb204
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-angular-project-template-release-candidate"></a>Utilizzare il modello di progetto angolare (versione finale candidata)

> [!NOTE]
> Questa documentazione non è sul modello di progetto angolare rilasciato. **Questa documentazione è sulla versione finale candidata del modello angolare.** Ci auguriamo che per la versione rilasciata 2018 anticipata.

Il modello di progetto angolare aggiornato fornisce un punto di partenza ideale per ASP.NET Core App con 5 angolare e CLI angolare per implementare un'interfaccia utente avanzata, sul lato client (UI).

Il modello è equivalente alla creazione di un progetto ASP.NET Core per agire come un back-end dell'API e un progetto CLI angolare come un'interfaccia utente. Il modello offre il vantaggio di hosting di entrambi i tipi di progetto in un progetto di app single. Di conseguenza, il progetto di applicazione da compilare e pubblicato come una singola unità.

## <a name="create-a-new-app"></a>Creare una nuova app

Per iniziare, assicurarsi di aver [installato il modello di progetto angolare aggiornato](xref:spa/index#installation). Queste istruzioni non si applicano al modello di progetto angolare precedente incluso in .NET Core SDK 2.0.

Creare un nuovo progetto da un prompt dei comandi utilizzando il comando `dotnet new angular` in una directory vuota. Ad esempio, i comandi seguenti creano l'app in un *my-nuova-app* directory e passare alla directory:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Eseguire l'app da Visual Studio o l'interfaccia CLI di .NET Core:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aprire generato *csproj* file ed eseguire l'app come al solito da qui.

Il processo di compilazione consente di ripristinare le dipendenze di npm alla prima esecuzione, che può richiedere alcuni minuti. Le compilazioni successive sono molto più veloce.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Assicurarsi di disporre di una variabile di ambiente denominata `ASPNETCORE_Environment` con un valore di `Development`. In Windows (in istruzioni non PowerShell), eseguire `SET ASPNETCORE_Environment=Development`. In Linux o Mac OS, eseguire `export ASPNETCORE_Environment=Development`.

Eseguire `dotnet build` di verificare l'applicazione venga compilata correttamente. Alla prima esecuzione, il processo di compilazione consente di ripristinare le dipendenze di npm, che possono richiedere alcuni minuti. Le compilazioni successive sono molto più veloce.

Eseguire `dotnet run` per avviare l'app. Viene registrato un messaggio simile al seguente:

```console
Now listening on: http://localhost:<port>
```

Passare a questo URL in un browser.

L'app viene avviata un'istanza del server CLI angolare in background. Viene registrato un messaggio simile al seguente: *NG in tempo reale del Server di sviluppo è in ascolto su localhost:&lt;otherport&gt;, aprire il browser in http://localhost:&lt;otherport&gt; /* . Ignorare questo messaggio&mdash;ha **non** l'URL per l'applicazione ASP.NET Core e CLI angolare combinato.

---

Il modello di progetto crea un'applicazione ASP.NET Core e un'app angolare. L'applicazione ASP.NET di base deve essere utilizzato per l'accesso ai dati, l'autorizzazione e altri problemi sul lato server. L'app angolare, che si trovano nel *ClientApp* sottodirectory, dovrà essere utilizzato per tutti i problemi dell'interfaccia utente.

## <a name="add-pages-images-styles-modules-etc"></a>Aggiungere pagine, immagini, stili, moduli e così via.

Il *ClientApp* directory contiene un'app CLI angolare standard. Vedere ufficiali [documentazione angolare](https://github.com/angular/angular-cli/wiki) per ulteriori informazioni.

Vi sono piccole differenze tra l'app angolare creato da questo modello e quello creato da angolare CLI (tramite `ng new`); tuttavia, non vengono modificate le funzionalità dell'app. L'app creata tramite il modello contiene un [Bootstrap](https://getbootstrap.com/)-base di layout e un esempio di routing di base.

## <a name="run-ng-commands"></a>Eseguire i comandi ng

In un prompt dei comandi, passare il *ClientApp* sottodirectory:

```console
cd ClientApp
```

Se si dispone di `ng` strumento installato globalmente, è possibile eseguire i relativi comandi. Ad esempio, è possibile eseguire `ng lint`, `ng test`, o qualsiasi altra [i comandi CLI angolare](https://github.com/angular/angular-cli/wiki#additional-commands). Non è necessario eseguire `ng serve` , tuttavia, poiché l'app ASP.NET Core si occupa del lato server sia lato client parti dell'app. Internamente, viene utilizzato `ng serve` in fase di sviluppo.

Se non si dispone di `ng` strumento installato, eseguire `npm run ng` invece. Ad esempio, è possibile eseguire `npm run ng lint` o `npm run ng test`.

## <a name="install-npm-packages"></a>Installare pacchetti npm

Per installare i pacchetti di terze parti npm, utilizzare un prompt dei comandi nel *ClientApp* sottodirectory. Ad esempio:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Pubblicare e distribuire

In fase di sviluppo, l'app viene eseguita in modalità ottimizzata per praticità per sviluppatori. Ad esempio, JavaScript bundle includono mapping di origine (in modo che durante il debug, è possibile visualizzare il codice TypeScript originale). L'applicazione verifica la presenza di modifiche al file TypeScript, HTML e CSS nel disco automaticamente ricompilazioni e ricarica quando rileva tali file modificare.

Nell'ambiente di produzione, utilizzare una versione dell'app che è ottimizzato per le prestazioni. Questo è configurato per eseguire automaticamente. Quando si pubblica, la configurazione della build genera un minimizzata, ahead di tempo (AoT) compilati compilazione del codice sul lato client. A differenza della compilazione di sviluppo, la compilazione di produzione non richiede Node.js essere installato nel server (a meno che non è stato abilitato [il rendering preliminare sul lato server](#server-side-rendering)).

È possibile utilizzare standard [metodi di distribuzione e hosting ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Eseguire "ng serve" in modo indipendente

Il progetto è configurato per avviare la propria istanza del server CLI angolare in background all'avvio dell'app di ASP.NET Core in modalità di sviluppo. Ciò risulta utile in quanto non è necessario eseguire manualmente un server separato.

È uno svantaggio di questo programma di installazione predefinito. Ogni volta che si modifica il codice c# e il ASP.NET Core app richiede il riavvio, il server di CLI angolare viene riavviato. Per avviare il backup, è necessario circa 10 secondi. Se si apportano spesso modifiche al codice c# e non si desidera attendere angolare CLI il riavvio, eseguire il server di CLI angolare esternamente, indipendentemente dal processo ASP.NET Core. A tale scopo:

1. In un prompt dei comandi, passare il *ClientApp* sottodirectory e avviare il server di sviluppo CLI angolare:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Utilizzare `npm start` per avviare il server di sviluppo angolare CLI non `ng serve`, in modo che la configurazione in *package. JSON* viene rispettato. Per passare parametri aggiuntivi al server angolare CLI, aggiungerli alla pertinente `scripts` riga il *package. JSON* file.

2. Modificare l'applicazione ASP.NET Core per utilizzare l'istanza di CLI angolare esterno anziché avviare uno dei propri. Nel *avvio* classe, sostituire il `spa.UseAngularCliServer` chiamata con quanto segue:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Quando si avvia l'app ASP.NET Core, non verrà avviato un server di CLI angolare. Viene utilizzato l'istanza che è stato avviato manualmente. Ciò consente di avviare e riavviare più velocemente. Non è in attesa per CLI angolare ricompilare l'app client ogni volta.

## <a name="server-side-rendering"></a>Rendering lato server

Come una funzionalità di prestazioni, è possibile pre-eseguire il rendering dell'app angolare sul server, nonché a eseguirlo sul client. Ciò significa che i browser ricevano il markup HTML che rappresenta di interfaccia utente iniziale dell'app, in modo che contengano anche prima del download e l'esecuzione i bundle di JavaScript. La maggior parte dell'implementazione di questo proviene da una funzione angolare [angolare universale](https://universal.angular.io/).

> [!TIP]
> Abilitazione di rendering lato server (SSR) introduce un numero di complessità aggiuntiva sia durante lo sviluppo e distribuzione. Lettura [svantaggi SSR](#drawbacks-of-ssr) per determinare se SSR è una scelta ottimale per le proprie esigenze.

Per abilitare SSR, è necessario eseguire un certo numero di aggiunte al progetto.

Nel *avvio* (classe), *dopo* la riga che configura `spa.Options.SourcePath`, e *prima* la chiamata a `UseAngularCliServer` o `UseProxyToSpaDevelopmentServer`, aggiungere quanto segue:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

In modalità di sviluppo, il codice tenta di compilare il bundle SSR eseguendo lo script `build:ssr`, definito in *ClientApp\package.json*. Si compila un'app angolare denominata `ssr`, che non è ancora definita. 

Alla fine del `apps` matrice *ClientApp/.angular-cli.json*, definire un'app aggiuntiva con nome `ssr`. Utilizzare le opzioni seguenti:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

La configurazione della nuova app abilitata SSR richiede due file ulteriore: *tsconfig.server.json* e *main.server.ts*. Il *tsconfig.server.json* file specifica le opzioni di compilazione TypeScript. Il *main.server.ts* file viene utilizzato come il punto di ingresso del codice durante SSR.

Aggiungere un nuovo file denominato *tsconfig.server.json* all'interno di *ClientApp/src* (insieme esistente *tsconfig.app.json*), che contiene le operazioni seguenti:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Questo file Configura del compilatore AoT del angolare per individuare un modulo denominato `app.server.module`. Aggiungere questo elemento mediante la creazione di un nuovo file in *ClientApp/src/app/app.server.module.ts* (insieme esistente *app.module.ts*) contenente le operazioni seguenti: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Questo modulo eredita da sul lato client `app.module` e definisce i moduli Angular aggiuntive sono disponibili durante SSR.

Tenere presente che il nuovo `ssr` voce *.angular cli.json* a cui fa riferimento a un file del punto di ingresso chiamato *main.server.ts*. Ancora non sono stati aggiunti file e a questo punto è necessario eseguire questa operazione. Creare un nuovo file in *ClientApp/src/main.server.ts* (insieme esistente *main.ts*), che contiene le operazioni seguenti:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Codice di questo file viene ASP.NET Core eseguito per ogni richiesta durante l'esecuzione di `UseSpaPrerendering` middleware che è stato aggiunto per il *avvio* classe. Si occupa delle ricezione `params` dal codice di .NET (ad esempio l'URL richiesto) e chiamate alle API SSR angolare per ottenere il codice HTML risultante. 

In-senso stretto, ciò è sufficiente abilitare SSR in modalità di sviluppo. È essenziale per apportare una modifica finale in modo che l'app funziona correttamente quando pubblicato. Nella finestra principale dell'app *csproj* file, impostare il `BuildServerSideRenderer` valore della proprietà da `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

In questo modo il processo di compilazione per eseguire `build:ssr` durante la pubblicazione e distribuzione dei file SSR al server. Se non si abilita questa SSR ha esito negativo in fase di produzione.

Quando l'app viene eseguita in modalità di sviluppo o di produzione, il codice angolare pre-esegue il rendering in formato HTML nel server. Il codice sul lato client viene eseguito normalmente.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Passare i dati dal codice .NET nel codice TypeScript

Durante SSR, si potrebbe decidere di passare dati per ogni richiesta dall'app ASP.NET Core nell'app angolare. Ad esempio, è possibile passare le informazioni sui cookie o un elemento leggere da un database. A tale scopo, modificare il *avvio* classe. Nel callback per `UseSpaPrerendering`, impostare un valore per `options.SupplyData` come illustrato di seguito:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Il `SupplyData` consente di callback di passare arbitrario, per ogni richiesta, dati serializzabili con JSON (ad esempio stringhe, valori booleani o numeri). Il *main.server.ts* codice riceve come `params.data`. Ad esempio, l'esempio di codice precedente passa un valore booleano come `params.data.isHttpsRequest` nel `createServerRenderer` callback. Questo è possibile passare ad altre parti dell'app in qualsiasi modo supportato dal angolare. Ad esempio, vedere come *main.server.ts* passa il `BASE_URL` valore a qualsiasi componente il cui costruttore viene dichiarato per la ricezione.

### <a name="drawbacks-of-ssr"></a>Svantaggi di SSR

Non tutte le app trarre vantaggio da SSR. Il vantaggio principale viene percepito delle prestazioni. Visitatori raggiungere l'app tramite una connessione di rete lenta o nei dispositivi mobili lenta vedere interfaccia utente iniziale rapidamente, anche se occorre un po' di tempo per recuperare o analizzare i bundle di JavaScript. Tuttavia, molti stabilimenti termali sono usate principalmente su reti aziendali interne, veloce nei computer veloci in cui l'app viene visualizzata quasi istantaneamente.

Allo stesso tempo, esistono alcune limitazioni significative all'abilitazione SSR. Il processo di sviluppo aumenta la complessità. Il codice deve essere eseguito in due diversi ambienti: lato client e lato server (in un ambiente di Node.js richiamato da ASP.NET Core). Di seguito sono illustrati alcuni aspetti da tenere presente:

* SSR richiede un'installazione di Node.js nei server di produzione. Ciò avviene automaticamente per alcuni scenari di distribuzione, ad esempio servizi di App di Azure, ma non per altri utenti, ad esempio Azure Service Fabric.
* Abilitazione di `BuildServerSideRenderer` cause di flag di compilazione il *node_modules* directory per la pubblicazione. Questa cartella contiene 20.000 file, che aumenta il tempo di distribuzione.
* Per eseguire il codice in un ambiente di Node.js, è possibile basarsi sull'esistenza di specifiche del browser JavaScript APIs, ad esempio `window` o `localStorage`. Se il codice (o alcuni libreria di terze parti che si fa riferimento) tenta di utilizzare queste API, si otterrà un errore durante la SSR. Ad esempio, non utilizzare jQuery perché fa riferimento API specifiche del browser in numerose posizioni. Per evitare errori, è necessario evitare SSR o evitare di API o librerie specifiche del browser. È possibile eseguire il wrapping di tutte le chiamate a tali API nei controlli per assicurarsi che non vengono richiamati durante SSR. Nel codice JavaScript o TypeScript, ad esempio, utilizzare un controllo simile al seguente:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
