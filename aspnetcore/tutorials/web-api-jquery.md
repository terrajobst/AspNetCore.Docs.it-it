---
title: "Esercitazione: Chiamare un'app API Web con jQuery usando ASP.NET Core"
author: rick-anderson
description: Informazioni su come chiamare un'app API Web ASP.NET Core con jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602570"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a>Esercitazione: Chiamare un'app API Web ASP.NET Core con jQuery

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra come chiamare un'app API Web ASP.NET Core con jQuery

::: moniker range="< aspnetcore-3.0"

Per ASP.NET Core 2.2, vedere la versione 2.2 di [Chiamare l'API Web con jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Prerequisiti

Completare [Esercitazione: Creare un'API Web](xref:tutorials/first-web-api)

## <a name="call-the-api-with-jquery"></a>Chiamare l'API con jQuery

In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web. jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.

Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

Creare una cartella *wwwroot* nella directory del progetto.

Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*. Sostituire il contenuto con il markup seguente:

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*. Sostituire il contenuto con il codice seguente:

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:

* Aprire *Properties\launchSettings.json*.
* Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.

È possibile ottenere jQuery in vari modi. Nel frammento precedente la libreria viene caricata da una rete CDN.

In questo esempio vengono chiamati tutti i metodi CRUD dell'API. Di seguito sono incluse le spiegazioni delle chiamate all'API.

### <a name="get-a-list-of-to-do-items"></a>Ottenere un elenco di elementi attività

La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività. La funzione di callback `success` viene chiamata se la richiesta ha esito positivo. Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Aggiungere un elemento attività

La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta. Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato. L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Aggiornare un elemento attività

L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo. L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Eliminare un elemento attività

È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end