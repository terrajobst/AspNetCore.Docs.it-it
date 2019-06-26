---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/24/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 96ef8fc055a6b92cd0808d02031d917b8446f305
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394750"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Creare un'interfaccia utente riutilizzabile tramite il progetto di libreria di classi Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le visualizzazioni Razor, pagine, i controller, modelli di pagina [componenti Razor](xref:blazor/class-libraries), [componenti di visualizzazione](xref:mvc/views/view-components), e i modelli di dati possono essere compilati in una libreria di classi Razor (RCL). La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente. Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute. Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.

Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Creare una libreria di classi contenente l'interfaccia utente Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**.
* Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**. Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.
* Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.
* Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.

Un RCL ha file di progetto seguente:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire `dotnet new razorclasslib` dalla riga di comando. Ad esempio:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new). Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.

---

Aggiungere i file Razor alla libreria di classi Razor.

I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella. Visualizzare [layout delle pagine RCL](#afs) per creare contenuto in un RCL che espone `~/Pages` anziché `~/Areas/Pages`.

## <a name="referencing-rcl-content"></a>Riferimento ai contenuti RCL

Il riferimento alla libreria di classi Razor può essere eseguito da:

* Pacchetto NuGet. Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Procedura dettagliata: Creare un progetto RCL e usare da un progetto Razor Pages

È possibile scaricare il [progetto completo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) e testarlo anziché crearlo. Il download di esempio contiene codice aggiuntivo e collegamenti che rendono più semplice testare il progetto. È possibile lasciare commenti e suggerimenti in [questa discussione su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) con i commenti sui download di esempio rispetto alle istruzioni dettagliate.

### <a name="test-the-download-app"></a>Testare l'app di download

Se non è stata scaricata l'app completa e si vuole invece creare il progetto della procedura dettagliata, passare alla [prossima sezione](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aprire il file con estensione *sln* in Visual Studio. Eseguire l'app.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Al prompt dei comandi nella directory *cli*, compilare la libreria di classi Razor e l'app Web.

```console
dotnet build
```

Passare alla directory *WebApp1* ed eseguire l'app:

```console
dotnet run
```

---

Seguire le istruzioni indicate in [Testare WebApp1](#test)

## <a name="create-an-rcl"></a>Creare un RCL

In questa sezione viene creato un RCL. I file Razor vengono aggiunti alla libreria di classi Razor.

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

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

I comandi precedenti:

* Crea il `RazorUIClassLib` RCL.
* Crea una pagina Razor _Message e la aggiunge alla libreria di classi Razor. Il parametro `-np` crea la pagina senza un `PageModel`.
* Crea una [viewstart. cshtml](xref:mvc/views/layout#running-code-before-each-view) file e lo aggiunge al RCL.

Il *viewstart. cshtml* file è necessario per usare il layout del progetto Razor Pages (che viene aggiunto nella sezione successiva).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Aggiungere i file Razor e cartelle per il progetto

* Sostituire il markup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* con il codice seguente:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Sostituire il markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* con il codice seguente:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` è necessario per usare la visualizzazione parziale (`<partial name="_Message" />`). Anziché includere la direttiva `@addTagHelper`, è possibile aggiungere un file *_ViewImports.cshtml*. Ad esempio:

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Per ulteriori informazioni sul *viewimports. cshtml*, vedere [importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives)

* Compilare la libreria di classi per verificare che non siano presenti errori del compilatore:

```console
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

Creare un'app web Razor Pages e un file di soluzione che contiene l'app Razor Pages e la RCL:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Compilare ed eseguire l'app Web:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Testare WebApp1

Verificare che la libreria di classi Razor dell'interfaccia utente sia in uso:

* Passare a `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine

Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza. Ad esempio, aggiungere *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* per App Web 1, e la pagina Page1 nell'App Web 1 avrà la precedenza sulla pagina Page1 nella RCL.

Nel download di esempio, rinominare *WebApp1/aree/MyFeature2* in *WebApp1/aree/MyFeature* per testare la precedenza.

Copiare la visualizzazione parziali *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* in *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Aggiornare il markup per indicare la nuova posizione. Compilare ed eseguire l'app per verificare quale versione del parziale sia in uso.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Layout delle pagine RCL

Per riferimento come se facesse parte dell'app web di contenuto RCL *pagine* cartella, creare il progetto RCL con la struttura file seguente:

* *RazorUIClassLib o delle pagine*
* *RazorUIClassLib/pagine/Shared*

Si supponga *RazorUIClassLib/pagine/Shared* contiene due file parziali: *_Header.cshtml* e *_Footer.cshtml*. Il `<partial>` tag può essere aggiunto al *layout. cshtml* file:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Creare un RCL con Asset statici

Un RCL potrebbero richiedere asset statici complementari che possono essere utilizzati dalle app dispendiosa in termini del RCL. ASP.NET Core consente di creare RCLs che includono asset statici disponibili per un'app consumer.

Per includere gli asset complementare come parte di un RCL, creare un *wwwroot* cartella nella libreria di classi e includere eventuali file richiesti in tale cartella.

La creazione di un RCL, companion in tutti gli asset nel *wwwroot* cartella vengono automaticamente inclusi nel pacchetto e vengono rese disponibile per le app che fa riferimento il pacchetto.

### <a name="consume-content-from-a-referenced-rcl"></a>Utilizzare il contenuto di un riferimento RCL

I file inclusi nel *wwwroot* cartella della RCL vengono esposte all'app dispendiosa in termini di sotto del prefisso `_content/{LIBRARY NAME}/`. `{LIBRARY NAME}` è il nome del progetto libreria convertito in lettere minuscole con periodi (`.`) rimosso. Ad esempio, una libreria denominata *Razor.Class.Lib* risultante in un percorso per il contenuto statico in `_content/razorclasslib/`.

Le app consumer fa riferimento all'asset statici forniti dalla libreria con `<script>`, `<style>`, `<img>`e gli altri tag HTML. Le app consumer deve avere [supporto dei file statici](xref:fundamentals/static-files) abilitata.

### <a name="multi-project-development-flow"></a>Flusso di sviluppo basati su più progetti

Quando viene eseguita l'app dispendiosa in termini di:

* Le risorse nel RCL si trovino nelle rispettive cartelle originali. Gli asset non vengono spostati all'app consumer.
* Qualsiasi modifica apportata all'interno di RCL *wwwroot* cartella viene riflessa nell'app consumer dopo il RCL viene ricompilato e senza ricompilare l'app consumer.

Quando viene compilato il RCL, viene generato un manifesto che descrive le posizioni di asset web statico. Le app consumer legge il manifesto in fase di esecuzione per utilizzare le risorse da cui viene fatto riferimento progetti e pacchetti. Quando un nuovo asset viene aggiunto a un RCL, è necessario ricompilare il RCL per aggiornare il relativo manifesto prima che un'app consumer possa accedere il nuovo asset.

### <a name="publish"></a>Pubblica

Quando l'app viene pubblicata, gli asset complementare da tutti i progetti e riferimento i pacchetti vengono copiati le *wwwroot* cartella dell'app pubblicata in `_content/{LIBRARY NAME}/`.
