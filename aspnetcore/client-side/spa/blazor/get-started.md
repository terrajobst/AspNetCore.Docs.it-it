---
title: Introduzione a Blazor
author: guardrex
description: Informazioni su come iniziare a utilizzare Blazor creando e modificando un progetto Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 667c57d536450fa2f8ae1cabc7c5a76a16d38a55
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665587"
---
# <a name="get-started-with-blazor"></a>Introduzione a Blazor

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Prerequisiti:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Per creare il primo progetto Blazor in Visual Studio:

1. Installare la versione più recente [estensione servizi di linguaggio Blazor](https://go.microsoft.com/fwlink/?linkid=870389) da Visual Studio Marketplace. Questo passaggio rende disponibili i modelli Blazor a Visual Studio.
1. Rendere disponibili per l'uso con la CLI di .NET Core i modelli Blazor eseguendo il comando seguente in una shell dei comandi:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Selezionare **File** > **nuovo progetto** > **Web** > **applicazione Web ASP.NET Core**.
1. Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.
1. Scegliere il modello **Blazor** e selezionare **OK**.
1. Premere **F5** per eseguire l'app.

La procedura è stata completata. Sono stati eseguiti la prima app Blazor!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

Prerequisiti:

* [.NET Core SDK 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Aggiungere i modelli Blazor eseguendo il comando seguente in una shell dei comandi:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Creare il primo progetto Blazor in una shell dei comandi:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. In un browser passare a `https://localhost:5001`.

La procedura è stata completata. Sono stati eseguiti la prima app Blazor!

---

## <a name="blazor-project"></a>Progetto Blazor

Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:

* Home
* Counter
* Recuperare i dati

Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina. Incrementare un contatore in una pagina Web in genere richiede la scrittura di JavaScript, ma Blazor offre un approccio migliore usando C#.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto. Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

Ogni volta che il **Click me** pulsante è selezionato:

* Il `onclick` evento.
* Viene chiamato il metodo `IncrementCount` .
* Il `currentCount` viene incrementato.
* Il componente viene eseguito il rendering nuovamente.

Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).

Aggiungere un componente a un altro componente usando una sintassi simile all'HTML. Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio. Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.

Nelle *cshtml*, sostituire il messaggio di richiesta di sondaggio componente con un componente del contatore:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Eseguire l'app. Home page ha un proprio contatore.

Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:

* Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.
* Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.

*Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Eseguire l'app. Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.

## <a name="next-steps"></a>Passaggi successivi

<xref:tutorials/first-razor-components-app>
