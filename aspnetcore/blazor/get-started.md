---
title: Introduzione a ASP.NET Core Blazor
author: guardrex
description: Inizia a usare Blazor compilando un'app Blazor con gli strumenti che preferisci.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/25/2019
uid: blazor/get-started
ms.openlocfilehash: ef9113dbfdbbd5920c4358cdac0c77c60f40b7c8
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288792"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Introduzione a ASP.NET Core Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Introduzione a Blazor:

1. Installare la versione più recente di [.NET Core 3,0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) .

1. Installare il modello [Webassembly Blazor](xref:blazor/hosting-models#blazor-webassembly) eseguendo il comando seguente in una shell dei comandi:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19465.2
   ```

1. Seguire le istruzioni per la scelta degli strumenti:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Installare la versione più recente di [Visual Studio](https://visualstudio.com/vs/) con il carico di lavoro **sviluppo di ASP.NET e Web** .

   2 \. Creare un nuovo progetto.

   3 \. Selezionare **app Blazer**. Selezionare **Avanti**.

   4 \. Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito. Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto. Selezionare **Create**.

   5 \. Per un'esperienza di webassembly blazer, scegliere il modello **app Webassembly Blazer** . Per un'esperienza del server blazer, scegliere il modello di **app del server Blazer** . Selezionare **Create**. Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.

   6 \. Premere **F5** per eseguire l'app.

   > [!NOTE]
   > Se è stata installata l'estensione di Visual Studio Blazor per una versione di anteprima precedente di ASP.NET Core Blazor (anteprima 6 o precedente), è possibile disinstallare l'estensione. L'installazione dei modelli di Blazor in una shell dei comandi è ora sufficiente per esporre i modelli in Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1 \. Installare [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Installare la versione più recente [ C# di per l'estensione Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Per un'esperienza di webassembly blazer, eseguire il comando seguente in una shell dei comandi:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      Per un'esperienza del server Blazor, eseguire il comando seguente in una shell dei comandi:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.

   4 \. Aprire la cartella *WebApplication1* in Visual Studio Code.

   5 \. Per un progetto server blazer, l'IDE richiede l'aggiunta di asset per compilare ed eseguire il debug del progetto. Selezionare **Sì**.

   6 \. Se si usa un'app del server blazer, eseguire l'app usando il debugger Visual Studio Code. Se si usa un'app webassembly blazer, eseguire `dotnet run` dalla cartella del progetto dell'app.

   7 \. In un browser passare a `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

   Per un'esperienza di webassembly Blazor, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per un'esperienza del server Blazor, eseguire i comandi seguenti in una shell dei comandi:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Per informazioni sui due modelli di hosting Blazor, *Server Blazor* e *webassembly Blazor*, vedere <xref:blazor/hosting-models>.

   In un browser passare a `https://localhost:5001`.

   ---

Nelle schede della barra laterale sono disponibili più pagine:

* Home
* Counter
* Recuperare i dati

Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina. Per incrementare un contatore in una pagina Web è in genere necessario scrivere JavaScript, ma con blazer C#è possibile usare.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Una richiesta per `/counter` nel browser, come specificato dalla direttiva `@page` nella parte superiore, causa il rendering del contenuto da parte del componente `Counter`. I componenti eseguono il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

Ogni volta che viene selezionato il pulsante **Click me** :

* Viene generato l'evento `onclick`.
* Viene chiamato il metodo `IncrementCount` .
* Il `currentCount` viene incrementato.
* Il rendering del componente viene eseguito nuovamente.

Il runtime confronta il nuovo contenuto con il contenuto precedente e applica solo il contenuto modificato al Document Object Model (DOM).

Aggiungere un componente a un altro componente usando la sintassi HTML. Ad esempio, aggiungere il componente `Counter` alla Home page dell'app aggiungendo un elemento `<Counter />` al componente `Index`.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Eseguire l'app. Il contatore della Home page è fornito dal componente `Counter`.

I parametri del componente vengono specificati utilizzando attributi o [contenuto figlio](xref:blazor/components#child-content), che consentono di impostare le proprietà per il componente figlio. Per aggiungere un parametro al componente `Counter`, aggiornare il blocco `@code` del componente:

* Aggiungere una proprietà pubblica per `IncrementAmount` con un attributo `[Parameter]`.
* Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Specificare il `IncrementAmount` nell'elemento `<Counter>` del componente `Index` usando un attributo.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Eseguire l'app. Il componente `Index` ha il proprio contatore che viene incrementato di dieci ogni volta che viene selezionato il pulsante **Click me** . Il componente `Counter` (*Counter. Razor*) in `/counter` continua a incrementare di uno.

## <a name="next-steps"></a>Passaggi successivi

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:signalr/introduction>
