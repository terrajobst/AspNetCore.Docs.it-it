---
title: Usare gli analizzatori dell'API Web
author: pranavkm
description: Informazioni sugli analizzatori dell'API Web in Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7558552586d3056c43d8bfd9ef74cbcb3396726f
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425094"
---
# <a name="use-web-api-analyzers"></a>Usare gli analizzatori dell'API Web

ASP.NET Core 2.2 e versioni successive include il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers), che contiene analizzatori per le API Web. Gli analizzatori funzionano con i controller annotati con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, sulla base delle [convenzioni API](xref:web-api/advanced/conventions).

## <a name="package-installation"></a>Installazione del pacchetto

È possibile aggiungere `Microsoft.AspNetCore.Mvc.Api.Analyzers` con uno degli approcci seguenti:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dalla finestra **Console di Gestione pacchetti**:
  * Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**.
  * Passare alla directory che contiene il file *ApiConventions.csproj*.
  * Eseguire il seguente comando:

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* Dalla finestra di dialogo **Gestisci pacchetti NuGet**:
  * Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**.
  * Impostare **Origine pacchetto** su "nuget.org".
  * Immettere "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella casella di ricerca.
  * Selezionare il pacchetto "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella scheda **Sfoglia** e fare clic su **Installa**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione)  > **Aggiungi pacchetti**.
* Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org".
* Immettere "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella casella di ricerca.
* Selezionare il pacchetto "Microsoft.AspNetCore.Mvc.Api.Analyzers" nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente da **Terminale integrato**:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Eseguire il comando seguente:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>Analizzatori per le convenzioni API

I documenti OpenAPI contengono i codici di stato e i tipi di risposta che può restituire un'azione. In ASP.NET Core MVC, per documentare un'azione vengono usati attributi come <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> e <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>. <xref:tutorials/web-api-help-pages-using-swagger> illustra in dettaglio la documentazione dell'API.

Uno degli analizzatori del pacchetto verifica i controller annotati con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> e identifica le azioni che non documentano completamente le relative risposte. Si consideri l'esempio seguente:

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

L'azione precedente documenta il tipo restituito HTTP 200 (operazione riuscita) ma non documenta il codice di stato HTTP 404 (operazione non riuscita). L'analizzatore segnala l'assenza di documentazione per il codice di stato HTTP 404 come un avviso. È disponibile un'opzione per risolvere il problema.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* [Annotazione con l'attributo ApiController](xref:web-api/index#annotation-with-apicontroller-attribute)
