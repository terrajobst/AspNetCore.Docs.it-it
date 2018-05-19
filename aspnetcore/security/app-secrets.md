---
title: Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare le informazioni riservate come segreti dell'app durante lo sviluppo di un'app di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="b47f4-103">Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b47f4-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="b47f4-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="b47f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b47f4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b47f4-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b47f4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="b47f4-107">Questo documento illustra le tecniche per archiviare e recuperare i dati sensibili durante lo sviluppo di un'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b47f4-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="b47f4-108">Evitare di archiviare le password o altri dati sensibili nel codice sorgente e non devono utilizzare i segreti di produzione in fase di sviluppo o dalla modalità.</span><span class="sxs-lookup"><span data-stu-id="b47f4-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="b47f4-109">È possibile archiviare e proteggere i segreti Azure di test e produzione con la [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="b47f4-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="b47f4-110">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="b47f4-110">Environment variables</span></span>

<span data-ttu-id="b47f4-111">Per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locale vengono utilizzate variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b47f4-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="b47f4-112">Le variabili di ambiente di eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b47f4-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-113">Configurare la lettura dei valori di variabili di ambiente chiamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="b47f4-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="b47f4-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="b47f4-115">Si consideri un'applicazione web ASP.NET Core in cui **singoli account utente di** è protetta.</span><span class="sxs-lookup"><span data-stu-id="b47f4-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="b47f4-116">Una stringa di connessione del database predefinito è incluso il progetto *appSettings* file con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="b47f4-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="b47f4-117">La stringa di connessione predefinita è per LocalDB, viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="b47f4-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="b47f4-118">Durante la distribuzione di app, il `DefaultConnection` valore chiave può essere sostituito con il valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b47f4-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="b47f4-119">La variabile di ambiente può archiviare la stringa di connessione completa con credenziali sensibili.</span><span class="sxs-lookup"><span data-stu-id="b47f4-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="b47f4-120">Le variabili di ambiente vengono in genere archiviate in testo non crittografato.</span><span class="sxs-lookup"><span data-stu-id="b47f4-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="b47f4-121">Se il computer o processi sono compromesso, le variabili di ambiente sono accessibili da parti non attendibili.</span><span class="sxs-lookup"><span data-stu-id="b47f4-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="b47f4-122">Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessari.</span><span class="sxs-lookup"><span data-stu-id="b47f4-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="b47f4-123">Segreto Manager</span><span class="sxs-lookup"><span data-stu-id="b47f4-123">Secret Manager</span></span>

<span data-ttu-id="b47f4-124">Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b47f4-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="b47f4-125">In questo contesto, una parte dei dati sensibili è un segreto dell'app.</span><span class="sxs-lookup"><span data-stu-id="b47f4-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="b47f4-126">App segreti sono archiviati in una posizione separata dalla struttura del progetto.</span><span class="sxs-lookup"><span data-stu-id="b47f4-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="b47f4-127">I segreti dell'app sono associati a un progetto specifico o condivisa tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="b47f4-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="b47f4-128">I segreti dell'app non sono archiviati nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b47f4-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="b47f4-129">Lo strumento di gestione di segreto non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="b47f4-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="b47f4-130">È solo a fini di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b47f4-130">It's for development purposes only.</span></span> <span data-ttu-id="b47f4-131">Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b47f4-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="b47f4-132">Il segreto Manager funzionamento dello strumento</span><span class="sxs-lookup"><span data-stu-id="b47f4-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="b47f4-133">Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati.</span><span class="sxs-lookup"><span data-stu-id="b47f4-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="b47f4-134">È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b47f4-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="b47f4-135">I valori vengono archiviati un [JSON](https://json.org/) file di configurazione in una cartella del profilo utente sistema protetto nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="b47f4-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="b47f4-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b47f4-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="b47f4-137">& MacOS Linux: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b47f4-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="b47f4-138">Nel passaggio precedente percorsi di file, sostituire `<user_secrets_id>` con il `UserSecretsId` specificato nel valore di *con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b47f4-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="b47f4-139">Non scrivere codice che dipende dal percorso o formato di dati salvati con lo strumento di gestione di chiave privata.</span><span class="sxs-lookup"><span data-stu-id="b47f4-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="b47f4-140">Questi dettagli di implementazione potrebbero cambiare.</span><span class="sxs-lookup"><span data-stu-id="b47f4-140">These implementation details may change.</span></span> <span data-ttu-id="b47f4-141">Ad esempio, i valori segreti non sono crittografati, ma potrebbe essere in futuro.</span><span class="sxs-lookup"><span data-stu-id="b47f4-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="b47f4-142">Installare lo strumento di gestione segreto</span><span class="sxs-lookup"><span data-stu-id="b47f4-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="b47f4-143">Lo strumento di gestione di chiave privata viene fornito con .NET Core CLI in .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="b47f4-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="b47f4-144">Per .NET Core SDK 2.0 e versioni precedenti, è necessario installazione dello strumento.</span><span class="sxs-lookup"><span data-stu-id="b47f4-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="b47f4-145">Installare il [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacchetto NuGet nel progetto ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b47f4-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="b47f4-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="b47f4-147">Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:</span><span class="sxs-lookup"><span data-stu-id="b47f4-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="b47f4-148">Consente di visualizzare lo strumento Gestione segreto utilizzo dell'esempio, le opzioni e la Guida di comando:</span><span class="sxs-lookup"><span data-stu-id="b47f4-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="b47f4-149">È necessario essere nella stessa directory il *csproj* l'esecuzione di strumenti definiti nel file il *con estensione csproj* file `DotNetCliToolReference` elementi.</span><span class="sxs-lookup"><span data-stu-id="b47f4-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="b47f4-150">Impostare un segreto</span><span class="sxs-lookup"><span data-stu-id="b47f4-150">Set a secret</span></span>

<span data-ttu-id="b47f4-151">Lo strumento Gestione segreto opera sulle impostazioni di configurazione specifici del progetto archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="b47f4-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="b47f4-152">Per utilizzare informazioni riservate dell'utente, definire un `UserSecretsId` elemento all'interno di un `PropertyGroup` del *con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b47f4-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="b47f4-153">Il valore di `UserSecretsId` è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="b47f4-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="b47f4-154">Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="b47f4-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b47f4-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="b47f4-157">In Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b47f4-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b47f4-158">Questa azione aggiunge una `UserSecretsId` elemento, popolato con un GUID per il *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b47f4-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="b47f4-159">Visual Studio apre una *secrets.json* file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b47f4-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="b47f4-160">Sostituire il contenuto di *secrets.json* con le coppie chiave-valore da archiviare.</span><span class="sxs-lookup"><span data-stu-id="b47f4-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="b47f4-161">Ad esempio: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="b47f4-162">Definire un segreto dell'applicazione costituito da una chiave e il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="b47f4-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="b47f4-163">Il segreto è associato al progetto `UserSecretsId` valore.</span><span class="sxs-lookup"><span data-stu-id="b47f4-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="b47f4-164">Ad esempio, eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="b47f4-165">Nell'esempio precedente, i due punti indica che `Movies` è un oggetto letterale con un `ServiceApiKey` proprietà.</span><span class="sxs-lookup"><span data-stu-id="b47f4-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="b47f4-166">Lo strumento di gestione di chiave privata utilizzabile da altre directory troppo.</span><span class="sxs-lookup"><span data-stu-id="b47f4-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="b47f4-167">Usare la `--project` possibilità di fornire il percorso del file system in corrispondenza del quale il *csproj* file esista.</span><span class="sxs-lookup"><span data-stu-id="b47f4-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="b47f4-168">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b47f4-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="b47f4-169">Impostare più segreti</span><span class="sxs-lookup"><span data-stu-id="b47f4-169">Set multiple secrets</span></span>

<span data-ttu-id="b47f4-170">Un batch dei segreti può essere impostato tramite pipe JSON per il `set` comando.</span><span class="sxs-lookup"><span data-stu-id="b47f4-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="b47f4-171">Nell'esempio seguente, il *input.json* contenuto del file viene inviati tramite pipe per il `set` comando in Windows:</span><span class="sxs-lookup"><span data-stu-id="b47f4-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="b47f4-172">Usare il comando seguente in macOS e Linux:</span><span class="sxs-lookup"><span data-stu-id="b47f4-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="b47f4-173">Accedere a una chiave privata</span><span class="sxs-lookup"><span data-stu-id="b47f4-173">Access a secret</span></span>

<span data-ttu-id="b47f4-174">Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Manager segreto.</span><span class="sxs-lookup"><span data-stu-id="b47f4-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="b47f4-175">Se la destinazione di .NET Core 1.x o .NET Framework, installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="b47f4-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-176">Aggiungere l'origine di configurazione di segreti utente per il `Startup` costruttore:</span><span class="sxs-lookup"><span data-stu-id="b47f4-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="b47f4-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="b47f4-178">Informazioni riservate dell'utente possono essere recuperati tramite il `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="b47f4-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b47f4-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="b47f4-181">Sostituire le stringhe dei segreti</span><span class="sxs-lookup"><span data-stu-id="b47f4-181">String replacement with secrets</span></span>

<span data-ttu-id="b47f4-182">L'archiviazione delle password in testo normale è rischiosa.</span><span class="sxs-lookup"><span data-stu-id="b47f4-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="b47f4-183">Ad esempio, una stringa di connessione di database archiviati *appSettings* può includere una password per l'utente specificato:</span><span class="sxs-lookup"><span data-stu-id="b47f4-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="b47f4-184">Un approccio più sicuro consiste nell'archiviare la password come una chiave privata.</span><span class="sxs-lookup"><span data-stu-id="b47f4-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="b47f4-185">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b47f4-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="b47f4-186">Sostituire la password nella *appSettings* con un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="b47f4-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="b47f4-187">Nell'esempio seguente, `{0}` viene utilizzato come segnaposto al form un [stringa di formato composita](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="b47f4-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="b47f4-188">Valore di un segreto può essere inserite nel segnaposto per completare la stringa di connessione:</span><span class="sxs-lookup"><span data-stu-id="b47f4-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="b47f4-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="b47f4-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="b47f4-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="b47f4-191">Elenco dei segreti</span><span class="sxs-lookup"><span data-stu-id="b47f4-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b47f4-192">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="b47f4-193">Viene visualizzato il seguente output:</span><span class="sxs-lookup"><span data-stu-id="b47f4-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="b47f4-194">Nell'esempio precedente, i due punti nei nomi di chiave indica la gerarchia degli oggetti all'interno di *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="b47f4-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="b47f4-195">Rimuovere un singolo segreto</span><span class="sxs-lookup"><span data-stu-id="b47f4-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b47f4-196">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="b47f4-197">L'app *secrets.json* apportata per rimuovere la coppia chiave-valore associate al file il `MoviesConnectionString` chiave:</span><span class="sxs-lookup"><span data-stu-id="b47f4-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="b47f4-198">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="b47f4-199">Rimuovere tutti i segreti</span><span class="sxs-lookup"><span data-stu-id="b47f4-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="b47f4-200">Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="b47f4-201">Tutte le informazioni riservate dell'utente per l'app sono state eliminate dal *secrets.json* file:</span><span class="sxs-lookup"><span data-stu-id="b47f4-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="b47f4-202">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="b47f4-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="b47f4-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b47f4-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>