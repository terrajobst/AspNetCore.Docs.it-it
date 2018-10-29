---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'API Web di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 6c7d76e2202bf47c8d3e5d296e64e9e8c820e2a1
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207849"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introduzione a NSwag e ad ASP.NET Core

Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)

::: moniker range=">= aspnetcore-2.1"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker-end

Registrare i middleware NSwag per:

* Generare la specifica Swagger per l'API Web implementata.
* Usare l'interfaccia utente di Swagger per esplorare e testare l'API Web.

Per usare i middleware ASP.NET Core di [NSwag](https://github.com/RSuter/NSwag), installare il pacchetto NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Questo pacchetto contiene i middleware necessari per generare e usare la specifica Swagger, l'interfaccia utente di Swagger (v2 e v3) e l'[interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).

È inoltre consigliabile usare le funzionalità di generazione del codice di NSwag. Scegliere una delle opzioni seguenti per usare le funzionalità di generazione del codice:

* Usare [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), un'applicazione desktop di Windows per generare il codice client in C# e TypeScript per l'API.
* Usare i pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto.
* Usare NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Usare il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).

## <a name="features"></a>Funzionalità

È consigliabile usare NSwag soprattutto perché consente non solo di introdurre l'interfaccia utente e il generatore di Swagger, ma anche di usare le funzionalità flessibili di generazione del codice. Non occorre un'API esistente. È possibile usare le API di terze parti che includono Swagger e consentire a NSwag di generare un'implementazione client. Il ciclo di sviluppo risulta in ogni caso velocizzato ed è facile adattarsi alle modifiche dell'API.

## <a name="package-installation"></a>Installazione del pacchetto

È possibile aggiungere il pacchetto NuGet NSwag con gli approcci seguenti:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dalla finestra **Console di Gestione pacchetti**:
  * Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**
  * Passare alla directory che contiene il file *TodoApi.csproj*
  * Eseguire il seguente comando:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Dalla finestra di dialogo **Gestisci pacchetti NuGet**:
  * Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**
  * Impostare **Origine pacchetto** su "nuget.org"
  * Immettere "NSwag.AspNetCore" nella casella di ricerca
  * Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**
* Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"
* Immettere "NSwag.AspNetCore" nella casella di ricerca
* Selezionare il pacchetto "NSwag.AspNetCore" dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente da **Terminale integrato**:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente:

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Aggiungere e configurare il middleware di Swagger

Importare gli spazi dei nomi seguenti nella classe `Startup`:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

Nel metodo `Startup.ConfigureServices` registrare i servizi Swagger necessari: 

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

Nel metodo `Startup.Configure` abilitare il middleware per la gestione della specifica Swagger generata e dell'interfaccia utente di Swagger v3:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

Avviare l'app. Passare a `http://localhost:<port>/swagger` per visualizzare l'interfaccia utente di Swagger. Passare a `http://localhost:<port>/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.

## <a name="code-generation"></a>Generazione del codice

### <a name="via-nswagstudio"></a>Tramite NSwagStudio

* Installare NSwagStudio dal [repository GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) ufficiale.
* Tramite NSwagStudio. Immettere l'URL del file *swagger.json* nella casella di testo **Swagger Specification URL** (URL di specifica Swagger) e fare clic sul pulsante **Create local Copy** (Crea copia locale).
* Selezionare il tipo di output del client **CSharp Client** (Client CSharp). Le opzioni includono **TypeScript** (Client TypeScript) e **CSharp Web API Controller** (Controller API Web CSharp). L'uso di un controller API Web è sostanzialmente una generazione inversa. Viene usata una specifica di un servizio per rigenerare il servizio.
* Fare clic sul pulsante **Generate Outputs** (Genera output). Verrà generata un'implementazione client completa C# del progetto *TodoApi.NSwag*. Fare clic sulla scheda **CSharp Client** (Client CSharp) dalla sezione **Outputs** (Output) per visualizzare il codice client generato:

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> Il codice client C# viene generato in base alle impostazioni definite nella scheda **Settings** (Impostazioni) della scheda **CSharp Client** (Client CSharp). Modificare le impostazioni per eseguire attività come la ridenominazione dello spazio dei nomi predefinito e la generazione di metodi sincrona.

* Copiare il codice C# generato in un file di un progetto client, ad esempio un'app [Xamarin.Forms](/xamarin/xamarin-forms/).
* Iniziare a usare l'API Web:

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> È possibile inserire un URL di base e/o un client HTTP nel client dell'API. È sempre consigliabile [riusare HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

### <a name="other-ways-to-generate-client-code"></a>Altri modi per generare codice client

È possibile generare il codice client in altri modi, più adatti al flusso di lavoro:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Nel codice](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Modelli T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>Personalizza

Swagger offre opzioni per documentare il modello a oggetti e semplificare l'uso dell'API Web.

### <a name="api-info-and-description"></a>Informazioni e descrizione API

Nel metodo `UseSwagger` l'azione di configurazione passata al metodo `Startup.Configure` aggiunge informazioni come autore, licenza e descrizione:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

L'interfaccia utente di Swagger visualizza le informazioni sulla versione:

![Interfaccia utente Swagger con informazioni sulla versione](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML (commenti)

I commenti XML vengono abilitati con gli approcci seguenti:

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

::: moniker range=">= aspnetcore-2.0"

* Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Modifica <nome_progetto>.csproj**.
* Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**
* Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**

::: moniker-end

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio per Mac](#tab/visual-studio-mac-xml/)

::: moniker range=">= aspnetcore-2.0"

* Dal *riquadro della soluzione* premere **controllo** e fare clic sul nome del progetto. Passare a **Strumenti** > **Modifica file**.
* Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**.
* Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**

::: moniker-end

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

Aggiungere manualmente le righe evidenziate al file con estensione *csproj*:

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement&highlight=1-2,4)]

::: moniker-end

---

### <a name="data-annotations"></a>Annotazioni dei dati

::: moniker range="<= aspnetcore-2.0"

NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Di conseguenza, non è possibile per NSwag dedurre l'azione e il valore restituito. Si consideri l'esempio seguente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione. Decorare l'azione con gli attributi seguenti:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection). Il tipo restituito consigliato per le azioni dell'API Web è [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1). Di conseguenza, NSwag può solo dedurre il tipo restituito definito da `T`. Non è possibile dedurre altri tipi restituiti possibili dell'azione. Si consideri l'esempio seguente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

L'azione precedente restituisce `ActionResult<T>`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute). Poiché il controller è decorato con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), è anche possibile una risposta [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses) per altre informazioni. Le annotazioni dei dati vengono usate per indicare ai client i codici di stato HTTP restituiti da questa azione. Decorare l'azione con gli attributi seguenti:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint. È consigliabile la decorazione di tutte le azioni con questi attributi. Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
