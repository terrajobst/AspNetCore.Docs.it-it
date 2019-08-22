---
title: Interfaccia utente Razor riutilizzabile nelle librerie di classi con ASP.NET Core
author: Rick-Anderson
description: Viene illustrato come creare un'interfaccia Razor riutilizzabili tramite le visualizzazioni parziali in una libreria di classi in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 468d961c291810ca4dfbe615acd972cfd6e7572a
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886399"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Creare un'interfaccia utente riutilizzabile usando il progetto libreria di classi Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le visualizzazioni Razor, le pagine, i controller, i modelli di pagina, i [componenti Razor](xref:blazor/class-libraries), i [componenti di visualizzazione](xref:mvc/views/view-components)e i modelli di dati possono essere incorporati in una libreria di classi Razor (RCL). La libreria di classi Razor può essere inclusa nel pacchetto e usata nuovamente. Le applicazioni possono includere la libreria di classi Razor ed eseguire l'override delle visualizzazioni e pagine in essa contenute. Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza.

Questa funzionalità richiede [!INCLUDE[](~/includes/2.1-SDK.md)]

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Creare una libreria di classi contenente l'interfaccia utente Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**.
* Assegnare un nome di libreria (ad esempio, "RazorClassLib") > **OK**. Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.
* Verificare che sia selezionato **ASP.NET Core 2.1** o versioni successive.
* Selezionare **Razor Class Library** (Libreria di classi Razor) > **OK**.

Un RCL ha il seguente file di progetto:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire `dotnet new razorclasslib` dalla riga di comando. Ad esempio:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Per altre informazioni, vedere [dotnet new](/dotnet/core/tools/dotnet-new). Per evitare un conflitto di nomi di file con la libreria di visualizzazione generata, verificare che il nome della libreria non finisca per `.Views`.

---

Aggiungere i file Razor alla libreria di classi Razor.

I modelli ASP.NET Core presuppongono il contenuto RCL sia impostato il *aree* cartella. Vedere [layout di pagine RCL](#afs) per creare un RCL che espone il `~/Pages` contenuto in `~/Areas/Pages`anziché.

## <a name="referencing-rcl-content"></a>Riferimento al contenuto di RCL

Il riferimento alla libreria di classi Razor può essere eseguito da:

* Pacchetto NuGet. Vedere [Creazione di pacchetti NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Creare e pubblicare un pacchetto NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. Vedere [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Procedura dettagliata: Creare un progetto RCL e usarlo da un progetto Razor Pages

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

* Crea l' `RazorUIClassLib` oggetto RCL.
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

Creare un'app Web Razor Pages e un file di soluzione contenente l'app Razor Pages e il RCL:

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

Verificare che la libreria di classi dell'interfaccia utente Razor sia in uso:

* Passare a `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Eseguire l'override di visualizzazioni, visualizzazioni parziali e pagine

Quando viene trovata una visualizzazione, visualizzazione parziale o pagina Razor sia nell'app Web che nella libreria di classi Razor, il markup Razor (file con estensione *cshtml*) nell'app Web ha la precedenza. Ad esempio, aggiungere *app Web 1/areas/la funzionalità/Pages/Pagina1. cshtml* a app Web 1 e Pagina1 in app Web 1 avrà la precedenza su PAGINA1 in RCL.

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

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a>Creare un RCL con asset statici

Un RCL può richiedere risorse statiche complementari a cui è possibile fare riferimento dall'app consumer di RCL. ASP.NET Core consente di creare RCL che includono risorse statiche disponibili per un'app di consumo.

Per includere asset complementari come parte di un RCL, creare una cartella *wwwroot* nella libreria di classi e includere tutti i file necessari in tale cartella.

Quando si esegue la compressione di un RCL, tutti gli asset complementari nella cartella *wwwroot* vengono inclusi automaticamente nel pacchetto.

### <a name="consume-content-from-a-referenced-rcl"></a>Utilizzare il contenuto da un RCL a cui si fa riferimento

I file inclusi nella cartella *wwwroot* di RCL vengono esposti all'app consumer sotto il prefisso `_content/{LIBRARY NAME}/`. Ad esempio, una libreria denominata *Razor. Class. lib* genera un percorso al contenuto statico in `_content/Razor.Class.Lib/`.

L'app consumer fa riferimento a risorse statiche fornite dalla libreria `<script>`con `<style>` `<img>`,, e altri tag HTML. Per l'app consumer deve essere abilitato il [supporto file statico](xref:fundamentals/static-files) in `Startup.Configure`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

Quando si esegue l'app consumer dall'output di`dotnet run`compilazione (), le risorse Web statiche sono abilitate per impostazione predefinita nell'ambiente di sviluppo. Per supportare asset in altri ambienti durante l'esecuzione dall'output di compilazione `UseStaticWebAssets` , chiamare sul generatore host in *Program.cs*:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

Quando `UseStaticWebAssets` si esegue un'app dall'output pubblicato (`dotnet publish`), la chiamata non è obbligatoria.

### <a name="multi-project-development-flow"></a>Flusso di sviluppo per più progetti

Quando viene eseguita l'app consumer:

* Gli asset nel RCL rimanere nelle cartelle originali. Gli asset non vengono spostati nell'app consumer.
* Tutte le modifiche apportate all'interno della cartella *wwwroot* di RCL vengono riflesse nell'app consumer dopo che il RCL viene ricompilato e senza ricompilare l'app consumer.

Quando viene compilato il RCL, viene generato un manifesto che descrive i percorsi di asset Web statici. L'app consumer legge il manifesto in fase di esecuzione per usare gli asset dei progetti e dei pacchetti a cui si fa riferimento. Quando si aggiunge un nuovo asset a un RCL, è necessario ricompilare il RCL per aggiornare il manifesto prima che un'app che utilizza possa accedere al nuovo asset.

### <a name="publish"></a>Pubblica

Quando viene pubblicata l'app, gli asset complementari di tutti i progetti e i pacchetti a cui viene fatto riferimento vengono copiati nella cartella wwwroot `_content/{LIBRARY NAME}/`dell'app pubblicata in.

::: moniker-end
