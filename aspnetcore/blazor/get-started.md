---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 09400a076849bdec35beb284a488d01feb8a84c2
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160002"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>Introduzione a ASP.NET Core Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Introduzione a Blazor:

1. Installare [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Facoltativamente, installare il modello di [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :
   * Installare [.NET Core 3,1 o versione successiva (anteprima) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Eseguire il comando seguente in una shell dei comandi. [Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Il pacchetto di modelli include una versione di anteprima mentre Blazor webassembly è in anteprima.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. Seguire le istruzioni per la scelta degli strumenti:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Installare [Visual Studio 16,4 o versione successiva](https://visualstudio.microsoft.com/vs/preview/) con il carico di lavoro **sviluppo di ASP.NET e Web** .

   2 \. Creare un nuovo progetto.

   3 \. Selezionare **Blazor app**. Scegliere **Avanti**.

   4 \. Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito. Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto. Scegliere **Crea**.

   5 \. Per un'esperienza Blazor webassembly, scegliere il modello **app webassemblyBlazor** . Per un'esperienza di Blazor server, scegliere il modello **app serverBlazor** . Scegliere **Crea**. Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.

   6 \. Premere **CTRL**+**F5** per eseguire l'app.

   > [!NOTE]
   > Se è stato installato il Blazor estensione di Visual Studio per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione. L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. Installare [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Per un'esperienza Blazor webassembly, eseguire il comando seguente in una shell dei comandi:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Per un'esperienza di Blazor server, eseguire il comando seguente in una shell dei comandi:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.

   4 \. Aprire la cartella *WebApplication1* in Visual Studio Code.

   5 \. Per un progetto di Blazor server, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto. Selezionare **Sì**.

   6 \. Se si usa un'app Server Blazor, eseguire l'app usando il debugger Visual Studio Code. Se si usa un'app webassembly Blazor, eseguire `dotnet run` dalla cartella del progetto dell'app.

   7 \. In un browser passare a `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

   1 \. Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).

   2 \. Selezionare **File** > **nuova soluzione** o creare un **nuovo progetto**.

   3 \. Nella barra laterale selezionare **.NET Core** > **app**.

   4 \. Selezionare il modello **app ServerBlazor** . Attualmente è disponibile in Visual Studio per Mac solo il modello di Blazor server. Per un'esperienza Blazor webassembly, seguire le istruzioni riportate nella scheda **interfaccia della riga di comando di .NET Core** . Dopo aver selezionato il modello di Blazor server, fare clic su **Avanti**. Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Impostare il **Framework di destinazione** su **.NET Core 3,1** e selezionare **Avanti**.

   6 \. Nel campo **nome progetto** assegnare un nome all'app `WebApplication1`. Scegliere **Crea**.

   7 \. Selezionare **esegui** > **Esegui senza eseguire debug** per eseguire l'app *senza il debugger*. Eseguire l'app con **Avvia debug** per eseguire l'app *con il debugger*.

   Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.

   # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

   Per un'esperienza Blazor webassembly, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per un'esperienza di Blazor server, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per informazioni sui due modelli di hosting Blazor, *Blazor server* e *Blazor webassembly*, vedere <xref:blazor/hosting-models>.

   In un browser passare a `https://localhost:5001`.

   ---

Nelle schede della barra laterale sono disponibili più pagine:

* Home page di
* Counter
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
