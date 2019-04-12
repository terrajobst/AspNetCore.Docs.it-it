---
title: Introduzione a Razor componenti
author: guardrex
description: Informazioni su come iniziare con i componenti di Razor creando e modificando un progetto Razor componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515530"
---
# <a name="get-started-with-razor-components"></a>Introduzione a Razor componenti

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

# [<a name="visual-studio"></a>Visual Studio](#tab/visual-studio)

Prerequisiti:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Per creare il primo progetto Razor componenti in Visual Studio:

1. Installare la versione più recente [.NET Core 3.0 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.
1. Consentire a Visual Studio usare l'anteprima di SDK:
   1. Aprire **degli strumenti** > **opzioni** nella barra dei menu.
   1. Aprire il **progetti e soluzioni** nodo. Aprire il **.NET Core** scheda.
   1. Selezionare la casella **utilizzano le anteprime di .NET Core SDK**. Scegliere **OK**.
1. Creare un nuovo progetto.
1. Selezionare **Applicazione Web ASP.NET Core**. Scegliere **Avanti**.
1. Specificare un nome nella **nome progetto** campo. Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto. Scegliere **Crea**.
1. Assicurarsi che **.NET Core** e **ASP.NET Core 3.0** sono selezionate nella parte superiore.
1. Scegliere il **componenti Razor** modello e selezionare **crea**.
1. Premere **F5** per eseguire l'app.

La procedura è stata completata. Sono stati eseguiti la prima app Razor componenti!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a>Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

Prerequisiti:

* [.NET Core SDK 3.0 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Per creare il primo progetto di componenti Razor da una shell dei comandi:

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. In un browser passare a `https://localhost:5001`.

La procedura è stata completata. Sono stati eseguiti la prima app Razor componenti!

---

## <a name="razor-components-project"></a>Progetti di componenti Razor

Componenti di Razor vengono creati utilizzando la sintassi Razor, ma vengono compilati in modo diverso da visualizzazioni MVC e Razor Pages. Il *.razor* estensione di file viene usato per specificare un componente di Razor. Razor Pages ed MVC viste continuano a usare il *cshtml* estensione di file.

> [!NOTE]
> Razor componenti possono essere creati con la *. cshtml* estensione di file, purché tali file vengono identificati come i file Razor componente utilizzando il `_RazorComponentInclude` proprietà MSBuild. Ad esempio, un'app creata usando il modello del componente Razor specifica che tutti i *. cshtml* file sotto il *componenti* cartella deve essere trattata come componenti Razor:
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

Quando si esegue l'app, più pagine sono disponibili le schede nella barra laterale:

* Home
* Counter
* Recuperare i dati

Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina. Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

Una richiesta per `/counter` nel browser, come specificato da di `@page` direttiva all'inizio, fa sì che il componente di contatore per il rendering del relativo contenuto. Componenti di eseguire il rendering in una rappresentazione in memoria della struttura di rendering che può quindi essere utilizzata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.

Ogni volta che il **Click me** pulsante è selezionato:

* Il `onclick` evento.
* Viene chiamato il metodo `IncrementCount` .
* Il `currentCount` viene incrementato.
* Il componente viene eseguito il rendering nuovamente.

Il runtime consente di confrontare il nuovo contenuto per il contenuto precedente e si applica solo il contenuto modificato per il provider di servizi Internet (DOM, Document Object Model).

Aggiungere un componente a un altro componente usando una sintassi simile all'HTML. Parametri del componente vengono specificati utilizzando gli attributi o contenuto figlio. Ad esempio, un componente del contatore può essere aggiunto alla home page dell'app aggiungendo un `<Counter />` elemento per il componente dell'indice.

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Eseguire l'app. Home page ha un proprio contatore.

Per aggiungere un parametro per il componente del contatore, aggiornare il componente `@functions` blocco:

* Aggiungere una proprietà per `IncrementAmount` decorata con il `[Parameter]` attributo.
* Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

Eseguire l'app. Home page ha un proprio contatore che viene incrementato di dieci ogni volta che il **Click me** pulsante è selezionato.

## <a name="next-steps"></a>Passaggi successivi

<xref:tutorials/first-razor-components-app>
