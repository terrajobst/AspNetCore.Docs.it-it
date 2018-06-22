---
title: Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare le informazioni riservate come segreti dell'app durante lo sviluppo di un'app di ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
uid: security/app-secrets
ms.openlocfilehash: 8db1d1069958d3b08bbb3b9727ddbc47ae5747df
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275425"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="f2f17-103">Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2f17-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="f2f17-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f2f17-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f2f17-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f2f17-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f2f17-106">Questo documento illustra le tecniche per archiviare e recuperare i dati sensibili durante lo sviluppo di un'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2f17-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="f2f17-107">Evitare di archiviare le password o altri dati sensibili nel codice sorgente e non devono utilizzare i segreti di produzione in fase di sviluppo o dalla modalità.</span><span class="sxs-lookup"><span data-stu-id="f2f17-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="f2f17-108">È possibile archiviare e proteggere i segreti Azure di test e produzione con la [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="f2f17-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f2f17-109">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f2f17-109">Environment variables</span></span>

<span data-ttu-id="f2f17-110">Per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locale vengono utilizzate variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2f17-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="f2f17-111">Le variabili di ambiente di eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f2f17-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f2f17-112">Configurare la lettura dei valori di variabili di ambiente chiamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="f2f17-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="f2f17-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="f2f17-114">Si consideri un'applicazione web ASP.NET Core in cui **singoli account utente di** è protetta.</span><span class="sxs-lookup"><span data-stu-id="f2f17-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="f2f17-115">Una stringa di connessione del database predefinito è incluso il progetto *appSettings* file con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f2f17-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="f2f17-116">La stringa di connessione predefinita è per LocalDB, viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="f2f17-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f2f17-117">Durante la distribuzione di app, il `DefaultConnection` valore chiave può essere sostituito con il valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f2f17-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="f2f17-118">La variabile di ambiente può archiviare la stringa di connessione completa con credenziali sensibili.</span><span class="sxs-lookup"><span data-stu-id="f2f17-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="f2f17-119">Le variabili di ambiente vengono in genere archiviate in testo non crittografato.</span><span class="sxs-lookup"><span data-stu-id="f2f17-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="f2f17-120">Se il computer o processi sono compromesso, le variabili di ambiente sono accessibili da parti non attendibili.</span><span class="sxs-lookup"><span data-stu-id="f2f17-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f2f17-121">Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessari.</span><span class="sxs-lookup"><span data-stu-id="f2f17-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="f2f17-122">Segreto Manager</span><span class="sxs-lookup"><span data-stu-id="f2f17-122">Secret Manager</span></span>

<span data-ttu-id="f2f17-123">Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2f17-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="f2f17-124">In questo contesto, una parte dei dati sensibili è un segreto dell'app.</span><span class="sxs-lookup"><span data-stu-id="f2f17-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="f2f17-125">App segreti sono archiviati in una posizione separata dalla struttura del progetto.</span><span class="sxs-lookup"><span data-stu-id="f2f17-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="f2f17-126">I segreti dell'app sono associati a un progetto specifico o condivisa tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="f2f17-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="f2f17-127">I segreti dell'app non sono archiviati nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="f2f17-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="f2f17-128">Lo strumento di gestione di segreto non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="f2f17-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="f2f17-129">È solo a fini di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f2f17-129">It's for development purposes only.</span></span> <span data-ttu-id="f2f17-130">Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="f2f17-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f2f17-131">Il segreto Manager funzionamento dello strumento</span><span class="sxs-lookup"><span data-stu-id="f2f17-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="f2f17-132">Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati.</span><span class="sxs-lookup"><span data-stu-id="f2f17-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f2f17-133">È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="f2f17-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f2f17-134">I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente sistema protetto nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="f2f17-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f2f17-135">Windows</span><span class="sxs-lookup"><span data-stu-id="f2f17-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="f2f17-136">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="f2f17-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="f2f17-137">macOS</span><span class="sxs-lookup"><span data-stu-id="f2f17-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="f2f17-138">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="f2f17-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="f2f17-139">Linux</span><span class="sxs-lookup"><span data-stu-id="f2f17-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="f2f17-140">Percorso del file system:</span><span class="sxs-lookup"><span data-stu-id="f2f17-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="f2f17-141">Nel passaggio precedente percorsi di file, sostituire `<user_secrets_id>` con il `UserSecretsId` specificato nel valore di *con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="f2f17-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="f2f17-142">Non scrivere codice che dipende dal percorso o formato di dati salvati con lo strumento di gestione di chiave privata.</span><span class="sxs-lookup"><span data-stu-id="f2f17-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="f2f17-143">Questi dettagli di implementazione potrebbero cambiare.</span><span class="sxs-lookup"><span data-stu-id="f2f17-143">These implementation details may change.</span></span> <span data-ttu-id="f2f17-144">Ad esempio, i valori segreti non sono crittografati, ma potrebbe essere in futuro.</span><span class="sxs-lookup"><span data-stu-id="f2f17-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="f2f17-145">Installare lo strumento di gestione segreto</span><span class="sxs-lookup"><span data-stu-id="f2f17-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="f2f17-146">Lo strumento di gestione di chiave privata viene fornito con l'interfaccia CLI dei componenti di base .NET a partire da .NET Core SDK 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="f2f17-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="f2f17-147">Per le versioni di .NET Core SDK precedenti 2.1.300, è necessario installazione dello strumento.</span><span class="sxs-lookup"><span data-stu-id="f2f17-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="f2f17-148">Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione di .NET Core SDK installato.</span><span class="sxs-lookup"><span data-stu-id="f2f17-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="f2f17-149">Viene visualizzato un avviso se viene utilizzato .NET Core SDK include lo strumento:</span><span class="sxs-lookup"><span data-stu-id="f2f17-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="f2f17-150">Installare il [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacchetto NuGet nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2f17-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="f2f17-151">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2f17-151">For example:</span></span>

<span data-ttu-id="f2f17-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="f2f17-153">Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:</span><span class="sxs-lookup"><span data-stu-id="f2f17-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="f2f17-154">Consente di visualizzare lo strumento Gestione segreto utilizzo dell'esempio, le opzioni e la Guida di comando:</span><span class="sxs-lookup"><span data-stu-id="f2f17-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="f2f17-155">È necessario essere nella stessa directory il *csproj* l'esecuzione di strumenti definiti nel file il *con estensione csproj* file `DotNetCliToolReference` elementi.</span><span class="sxs-lookup"><span data-stu-id="f2f17-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="f2f17-156">Impostare un segreto</span><span class="sxs-lookup"><span data-stu-id="f2f17-156">Set a secret</span></span>

<span data-ttu-id="f2f17-157">Lo strumento Gestione segreto opera sulle impostazioni di configurazione specifici del progetto archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="f2f17-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="f2f17-158">Per utilizzare informazioni riservate dell'utente, definire un `UserSecretsId` elemento all'interno di un `PropertyGroup` del *con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="f2f17-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f2f17-159">Il valore di `UserSecretsId` è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="f2f17-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="f2f17-160">Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f2f17-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f2f17-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f2f17-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="f2f17-163">In Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f2f17-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f2f17-164">Questa azione aggiunge una `UserSecretsId` elemento, popolato con un GUID per il *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="f2f17-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="f2f17-165">Visual Studio apre una *secrets.json* file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="f2f17-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="f2f17-166">Sostituire il contenuto di *secrets.json* con le coppie chiave-valore da archiviare.</span><span class="sxs-lookup"><span data-stu-id="f2f17-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="f2f17-167">Ad esempio: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="f2f17-168">Definire un segreto dell'applicazione costituito da una chiave e il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="f2f17-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="f2f17-169">Il segreto è associato al progetto `UserSecretsId` valore.</span><span class="sxs-lookup"><span data-stu-id="f2f17-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="f2f17-170">Ad esempio, eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="f2f17-171">Nell'esempio precedente, i due punti indica che `Movies` è un oggetto letterale con un `ServiceApiKey` proprietà.</span><span class="sxs-lookup"><span data-stu-id="f2f17-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="f2f17-172">Lo strumento di gestione di chiave privata utilizzabile da altre directory troppo.</span><span class="sxs-lookup"><span data-stu-id="f2f17-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="f2f17-173">Usare la `--project` possibilità di fornire il percorso del file system in corrispondenza del quale il *csproj* file esista.</span><span class="sxs-lookup"><span data-stu-id="f2f17-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="f2f17-174">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2f17-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="f2f17-175">Impostare più segreti</span><span class="sxs-lookup"><span data-stu-id="f2f17-175">Set multiple secrets</span></span>

<span data-ttu-id="f2f17-176">Un batch dei segreti può essere impostato tramite pipe JSON per il `set` comando.</span><span class="sxs-lookup"><span data-stu-id="f2f17-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="f2f17-177">Nell'esempio seguente, il *input.json* contenuto del file viene inviati tramite pipe per il `set` comando.</span><span class="sxs-lookup"><span data-stu-id="f2f17-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f2f17-178">Windows</span><span class="sxs-lookup"><span data-stu-id="f2f17-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="f2f17-179">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="f2f17-180">macOS</span><span class="sxs-lookup"><span data-stu-id="f2f17-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="f2f17-181">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="f2f17-182">Linux</span><span class="sxs-lookup"><span data-stu-id="f2f17-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="f2f17-183">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="f2f17-184">Accedere a una chiave privata</span><span class="sxs-lookup"><span data-stu-id="f2f17-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f2f17-185">Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Manager segreto.</span><span class="sxs-lookup"><span data-stu-id="f2f17-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f2f17-186">Installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="f2f17-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f2f17-187">Aggiungere l'origine di configurazione di segreti utente con una chiamata a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="f2f17-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="f2f17-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f2f17-189">Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Manager segreto.</span><span class="sxs-lookup"><span data-stu-id="f2f17-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="f2f17-190">Se il progetto è destinato a .NET Framework, installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="f2f17-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f2f17-191">In ASP.NET Core 2.0 o versione successiva, l'origine di configurazione di segreti utente viene aggiunto automaticamente in modalità di sviluppo quando si chiama il progetto [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per inizializzare una nuova istanza dell'host con valori predefiniti preconfigurati.</span><span class="sxs-lookup"><span data-stu-id="f2f17-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="f2f17-192">`CreateDefaultBuilder` le chiamate [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) quando il [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) è [sviluppo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="f2f17-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="f2f17-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="f2f17-194">Quando si `CreateDefaultBuilder` non viene chiamato durante la costruzione di host, aggiungere l'origine di configurazione di segreti utente con una chiamata a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="f2f17-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="f2f17-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="f2f17-196">Informazioni riservate dell'utente possono essere recuperati tramite il `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="f2f17-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f2f17-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f2f17-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="f2f17-199">Sostituire le stringhe dei segreti</span><span class="sxs-lookup"><span data-stu-id="f2f17-199">String replacement with secrets</span></span>

<span data-ttu-id="f2f17-200">L'archiviazione delle password in testo normale non è protetto.</span><span class="sxs-lookup"><span data-stu-id="f2f17-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="f2f17-201">Ad esempio, una stringa di connessione di database archiviati *appSettings* può includere una password per l'utente specificato:</span><span class="sxs-lookup"><span data-stu-id="f2f17-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="f2f17-202">Un approccio più sicuro consiste nell'archiviare la password come una chiave privata.</span><span class="sxs-lookup"><span data-stu-id="f2f17-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="f2f17-203">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2f17-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="f2f17-204">Rimuovere il `Password` coppia chiave-valore specificata dalla stringa di connessione in *appSettings*.</span><span class="sxs-lookup"><span data-stu-id="f2f17-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="f2f17-205">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f2f17-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="f2f17-206">Valore di un segreto può essere impostato su un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) dell'oggetto [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) proprietà per completare la stringa di connessione:</span><span class="sxs-lookup"><span data-stu-id="f2f17-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="f2f17-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="f2f17-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="f2f17-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="f2f17-209">Elenco dei segreti</span><span class="sxs-lookup"><span data-stu-id="f2f17-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f2f17-210">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="f2f17-211">Viene visualizzato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="f2f17-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="f2f17-212">Nell'esempio precedente, i due punti nei nomi di chiave indica la gerarchia degli oggetti all'interno di *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="f2f17-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="f2f17-213">Rimuovere un singolo segreto</span><span class="sxs-lookup"><span data-stu-id="f2f17-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f2f17-214">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="f2f17-215">L'app *secrets.json* apportata per rimuovere la coppia chiave-valore associate al file il `MoviesConnectionString` chiave:</span><span class="sxs-lookup"><span data-stu-id="f2f17-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f2f17-216">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="f2f17-217">Rimuovere tutti i segreti</span><span class="sxs-lookup"><span data-stu-id="f2f17-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f2f17-218">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="f2f17-219">Tutte le informazioni riservate dell'utente per l'app sono state eliminate dal *secrets.json* file:</span><span class="sxs-lookup"><span data-stu-id="f2f17-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="f2f17-220">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="f2f17-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="f2f17-221">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f2f17-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
