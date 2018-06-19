---
title: Pagine della Guida dell'API Web ASP.NET Core con Swagger/OpenAPI
author: rsuter
description: In questa esercitazione viene descritta una procedura dettagliata per aggiungere Swagger e generare la documentazione e le pagine della Guida di un'app API Web.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077336"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="6227e-103">Pagine della Guida dell'API Web ASP.NET Core con Swagger/OpenAPI</span><span class="sxs-lookup"><span data-stu-id="6227e-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="6227e-104">Di [Christoph Nienaber](https://twitter.com/zuckerthoben) e [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="6227e-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="6227e-105">Quando si usa un'API Web, la comprensione dei vari metodi che la compongono può risultare complessa per gli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="6227e-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="6227e-106">[Swagger](https://swagger.io/), detto anche OpenAPI, risolve il problema della generazione di pagine della Guida e documentazione utili per le API Web.</span><span class="sxs-lookup"><span data-stu-id="6227e-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="6227e-107">Offre vantaggi quali la documentazione interattiva, la generazione di SDK client e l'individuabilità delle API.</span><span class="sxs-lookup"><span data-stu-id="6227e-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="6227e-108">Questo articolo illustra le implementazioni .NET di Swagger [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) e [NSwag](https://github.com/RSuter/NSwag):</span><span class="sxs-lookup"><span data-stu-id="6227e-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="6227e-109">**Swashbuckle.AspNetCore** è un progetto open source per la generazione di documenti Swagger per le API Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6227e-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="6227e-110">**NSwag** è un altro progetto open source per l'integrazione di [Swagger UI](https://swagger.io/swagger-ui/) o [ReDoc](https://github.com/Rebilly/ReDoc) nelle API Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6227e-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="6227e-111">Offre approcci per generare codice client C# e TypeScript per l'API.</span><span class="sxs-lookup"><span data-stu-id="6227e-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="6227e-112">Che cos'è Swagger/OpenAPI?</span><span class="sxs-lookup"><span data-stu-id="6227e-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="6227e-113">Swagger è una specifica indipendente dal linguaggio per la descrizione delle API [REST](https://en.wikipedia.org/wiki/Representational_state_transfer).</span><span class="sxs-lookup"><span data-stu-id="6227e-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="6227e-114">Il progetto Swagger è stato donato all'[iniziativa OpenAPI](https://www.openapis.org/), in cui è ora denominato OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="6227e-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="6227e-115">I nomi sono intercambiabili, ma è preferibile usare OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="6227e-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="6227e-116">Questo strumento consente a computer e utenti di comprendere le funzionalità di un servizio senza accedere direttamente all'implementazione (codice sorgente, accesso alla rete, documentazione).</span><span class="sxs-lookup"><span data-stu-id="6227e-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="6227e-117">Un obiettivo è la riduzione al minimo della quantità di lavoro necessaria per la connessione di servizi non associati.</span><span class="sxs-lookup"><span data-stu-id="6227e-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="6227e-118">Un altro obiettivo è la riduzione del tempo necessario per documentare in modo accurato un servizio.</span><span class="sxs-lookup"><span data-stu-id="6227e-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="6227e-119">Specifica Swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="6227e-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="6227e-120">Il nucleo del flusso di Swagger è la specifica Swagger, che per impostazione predefinita è un documento denominato *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="6227e-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="6227e-121">La specifica viene generata dalla catena di strumenti Swagger (o da implementazioni di terzi della catena) a seconda del servizio.</span><span class="sxs-lookup"><span data-stu-id="6227e-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="6227e-122">Descrive le funzionalità dell'API e come accedervi mediante HTTP.</span><span class="sxs-lookup"><span data-stu-id="6227e-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="6227e-123">Gestisce Swagger UI ed è usata dalla catena di strumenti per abilitare l'individuazione e la generazione del codice client.</span><span class="sxs-lookup"><span data-stu-id="6227e-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="6227e-124">Di seguito è riportato un esempio di specifica di Swagger, ridotto per ragioni di brevità:</span><span class="sxs-lookup"><span data-stu-id="6227e-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="6227e-125">Interfaccia utente di Swagger</span><span class="sxs-lookup"><span data-stu-id="6227e-125">Swagger UI</span></span>

<span data-ttu-id="6227e-126">[Swagger UI](https://swagger.io/swagger-ui/) offre un'interfaccia utente basata sul web che produce informazioni sul servizio usando la specifica di Swagger generata.</span><span class="sxs-lookup"><span data-stu-id="6227e-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="6227e-127">Sia Swashbuckle sia NSwag includono una versione incorporata di Swagger UI che può essere ospitata nell'applicazione ASP.NET Core dell'utente con una chiamata di registrazione middleware.</span><span class="sxs-lookup"><span data-stu-id="6227e-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="6227e-128">L'interfaccia utente Web è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="6227e-128">The web UI looks like this:</span></span>

![Interfaccia utente di Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="6227e-130">Ogni metodo di azione pubblico nei controller può essere testato dall'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="6227e-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="6227e-131">Fare clic su un nome di metodo per espandere la sezione.</span><span class="sxs-lookup"><span data-stu-id="6227e-131">Click a method name to expand the section.</span></span> <span data-ttu-id="6227e-132">Aggiungere i parametri necessari e fare clic su **Prova**.</span><span class="sxs-lookup"><span data-stu-id="6227e-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Esempio test GET di Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="6227e-134">La versione di Swagger UI usata per le schermate è la versione 2.</span><span class="sxs-lookup"><span data-stu-id="6227e-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="6227e-135">Per un esempio con la versione 3, vedere l'[esempio Petstore](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="6227e-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6227e-136">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6227e-136">Next steps</span></span>

* [<span data-ttu-id="6227e-137">Introduzione a Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="6227e-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="6227e-138">Introduzione a NSwag</span><span class="sxs-lookup"><span data-stu-id="6227e-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
