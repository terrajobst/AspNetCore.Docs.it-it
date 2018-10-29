---
title: Archiviazione sicura dei segreti delle app durante lo sviluppo di ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare informazioni riservate come segreti dell'app durante lo sviluppo di un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 385d0ecc6ea19d5f84a9fe3c2754f5256a2a5576
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207433"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="b6dc8-103">Archiviazione sicura dei segreti delle app durante lo sviluppo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6dc8-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="b6dc8-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b6dc8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="b6dc8-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6dc8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b6dc8-106">Questo documento illustra le tecniche per archiviare e recuperare i dati sensibili durante lo sviluppo di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="b6dc8-107">Non archiviare mai le password o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="b6dc8-108">I segreti di produzione non devono essere usati per lo sviluppo o test.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="b6dc8-109">È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="b6dc8-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="b6dc8-110">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="b6dc8-110">Environment variables</span></span>

<span data-ttu-id="b6dc8-111">Le variabili di ambiente vengono utilizzate per evitare l'archiviazione dei segreti delle app nel codice o nei file di configurazione locale.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="b6dc8-112">Le variabili di ambiente di eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="b6dc8-113">Configurare la lettura dei valori di variabili di ambiente tramite una chiamata [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="b6dc8-114">Si consideri un'applicazione web ASP.NET Core in cui **singoli account utente di** viene abilitata la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="b6dc8-115">Una stringa di connessione del database predefinito è incluso del progetto *appSettings. JSON* file con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="b6dc8-116">La stringa di connessione predefinita è per LocalDB, che viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="b6dc8-117">Durante la distribuzione di app, il `DefaultConnection` valore chiave può essere sostituito con il valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="b6dc8-118">La variabile di ambiente può archiviare la stringa di connessione completa con le credenziali sensibili.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="b6dc8-119">Le variabili di ambiente vengono in genere archiviate in testo normale e non crittografato.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="b6dc8-120">Se il computer o il processo è compromesso, le variabili di ambiente sono accessibili da parti non attendibili.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="b6dc8-121">Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessari.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="b6dc8-122">SECRET Manager</span><span class="sxs-lookup"><span data-stu-id="b6dc8-122">Secret Manager</span></span>

<span data-ttu-id="b6dc8-123">Lo strumento Secret Manager archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="b6dc8-124">In questo contesto, una porzione di dati sensibili è un segreto dell'app.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="b6dc8-125">I segreti dell'App vengono archiviati in una posizione separata dall'albero di progetto.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="b6dc8-126">I segreti dell'app sono associati a un progetto specifico o condivisa tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="b6dc8-127">I segreti dell'app non vengono controllati nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="b6dc8-128">Lo strumento Secret Manager non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="b6dc8-129">È solo a fini di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-129">It's for development purposes only.</span></span> <span data-ttu-id="b6dc8-130">Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="b6dc8-131">Come funziona lo strumento Secret Manager</span><span class="sxs-lookup"><span data-stu-id="b6dc8-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="b6dc8-132">Lo strumento Secret Manager estrae i dettagli di implementazione, ad esempio come e dove vengono archiviati i valori.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="b6dc8-133">È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="b6dc8-134">I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente sistema protetto nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6dc8-135">Windows</span><span class="sxs-lookup"><span data-stu-id="b6dc8-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="b6dc8-136">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="b6dc8-137">macOS</span><span class="sxs-lookup"><span data-stu-id="b6dc8-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="b6dc8-138">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6dc8-139">Linux</span><span class="sxs-lookup"><span data-stu-id="b6dc8-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="b6dc8-140">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="b6dc8-141">Nel precedente i percorsi di file, sostituire `<user_secrets_id>` con il `UserSecretsId` valore specificato nelle *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="b6dc8-142">Non scrivere codice che dipende dal percorso o formato di dati salvati con lo strumento Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="b6dc8-143">Questi dettagli di implementazione possono cambiare.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-143">These implementation details may change.</span></span> <span data-ttu-id="b6dc8-144">Ad esempio, i valori del segreto non vengono crittografati, ma potrebbe essere in futuro.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="b6dc8-145">Installare lo strumento Secret Manager</span><span class="sxs-lookup"><span data-stu-id="b6dc8-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="b6dc8-146">Lo strumento Secret Manager è in bundle con la CLI di .NET Core in .NET Core SDK 2.1.300 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="b6dc8-147">Per le versioni di .NET Core SDK 2.1.300 prima di, è necessario installazione dello strumento.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="b6dc8-148">Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione di .NET Core SDK installato.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="b6dc8-149">Se .NET Core SDK in uso include lo strumento, viene visualizzato un avviso:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="b6dc8-150">Installare il [Microsoft](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacchetto NuGet nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="b6dc8-151">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="b6dc8-152">Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="b6dc8-153">Lo strumento Secret Manager consente di visualizzare esempio di utilizzo, opzioni e Guida dei comandi:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="b6dc8-154">È necessario essere nella stessa directory il *file con estensione csproj* l'esecuzione di strumenti definiti nel file la *csproj* del file `DotNetCliToolReference` elementi.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="b6dc8-155">Impostare un segreto</span><span class="sxs-lookup"><span data-stu-id="b6dc8-155">Set a secret</span></span>

<span data-ttu-id="b6dc8-156">Lo strumento Secret Manager opera sulle impostazioni di configurazione specifici del progetto archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="b6dc8-157">Per usare i segreti utente, definire un `UserSecretsId` elemento all'interno di un `PropertyGroup` delle *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="b6dc8-158">Il valore di `UserSecretsId` è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="b6dc8-159">Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="b6dc8-160">In Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **Gestisci segreti utente** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b6dc8-161">Questa azione aggiunge un `UserSecretsId` elemento, popolato con un GUID al *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="b6dc8-162">Visual Studio apre una *Secrets* file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="b6dc8-163">Sostituire il contenuto del *Secrets* con le coppie chiave-valore da archiviare.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="b6dc8-164">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="b6dc8-165">La struttura JSON viene reso flat seguito alle modifiche apportate tramite `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="b6dc8-166">Ad esempio, in esecuzione `dotnet user-secrets remove "Movies:ConnectionString"` comprime il `Movies` valore letterale di oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="b6dc8-167">Il file modificato simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="b6dc8-168">Definire un segreto dell'app costituito da una chiave e il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="b6dc8-169">Il segreto è associato al progetto `UserSecretsId` valore.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="b6dc8-170">Ad esempio, eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="b6dc8-171">Nell'esempio precedente, i due punti indica che `Movies` è un oggetto letterale con una `ServiceApiKey` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="b6dc8-172">Lo strumento Secret Manager può essere utilizzato da altre directory troppo.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="b6dc8-173">Usare la `--project` opzione per specificare il percorso del file system in corrispondenza del quale il *csproj* file esista.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="b6dc8-174">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="b6dc8-175">Impostare più chiavi private</span><span class="sxs-lookup"><span data-stu-id="b6dc8-175">Set multiple secrets</span></span>

<span data-ttu-id="b6dc8-176">Un batch dei segreti può essere impostato da inviare tramite pipe da JSON al `set` comando.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="b6dc8-177">Nell'esempio seguente, il *input. JSON* contenuto del file viene inviati tramite pipe al `set` comando.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b6dc8-178">Windows</span><span class="sxs-lookup"><span data-stu-id="b6dc8-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="b6dc8-179">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="b6dc8-180">macOS</span><span class="sxs-lookup"><span data-stu-id="b6dc8-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="b6dc8-181">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="b6dc8-182">Linux</span><span class="sxs-lookup"><span data-stu-id="b6dc8-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="b6dc8-183">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="b6dc8-184">Accedere a un segreto</span><span class="sxs-lookup"><span data-stu-id="b6dc8-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b6dc8-185">Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="b6dc8-186">Se il progetto è destinato a .NET Framework, installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="b6dc8-187">In ASP.NET Core 2.0 o versione successiva, l'origine di configurazione di segreti utente viene aggiunto automaticamente in modalità di sviluppo se il progetto chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per inizializzare una nuova istanza dell'host con i valori predefiniti preconfigurati.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="b6dc8-188">`CreateDefaultBuilder` le chiamate [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) quando il [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) viene [sviluppo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="b6dc8-188">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="b6dc8-189">Quando `CreateDefaultBuilder` non viene chiamato durante la creazione di host, aggiungere l'origine di configurazione di segreti utente con una chiamata a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-189">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="b6dc8-190">Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-190">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="b6dc8-191">Installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-191">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="b6dc8-192">Aggiungere l'origine di configurazione di segreti utente con una chiamata a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-192">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="b6dc8-193">Informazioni riservate dell'utente possono essere recuperati tramite il `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-193">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="b6dc8-194">Eseguire il mapping di segreti da un oggetto POCO</span><span class="sxs-lookup"><span data-stu-id="b6dc8-194">Map secrets to a POCO</span></span>

<span data-ttu-id="b6dc8-195">Mapping di un intero valore letterale di oggetto a un oggetto POCO (una classe .NET semplice con proprietà) è utile per l'aggregazione di proprietà correlate.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-195">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b6dc8-196">Per mappare i segreti precedenti a un oggetto POCO, usare il `Configuration` dell'API [oggetto associazione di graph](xref:fundamentals/configuration/index#bind-to-an-object-graph) funzionalità.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-196">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="b6dc8-197">Il codice seguente viene associata a una classe personalizzata `MovieSettings` POCO e gli accessi di `ServiceApiKey` valore proprietà:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-197">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="b6dc8-198">Il `Movies:ConnectionString` e `Movies:ServiceApiKey` segreti vengono eseguito il mapping nelle rispettive proprietà `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-198">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="b6dc8-199">Sostituire le stringhe con segreti</span><span class="sxs-lookup"><span data-stu-id="b6dc8-199">String replacement with secrets</span></span>

<span data-ttu-id="b6dc8-200">L'archiviazione delle password in testo normale non è sicura.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="b6dc8-201">Ad esempio, una stringa di connessione di database archiviati nelle *appSettings. JSON* può includere una password per l'utente specificato:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="b6dc8-202">Un approccio più sicuro consiste nell'archiviare la password come segreto.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="b6dc8-203">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="b6dc8-204">Rimuovere il `Password` coppia chiave-valore dalla stringa di connessione nel *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="b6dc8-205">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="b6dc8-206">Valore del segreto può essere impostato su un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) dell'oggetto [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) proprietà per completare la stringa di connessione:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="b6dc8-207">Elenca i segreti</span><span class="sxs-lookup"><span data-stu-id="b6dc8-207">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b6dc8-208">Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-208">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="b6dc8-209">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-209">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="b6dc8-210">Nell'esempio precedente, i due punti nei nomi delle chiavi denota la gerarchia di oggetti all'interno *Secrets*.</span><span class="sxs-lookup"><span data-stu-id="b6dc8-210">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="b6dc8-211">Rimuovere un singolo segreto</span><span class="sxs-lookup"><span data-stu-id="b6dc8-211">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b6dc8-212">Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="b6dc8-213">L'app *Secrets* apportata per rimuovere la coppia chiave-valore associate al file il `MoviesConnectionString` chiave:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-213">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="b6dc8-214">Esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-214">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="b6dc8-215">Rimuovere tutti i segreti</span><span class="sxs-lookup"><span data-stu-id="b6dc8-215">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b6dc8-216">Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="b6dc8-217">Tutti i segreti utente dell'App sono stati eliminati dal *Secrets* file:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-217">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="b6dc8-218">Esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="b6dc8-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="b6dc8-219">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b6dc8-219">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
