---
title: Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP
author: scottaddie
description: Informazioni su come usare lo strumento globale REPL HTTP .NET Core per esplorare e testare un'API Web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2019
uid: web-api/http-repl
ms.openlocfilehash: 1ceda6182c62bb1be06cd95f14e6a46a1809253e
ms.sourcegitcommit: 059ab380744fa3be3b69aa90d431b563c57092cf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/23/2019
ms.locfileid: "68410883"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="42975-103">Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP</span><span class="sxs-lookup"><span data-stu-id="42975-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="42975-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="42975-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="42975-105">Il ciclo Read-Eval-Print (REPL) HTTP:</span><span class="sxs-lookup"><span data-stu-id="42975-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="42975-106">È un strumento da riga di comando multipiattaforma leggero supportato ovunque sia supportato .NET Core.</span><span class="sxs-lookup"><span data-stu-id="42975-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="42975-107">Consente di eseguire richieste HTTP per testare le API Web ASP.NET Core e visualizzare i relativi risultati.</span><span class="sxs-lookup"><span data-stu-id="42975-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="42975-108">Sono supportati i [verbi HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) seguenti:</span><span class="sxs-lookup"><span data-stu-id="42975-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="42975-109">DELETE</span><span class="sxs-lookup"><span data-stu-id="42975-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="42975-110">GET</span><span class="sxs-lookup"><span data-stu-id="42975-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="42975-111">HEAD</span><span class="sxs-lookup"><span data-stu-id="42975-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="42975-112">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="42975-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="42975-113">PATCH</span><span class="sxs-lookup"><span data-stu-id="42975-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="42975-114">POST</span><span class="sxs-lookup"><span data-stu-id="42975-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="42975-115">PUT</span><span class="sxs-lookup"><span data-stu-id="42975-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="42975-116">Per continuare, [visualizzare o scaricare l'API Web ASP.NET Core di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([come scaricare](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="42975-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42975-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="42975-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="42975-118">Installazione</span><span class="sxs-lookup"><span data-stu-id="42975-118">Installation</span></span>

<span data-ttu-id="42975-119">Per installare il ciclo Read-Eval-Print HTTP, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version 3.0.0-*
```

<span data-ttu-id="42975-120">Viene installato uno [strumento globale .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) dal pacchetto NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="42975-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="42975-121">Utilizzo</span><span class="sxs-lookup"><span data-stu-id="42975-121">Usage</span></span>

<span data-ttu-id="42975-122">Dopo aver completato l'installazione dello strumento, eseguire il comando seguente per avviare il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="42975-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="42975-123">Per visualizzare i comandi del ciclo Read-Eval-Print HTTP disponibili, eseguire uno dei comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="42975-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="42975-124">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-124">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Sets the swagger document to use for information about the current server
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="42975-125">Il ciclo Read-Eval-Print HTTP offre il completamento del comando.</span><span class="sxs-lookup"><span data-stu-id="42975-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="42975-126">Premendo il tasto <kbd>TAB</kbd> è possibile scorrere l'elenco dei comandi che completano i caratteri o l'endpoint API digitato.</span><span class="sxs-lookup"><span data-stu-id="42975-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="42975-127">Le sezioni seguenti descrivono i comandi dell'interfaccia della riga di comando disponibili.</span><span class="sxs-lookup"><span data-stu-id="42975-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="42975-128">Connettersi all'API Web</span><span class="sxs-lookup"><span data-stu-id="42975-128">Connect to the web API</span></span>

<span data-ttu-id="42975-129">Connettersi all'API Web eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="42975-130">`<BASE URI>` è l'URI di base dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="42975-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="42975-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="42975-132">In alternativa, eseguire il comando seguente in qualsiasi momento mentre è in esecuzione il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="42975-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="42975-133">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="42975-134">Puntare al documento di spavalderia Swagger dell'API Web</span><span class="sxs-lookup"><span data-stu-id="42975-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="42975-135">Per esaminare correttamente l'API Web, impostare l'URI relativo sul documento Swagger dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="42975-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="42975-136">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="42975-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="42975-138">Esplorare l'API Web</span><span class="sxs-lookup"><span data-stu-id="42975-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="42975-139">Visualizzare gli endpoint disponibili</span><span class="sxs-lookup"><span data-stu-id="42975-139">View available endpoints</span></span>

<span data-ttu-id="42975-140">Per visualizzare l'elenco dei diversi endpoint (controller) nel percorso corrente dell'indirizzo dell'API Web, eseguire il comando `ls` o `dir`:</span><span class="sxs-lookup"><span data-stu-id="42975-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="42975-141">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="42975-142">L'output precedente indica che sono disponibili due controller: `Fruits` e `People`.</span><span class="sxs-lookup"><span data-stu-id="42975-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="42975-143">Entrambi i controller supportano operazioni HTTP GET e POST senza parametri.</span><span class="sxs-lookup"><span data-stu-id="42975-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="42975-144">L'esplorazione di un controller specifico offre altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="42975-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="42975-145">Ad esempio, l'output del comando seguente mostra che il controller `Fruits` supporta anche le operazioni HTTP GET, PUT e DELETE.</span><span class="sxs-lookup"><span data-stu-id="42975-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="42975-146">Ognuna di queste operazioni prevede un parametro `id` nella route:</span><span class="sxs-lookup"><span data-stu-id="42975-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="42975-147">In alternativa, eseguire il comando `ui` per aprire la pagina dell'interfaccia utente di Swagger dell'API Web in un browser.</span><span class="sxs-lookup"><span data-stu-id="42975-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="42975-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="42975-149">Passare a un endpoint</span><span class="sxs-lookup"><span data-stu-id="42975-149">Navigate to an endpoint</span></span>

<span data-ttu-id="42975-150">Per passare a un endpoint diverso nell'API Web, eseguire il comando `cd`:</span><span class="sxs-lookup"><span data-stu-id="42975-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="42975-151">Nel percorso che segue il comando `cd` non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="42975-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="42975-152">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="42975-153">Personalizzare il ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="42975-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="42975-154">I [colori](#set-color-preferences) predefiniti del ciclo Read-Eval-Print HTTP possono essere personalizzati.</span><span class="sxs-lookup"><span data-stu-id="42975-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="42975-155">È anche possibile definire un [editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="42975-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="42975-156">Le preferenze del ciclo Read-Eval-Print HTTP vengono mantenute nella sessione corrente e nelle sessioni future.</span><span class="sxs-lookup"><span data-stu-id="42975-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="42975-157">Dopo essere state modificate, le preferenze vengono archiviate nel file seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="42975-158">Linux</span><span class="sxs-lookup"><span data-stu-id="42975-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="42975-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="42975-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="42975-160">macOS</span><span class="sxs-lookup"><span data-stu-id="42975-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="42975-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="42975-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="42975-162">Windows</span><span class="sxs-lookup"><span data-stu-id="42975-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="42975-163">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="42975-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="42975-164">Il file con estensione *httpreplprefs* viene caricato all'avvio e non viene monitorato per le modifiche in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="42975-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="42975-165">Le modifiche manuali apportate al file diventano effettive solo dopo il riavvio dello strumento.</span><span class="sxs-lookup"><span data-stu-id="42975-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="42975-166">Visualizzare le impostazioni</span><span class="sxs-lookup"><span data-stu-id="42975-166">View the settings</span></span>

<span data-ttu-id="42975-167">Per visualizzare le impostazioni disponibili, eseguire il comando `pref get`.</span><span class="sxs-lookup"><span data-stu-id="42975-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="42975-168">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="42975-169">Il comando precedente visualizza le coppie chiave-valore disponibili:</span><span class="sxs-lookup"><span data-stu-id="42975-169">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="42975-170">Impostare le preferenze colori</span><span class="sxs-lookup"><span data-stu-id="42975-170">Set color preferences</span></span>

<span data-ttu-id="42975-171">La colorazione delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="42975-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="42975-172">Per personalizzare la colorazione predefinita dello strumento del ciclo Read-Eval-Print, individuare la chiave corrispondente al colore da modificare.</span><span class="sxs-lookup"><span data-stu-id="42975-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="42975-173">Per istruzioni su come trovare le chiavi, vedere la sezione [Visualizzare le impostazioni](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="42975-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="42975-174">Ad esempio, modificare il valore della chiave `colors.json` da `Green` a `White` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="42975-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="42975-175">È possibile usare solo i [colori consentiti](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="42975-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="42975-176">Output delle richieste HTTP successive con la nuova colorazione.</span><span class="sxs-lookup"><span data-stu-id="42975-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="42975-177">Quando non sono impostate chiavi di colore specifiche, vengono considerate chiavi più generiche.</span><span class="sxs-lookup"><span data-stu-id="42975-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="42975-178">Per comprendere questo comportamento di fallback, si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="42975-179">Se `colors.json.name` non ha un valore, viene usato `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="42975-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="42975-180">Se `colors.json.string` non ha un valore, viene usato `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="42975-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="42975-181">Se `colors.json.literal` non ha un valore, viene usato `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="42975-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="42975-182">Se `colors.json` non ha un valore, viene usato il colore del testo predefinito della shell dei comandi (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="42975-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="42975-183">Impostare la dimensione del rientro</span><span class="sxs-lookup"><span data-stu-id="42975-183">Set indentation size</span></span>

<span data-ttu-id="42975-184">La personalizzazione della dimensione del rientro delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="42975-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="42975-185">La dimensione del rientro predefinita è di due spazi.</span><span class="sxs-lookup"><span data-stu-id="42975-185">The default size is two spaces.</span></span> <span data-ttu-id="42975-186">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-186">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="42975-187">Per modificare la dimensione predefinita, impostare la chiave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="42975-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="42975-188">Ad esempio, per usare sempre quattro spazi:</span><span class="sxs-lookup"><span data-stu-id="42975-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="42975-189">L'impostazione dei quattro spazi viene applicata alle risposte successive:</span><span class="sxs-lookup"><span data-stu-id="42975-189">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-indentation-size"></a><span data-ttu-id="42975-190">Impostare la dimensione del rientro</span><span class="sxs-lookup"><span data-stu-id="42975-190">Set indentation size</span></span>

<span data-ttu-id="42975-191">La personalizzazione della dimensione del rientro delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="42975-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="42975-192">La dimensione del rientro predefinita è di due spazi.</span><span class="sxs-lookup"><span data-stu-id="42975-192">The default size is two spaces.</span></span> <span data-ttu-id="42975-193">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-193">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="42975-194">Per modificare la dimensione predefinita, impostare la chiave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="42975-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="42975-195">Ad esempio, per usare sempre quattro spazi:</span><span class="sxs-lookup"><span data-stu-id="42975-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="42975-196">L'impostazione dei quattro spazi viene applicata alle risposte successive:</span><span class="sxs-lookup"><span data-stu-id="42975-196">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="42975-197">Impostare l'editor di testo predefinito</span><span class="sxs-lookup"><span data-stu-id="42975-197">Set the default text editor</span></span>

<span data-ttu-id="42975-198">Per impostazione predefinita, il ciclo Read-Eval-Print HTTP non ha un editor di testo configurato per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="42975-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="42975-199">Per testare i metodi dell'API Web che richiedono un corpo della richiesta HTTP, è necessario impostare un editor di testo predefinito.</span><span class="sxs-lookup"><span data-stu-id="42975-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="42975-200">Lo strumento del ciclo Read-Eval-Print HTTP avvia l'editor di testo configurato al solo scopo di comporre il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="42975-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="42975-201">Eseguire il comando seguente per impostare l'editor di testo preferito come predefinito:</span><span class="sxs-lookup"><span data-stu-id="42975-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="42975-202">Nel comando precedente `<EXECUTABLE>` è il percorso completo del file eseguibile dell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="42975-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="42975-203">Ad esempio, eseguire il comando seguente per impostare Visual Studio Code come editor di testo predefinito:</span><span class="sxs-lookup"><span data-stu-id="42975-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="42975-204">Linux</span><span class="sxs-lookup"><span data-stu-id="42975-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="42975-205">macOS</span><span class="sxs-lookup"><span data-stu-id="42975-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="42975-206">Windows</span><span class="sxs-lookup"><span data-stu-id="42975-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="42975-207">Per avviare l'editor di testo predefinito con argomenti dell'interfaccia della riga di comando specifici, impostare la chiave `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="42975-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="42975-208">Si supponga, ad esempio, che Visual Studio Code sia l'editor di testo predefinito e che si voglia che il ciclo Read-Eval-Print apra sempre Visual Studio Code in una nuova sessione con le estensioni disabilitate.</span><span class="sxs-lookup"><span data-stu-id="42975-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="42975-209">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="42975-210">Testare le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="42975-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-211">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-212">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-213">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-214">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-214">Options</span></span>

<span data-ttu-id="42975-215">Per il comando `get` sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="42975-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="42975-216">Esempio</span><span class="sxs-lookup"><span data-stu-id="42975-216">Example</span></span>

<span data-ttu-id="42975-217">Per inviare una richiesta HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="42975-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="42975-218">Eseguire il comando `get` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="42975-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="42975-219">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-219">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="42975-220">Recuperare un record specifico passando un parametro al comando `get`:</span><span class="sxs-lookup"><span data-stu-id="42975-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="42975-221">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-221">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="42975-222">Testare le richieste HTTP POST</span><span class="sxs-lookup"><span data-stu-id="42975-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-223">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-224">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-225">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-226">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="42975-227">Esempio</span><span class="sxs-lookup"><span data-stu-id="42975-227">Example</span></span>

<span data-ttu-id="42975-228">Per inviare una richiesta HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="42975-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="42975-229">Eseguire il comando `post` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="42975-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="42975-230">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="42975-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="42975-231">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42975-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="42975-232">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="42975-233">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="42975-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="42975-234">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="42975-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="42975-235">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="42975-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="42975-236">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-236">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="42975-237">Testare le richieste HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="42975-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-238">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-239">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-240">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-241">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="42975-242">Esempio</span><span class="sxs-lookup"><span data-stu-id="42975-242">Example</span></span>

<span data-ttu-id="42975-243">Per inviare una richiesta HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="42975-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="42975-244">*Facoltativa*: Eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="42975-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="42975-245">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="42975-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="42975-246">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42975-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="42975-247">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="42975-248">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="42975-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="42975-249">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="42975-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="42975-250">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="42975-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="42975-251">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="42975-252">*Facoltativa*: Eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="42975-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="42975-253">Ad esempio, se si digita "Cherry" nell'editor di testo, `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="42975-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="42975-254">Testare le richieste HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="42975-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-255">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-256">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-257">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-258">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="42975-259">Esempio</span><span class="sxs-lookup"><span data-stu-id="42975-259">Example</span></span>

<span data-ttu-id="42975-260">Per inviare una richiesta HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="42975-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="42975-261">*Facoltativa*: Eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="42975-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="42975-262">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="42975-263">*Facoltativa*: Eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="42975-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="42975-264">In questo esempio `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="42975-264">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="42975-265">Testare le richieste HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="42975-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-266">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-267">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-268">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-269">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="42975-270">Testare le richieste HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="42975-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-271">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-272">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-273">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-274">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="42975-275">Testare le richieste HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="42975-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="42975-276">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="42975-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="42975-277">Argomenti</span><span class="sxs-lookup"><span data-stu-id="42975-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="42975-278">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="42975-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="42975-279">Opzioni</span><span class="sxs-lookup"><span data-stu-id="42975-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="42975-280">Impostare le intestazioni delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="42975-280">Set HTTP request headers</span></span>

<span data-ttu-id="42975-281">Per impostare l'intestazione di una richiesta HTTP, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="42975-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="42975-282">Impostare inline con la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42975-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="42975-283">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="42975-284">Con l'approccio precedente, ogni singola intestazione di richiesta HTTP richiede una propria opzione `-h`.</span><span class="sxs-lookup"><span data-stu-id="42975-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="42975-285">Impostare prima di inviare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="42975-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="42975-286">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="42975-287">Quando si imposta l'intestazione prima di inviare una richiesta, l'intestazione rimane impostata per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="42975-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="42975-288">Per cancellare l'intestazione, specificare un valore vuoto.</span><span class="sxs-lookup"><span data-stu-id="42975-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="42975-289">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="42975-290">Attivare o disattivare la visualizzazione delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="42975-290">Toggle HTTP request display</span></span>

<span data-ttu-id="42975-291">Per impostazione predefinita, la visualizzazione della richiesta HTTP inviata viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="42975-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="42975-292">È possibile modificare l'impostazione corrispondente per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="42975-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="42975-293">Abilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="42975-293">Enable request display</span></span>

<span data-ttu-id="42975-294">Visualizzare la richiesta HTTP inviata eseguendo il comando `echo on`.</span><span class="sxs-lookup"><span data-stu-id="42975-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="42975-295">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="42975-296">Le richieste HTTP successive della sessione corrente visualizzano le intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="42975-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="42975-297">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-297">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="42975-298">Disabilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="42975-298">Disable request display</span></span>

<span data-ttu-id="42975-299">Eliminare la visualizzazione della richiesta HTTP inviata eseguendo il comando `echo off`.</span><span class="sxs-lookup"><span data-stu-id="42975-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="42975-300">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="42975-301">Eseguire uno script</span><span class="sxs-lookup"><span data-stu-id="42975-301">Run a script</span></span>

<span data-ttu-id="42975-302">Se si esegue spesso lo stesso set di comandi del ciclo Read-Eval-Print HTTP, può essere utile archiviarlo in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="42975-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="42975-303">I comandi nel file hanno lo stesso formato di quelli eseguiti manualmente nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="42975-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="42975-304">I comandi possono essere eseguiti in modalità batch usando il comando `run`.</span><span class="sxs-lookup"><span data-stu-id="42975-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="42975-305">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-305">For example:</span></span>

1. <span data-ttu-id="42975-306">Creare un file di testo contenente un set di comandi delimitati da una nuova riga.</span><span class="sxs-lookup"><span data-stu-id="42975-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="42975-307">Si consideri ad esempio un file *people-script.txt* contenente i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="42975-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="42975-308">Eseguire il comando `run` passando il percorso del file di testo.</span><span class="sxs-lookup"><span data-stu-id="42975-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="42975-309">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="42975-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="42975-310">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-310">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="42975-311">Cancellare l'output</span><span class="sxs-lookup"><span data-stu-id="42975-311">Clear the output</span></span>

<span data-ttu-id="42975-312">Per rimuovere l'intero output scritto nella shell dei comandi dallo strumento del ciclo Read-Eval-Print HTTP, eseguire il comando `clear` o `cls`.</span><span class="sxs-lookup"><span data-stu-id="42975-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="42975-313">Si supponga ad esempio che la shell dei comandi includa l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-313">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="42975-314">Eseguire il comando seguente per cancellare l'output:</span><span class="sxs-lookup"><span data-stu-id="42975-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="42975-315">Dopo aver eseguito il comando precedente, la shell dei comandi contiene solo l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="42975-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="42975-316">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42975-316">Additional resources</span></span>

* [<span data-ttu-id="42975-317">Richieste API REST</span><span class="sxs-lookup"><span data-stu-id="42975-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="42975-318">Repository GitHub del ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="42975-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
