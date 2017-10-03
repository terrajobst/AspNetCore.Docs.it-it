---
title: Pagine della Guida dell'API Web ASP.NET Core con Swagger
author: spboyer
description: In questa esercitazione viene descritta una procedura dettagliata per aggiungere Swagger per generare la documentazione e le pagine della Guida di un'applicazione API Web.
keywords: ASP.NET Core,Swagger,Swashbuckle,pagine Guida,API Web
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 7eeb8f0517b8806cabdd59e7d81f8c2272238615
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="aspnet-web-api-help-pages-using-swagger"></a>Pagine della Guida dell'API Web ASP.NET con Swagger

<a name=web-api-help-pages-using-swagger></a>

Di [Shayne Boyer](https://twitter.com/spboyer) e [Scott Addie](https://twitter.com/Scott_Addie)

Capire i diversi metodi di un'API può risultare difficile per gli sviluppatori che compilano applicazioni a consumo elevato.

La generazione di pagine di Guida e di una documentazione valide per l'API Web tramite [Swagger](https://swagger.io/) con l'implementazione di .NET Core [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), è facile come aggiungere un paio di pacchetti NuGet e modificare *Startup.cs*.

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) è un progetto open source per la generazione di documenti Swagger per le API Web di ASP.NET Core.

* [Swagger](https://swagger.io/) è una rappresentazione leggibile al computer di un'API RESTful che consente il supporto per la documentazione interattiva, la generazione di client SDK e l'esposizione al rilevamento.

Questa esercitazione si basa sull'esempio su [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api) (Compilazione di un'API Web con ASP.NET Core MVC e Visual Studio). Se si vuole proseguire, scaricare l'esempio all'indirizzo [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).

## <a name="getting-started"></a>Introduzione

Esistono tre componenti principali di Swashbuckle:

* `Swashbuckle.AspNetCore.Swagger`: un modello a oggetti Swagger e middleware per esporre oggetti `SwaggerDocument` come endpoint JSON.

* `Swashbuckle.AspNetCore.SwaggerGen`: un generatore Swagger che compila oggetti `SwaggerDocument` direttamente da route, controller e modelli. È in genere combinato con il middleware di endpoint Swagger per esporre automaticamente il JSON Swagger.

* `Swashbuckle.AspNetCore.SwaggerUI`: una versione incorporata dello strumento interfaccia utente di Swagger che interpreta il JSON Swagger per descrivere in modo completo e personalizzabile la funzionalità dell'API Web. Include test harness predefiniti per i metodi pubblici.

## <a name="nuget-packages"></a>Pacchetti NuGet

È possibile aggiungere Swashbuckle con gli approcci seguenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dalla finestra **Console di Gestione pacchetti**:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* Dalla finestra di dialogo **Gestisci pacchetti NuGet**:

     * Fare clic con il pulsante destro del mouse su **Esplora soluzioni** > **Gestisci pacchetti NuGet**
     * Impostare **Origine pacchetto** su "nuget.org"
     * Immettere "Swashbuckle.AspNetCore" nella casella di ricerca
     * Selezionare il pacchetto "Swashbuckle.AspNetCore" dalla scheda **Sfoglia** e fare clic su **Installa**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**
* Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"
* Immettere Swashbuckle.AspNetCore nella casella di ricerca
* Selezionare il pacchetto Swashbuckle.AspNetCore dal riquadro dei risultati e fare clic su **Aggiungi pacchetto**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente da **Terminale integrato**:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente:

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Aggiungere e configurare Swagger per il middleware

Aggiungere il generatore di Swagger alla raccolta di servizi nel metodo `ConfigureServices` di *Startup.cs*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

Aggiungere l'istruzione using seguente per la classe `Info`:

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

Nel metodo `Configure` di *Startup.cs* abilitare il middleware per la gestione del documento JSON generato e l'interfaccia utente di Swagger:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

Avviare l'app e passare a `http://localhost:<random_port>/swagger/v1/swagger.json`. Viene visualizzato il documento generato che descrive gli endpoint.

**Nota:** Microsoft Edge, Google Chrome e Firefox visualizzano i documenti JSON in modo nativo. Sono disponibili estensioni per Chrome che formattano il documento per agevolare la lettura. *L'esempio seguente viene ridotto per brevità.*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

Il documento attiva l'interfaccia utente di Swagger che è possibile visualizzare accedendo a `http://localhost:<random_port>/swagger`:

![Interfaccia utente di Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Ogni metodo di azione pubblico in `TodoController` può essere testato dall'interfaccia utente. Fare clic su un nome di metodo per espandere la sezione. Aggiungere i parametri necessari e fare clic su "Try it out!" (Provalo!).

![Esempio test GET di Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>Personalizzazione ed estendibilità

Swagger offre opzioni per documentare il modello a oggetti e personalizzare l'interfaccia utente in modo che corrisponda al tema.

### <a name="api-info-and-description"></a>Informazioni e descrizione API

L'azione di configurazione passata al metodo `AddSwaggerGen` può essere usata per aggiungere informazioni come ad esempio autore, licenza e descrizione:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

Nella figura seguente viene illustrata l'interfaccia utente di Swagger che visualizza le informazioni sulla versione:

![Interfaccia utente di Swagger con informazioni sulla versione: descrizione, autore e altri collegamenti](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>Commenti XML

I commenti XML possono essere abilitati con gli approcci seguenti:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e selezionare **Proprietà**
* Selezionare la casella di controllo **File di documentazione XML** nella sezione **Output** della scheda **Compilazione**:

![Scheda Compilazione delle proprietà del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Aprire la finestra di dialogo **Opzioni progetto** > **Compila** > **Compilatore**
* Selezionare la casella di controllo **Genera la documentazione XML** nella sezione **Opzioni generali**:

![Sezione Opzioni generali delle opzioni del progetto](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aggiungere manualmente il frammento di codice seguente al file *csproj*:

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

---

Configurare Swagger in modo che usi il file XML generato. Per Linux o sistemi operativi diversi da Windows, i percorsi e i nomi di file possono fare distinzione tra maiuscole e minuscole. Ad esempio, un file *ToDoApi.XML* potrebbe trovarsi in Windows ma non in CentOS.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

Nel codice precedente `ApplicationBasePath` ottiene il percorso base dell'app. Il percorso base viene usato per individuare il file di commenti XML. *TodoApi.xml* funziona solo per questo esempio, poiché il nome del file di commenti XML generato è basato sul nome dell'applicazione.

L'aggiunta al metodo di commenti a barra tripla migliora l'interfaccia utente di Swagger poiché viene aggiunta la descrizione all'intestazione della sezione:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Interfaccia utente di Swagger che visualizza il commento XML "Deletes a specific TodoItem." per il metodo DELETE](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

L'interfaccia utente viene attivata dal file JSON generato, che contiene anche i commenti seguenti:

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

Aggiungere un tag [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) alla documentazione del metodo di azione `Create`. In questo modo vengono integrate le informazioni specificate nel tag `<summary>` e l'interfaccia utente di Swagger risulta più affidabile. Il contenuto del tag `<remarks>` può essere costituito da testo, JSON o XML.

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

Si notino i miglioramenti dell'interfaccia utente con questi commenti aggiuntivi.

![Interfaccia utente di Swagger con commenti aggiuntivi visualizzati](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>Annotazioni dei dati

Complementare il modello con gli attributi, disponibili in `System.ComponentModel.DataAnnotations`, per attivare i componenti dell'interfaccia utente di Swagger.

Aggiungere l'attributo `[Required]` alla proprietà `Name` della classe `TodoItem`:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

La presenza di questo attributo modifica il comportamento dell'interfaccia utente e lo schema JSON sottostante:

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

Aggiungere l'attributo `[Produces("application/json")]` al controller API. Il suo scopo consiste nel dichiarare che le azioni del controller supportano un tipo di contenuto restituito *application/json*:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

L'elenco a discesa **Response Content Type** (Tipo di contenuto risposta) include questo tipo di contenuto come selezione predefinita per le azioni GET del controller:

![Interfaccia utente di Swagger con tipo di contenuto di risposta predefinito](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Con l'aumento dell'uso delle annotazioni dei dati nell'API Web, le pagine della Guida dell'API e dell'interfaccia utente diventano più descrittive e utili.

### <a name="describing-response-types"></a>Descrizione dei tipi di risposta

Per gli sviluppatori delle applicazioni a consumo elevato è più importante ciò che viene restituito, in particolare i tipi di risposta e i codici di errore, se diversi da quelli standard. Questi vengono gestiti nei commenti XML e nelle annotazioni dei dati.

L'azione `Create` restituisce `201 Created` in caso di esito positivo o `400 Bad Request` quando il corpo della richiesta inviata è Null. Senza la documentazione appropriata nell'interfaccia utente di Swagger, il consumer non riconosce tali risultati previsti. Il problema si risolve aggiungendo le righe evidenziate nell'esempio seguente:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

L'interfaccia utente di Swagger ora documenta chiaramente i codici di risposta HTTP previsti:

![Interfaccia utente di Swagger che visualizza la descrizione della classe risposta POST "Returns the newly created Todo item" e "400 - If the item is null" per il codice di stato e il motivo nei messaggi di risposta](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>Personalizzazione dell'interfaccia utente

L'interfaccia utente è funzionale e presentabile ma, quando si compilano pagine della documentazione per l'API, è necessario che rappresenti il proprio marchio o tema. Per eseguire questa attività con i componenti Swashbuckle è necessario aggiungere le risorse per gestire i file statici e quindi compilare la struttura di cartelle per ospitare i file.

Se la destinazione è .NET Framework, aggiungere il pacchetto NuGet `Microsoft.AspNetCore.StaticFiles` al progetto:

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

Abilitare il middleware dei file statici:

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

Acquisire il contenuto della cartella *dist* dall'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui/tree/2.x/dist). La cartella contiene le risorse necessarie per la pagina dell'interfaccia utente di Swagger.

Creare una cartella *wwwroot/swagger/ui* e copiarvi il contenuto della cartella *dist*.

Creare un file *wwwroot/swagger/ui/css/custom.css* con il CSS riportato di seguito per personalizzare l'intestazione della pagina:

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

Creare il riferimento a *custom.css* nel file *index.html*:

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

Passare alla pagina *index.html* all'indirizzo `http://localhost:<random_port>/swagger/ui/index.html`. Immettere `http://localhost:<random_port>/swagger/v1/swagger.json` nella casella di testo dell'intestazione e scegliere il pulsante **Explore** (Esplora). La pagina risultante è simile alla seguente:

![Interfaccia utente di Swagger con il titolo dell'intestazione personalizzato](web-api-help-pages-using-swagger/_static/custom-header.png)

Si può fare molto altro con la pagina. Per vedere le funzionalità complete per le risorse dell'interfaccia utente, accedere all'[archivio dell'interfaccia utente di Swagger di GitHub](https://github.com/swagger-api/swagger-ui).
