---
title: Utilizzare il modello di progetto React
author: SteveSandersonMS
description: Informazioni su come iniziare con il modello di progetto ASP.NET Core a pagina singola applicazione (SPA) versione finale candidata per React e creare app di react.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 5978094083a098a771f5dca103434ea8fcce7777
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-project-template-release-candidate"></a>Utilizzare il modello di progetto React (versione finale candidata)

> [!NOTE]
> Questa documentazione non è sul modello di progetto React rilasciato. **Questa documentazione è sulla versione finale candidata del modello React.** Ci auguriamo che per la versione rilasciata 2018 anticipata.

Il modello di progetto aggiornato React fornisce un punto di partenza ideale per ASP.NET Core App scritte in React e [creare app di reazione](https://github.com/facebookincubator/create-react-app) convenzioni (CRA) per implementare un'interfaccia utente avanzata, sul lato client (UI).

Il modello è equivalente alla creazione di un progetto ASP.NET Core per agire come un back-end dell'API e un progetto standard CRA rispondere ad agire come un'interfaccia utente, ma con la praticità di hosting sia in un progetto di app singola che può essere creato e pubblicato come una singola unità.

## <a name="create-a-new-app"></a>Creare una nuova app

Per iniziare, assicurarsi di aver [installato il modello di progetto aggiornato React](xref:spa/index#installation). Queste istruzioni non si applicano al modello di progetto React precedente incluso in .NET Core SDK 2.0.

Creare un nuovo progetto da un prompt dei comandi utilizzando il comando `dotnet new react` in una directory vuota. Ad esempio, i comandi seguenti creano l'app in un *my-nuova-app* directory e passare alla directory:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Eseguire l'app da Visual Studio o l'interfaccia CLI di .NET Core:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aprire generato *csproj* file ed eseguire l'app come al solito da qui.

Il processo di compilazione consente di ripristinare le dipendenze di npm alla prima esecuzione, che può richiedere alcuni minuti. Le compilazioni successive sono molto più veloce.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Assicurarsi di disporre di una variabile di ambiente denominata `ASPNETCORE_Environment` con valore `Development`. In Windows (in istruzioni non PowerShell), eseguire `SET ASPNETCORE_Environment=Development`. In Linux o Mac OS, eseguire `export ASPNETCORE_Environment=Development`.

Eseguire `dotnet build` per verificare l'applicazione venga compilata correttamente. Alla prima esecuzione, il processo di compilazione consente di ripristinare le dipendenze di npm, che possono richiedere alcuni minuti. Le compilazioni successive sono molto più veloce.

Eseguire `dotnet run` per avviare l'app.

---

Il modello di progetto crea un'applicazione ASP.NET Core e un'app React. L'applicazione ASP.NET di base deve essere utilizzato per l'accesso ai dati, l'autorizzazione e altri problemi sul lato server. L'app React, che si trovano nel *ClientApp* sottodirectory, dovrà essere utilizzato per tutti i problemi dell'interfaccia utente.

## <a name="add-pages-images-styles-modules-etc"></a>Aggiungere pagine, immagini, stili, moduli e così via.

Il *ClientApp* directory è un'applicazione di rispondere CRA standard. Vedere ufficiali [documentazione CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) per ulteriori informazioni.

Vi sono piccole differenze tra l'app React creato da questo modello e quello creato da CRA stesso. Tuttavia, non vengono modificate le funzionalità dell'app. L'app creata tramite il modello contiene un [Bootstrap](https://getbootstrap.com/)-base di layout e un esempio di routing di base.

## <a name="install-npm-packages"></a>Installare pacchetti npm

Per installare i pacchetti di terze parti npm, utilizzare un prompt dei comandi nel *ClientApp* sottodirectory. Ad esempio:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Pubblicare e distribuire

In fase di sviluppo, l'app viene eseguita in modalità ottimizzata per praticità per sviluppatori. Ad esempio, JavaScript bundle includono mapping di origine (in modo che durante il debug, è possibile visualizzare il codice sorgente originale). L'applicazione verifica la presenza di JavaScript, HTML e CSS file modifiche disco automaticamente ricompilazioni e ricarica quando rileva tali file modificare.

Nell'ambiente di produzione, utilizzare una versione dell'app che è ottimizzato per le prestazioni. Questo è configurato per eseguire automaticamente. Quando si pubblica, la configurazione della build genera una compilazione transpiled minimizzata, del codice sul lato client. A differenza della compilazione di sviluppo, la compilazione di produzione non richiede Node.js essere installato nel server.

È possibile utilizzare standard [metodi di distribuzione e hosting ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Eseguire in modo indipendente il server CRA

Il progetto è configurato per avviare la propria istanza del server di sviluppo CRA in background all'avvio dell'app di ASP.NET Core in modalità di sviluppo. Ciò risulta utile in quanto significa che non è necessario eseguire manualmente un server separato.

È uno svantaggio di questo programma di installazione predefinito. Ogni volta che si modifica il codice c# e il ASP.NET Core app richiede il riavvio, il server CRA viene riavviato. Per avviare il backup, sono necessari alcuni secondi. Se si desidera apportare frequenti modifiche al codice c# e non si desidera attendere il riavvio del server CRA, eseguire il server CRA esternamente, indipendentemente dal processo ASP.NET Core. A tale scopo:

1. In un prompt dei comandi, passare il *ClientApp* sottodirectory e avviare il server di sviluppo CRA:

    ```console
    cd ClientApp
    npm start
    ```

2. Modificare l'applicazione ASP.NET Core per utilizzare l'istanza del server esterno CRA anziché avviare uno dei propri. Nel *avvio* classe, sostituire il `spa.UseReactDevelopmentServer` chiamata con quanto segue:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Quando si avvia l'app ASP.NET Core, non verrà avviato un server CRA. Viene utilizzato l'istanza che è stato avviato manualmente. Ciò consente di avviare e riavviare più velocemente. Non è in attesa per l'app React ricompilare ogni volta.
