---
title: Usare gli analizzatori dell'API Web
author: pranavkm
description: Informazioni sugli analizzatori dell'API Web in Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 2aaef738ab2a64f85cb85708f63d2375c04cacb5
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538558"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="e9969-103">Usare gli analizzatori dell'API Web</span><span class="sxs-lookup"><span data-stu-id="e9969-103">Use web API analyzers</span></span>

<span data-ttu-id="e9969-104">ASP.NET Core 2.2 e versioni successive include il pacchetto NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers), che contiene analizzatori per le API Web.</span><span class="sxs-lookup"><span data-stu-id="e9969-104">ASP.NET Core 2.2 and later includes the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="e9969-105">Gli analizzatori funzionano con i controller annotati con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, sulla base delle [convenzioni API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="e9969-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="e9969-106">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="e9969-106">Package installation</span></span>

<span data-ttu-id="e9969-107">È possibile aggiungere `Microsoft.AspNetCore.Mvc.Api.Analyzers` con uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="e9969-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e9969-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9969-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e9969-109">Dalla finestra **Console di Gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="e9969-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="e9969-110">Passare a **Vista** > **Altre finestre** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="e9969-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="e9969-111">Passare alla directory che contiene il file *ApiConventions.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e9969-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="e9969-112">Eseguire il seguente comando:</span><span class="sxs-lookup"><span data-stu-id="e9969-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="e9969-113">Dalla finestra di dialogo **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="e9969-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="e9969-114">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e9969-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="e9969-115">Impostare **Origine pacchetto** su "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="e9969-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="e9969-116">Immettere "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="e9969-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="e9969-117">Selezionare il pacchetto "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella scheda **Sfoglia** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="e9969-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e9969-118">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e9969-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e9969-119">Fare clic con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione)  > **Aggiungi pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="e9969-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="e9969-120">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="e9969-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="e9969-121">Immettere "Microsoft.AspNetCore.Mvc.Api.Analyzers" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="e9969-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="e9969-122">Selezionare il pacchetto "Microsoft.AspNetCore.Mvc.Api.Analyzers" nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="e9969-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e9969-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e9969-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e9969-124">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="e9969-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e9969-125">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="e9969-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e9969-126">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e9969-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="e9969-127">Analizzatori per le convenzioni API</span><span class="sxs-lookup"><span data-stu-id="e9969-127">Analyzers for API conventions</span></span>

<span data-ttu-id="e9969-128">I documenti OpenAPI contengono i codici di stato e i tipi di risposta che può restituire un'azione.</span><span class="sxs-lookup"><span data-stu-id="e9969-128">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="e9969-129">In ASP.NET Core MVC, per documentare un'azione vengono usati attributi come <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> e <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>.</span><span class="sxs-lookup"><span data-stu-id="e9969-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="e9969-130"><xref:tutorials/web-api-help-pages-using-swagger> illustra in dettaglio la documentazione dell'API.</span><span class="sxs-lookup"><span data-stu-id="e9969-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="e9969-131">Uno degli analizzatori del pacchetto verifica i controller annotati con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> e identifica le azioni che non documentano completamente le relative risposte.</span><span class="sxs-lookup"><span data-stu-id="e9969-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="e9969-132">Si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e9969-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="e9969-133">L'azione precedente documenta il tipo restituito HTTP 200 (operazione riuscita) ma non documenta il codice di stato HTTP 404 (operazione non riuscita).</span><span class="sxs-lookup"><span data-stu-id="e9969-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="e9969-134">L'analizzatore segnala l'assenza di documentazione per il codice di stato HTTP 404 come un avviso.</span><span class="sxs-lookup"><span data-stu-id="e9969-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="e9969-135">È disponibile un'opzione per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="e9969-135">An option to fix the problem is provided.</span></span>

![Analizzatore che restituisce un avviso](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a><span data-ttu-id="e9969-137">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e9969-137">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
