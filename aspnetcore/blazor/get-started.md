---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306449"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Introduzione a ASP.NET Core Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Per iniziare a usare blazer, seguire le istruzioni per la scelta degli strumenti:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Per creare app del server blazer, installare [Visual Studio 2019 versione 16,4 o successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro di **sviluppo ASP.NET e Web** .

   Per creare app di webassembly per server e blazer, installare Visual Studio 2019 16,6 Preview 2 o versione successiva con il carico di lavoro **sviluppo di ASP.NET e Web** .

   Per informazioni sui due modelli di hosting blazer, *Blazer webassembly* e *Blazer Server*, vedere <xref:blazor/hosting-models>.

1. Creare un nuovo progetto.

1. Selezionare **app Blazer**. Selezionare **Avanti**.

1. Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito. Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto. Selezionare **Create** (Crea).

1. Per un'esperienza di webassembly blazer (Visual Studio 16,6 Preview 2 o versione successiva), scegliere il modello di **app Webassembly Blazer** . Per un'esperienza del server blazer (Visual Studio 16,4 o versione successiva), scegliere il modello di **app del server Blazer** . Selezionare **Create** (Crea).

1. Premere <kbd>CTRL</kbd>+<kbd>F5</kbd> per eseguire l'app.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Facoltativamente, installare il modello di anteprima di [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > Per usare il modello di assembly webassembly 3,2 Preview 3 blazer è **necessario** il [.NET Core SDK versione 3.1.201 o successiva](https://dotnet.microsoft.com/download/dotnet-core/3.1) . Verificare la versione di .NET Core SDK installata eseguendo `dotnet --version` in una shell dei comandi.

1. Installare [Visual Studio Code](https://code.visualstudio.com/).

1. Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) e il [debugger JavaScript (notturno)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) con `debug.javascript.usePreview` impostato su `true`.

1. Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   Per un'esperienza di webassembly Blazor, eseguire il comando seguente in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.

1. Aprire la cartella *WebApplication1* in Visual Studio Code.

1. Le richieste dell'IDE aggiungono risorse per compilare ed eseguire il debug del progetto. Selezionare **Sì**.

1. TUN l'app usando il debugger Visual Studio Code.

1. In un browser passare a `https://localhost:5001`.

# <a name="visual-studio-for-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Il server blazer è supportato in Visual Studio per Mac. Il webassembly Blazer non è supportato in questo momento. Per compilare app webassembly di Blazer in macOS, seguire le istruzioni nella scheda **interfaccia della riga di comando di .NET Core** .

1. Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).

1. Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.

1. Nella barra laterale selezionare **.NET Core** > **app**.

1. Selezionare il modello **applicazione server Blazer** . Selezionare **Create** (Crea).

   Per informazioni sul modello di hosting del server blazer, vedere <xref:blazor/hosting-models>.

1. Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.

1. Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`. Selezionare **Create** (Crea).

1. Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*. Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.

Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.

# <a name="net-core-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

1. Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Facoltativamente, installare il modello di anteprima di [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > Per usare il modello di assembly webassembly 3,2 Preview 3 blazer è **necessario** il [.NET Core SDK versione 3.1.201 o successiva](https://dotnet.microsoft.com/download/dotnet-core/3.1) . Verificare la versione di .NET Core SDK installata eseguendo `dotnet --version` in una shell dei comandi.

1. Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per informazioni sui due modelli di hosting blazer, *Server Blazer* e *webassembly Blazer*, vedere <xref:blazor/hosting-models>.

1. In un browser passare a `https://localhost:5001`.

---

Nelle schede della barra laterale sono disponibili più pagine:

* Home
* Contatore
* Recuperare i dati

Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina. Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con Blazor C#è possibile utilizzare.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Una richiesta di `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, fa sì che il componente `Counter` esegua il rendering del contenuto. I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

Ogni volta che viene selezionato il pulsante **Click me** :

* Viene generato l'evento `onclick`.
* Viene chiamato il metodo `IncrementCount`.
* Il `currentCount` viene incrementato.
* Il rendering del componente viene eseguito nuovamente.

Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).

Aggiungere un componente a un altro componente usando la sintassi HTML. Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Eseguire l'app. La Home page presenta il proprio contatore fornito dal componente `Counter`.

I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio. Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:

* Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.
* Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.

*Pages/Counter.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente di `Index` utilizzando un attributo.

*Pages/Index.razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Eseguire l'app. Il componente `Index` dispone di un contatore che aumenta di dieci ogni volta che viene selezionato il pulsante **Click me** . Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.

## <a name="next-steps"></a>Passaggi successivi

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/templates>
* <xref:signalr/introduction>
