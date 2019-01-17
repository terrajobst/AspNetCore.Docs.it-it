---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'API Web di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/30/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: c03e7513edc3240f3f13f0c190e1ca9480e476af
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098732"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introduzione a NSwag e ad ASP.NET Core

Di [Christoph Nienaber](https://twitter.com/zuckerthoben), [Rico Suter](https://rsuter.com) e [Dave Brock](https://twitter.com/daveabrock)

::: moniker range=">= aspnetcore-2.1"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procedura per il download](xref:index#how-to-download-a-sample))

::: moniker-end

NSwag offre le funzionalità seguenti:

 * La possibilità di usare l'interfaccia utente di Swagger e un generatore Swagger.
 * Funzionalità di generazione del codice flessibili.

Con NSwag non è necessaria un'API esistente. È possibile usare le API di terze parti che includono Swagger e generare un'implementazione client. NSwag consente di velocizzare il ciclo di sviluppo e adattarsi facilmente alle modifiche dell'API.

## <a name="register-the-nswag-middleware"></a>Registrare il middleware NSwag

Registrare il middleware NSwag per:

 * Generare la specifica Swagger per l'API Web implementata.
 * Usare l'interfaccia utente di Swagger per esplorare e testare l'API Web.

Per usare il middleware ASP.NET Core di [NSwag](https://github.com/RSuter/NSwag), installare il pacchetto NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Questo pacchetto contiene il middleware necessario per generare e usare la specifica Swagger, l'interfaccia utente di Swagger (v2 e v3) e l'[interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).

Usare uno degli approcci seguenti per installare il pacchetto NuGet NSwag:

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

 Aggiungere e configurare Swagger nell'app ASP.NET Core eseguendo i passaggi seguenti nella classe `Startup`:

* Importare gli spazi dei nomi seguenti:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

* Nel metodo `ConfigureServices` registrare i servizi Swagger necessari:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_ConfigureServices&highlight=8)]

 * Nel metodo `Configure` abilitare il middleware per la gestione della specifica generata e dell'interfaccia utente di Swagger:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-7)]

 * Avviare l'app. Passare a:
   * `http://localhost:<port>/swagger` per visualizzare l'interfaccia utente di Swagger.
   * `http://localhost:<port>/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.

## <a name="code-generation"></a>Generazione del codice

È possibile sfruttare i vantaggi delle funzionalità di generazione del codice di NSwag scegliendo una delle opzioni seguenti:

 * [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) &ndash; Un'app desktop di Windows per generare il codice client in C# o TypeScript per l'API.
 * I pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto.
* NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine).
 * Il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).


### <a name="generate-code-with-nswagstudio"></a>Generare il codice con NSwagStudio

* Installare NSwagStudio seguendo le istruzioni riportate nel [repository di GitHub NSwagStudio](https://github.com/RSuter/NSwag/wiki/NSwagStudio).
 * Avviare NSwagStudio e immettere l'URL del file *swagger.json* nella casella di testo **Swagger Specification URL** (URL di specifica Swagger). Ad esempio: *http://localhost:44354/swagger/v1/swagger.json*.
* Fare clic sul pulsante **Create local Copy** (Crea copia locale) per generare una rappresentazione JSON della specifica di Swagger.

  ![Creare una copia locale della specifica di Swagger](web-api-help-pages-using-swagger/_static/CreateLocalCopy-NSwagStudio.PNG)

 * Nell'area **Outputs** (Output) fare clic sulla casella di controllo **CSharp Client** (Client CSharp). A seconda del progetto, è anche possibile scegliere **TypeScript Client** (Client TypeScript) o **CSharp Web API Controller** (Controller API Web CSharp). Se si seleziona **CSharp Web API Controller** (Controller API Web CSharp), una specifica del servizio ricompila il servizio, fungendo da generazione inversa.
* Fare clic su **Generate Outputs** (Genera output) per generare un'implementazione client completa C# del progetto *TodoApi.NSwag*. Per visualizzare il codice client generato, fare clic sulla scheda **CSharp Client** (Client CSharp):

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable

    [System.CodeDom.Compiler.GeneratedCode("NSwag", "12.0.9.0 (NJsonSchema v9.13.10.0 (Newtonsoft.Json v11.0.0.0))")]
    public partial class TodoClient 
    {
        private string _baseUrl = "https://localhost:44354";
        private System.Net.Http.HttpClient _httpClient;
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;
    
        public TodoClient(System.Net.Http.HttpClient httpClient)
        {
            _httpClient = httpClient; 
            _settings = new System.Lazy<Newtonsoft.Json.JsonSerializerSettings>(() => 
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
 > Il codice client C# viene generato in base alle selezioni nella scheda **Settings** (Impostazioni). Modificare le impostazioni per eseguire attività come la ridenominazione dello spazio dei nomi predefinito e la generazione di metodi sincrona.

 * Copiare il codice C# generato in un file nel progetto client che utilizzerà l'API.
* Iniziare a usare l'API Web:

```csharp
 var todoClient = new TodoClient();

// Gets all to-dos from the API
 var allTodos = await todoClient.GetAllAsync();

 // Create a new TodoItem, and save it via the API.
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

## <a name="customize-api-documentation"></a>Personalizzare la documentazione dell'API

Swagger offre opzioni per documentare il modello a oggetti e semplificare l'uso dell'API Web.

### <a name="api-info-and-description"></a>Informazioni e descrizione API

Nel metodo `AddSwaggerDocument` l'azione di configurazione passata al metodo `Startup.ConfigureServices` aggiunge informazioni come autore, licenza e descrizione:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_AddSwaggerDocument)]

L'interfaccia utente di Swagger visualizza le informazioni sulla versione:

![Interfaccia utente Swagger con informazioni sulla versione](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML (commenti)

 Per abilitare i commenti XML, seguire questa procedura:

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

 Dato che NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) e il tipo restituito consigliato per le azioni API Web è [IActionResult](xref:Microsoft.AspNetCore.Mvc.IActionResult), non è possibile dedurre cosa sta facendo l'azione e il risultato.

Si consideri l'esempio seguente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

 L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*) o [BadRequest](xref:System.Web.Http.ApiController.BadRequest*). Usare le annotazioni dei dati per indicare ai client i codici di stato HTTP restituiti da questa azione. Decorare l'azione con gli attributi seguenti:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

 Dato che NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection) e il tipo restituito consigliato per le azioni API Web è [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult`1), può solo dedurre il tipo restituito definito da `T`. Non è possibile dedurre automaticamente altri tipi restituiti possibili. 

Si consideri l'esempio seguente:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

L'azione precedente restituisce `ActionResult<T>`. All'interno dell'azione, viene restituito [CreatedAtRoute](xref:System.Web.Http.ApiController.CreatedAtRoute*). Poiché il controller è decorato con l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), è anche possibile una risposta [BadRequest](xref:System.Web.Http.ApiController.BadRequest*). Per altre informazioni, vedere [Risposte HTTP 400 automatiche](xref:web-api/index#automatic-http-400-responses). Usare le annotazioni dei dati per indicare ai client i codici di stato HTTP restituiti da questa azione. Decorare l'azione con gli attributi seguenti:

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]

In ASP.NET Core 2.2 o versioni successive, è possibile usare convenzioni in alternativa alla decorazione esplicita di azioni singole con `[ProducesResponseType]`. Per ulteriori informazioni, vedere <xref:web-api/advanced/conventions>.

::: moniker-end

 Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint. È consigliabile decorare tutte le azioni con questi attributi. 

Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
