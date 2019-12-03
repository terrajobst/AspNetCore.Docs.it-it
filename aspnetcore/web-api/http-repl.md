---
title: Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP
author: scottaddie
description: Informazioni su come usare lo strumento globale REPL HTTP .NET Core per esplorare e testare un'API Web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/02/2019
uid: web-api/http-repl
ms.openlocfilehash: c6e3ab5685b5bd0b154d20585fb0d187f81da641
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717165"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="2990d-103">Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP</span><span class="sxs-lookup"><span data-stu-id="2990d-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="2990d-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2990d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2990d-105">Il ciclo Read-Eval-Print (REPL) HTTP:</span><span class="sxs-lookup"><span data-stu-id="2990d-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="2990d-106">È un strumento da riga di comando multipiattaforma leggero supportato ovunque sia supportato .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2990d-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="2990d-107">Consente di eseguire richieste HTTP per testare le API Web ASP.NET Core (e le API Web non ASP.NET Core) e visualizzare i relativi risultati.</span><span class="sxs-lookup"><span data-stu-id="2990d-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="2990d-108">Riesce a testare le API Web ospitate in qualsiasi ambiente, inclusi localhost e Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="2990d-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="2990d-109">Sono supportati i [verbi HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="2990d-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="2990d-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="2990d-111">Ottieni</span><span class="sxs-lookup"><span data-stu-id="2990d-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="2990d-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="2990d-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="2990d-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="2990d-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="2990d-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="2990d-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="2990d-115">Inserisci</span><span class="sxs-lookup"><span data-stu-id="2990d-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="2990d-116">METTERE</span><span class="sxs-lookup"><span data-stu-id="2990d-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="2990d-117">Per continuare, [visualizzare o scaricare l'API Web ASP.NET Core di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([come scaricare](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2990d-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2990d-118">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="2990d-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="2990d-119">Installazione di</span><span class="sxs-lookup"><span data-stu-id="2990d-119">Installation</span></span>

<span data-ttu-id="2990d-120">Per installare il ciclo Read-Eval-Print HTTP, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-120">To install the HTTP REPL, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-httprepl
```

<span data-ttu-id="2990d-121">Viene installato uno [strumento globale .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) dal pacchetto NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="2990d-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="2990d-122">Usage</span><span class="sxs-lookup"><span data-stu-id="2990d-122">Usage</span></span>

<span data-ttu-id="2990d-123">Dopo aver completato l'installazione dello strumento, eseguire il comando seguente per avviare il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="2990d-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="2990d-124">Per visualizzare i comandi del ciclo Read-Eval-Print HTTP disponibili, eseguire uno dei comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="2990d-125">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-125">The following output is displayed:</span></span>

```console
Usage:
  httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
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

<span data-ttu-id="2990d-126">Il ciclo Read-Eval-Print HTTP offre il completamento del comando.</span><span class="sxs-lookup"><span data-stu-id="2990d-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="2990d-127">Premendo il tasto <kbd>TAB</kbd> è possibile scorrere l'elenco dei comandi che completano i caratteri o l'endpoint API digitato.</span><span class="sxs-lookup"><span data-stu-id="2990d-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="2990d-128">Le sezioni seguenti descrivono i comandi dell'interfaccia della riga di comando disponibili.</span><span class="sxs-lookup"><span data-stu-id="2990d-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="2990d-129">Connettersi all'API Web</span><span class="sxs-lookup"><span data-stu-id="2990d-129">Connect to the web API</span></span>

<span data-ttu-id="2990d-130">Connettersi all'API Web eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="2990d-131">`<ROOT URI>` è l'URI di base dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="2990d-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="2990d-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="2990d-133">In alternativa, eseguire il comando seguente in qualsiasi momento mentre è in esecuzione il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="2990d-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="2990d-134">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="2990d-135">Puntare manualmente al documento di Swagger per l'API Web</span><span class="sxs-lookup"><span data-stu-id="2990d-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="2990d-136">Il comando connect precedente tenterà di individuare automaticamente il documento di Swagger.</span><span class="sxs-lookup"><span data-stu-id="2990d-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="2990d-137">Se per qualche motivo l'individuazione non riesce, è possibile specificare l'URI del documento di Swagger per l'API Web usando l'opzione `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="2990d-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="2990d-138">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="2990d-139">Esplorare l'API Web</span><span class="sxs-lookup"><span data-stu-id="2990d-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="2990d-140">Visualizzare gli endpoint disponibili</span><span class="sxs-lookup"><span data-stu-id="2990d-140">View available endpoints</span></span>

<span data-ttu-id="2990d-141">Per visualizzare l'elenco dei diversi endpoint (controller) nel percorso corrente dell'indirizzo dell'API Web, eseguire il comando `ls` o `dir`:</span><span class="sxs-lookup"><span data-stu-id="2990d-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="2990d-142">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2990d-143">L'output precedente indica che sono disponibili due controller: `Fruits` e `People`.</span><span class="sxs-lookup"><span data-stu-id="2990d-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="2990d-144">Entrambi i controller supportano operazioni HTTP GET e POST senza parametri.</span><span class="sxs-lookup"><span data-stu-id="2990d-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="2990d-145">L'esplorazione di un controller specifico offre altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="2990d-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="2990d-146">Ad esempio, l'output del comando seguente mostra che il controller `Fruits` supporta anche le operazioni HTTP GET, PUT e DELETE.</span><span class="sxs-lookup"><span data-stu-id="2990d-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="2990d-147">Ognuna di queste operazioni prevede un parametro `id` nella route:</span><span class="sxs-lookup"><span data-stu-id="2990d-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="2990d-148">In alternativa, eseguire il comando `ui` per aprire la pagina dell'interfaccia utente di Swagger dell'API Web in un browser.</span><span class="sxs-lookup"><span data-stu-id="2990d-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="2990d-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="2990d-150">Passare a un endpoint</span><span class="sxs-lookup"><span data-stu-id="2990d-150">Navigate to an endpoint</span></span>

<span data-ttu-id="2990d-151">Per passare a un endpoint diverso nell'API Web, eseguire il comando `cd`:</span><span class="sxs-lookup"><span data-stu-id="2990d-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="2990d-152">Nel percorso che segue il comando `cd` non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="2990d-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="2990d-153">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="2990d-154">Personalizzare il ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="2990d-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="2990d-155">I [colori](#set-color-preferences) predefiniti del ciclo Read-Eval-Print HTTP possono essere personalizzati.</span><span class="sxs-lookup"><span data-stu-id="2990d-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="2990d-156">È anche possibile definire un [editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2990d-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="2990d-157">Le preferenze del ciclo Read-Eval-Print HTTP vengono mantenute nella sessione corrente e nelle sessioni future.</span><span class="sxs-lookup"><span data-stu-id="2990d-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="2990d-158">Dopo essere state modificate, le preferenze vengono archiviate nel file seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2990d-159">Linux</span><span class="sxs-lookup"><span data-stu-id="2990d-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="2990d-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2990d-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2990d-161">macOS</span><span class="sxs-lookup"><span data-stu-id="2990d-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="2990d-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2990d-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2990d-163">Windows</span><span class="sxs-lookup"><span data-stu-id="2990d-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="2990d-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="2990d-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="2990d-165">Il file con estensione *httpreplprefs* viene caricato all'avvio e non viene monitorato per le modifiche in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2990d-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="2990d-166">Le modifiche manuali apportate al file diventano effettive solo dopo il riavvio dello strumento.</span><span class="sxs-lookup"><span data-stu-id="2990d-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="2990d-167">Visualizzare le impostazioni</span><span class="sxs-lookup"><span data-stu-id="2990d-167">View the settings</span></span>

<span data-ttu-id="2990d-168">Per visualizzare le impostazioni disponibili, eseguire il comando `pref get`.</span><span class="sxs-lookup"><span data-stu-id="2990d-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="2990d-169">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="2990d-170">Il comando precedente visualizza le coppie chiave-valore disponibili:</span><span class="sxs-lookup"><span data-stu-id="2990d-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="2990d-171">Impostare le preferenze colori</span><span class="sxs-lookup"><span data-stu-id="2990d-171">Set color preferences</span></span>

<span data-ttu-id="2990d-172">La colorazione delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="2990d-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="2990d-173">Per personalizzare la colorazione predefinita dello strumento del ciclo Read-Eval-Print, individuare la chiave corrispondente al colore da modificare.</span><span class="sxs-lookup"><span data-stu-id="2990d-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="2990d-174">Per istruzioni su come trovare le chiavi, vedere la sezione [Visualizzare le impostazioni](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="2990d-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="2990d-175">Ad esempio, modificare il valore della chiave `colors.json` da `Green` a `White` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="2990d-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="2990d-176">È possibile usare solo i [colori consentiti](https://github.com/dotnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="2990d-176">Only the [allowed colors](https://github.com/dotnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="2990d-177">Output delle richieste HTTP successive con la nuova colorazione.</span><span class="sxs-lookup"><span data-stu-id="2990d-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="2990d-178">Quando non sono impostate chiavi di colore specifiche, vengono considerate chiavi più generiche.</span><span class="sxs-lookup"><span data-stu-id="2990d-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="2990d-179">Per comprendere questo comportamento di fallback, si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="2990d-180">Se `colors.json.name` non ha un valore, viene usato `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="2990d-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="2990d-181">Se `colors.json.string` non ha un valore, viene usato `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="2990d-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="2990d-182">Se `colors.json.literal` non ha un valore, viene usato `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="2990d-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="2990d-183">Se `colors.json` non ha un valore, viene usato il colore del testo predefinito della shell dei comandi (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="2990d-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="2990d-184">Impostare la dimensione del rientro</span><span class="sxs-lookup"><span data-stu-id="2990d-184">Set indentation size</span></span>

<span data-ttu-id="2990d-185">La personalizzazione della dimensione del rientro delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="2990d-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="2990d-186">La dimensione del rientro predefinita è di due spazi.</span><span class="sxs-lookup"><span data-stu-id="2990d-186">The default size is two spaces.</span></span> <span data-ttu-id="2990d-187">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-187">For example:</span></span>

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

<span data-ttu-id="2990d-188">Per modificare la dimensione predefinita, impostare la chiave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="2990d-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="2990d-189">Ad esempio, per usare sempre quattro spazi:</span><span class="sxs-lookup"><span data-stu-id="2990d-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="2990d-190">L'impostazione dei quattro spazi viene applicata alle risposte successive:</span><span class="sxs-lookup"><span data-stu-id="2990d-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="2990d-191">Impostare l'editor di testo predefinito</span><span class="sxs-lookup"><span data-stu-id="2990d-191">Set the default text editor</span></span>

<span data-ttu-id="2990d-192">Per impostazione predefinita, il ciclo Read-Eval-Print HTTP non ha un editor di testo configurato per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="2990d-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="2990d-193">Per testare i metodi dell'API Web che richiedono un corpo della richiesta HTTP, è necessario impostare un editor di testo predefinito.</span><span class="sxs-lookup"><span data-stu-id="2990d-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="2990d-194">Lo strumento del ciclo Read-Eval-Print HTTP avvia l'editor di testo configurato al solo scopo di comporre il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="2990d-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="2990d-195">Eseguire il comando seguente per impostare l'editor di testo preferito come predefinito:</span><span class="sxs-lookup"><span data-stu-id="2990d-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="2990d-196">Nel comando precedente `<EXECUTABLE>` è il percorso completo del file eseguibile dell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="2990d-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="2990d-197">Ad esempio, eseguire il comando seguente per impostare Visual Studio Code come editor di testo predefinito:</span><span class="sxs-lookup"><span data-stu-id="2990d-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2990d-198">Linux</span><span class="sxs-lookup"><span data-stu-id="2990d-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2990d-199">macOS</span><span class="sxs-lookup"><span data-stu-id="2990d-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="2990d-200">Windows</span><span class="sxs-lookup"><span data-stu-id="2990d-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="2990d-201">Per avviare l'editor di testo predefinito con argomenti dell'interfaccia della riga di comando specifici, impostare la chiave `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="2990d-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="2990d-202">Si supponga, ad esempio, che Visual Studio Code sia l'editor di testo predefinito e che si voglia che il ciclo Read-Eval-Print apra sempre Visual Studio Code in una nuova sessione con le estensioni disabilitate.</span><span class="sxs-lookup"><span data-stu-id="2990d-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="2990d-203">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="2990d-204">Impostare i percorsi di ricerca di Swagger</span><span class="sxs-lookup"><span data-stu-id="2990d-204">Set the Swagger search paths</span></span>

<span data-ttu-id="2990d-205">Per impostazione predefinita, HTTP REPL ha un set di percorsi relativi che usa per trovare il documento di Swagger quando si esegue il comando `connect` senza l'opzione `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="2990d-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="2990d-206">Questi percorsi relativi vengono combinati con i percorsi radice e di base specificati nel comando `connect`.</span><span class="sxs-lookup"><span data-stu-id="2990d-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="2990d-207">I percorsi relativi predefiniti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-207">The default relative paths are:</span></span>

- <span data-ttu-id="2990d-208">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2990d-208">*swagger.json*</span></span>
- <span data-ttu-id="2990d-209">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2990d-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="2990d-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2990d-210">*/swagger.json*</span></span>
- <span data-ttu-id="2990d-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="2990d-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="2990d-212">Per usare un set di percorsi di ricerca diverso nell'ambiente in uso, impostare la preferenza `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="2990d-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="2990d-213">Il valore deve essere un elenco di percorsi relativi delimitati da pipe.</span><span class="sxs-lookup"><span data-stu-id="2990d-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="2990d-214">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="2990d-215">Testare le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="2990d-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-216">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-217">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-218">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-219">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-219">Options</span></span>

<span data-ttu-id="2990d-220">Per il comando `get` sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2990d-221">Esempio</span><span class="sxs-lookup"><span data-stu-id="2990d-221">Example</span></span>

<span data-ttu-id="2990d-222">Per inviare una richiesta HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="2990d-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="2990d-223">Eseguire il comando `get` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="2990d-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="2990d-224">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-224">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="2990d-225">Recuperare un record specifico passando un parametro al comando `get`:</span><span class="sxs-lookup"><span data-stu-id="2990d-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="2990d-226">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-226">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="2990d-227">Testare le richieste HTTP POST</span><span class="sxs-lookup"><span data-stu-id="2990d-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-228">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-229">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-230">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-231">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2990d-232">Esempio</span><span class="sxs-lookup"><span data-stu-id="2990d-232">Example</span></span>

<span data-ttu-id="2990d-233">Per inviare una richiesta HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="2990d-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="2990d-234">Eseguire il comando `post` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="2990d-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="2990d-235">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="2990d-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2990d-236">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2990d-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2990d-237">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2990d-238">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2990d-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2990d-239">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="2990d-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="2990d-240">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="2990d-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2990d-241">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-241">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="2990d-242">Testare le richieste HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="2990d-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-243">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-244">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-245">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-246">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="2990d-247">Esempio</span><span class="sxs-lookup"><span data-stu-id="2990d-247">Example</span></span>

<span data-ttu-id="2990d-248">Per inviare una richiesta HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="2990d-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="2990d-249">*Facoltativo*: eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="2990d-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="2990d-250">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="2990d-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="2990d-251">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2990d-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="2990d-252">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="2990d-253">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="2990d-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="2990d-254">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="2990d-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="2990d-255">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="2990d-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="2990d-256">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2990d-257">*Facoltativo*: eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2990d-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2990d-258">Ad esempio, se si digita "Cherry" nell'editor di testo, `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2990d-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="2990d-259">Testare le richieste HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="2990d-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-260">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-261">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-262">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-263">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="2990d-264">Esempio</span><span class="sxs-lookup"><span data-stu-id="2990d-264">Example</span></span>

<span data-ttu-id="2990d-265">Per inviare una richiesta HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="2990d-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="2990d-266">*Facoltativo*: eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="2990d-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="2990d-267">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="2990d-268">*Facoltativo*: eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2990d-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="2990d-269">In questo esempio `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2990d-269">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="2990d-270">Testare le richieste HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="2990d-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-271">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-272">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-273">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-274">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="2990d-275">Testare le richieste HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="2990d-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-276">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-277">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-278">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-279">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="2990d-280">Testare le richieste HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="2990d-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="2990d-281">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2990d-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="2990d-282">Argomenti</span><span class="sxs-lookup"><span data-stu-id="2990d-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="2990d-283">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="2990d-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="2990d-284">Options</span><span class="sxs-lookup"><span data-stu-id="2990d-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="2990d-285">Impostare le intestazioni delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="2990d-285">Set HTTP request headers</span></span>

<span data-ttu-id="2990d-286">Per impostare l'intestazione di una richiesta HTTP, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-286">To set an HTTP request header, use one of the following approaches:</span></span>

* <span data-ttu-id="2990d-287">Impostare inline con la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2990d-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="2990d-288">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-288">For example:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```
    
    <span data-ttu-id="2990d-289">Con l'approccio precedente, ogni singola intestazione di richiesta HTTP richiede una propria opzione `-h`.</span><span class="sxs-lookup"><span data-stu-id="2990d-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

* <span data-ttu-id="2990d-290">Impostare prima di inviare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2990d-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="2990d-291">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-291">For example:</span></span>

    ```console
    https://localhost:5001/people~ set header Content-Type application/json
    ```
    
    <span data-ttu-id="2990d-292">Quando si imposta l'intestazione prima di inviare una richiesta, l'intestazione rimane impostata per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2990d-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="2990d-293">Per cancellare l'intestazione, specificare un valore vuoto.</span><span class="sxs-lookup"><span data-stu-id="2990d-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="2990d-294">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-294">For example:</span></span>
    
    ```console
    https://localhost:5001/people~ set header Content-Type
    ```

## <a name="test-secured-endpoints"></a><span data-ttu-id="2990d-295">Testare endpoint protetti</span><span class="sxs-lookup"><span data-stu-id="2990d-295">Test secured endpoints</span></span>

<span data-ttu-id="2990d-296">Il REPL HTTP supporta il test degli endpoint protetti tramite l'uso di intestazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2990d-296">The HTTP REPL supports the testing of secured endpoints through the use of HTTP request headers.</span></span> <span data-ttu-id="2990d-297">Esempi di schemi di autenticazione e autorizzazione supportati includono l'autenticazione di base, i token di porta JWT e l'autenticazione del digest.</span><span class="sxs-lookup"><span data-stu-id="2990d-297">Examples of supported authentication and authorization schemes include basic authentication, JWT bearer tokens, and digest authentication.</span></span> <span data-ttu-id="2990d-298">Ad esempio, è possibile inviare un bearer token a un endpoint con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-298">For example, you can send a bearer token to an endpoint with the following command:</span></span>

```console
set header Authorization "bearer <TOKEN VALUE>"
```

<span data-ttu-id="2990d-299">Per accedere a un endpoint ospitato da Azure o per usare l' [API REST di Azure](/rest/api/azure/), è necessario un Bearer token.</span><span class="sxs-lookup"><span data-stu-id="2990d-299">To access an Azure-hosted endpoint or to use the [Azure REST API](/rest/api/azure/), you need a bearer token.</span></span> <span data-ttu-id="2990d-300">Usare la procedura seguente per ottenere un bearer token per la sottoscrizione di Azure tramite l'interfaccia della riga di comando di [Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="2990d-300">Use the following steps to obtain a bearer token for your Azure subscription via the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="2990d-301">Il REPL HTTP imposta la bearer token in un'intestazione di richiesta HTTP e recupera un elenco di app Azure app Web del servizio.</span><span class="sxs-lookup"><span data-stu-id="2990d-301">The HTTP REPL sets the bearer token in an HTTP request header and retrieves a list of Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="2990d-302">Accedere ad Azure:</span><span class="sxs-lookup"><span data-stu-id="2990d-302">Log in to Azure:</span></span>

    ```azcli
    az login
    ```

1. <span data-ttu-id="2990d-303">Ottenere l'ID sottoscrizione con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-303">Get your subscription ID with the following command:</span></span>

    ```azcli
    az account show --query id
    ```

1. <span data-ttu-id="2990d-304">Copiare l'ID sottoscrizione ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-304">Copy your subscription ID and run the following command:</span></span>

    ```azcli
    az account set --subscription "<SUBSCRIPTION ID>"
    ```

1. <span data-ttu-id="2990d-305">Ottenere il bearer token con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-305">Get your bearer token with the following command:</span></span>

    ```azcli
    az account get-access-token --query accessToken
    ```

1. <span data-ttu-id="2990d-306">Connettersi all'API REST di Azure tramite il REPL HTTP:</span><span class="sxs-lookup"><span data-stu-id="2990d-306">Connect to the Azure REST API via the HTTP REPL:</span></span>

    ```console
    httprepl https://management.azure.com
    ```

1. <span data-ttu-id="2990d-307">Impostare l'intestazione della richiesta HTTP `Authorization`:</span><span class="sxs-lookup"><span data-stu-id="2990d-307">Set the `Authorization` HTTP request header:</span></span>

    ```console
    https://management.azure.com/> set header Authorization "bearer <ACCESS TOKEN>"
    ```

1. <span data-ttu-id="2990d-308">Passare alla sottoscrizione:</span><span class="sxs-lookup"><span data-stu-id="2990d-308">Navigate to the subscription:</span></span>

    ```console
    https://management.azure.com/> cd subscriptions/<SUBSCRIPTION ID>
    ```

1. <span data-ttu-id="2990d-309">Ottenere un elenco delle app Web del servizio app Azure della sottoscrizione:</span><span class="sxs-lookup"><span data-stu-id="2990d-309">Get a list of your subscription's Azure App Service Web Apps:</span></span>

    ```console
    https://management.azure.com/subscriptions/{SUBSCRIPTION ID}> get providers/Microsoft.Web/sites?api-version=2016-08-01
    ```

    <span data-ttu-id="2990d-310">Viene visualizzata la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-310">The following response is displayed:</span></span>

    ```console
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 35948
    Content-Type: application/json; charset=utf-8
    Date: Thu, 19 Sep 2019 23:04:03 GMT
    Expires: -1
    Pragma: no-cache
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    X-Content-Type-Options: nosniff
    x-ms-correlation-request-id: <em>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</em>
    x-ms-original-request-ids: <em>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx;xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</em>
    x-ms-ratelimit-remaining-subscription-reads: 11999
    x-ms-request-id: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    x-ms-routing-request-id: WESTUS:xxxxxxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx
    {
      "value": [
        <AZURE RESOURCES LIST>
      ]
    }
    ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="2990d-311">Attivare o disattivare la visualizzazione delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="2990d-311">Toggle HTTP request display</span></span>

<span data-ttu-id="2990d-312">Per impostazione predefinita, la visualizzazione della richiesta HTTP inviata viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="2990d-312">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="2990d-313">È possibile modificare l'impostazione corrispondente per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2990d-313">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="2990d-314">Abilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="2990d-314">Enable request display</span></span>

<span data-ttu-id="2990d-315">Visualizzare la richiesta HTTP inviata eseguendo il comando `echo on`.</span><span class="sxs-lookup"><span data-stu-id="2990d-315">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="2990d-316">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-316">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="2990d-317">Le richieste HTTP successive della sessione corrente visualizzano le intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="2990d-317">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="2990d-318">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-318">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="2990d-319">Disabilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="2990d-319">Disable request display</span></span>

<span data-ttu-id="2990d-320">Eliminare la visualizzazione della richiesta HTTP inviata eseguendo il comando `echo off`.</span><span class="sxs-lookup"><span data-stu-id="2990d-320">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="2990d-321">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-321">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="2990d-322">Eseguire uno script</span><span class="sxs-lookup"><span data-stu-id="2990d-322">Run a script</span></span>

<span data-ttu-id="2990d-323">Se si esegue spesso lo stesso set di comandi del ciclo Read-Eval-Print HTTP, può essere utile archiviarlo in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="2990d-323">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="2990d-324">I comandi nel file hanno lo stesso formato di quelli eseguiti manualmente nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2990d-324">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="2990d-325">I comandi possono essere eseguiti in modalità batch usando il comando `run`.</span><span class="sxs-lookup"><span data-stu-id="2990d-325">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="2990d-326">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-326">For example:</span></span>

1. <span data-ttu-id="2990d-327">Creare un file di testo contenente un set di comandi delimitati da una nuova riga.</span><span class="sxs-lookup"><span data-stu-id="2990d-327">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="2990d-328">Si consideri ad esempio un file *people-script.txt* contenente i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2990d-328">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="2990d-329">Eseguire il comando `run` passando il percorso del file di testo.</span><span class="sxs-lookup"><span data-stu-id="2990d-329">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="2990d-330">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2990d-330">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="2990d-331">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-331">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="2990d-332">Cancellare l'output</span><span class="sxs-lookup"><span data-stu-id="2990d-332">Clear the output</span></span>

<span data-ttu-id="2990d-333">Per rimuovere l'intero output scritto nella shell dei comandi dallo strumento del ciclo Read-Eval-Print HTTP, eseguire il comando `clear` o `cls`.</span><span class="sxs-lookup"><span data-stu-id="2990d-333">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="2990d-334">Si supponga ad esempio che la shell dei comandi includa l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-334">To illustrate, imagine the command shell contains the following output:</span></span>

```console
httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="2990d-335">Eseguire il comando seguente per cancellare l'output:</span><span class="sxs-lookup"><span data-stu-id="2990d-335">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="2990d-336">Dopo aver eseguito il comando precedente, la shell dei comandi contiene solo l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="2990d-336">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="2990d-337">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2990d-337">Additional resources</span></span>

* [<span data-ttu-id="2990d-338">Richieste API REST</span><span class="sxs-lookup"><span data-stu-id="2990d-338">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="2990d-339">Repository GitHub del ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="2990d-339">HTTP REPL GitHub repository</span></span>](https://github.com/dotnet/HttpRepl)
