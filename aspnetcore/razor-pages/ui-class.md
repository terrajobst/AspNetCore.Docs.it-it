---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: In questo articolo viene illustrato come creare un'interfaccia utente Razor riutilizzabile in una libreria di classi.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/31/2018
ms.locfileid: "42909366"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Creare un'interfaccia utente riutilizzabile tramite il progetto di libreria di classi Razor in ASP.NET Core.

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

È possibile compilare in una libreria di classi Razor visualizzazioni, pagine, controller, modelli di pagine, [Componenti di visualizzazione](xref:mvc/views/view-components) e modelli di dati Razor. La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente. Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute. Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.

Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Creare una libreria di classi contenente l'interfaccia utente Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**.
* Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**. Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.
* Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.
* Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.

Una libreria di classi Razor è il seguente file di progetto:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire `dotnet new razorclasslib` dalla riga di comando. Ad esempio:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new). Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.

------
Aggiungere i file Razor alla libreria di classi Razor.

I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella. Visualizzare [layout delle pagine RCL](#afs) per creare contenuto in un RCL che espone `~/Pages` anziché `~/Areas/Pages`.

## <a name="referencing-razor-class-library-content"></a>Riferimento ai contenuti della libreria di classi Razor

Il riferimento alla libreria di classi Razor può essere eseguito da:

* Pacchetto NuGet. Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Procedura dettagliata: Creare un progetto libreria di classi Razor e usarlo da un progetto di Razor Pages

È possibile scaricare il [progetto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo. Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto. È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.

### <a name="test-the-download-app"></a>Testare l'app di download

Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aprire il file con estensione *sln* in Visual Studio. Eseguire l'app.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.

``` CLI
dotnet build
```

Passare alla directory *WebApp1* ed eseguire l'app:

``` CLI
dotnet run
```
------

Seguire le istruzioni indicate in [Testare WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Creare una libreria di classi Razor

In questa sezione viene creata una libreria di classi Razor. I file Razor vengono aggiunti alla libreria di classi Razor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Creare il progetto di libreria di classi Razor:

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**.
* Denominare l'app **RazorUIClassLib** > **OK**.
* Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.
* Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.
* Aggiungere un file di visualizzazione parziale Razor denominato *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente dalla riga di comando:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

I comandi precedenti:

* Crea la libreria di classi Razor `RazorUIClassLib`.
* Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor. Il parametro `-np` crea la pagina senza un `PageModel`.
* Crea un file [viewstart](xref:mvc/views/layout#running-code-before-each-view) e lo aggiunge alla libreria di classi Razor.

Il file viewstart è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione seguente).

------

### <a name="add-razor-files-and-folders-to-the-project"></a>Aggiungere i file e le cartelle Razor al progetto.

* Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`). Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*. Ad esempio:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Per altre informazioni su viewimports, vedere [Importazione delle direttive condivise](xref:mvc/views/layout#importing-shared-directives)

* Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:

``` CLI
dotnet build RazorUIClassLib
```

L'output di compilazione contiene *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* contiene il contenuto Razor compilato.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Usare la libreria dell'interfaccia utente Razor da un progetto Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Creare l'app Web di Razor Pages:

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione > **Aggiungi** >  **Nuovo progetto**.
* Selezionare **Applicazione Web ASP.NET Core**.
* Denominare l'app **WebApp1**.
* Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.
* Selezionare **Applicazione Web** > **OK**.

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Imposta come progetto di avvio**.
* In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e selezionare **Dipendenze di compilazione** > **Dipendenze progetto**.
* Selezionare **RazorUIClassLib** come una dipendenza di **WebApp1**.
* In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **WebApp1** e scegliere **Aggiungi** > **Riferimento**.
* Nella finestra di dialogo **Gestione riferimenti** selezionare **RazorUIClassLib** > **OK**.

Eseguire l'app.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e la libreria di classi Razor:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Compilare ed eseguire l'app Web:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Testare WebApp1

Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso.

* Passare a `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine

Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza. Ad esempio, aggiungere *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* a WebApp1 e Page1 in WebApp1 avrà la precedenza su Page1 nella libreria di classi Razor.

Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.

Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Aggiornare il markup per indicare la nuova posizione. Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Layout delle pagine RCL

Per fare riferimento a contenuto RCL come se facesse parte della cartella delle pagine dell'app web, creare il progetto RCL con la struttura file seguente:

* *RazorUIClassLib o delle pagine*
* *RazorUIClassLib/pagine/Shared*

Si supponga *RazorUIClassLib/pagine/Shared* contiene due file: parziali *_Header.cshtml* e *_Footer.cshtml*. Il <partial> tag può essere aggiunto al *layout. cshtml* file: 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```
