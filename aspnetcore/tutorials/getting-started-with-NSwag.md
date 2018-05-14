---
title: Introduzione a NSwag e ad ASP.NET Core
author: zuckerthoben
description: Informazioni su come usare NSwag per generare le pagine della documentazione e della Guida per un'app per le API di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Introduzione a NSwag e ad ASP.NET Core

Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](https://rsuter.com)

Per usare [NSwag](https://github.com/RSuter/NSwag) con il middleware di ASP.NET Core è necessario il pacchetto NuGet[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Il pacchetto contiene un generatore di Swagger, l'interfaccia utente di Swagger (versione 2 e 3) e [l'interfaccia utente di ReDoc](https://github.com/Rebilly/ReDoc).

È consigliabile usare le funzionalità di generazione del codice di NSwag. Scegliere una delle opzioni seguenti per generare il codice:

* Usare [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), un'applicazione desktop di Windows per generare il codice client in C# e TypeScript per l'API
* Usare i pacchetti NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) o [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) per generare il codice all'interno del progetto
* Usare NSwag dalla [riga di comando](https://github.com/NSwag/NSwag/wiki/CommandLine)
* Usare il pacchetto NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild)

## <a name="features"></a>Funzionalità

È consigliabile usare NSwag soprattutto perché consente non solo di introdurre l'interfaccia utente e il generatore di Swagger, ma di usare le funzionalità flessibili di generazione del codice. Non occorre un'API esistente. È possibile usare le API di terze parti che includono Swagger e consentire a NSwag di generare un'implementazione client. Il ciclo di sviluppo risulta in ogni caso velocizzato ed è facile adattarsi alle modifiche dell'API.

## <a name="package-installation"></a>Installazione del pacchetto

È possibile aggiungere il pacchetto NuGet NSwag con gli approcci seguenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dalla finestra **Console di Gestione pacchetti**:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Dalla finestra di dialogo **Gestisci pacchetti NuGet**:
  * Fare clic con il pulsante destro del mouse su **Esplora soluzioni** > **Gestisci pacchetti NuGet**
  * Impostare **Origine pacchetto** su "nuget.org"
  * Immettere "NSwag.AspNetCore" nella casella di ricerca
  * Selezionare il pacchetto "NSwag.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**
* Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"
* Immettere NSwag.AspNetCore nella casella di ricerca
* Selezionare il pacchetto NSwag.AspNetCore dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente da **Terminale integrato**:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Aggiungere e configurare il middleware di Swagger

Importare gli spazi dei nomi seguenti nella classe `Info`:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

Nel metodo `Startup.Configure` abilitare il middleware per la gestione della specifica generata e dell'interfaccia utente di Swagger:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Avviare l'app. Passare a `/swagger` per visualizzare l'interfaccia utente di Swagger. Passare a `/swagger/v1/swagger.json` per visualizzare la specifica di Swagger.

## <a name="code-generation"></a>Generazione del codice

### <a name="via-nswagstudio"></a>Tramite NSwagStudio

* Installare `NSwagStudio` dal [repository GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) ufficiale.

* Tramite NSwagStudio. Immettere il percorso del file *swagger.json* oppure copiarlo direttamente:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Indicare il tipo di output del client desiderato. Le opzioni includono **Client TypeScript**, **Client CSharp** o **Controller API Web CSharp**. L'uso di un controller API Web è sostanzialmente una generazione inversa. Viene usata una specifica di un servizio per rigenerare il servizio.

* Fare clic su **Generate Outputs** (Genera output).

* Di seguito è possibile vedere un'implementazione client completa dell'esempio *TodoApi.NSwag* in C#:

![Output di NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Inserire il file in un progetto client, ad esempio un'app [Xamarin.Forms](/xamarin/xamarin-forms/). Iniziare a usare l'API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> È possibile inserire un URL di base e/o un client HTTP nel client dell'API. È sempre consigliabile [riusare HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).

È ora possibile avviare facilmente l'implementazione dell'API nei progetti client.

### <a name="other-ways-to-generate-client-code"></a>Altri modi per generare codice client

È possibile generare il codice in altri modi, più adatti al flusso di lavoro:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Nel codice](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [Modelli T4](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Personalizzazione

### <a name="xml-comments"></a>XML (commenti)

I commenti XML vengono abilitati con gli approcci seguenti:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**
* Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**:

![Scheda Compilazione delle proprietà del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio per Mac](#tab/visual-studio-mac-xml/)
* Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**
* Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**:

![Sezione Opzioni generali delle opzioni del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
Aggiungere manualmente il frammento di codice seguente al file *csproj*:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Annotazioni dei dati

NSwag usa la [reflection](/dotnet/csharp/programming-guide/concepts/reflection)Per le azioni API Web la procedura consigliata è la restituzione di [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Di conseguenza, non è possibile per NSwag dedurre l'azione e il valore restituito. Si consideri l'esempio seguente:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

L'azione precedente restituisce `IActionResult`, ma all'interno dell'azione viene restituito [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) o [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest). Le annotazioni dei dati vengono usate per indicare ai client la risposta HTTP restituita da questa azione. Decorare l'azione con gli attributi seguenti:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Il generatore di Swagger ora può descrivere accuratamente questa azione e i client generati conoscono la risposta che riceveranno quando si chiamerà l'endpoint. È consigliabile la decorazione di tutte le azioni con questi attributi. Per le linee guida sulle risposte HTTP restituite dalle azioni API, vedere la [specifica RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).
