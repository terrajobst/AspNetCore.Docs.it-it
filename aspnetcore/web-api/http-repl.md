---
title: Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP
author: scottaddie
description: Informazioni su come usare lo strumento globale REPL HTTP .NET Core per esplorare e testare un'API Web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: d2c5f774595e7a2223e84cc76eecdb9baa04adfe
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024797"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="c7e4d-103">Testare le API Web con il ciclo Read-Eval-Print (REPL) HTTP</span><span class="sxs-lookup"><span data-stu-id="c7e4d-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="c7e4d-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="c7e4d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="c7e4d-105">Il ciclo Read-Eval-Print (REPL) HTTP:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="c7e4d-106">È un strumento da riga di comando multipiattaforma leggero supportato ovunque sia supportato .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="c7e4d-107">Consente di eseguire richieste HTTP per testare le API Web ASP.NET Core (e le API Web non ASP.NET Core) e visualizzare i relativi risultati.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="c7e4d-108">Riesce a testare le API Web ospitate in qualsiasi ambiente, inclusi localhost e Servizio app di Azure.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="c7e4d-109">Sono supportati i [verbi HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="c7e4d-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="c7e4d-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="c7e4d-111">GET</span><span class="sxs-lookup"><span data-stu-id="c7e4d-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="c7e4d-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="c7e4d-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="c7e4d-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="c7e4d-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="c7e4d-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="c7e4d-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="c7e4d-115">POST</span><span class="sxs-lookup"><span data-stu-id="c7e4d-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="c7e4d-116">PUT</span><span class="sxs-lookup"><span data-stu-id="c7e4d-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="c7e4d-117">Per continuare, [visualizzare o scaricare l'API Web ASP.NET Core di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([come scaricare](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7e4d-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="c7e4d-119">Installazione</span><span class="sxs-lookup"><span data-stu-id="c7e4d-119">Installation</span></span>

<span data-ttu-id="c7e4d-120">Per installare il ciclo Read-Eval-Print HTTP, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="c7e4d-121">Viene installato uno [strumento globale .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) dal pacchetto NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="c7e4d-122">Utilizzo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-122">Usage</span></span>

<span data-ttu-id="c7e4d-123">Dopo aver completato l'installazione dello strumento, eseguire il comando seguente per avviare il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="c7e4d-124">Per visualizzare i comandi del ciclo Read-Eval-Print HTTP disponibili, eseguire uno dei comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="c7e4d-125">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

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

<span data-ttu-id="c7e4d-126">Il ciclo Read-Eval-Print HTTP offre il completamento del comando.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="c7e4d-127">Premendo il tasto <kbd>TAB</kbd> è possibile scorrere l'elenco dei comandi che completano i caratteri o l'endpoint API digitato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="c7e4d-128">Le sezioni seguenti descrivono i comandi dell'interfaccia della riga di comando disponibili.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="c7e4d-129">Connettersi all'API Web</span><span class="sxs-lookup"><span data-stu-id="c7e4d-129">Connect to the web API</span></span>

<span data-ttu-id="c7e4d-130">Connettersi all'API Web eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <ROOT URI>
```

<span data-ttu-id="c7e4d-131">`<ROOT URI>` è l'URI di base dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="c7e4d-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="c7e4d-133">In alternativa, eseguire il comando seguente in qualsiasi momento mentre è in esecuzione il ciclo Read-Eval-Print HTTP:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="c7e4d-134">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="c7e4d-135">Puntare manualmente al documento di Swagger per l'API Web</span><span class="sxs-lookup"><span data-stu-id="c7e4d-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="c7e4d-136">Il comando connect precedente tenterà di individuare automaticamente il documento di Swagger.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="c7e4d-137">Se per qualche motivo l'individuazione non riesce, è possibile specificare l'URI del documento di Swagger per l'API Web usando l'opzione `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="c7e4d-138">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="c7e4d-139">Esplorare l'API Web</span><span class="sxs-lookup"><span data-stu-id="c7e4d-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="c7e4d-140">Visualizzare gli endpoint disponibili</span><span class="sxs-lookup"><span data-stu-id="c7e4d-140">View available endpoints</span></span>

<span data-ttu-id="c7e4d-141">Per visualizzare l'elenco dei diversi endpoint (controller) nel percorso corrente dell'indirizzo dell'API Web, eseguire il comando `ls` o `dir`:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="c7e4d-142">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="c7e4d-143">L'output precedente indica che sono disponibili due controller: `Fruits` e `People`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="c7e4d-144">Entrambi i controller supportano operazioni HTTP GET e POST senza parametri.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="c7e4d-145">L'esplorazione di un controller specifico offre altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="c7e4d-146">Ad esempio, l'output del comando seguente mostra che il controller `Fruits` supporta anche le operazioni HTTP GET, PUT e DELETE.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="c7e4d-147">Ognuna di queste operazioni prevede un parametro `id` nella route:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="c7e4d-148">In alternativa, eseguire il comando `ui` per aprire la pagina dell'interfaccia utente di Swagger dell'API Web in un browser.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="c7e4d-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="c7e4d-150">Passare a un endpoint</span><span class="sxs-lookup"><span data-stu-id="c7e4d-150">Navigate to an endpoint</span></span>

<span data-ttu-id="c7e4d-151">Per passare a un endpoint diverso nell'API Web, eseguire il comando `cd`:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="c7e4d-152">Nel percorso che segue il comando `cd` non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="c7e4d-153">Viene visualizzato il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="c7e4d-154">Personalizzare il ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="c7e4d-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="c7e4d-155">I [colori](#set-color-preferences) predefiniti del ciclo Read-Eval-Print HTTP possono essere personalizzati.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="c7e4d-156">È anche possibile definire un [editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="c7e4d-157">Le preferenze del ciclo Read-Eval-Print HTTP vengono mantenute nella sessione corrente e nelle sessioni future.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="c7e4d-158">Dopo essere state modificate, le preferenze vengono archiviate nel file seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="c7e4d-159">Linux</span><span class="sxs-lookup"><span data-stu-id="c7e4d-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="c7e4d-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="c7e4d-161">macOS</span><span class="sxs-lookup"><span data-stu-id="c7e4d-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="c7e4d-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c7e4d-163">Windows</span><span class="sxs-lookup"><span data-stu-id="c7e4d-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="c7e4d-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="c7e4d-165">Il file con estensione *httpreplprefs* viene caricato all'avvio e non viene monitorato per le modifiche in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="c7e4d-166">Le modifiche manuali apportate al file diventano effettive solo dopo il riavvio dello strumento.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="c7e4d-167">Visualizzare le impostazioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-167">View the settings</span></span>

<span data-ttu-id="c7e4d-168">Per visualizzare le impostazioni disponibili, eseguire il comando `pref get`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="c7e4d-169">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="c7e4d-170">Il comando precedente visualizza le coppie chiave-valore disponibili:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="c7e4d-171">Impostare le preferenze colori</span><span class="sxs-lookup"><span data-stu-id="c7e4d-171">Set color preferences</span></span>

<span data-ttu-id="c7e4d-172">La colorazione delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="c7e4d-173">Per personalizzare la colorazione predefinita dello strumento del ciclo Read-Eval-Print, individuare la chiave corrispondente al colore da modificare.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="c7e4d-174">Per istruzioni su come trovare le chiavi, vedere la sezione [Visualizzare le impostazioni](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="c7e4d-175">Ad esempio, modificare il valore della chiave `colors.json` da `Green` a `White` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="c7e4d-176">È possibile usare solo i [colori consentiti](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="c7e4d-177">Output delle richieste HTTP successive con la nuova colorazione.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="c7e4d-178">Quando non sono impostate chiavi di colore specifiche, vengono considerate chiavi più generiche.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="c7e4d-179">Per comprendere questo comportamento di fallback, si consideri l'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="c7e4d-180">Se `colors.json.name` non ha un valore, viene usato `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="c7e4d-181">Se `colors.json.string` non ha un valore, viene usato `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="c7e4d-182">Se `colors.json.literal` non ha un valore, viene usato `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="c7e4d-183">Se `colors.json` non ha un valore, viene usato il colore del testo predefinito della shell dei comandi (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="c7e4d-184">Impostare la dimensione del rientro</span><span class="sxs-lookup"><span data-stu-id="c7e4d-184">Set indentation size</span></span>

<span data-ttu-id="c7e4d-185">La personalizzazione della dimensione del rientro delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="c7e4d-186">La dimensione del rientro predefinita è di due spazi.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-186">The default size is two spaces.</span></span> <span data-ttu-id="c7e4d-187">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-187">For example:</span></span>

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

<span data-ttu-id="c7e4d-188">Per modificare la dimensione predefinita, impostare la chiave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="c7e4d-189">Ad esempio, per usare sempre quattro spazi:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="c7e4d-190">L'impostazione dei quattro spazi viene applicata alle risposte successive:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="c7e4d-191">Impostare la dimensione del rientro</span><span class="sxs-lookup"><span data-stu-id="c7e4d-191">Set indentation size</span></span>

<span data-ttu-id="c7e4d-192">La personalizzazione della dimensione del rientro delle risposte è attualmente supportata solo per JSON.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="c7e4d-193">La dimensione del rientro predefinita è di due spazi.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-193">The default size is two spaces.</span></span> <span data-ttu-id="c7e4d-194">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-194">For example:</span></span>

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

<span data-ttu-id="c7e4d-195">Per modificare la dimensione predefinita, impostare la chiave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="c7e4d-196">Ad esempio, per usare sempre quattro spazi:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="c7e4d-197">L'impostazione dei quattro spazi viene applicata alle risposte successive:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-197">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="c7e4d-198">Impostare l'editor di testo predefinito</span><span class="sxs-lookup"><span data-stu-id="c7e4d-198">Set the default text editor</span></span>

<span data-ttu-id="c7e4d-199">Per impostazione predefinita, il ciclo Read-Eval-Print HTTP non ha un editor di testo configurato per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="c7e4d-200">Per testare i metodi dell'API Web che richiedono un corpo della richiesta HTTP, è necessario impostare un editor di testo predefinito.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="c7e4d-201">Lo strumento del ciclo Read-Eval-Print HTTP avvia l'editor di testo configurato al solo scopo di comporre il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="c7e4d-202">Eseguire il comando seguente per impostare l'editor di testo preferito come predefinito:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="c7e4d-203">Nel comando precedente `<EXECUTABLE>` è il percorso completo del file eseguibile dell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="c7e4d-204">Ad esempio, eseguire il comando seguente per impostare Visual Studio Code come editor di testo predefinito:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="c7e4d-205">Linux</span><span class="sxs-lookup"><span data-stu-id="c7e4d-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="c7e4d-206">macOS</span><span class="sxs-lookup"><span data-stu-id="c7e4d-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="c7e4d-207">Windows</span><span class="sxs-lookup"><span data-stu-id="c7e4d-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="c7e4d-208">Per avviare l'editor di testo predefinito con argomenti dell'interfaccia della riga di comando specifici, impostare la chiave `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="c7e4d-209">Si supponga, ad esempio, che Visual Studio Code sia l'editor di testo predefinito e che si voglia che il ciclo Read-Eval-Print apra sempre Visual Studio Code in una nuova sessione con le estensioni disabilitate.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="c7e4d-210">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="c7e4d-211">Impostare i percorsi di ricerca di Swagger</span><span class="sxs-lookup"><span data-stu-id="c7e4d-211">Set the Swagger search paths</span></span>

<span data-ttu-id="c7e4d-212">Per impostazione predefinita, HTTP REPL ha un set di percorsi relativi che usa per trovare il documento di Swagger quando si esegue il comando `connect` senza l'opzione `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-212">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="c7e4d-213">Questi percorsi relativi vengono combinati con i percorsi radice e di base specificati nel comando `connect`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-213">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="c7e4d-214">I percorsi relativi predefiniti sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-214">The default relative paths are:</span></span>

- <span data-ttu-id="c7e4d-215">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-215">*swagger.json*</span></span>
- <span data-ttu-id="c7e4d-216">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-216">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="c7e4d-217">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-217">*/swagger.json*</span></span>
- <span data-ttu-id="c7e4d-218">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="c7e4d-218">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="c7e4d-219">Per usare un set di percorsi di ricerca diverso nell'ambiente in uso, impostare la preferenza `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-219">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="c7e4d-220">Il valore deve essere un elenco di percorsi relativi delimitati da pipe.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-220">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="c7e4d-221">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-221">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json
```

## <a name="test-http-get-requests"></a><span data-ttu-id="c7e4d-222">Testare le richieste HTTP GET</span><span class="sxs-lookup"><span data-stu-id="c7e4d-222">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-223">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-223">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-224">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-225">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-226">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-226">Options</span></span>

<span data-ttu-id="c7e4d-227">Per il comando `get` sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-227">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="c7e4d-228">Esempio</span><span class="sxs-lookup"><span data-stu-id="c7e4d-228">Example</span></span>

<span data-ttu-id="c7e4d-229">Per inviare una richiesta HTTP GET:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-229">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="c7e4d-230">Eseguire il comando `get` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-230">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="c7e4d-231">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-231">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="c7e4d-232">Recuperare un record specifico passando un parametro al comando `get`:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-232">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="c7e4d-233">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-233">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="c7e4d-234">Testare le richieste HTTP POST</span><span class="sxs-lookup"><span data-stu-id="c7e4d-234">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-235">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-235">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-236">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-236">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-237">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-237">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-238">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-238">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="c7e4d-239">Esempio</span><span class="sxs-lookup"><span data-stu-id="c7e4d-239">Example</span></span>

<span data-ttu-id="c7e4d-240">Per inviare una richiesta HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-240">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="c7e4d-241">Eseguire il comando `post` in un endpoint che lo supporta:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-241">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="c7e4d-242">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-242">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="c7e4d-243">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-243">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="c7e4d-244">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-244">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="c7e4d-245">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-245">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="c7e4d-246">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-246">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="c7e4d-247">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-247">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="c7e4d-248">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-248">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="c7e4d-249">Testare le richieste HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="c7e4d-249">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-250">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-250">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-251">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-251">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-252">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-252">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-253">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-253">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="c7e4d-254">Esempio</span><span class="sxs-lookup"><span data-stu-id="c7e4d-254">Example</span></span>

<span data-ttu-id="c7e4d-255">Per inviare una richiesta HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-255">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="c7e4d-256">*Facoltativa*: Eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-256">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="c7e4d-257">Nel comando precedente l'intestazione della richiesta HTTP `Content-Type` è impostata per indicare un tipo di supporto del corpo della richiesta JSON.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-257">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="c7e4d-258">L'editor di testo predefinito apre un file con estensione *tmp* con un modello JSON che rappresenta il corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-258">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="c7e4d-259">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-259">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="c7e4d-260">Per impostare l'editor di testo predefinito, vedere la sezione [Impostare l'editor di testo predefinito](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="c7e4d-260">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="c7e4d-261">Modificare il modello JSON per soddisfare i requisiti di convalida del modello:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-261">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="c7e4d-262">Salvare il file con estensione *tmp* e chiudere l'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-262">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="c7e4d-263">Nella shell dei comandi viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-263">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="c7e4d-264">*Facoltativa*: Eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="c7e4d-265">Ad esempio, se si digita "Cherry" nell'editor di testo, `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-265">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="c7e4d-266">Testare le richieste HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="c7e4d-266">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-267">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-267">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-268">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-269">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-270">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="c7e4d-271">Esempio</span><span class="sxs-lookup"><span data-stu-id="c7e4d-271">Example</span></span>

<span data-ttu-id="c7e4d-272">Per inviare una richiesta HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-272">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="c7e4d-273">*Facoltativa*: Eseguire il comando `get` per visualizzare i dati prima di modificarli:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-273">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="c7e4d-274">Il comando precedente visualizza il formato di output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-274">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="c7e4d-275">*Facoltativa*: Eseguire un comando `get` per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-275">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="c7e4d-276">In questo esempio `get` restituisce quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-276">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="c7e4d-277">Testare le richieste HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="c7e4d-277">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-278">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-278">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-279">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-279">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-280">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-280">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-281">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-281">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="c7e4d-282">Testare le richieste HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="c7e4d-282">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-283">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-283">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-284">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-284">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-285">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-285">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-286">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-286">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="c7e4d-287">Testare le richieste HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="c7e4d-287">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="c7e4d-288">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c7e4d-288">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="c7e4d-289">Argomenti</span><span class="sxs-lookup"><span data-stu-id="c7e4d-289">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="c7e4d-290">Parametro di route, se presente, previsto dal metodo di azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-290">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="c7e4d-291">Opzioni</span><span class="sxs-lookup"><span data-stu-id="c7e4d-291">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="c7e4d-292">Impostare le intestazioni delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="c7e4d-292">Set HTTP request headers</span></span>

<span data-ttu-id="c7e4d-293">Per impostare l'intestazione di una richiesta HTTP, usare uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-293">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="c7e4d-294">Impostare inline con la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-294">Set inline with the HTTP request.</span></span> <span data-ttu-id="c7e4d-295">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-295">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="c7e4d-296">Con l'approccio precedente, ogni singola intestazione di richiesta HTTP richiede una propria opzione `-h`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-296">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="c7e4d-297">Impostare prima di inviare la richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-297">Set before sending the HTTP request.</span></span> <span data-ttu-id="c7e4d-298">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-298">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="c7e4d-299">Quando si imposta l'intestazione prima di inviare una richiesta, l'intestazione rimane impostata per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-299">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="c7e4d-300">Per cancellare l'intestazione, specificare un valore vuoto.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-300">To clear the header, provide an empty value.</span></span> <span data-ttu-id="c7e4d-301">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-301">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="c7e4d-302">Attivare o disattivare la visualizzazione delle richieste HTTP</span><span class="sxs-lookup"><span data-stu-id="c7e4d-302">Toggle HTTP request display</span></span>

<span data-ttu-id="c7e4d-303">Per impostazione predefinita, la visualizzazione della richiesta HTTP inviata viene eliminata.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-303">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="c7e4d-304">È possibile modificare l'impostazione corrispondente per la durata della sessione della shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-304">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="c7e4d-305">Abilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="c7e4d-305">Enable request display</span></span>

<span data-ttu-id="c7e4d-306">Visualizzare la richiesta HTTP inviata eseguendo il comando `echo on`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-306">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="c7e4d-307">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-307">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="c7e4d-308">Le richieste HTTP successive della sessione corrente visualizzano le intestazioni delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-308">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="c7e4d-309">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-309">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="c7e4d-310">Disabilitare la visualizzazione delle richieste</span><span class="sxs-lookup"><span data-stu-id="c7e4d-310">Disable request display</span></span>

<span data-ttu-id="c7e4d-311">Eliminare la visualizzazione della richiesta HTTP inviata eseguendo il comando `echo off`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-311">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="c7e4d-312">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-312">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="c7e4d-313">Eseguire uno script</span><span class="sxs-lookup"><span data-stu-id="c7e4d-313">Run a script</span></span>

<span data-ttu-id="c7e4d-314">Se si esegue spesso lo stesso set di comandi del ciclo Read-Eval-Print HTTP, può essere utile archiviarlo in un file di testo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-314">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="c7e4d-315">I comandi nel file hanno lo stesso formato di quelli eseguiti manualmente nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-315">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="c7e4d-316">I comandi possono essere eseguiti in modalità batch usando il comando `run`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-316">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="c7e4d-317">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-317">For example:</span></span>

1. <span data-ttu-id="c7e4d-318">Creare un file di testo contenente un set di comandi delimitati da una nuova riga.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-318">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="c7e4d-319">Si consideri ad esempio un file *people-script.txt* contenente i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-319">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="c7e4d-320">Eseguire il comando `run` passando il percorso del file di testo.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-320">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="c7e4d-321">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-321">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="c7e4d-322">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-322">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="c7e4d-323">Cancellare l'output</span><span class="sxs-lookup"><span data-stu-id="c7e4d-323">Clear the output</span></span>

<span data-ttu-id="c7e4d-324">Per rimuovere l'intero output scritto nella shell dei comandi dallo strumento del ciclo Read-Eval-Print HTTP, eseguire il comando `clear` o `cls`.</span><span class="sxs-lookup"><span data-stu-id="c7e4d-324">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="c7e4d-325">Si supponga ad esempio che la shell dei comandi includa l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-325">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="c7e4d-326">Eseguire il comando seguente per cancellare l'output:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-326">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="c7e4d-327">Dopo aver eseguito il comando precedente, la shell dei comandi contiene solo l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="c7e4d-327">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="c7e4d-328">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c7e4d-328">Additional resources</span></span>

* [<span data-ttu-id="c7e4d-329">Richieste API REST</span><span class="sxs-lookup"><span data-stu-id="c7e4d-329">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="c7e4d-330">Repository GitHub del ciclo Read-Eval-Print HTTP</span><span class="sxs-lookup"><span data-stu-id="c7e4d-330">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
