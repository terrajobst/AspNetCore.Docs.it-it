---
title: Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare informazioni riservate come segreti dell'app durante lo sviluppo di un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080709"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="23c5f-103">Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23c5f-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="23c5f-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="23c5f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="23c5f-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23c5f-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="23c5f-106">Questo documento illustra le tecniche per l'archiviazione e il recupero di dati sensibili durante lo sviluppo di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c5f-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="23c5f-107">Non archiviare mai le password o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="23c5f-108">I segreti di produzione non devono essere usati per lo sviluppo o il test.</span><span class="sxs-lookup"><span data-stu-id="23c5f-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="23c5f-109">È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="23c5f-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="23c5f-110">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="23c5f-110">Environment variables</span></span>

<span data-ttu-id="23c5f-111">Le variabili di ambiente vengono usate per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locali.</span><span class="sxs-lookup"><span data-stu-id="23c5f-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="23c5f-112">Le variabili di ambiente sostituiscono i valori di configurazione per tutte le origini di configurazione precedentemente specificate.</span><span class="sxs-lookup"><span data-stu-id="23c5f-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="23c5f-113">Configurare la lettura dei valori delle variabili di <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ambiente chiamando `Startup` nel costruttore:</span><span class="sxs-lookup"><span data-stu-id="23c5f-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="23c5f-114">Si consideri un'app Web ASP.NET Core in cui è abilitata la sicurezza dei **singoli account utente** .</span><span class="sxs-lookup"><span data-stu-id="23c5f-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="23c5f-115">Una stringa di connessione al database predefinita è inclusa nel file *appSettings. JSON* del progetto con la `DefaultConnection`chiave.</span><span class="sxs-lookup"><span data-stu-id="23c5f-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="23c5f-116">La stringa di connessione predefinita è per il database locale, che viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="23c5f-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="23c5f-117">Durante la distribuzione dell'app `DefaultConnection` , il valore della chiave può essere sostituito con il valore di una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="23c5f-118">La variabile di ambiente può archiviare la stringa di connessione completa con credenziali riservate.</span><span class="sxs-lookup"><span data-stu-id="23c5f-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="23c5f-119">Le variabili di ambiente vengono in genere archiviate in testo normale e non crittografato.</span><span class="sxs-lookup"><span data-stu-id="23c5f-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="23c5f-120">Se il computer o il processo è compromesso, le variabili di ambiente possono essere accessibili da entità non attendibili.</span><span class="sxs-lookup"><span data-stu-id="23c5f-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="23c5f-121">Potrebbero essere necessarie misure aggiuntive per impedire la divulgazione di segreti utente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="23c5f-122">Gestione segreta</span><span class="sxs-lookup"><span data-stu-id="23c5f-122">Secret Manager</span></span>

<span data-ttu-id="23c5f-123">Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c5f-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="23c5f-124">In questo contesto, una parte di dati sensibili è un segreto dell'app.</span><span class="sxs-lookup"><span data-stu-id="23c5f-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="23c5f-125">I segreti dell'app vengono archiviati in un percorso separato dall'albero del progetto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="23c5f-126">I segreti dell'app sono associati a un progetto specifico o condivisi tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="23c5f-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="23c5f-127">I segreti dell'app non vengono archiviati nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="23c5f-128">Lo strumento Secret Manager non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="23c5f-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="23c5f-129">È solo a scopo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="23c5f-129">It's for development purposes only.</span></span> <span data-ttu-id="23c5f-130">Le chiavi e i valori vengono archiviati in un file di configurazione JSON nella directory del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="23c5f-131">Funzionamento dello strumento di gestione dei segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="23c5f-132">Lo strumento di gestione dei segreti estrae i dettagli di implementazione, ad esempio dove e come vengono archiviati i valori.</span><span class="sxs-lookup"><span data-stu-id="23c5f-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="23c5f-133">È possibile utilizzare lo strumento senza conoscere i dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="23c5f-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="23c5f-134">I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente protetta dal sistema nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="23c5f-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="23c5f-135">Windows</span><span class="sxs-lookup"><span data-stu-id="23c5f-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="23c5f-136">Percorso del file System:</span><span class="sxs-lookup"><span data-stu-id="23c5f-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="23c5f-137">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="23c5f-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="23c5f-138">Percorso del file System:</span><span class="sxs-lookup"><span data-stu-id="23c5f-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="23c5f-139">Nei percorsi di file precedenti sostituire `<user_secrets_id>` con il `UserSecretsId` valore specificato nel file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="23c5f-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="23c5f-140">Non scrivere codice che dipende dalla posizione o dal formato dei dati salvati con lo strumento di gestione dei segreti.</span><span class="sxs-lookup"><span data-stu-id="23c5f-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="23c5f-141">Questi dettagli di implementazione possono cambiare.</span><span class="sxs-lookup"><span data-stu-id="23c5f-141">These implementation details may change.</span></span> <span data-ttu-id="23c5f-142">Ad esempio, i valori dei segreti non sono crittografati, ma potrebbero essere futuri.</span><span class="sxs-lookup"><span data-stu-id="23c5f-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="23c5f-143">Installare lo strumento di gestione dei segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="23c5f-144">Lo strumento di gestione dei segreti è integrato con il interfaccia della riga di comando di .NET Core in .NET Core SDK 2.1.300 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="23c5f-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="23c5f-145">Per .NET Core SDK versioni precedenti a 2.1.300, è necessaria l'installazione dello strumento.</span><span class="sxs-lookup"><span data-stu-id="23c5f-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="23c5f-146">Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione di .NET Core SDK installato.</span><span class="sxs-lookup"><span data-stu-id="23c5f-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="23c5f-147">Se il .NET Core SDK usato include lo strumento, viene visualizzato un avviso:</span><span class="sxs-lookup"><span data-stu-id="23c5f-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="23c5f-148">Installare il pacchetto NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="23c5f-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="23c5f-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23c5f-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="23c5f-150">Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:</span><span class="sxs-lookup"><span data-stu-id="23c5f-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="23c5f-151">Lo strumento Gestione segreta Visualizza l'utilizzo di esempio, le opzioni e la guida ai comandi:</span><span class="sxs-lookup"><span data-stu-id="23c5f-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="23c5f-152">È necessario trovarsi nella stessa directory del file con *estensione csproj* per eseguire gli strumenti definiti negli `DotNetCliToolReference` elementi del file con estensione csproj.</span><span class="sxs-lookup"><span data-stu-id="23c5f-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="23c5f-153">Abilita archiviazione segreta</span><span class="sxs-lookup"><span data-stu-id="23c5f-153">Enable secret storage</span></span>

<span data-ttu-id="23c5f-154">Lo strumento Gestione Secret funziona sulle impostazioni di configurazione specifiche del progetto archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="23c5f-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="23c5f-155">Lo strumento di gestione dei segreti `init` include un comando in .NET Core SDK 3.0.100 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="23c5f-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="23c5f-156">Per usare i segreti utente, eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="23c5f-156">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="23c5f-157">Il comando precedente aggiunge un `UserSecretsId` elemento all'interno `PropertyGroup` di un del file con estensione *csproj* .</span><span class="sxs-lookup"><span data-stu-id="23c5f-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="23c5f-158">Per impostazione predefinita, il testo interno `UserSecretsId` di è un GUID.</span><span class="sxs-lookup"><span data-stu-id="23c5f-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="23c5f-159">Il testo interno è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="23c5f-160">Per usare i segreti utente, definire `UserSecretsId` un elemento in `PropertyGroup` un del file con estensione *csproj* .</span><span class="sxs-lookup"><span data-stu-id="23c5f-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="23c5f-161">Il testo interno di `UserSecretsId` è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="23c5f-162">Gli sviluppatori generano in genere un `UserSecretsId`GUID per il.</span><span class="sxs-lookup"><span data-stu-id="23c5f-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="23c5f-163">In Visual Studio fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci segreti utente** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="23c5f-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="23c5f-164">Questo movimento aggiunge un `UserSecretsId` elemento, popolato con un GUID, al file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="23c5f-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="23c5f-165">Imposta un segreto</span><span class="sxs-lookup"><span data-stu-id="23c5f-165">Set a secret</span></span>

<span data-ttu-id="23c5f-166">Definire un segreto dell'app costituito da una chiave e dal relativo valore.</span><span class="sxs-lookup"><span data-stu-id="23c5f-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="23c5f-167">Il segreto è associato al `UserSecretsId` valore del progetto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="23c5f-168">Ad esempio, eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="23c5f-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="23c5f-169">Nell'esempio precedente, i due punti indicano che `Movies` è un valore letterale di oggetto con una `ServiceApiKey` proprietà.</span><span class="sxs-lookup"><span data-stu-id="23c5f-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="23c5f-170">Lo strumento Gestione Secret può essere usato anche da altre directory.</span><span class="sxs-lookup"><span data-stu-id="23c5f-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="23c5f-171">Utilizzare l' `--project` opzione per specificare il percorso di file System in cui è presente il file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="23c5f-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="23c5f-172">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23c5f-172">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="23c5f-173">Flat Structure JSON in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="23c5f-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="23c5f-174">Il gesto di **gestione dei segreti utente** di Visual Studio apre un file *Secrets. JSON* nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="23c5f-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="23c5f-175">Sostituire il contenuto di *Secrets. JSON* con le coppie chiave-valore da archiviare.</span><span class="sxs-lookup"><span data-stu-id="23c5f-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="23c5f-176">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23c5f-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="23c5f-177">La struttura JSON è bidimensionale dopo le modifiche `dotnet user-secrets remove` tramite `dotnet user-secrets set`o.</span><span class="sxs-lookup"><span data-stu-id="23c5f-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="23c5f-178">Ad esempio, l' `dotnet user-secrets remove "Movies:ConnectionString"` esecuzione comprime il `Movies` valore letterale dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="23c5f-179">Il file modificato è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="23c5f-180">Impostare più segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-180">Set multiple secrets</span></span>

<span data-ttu-id="23c5f-181">È possibile impostare un batch di segreti inviando tramite pipe JSON `set` al comando.</span><span class="sxs-lookup"><span data-stu-id="23c5f-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="23c5f-182">Nell'esempio seguente il contenuto del file *input. JSON* viene inviato tramite pipe al `set` comando.</span><span class="sxs-lookup"><span data-stu-id="23c5f-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="23c5f-183">Windows</span><span class="sxs-lookup"><span data-stu-id="23c5f-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="23c5f-184">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-184">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="23c5f-185">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="23c5f-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="23c5f-186">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-186">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="23c5f-187">Accedere a un segreto</span><span class="sxs-lookup"><span data-stu-id="23c5f-187">Access a secret</span></span>

<span data-ttu-id="23c5f-188">L' [API di configurazione ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti di gestione segreti.</span><span class="sxs-lookup"><span data-stu-id="23c5f-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="23c5f-189">Se il progetto è destinato .NET Framework, installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="23c5f-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="23c5f-190">In ASP.NET Core 2,0 o versioni successive, l'origine configurazione dei segreti utente viene aggiunta automaticamente in modalità di sviluppo quando <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> il progetto chiama per inizializzare una nuova istanza dell'host con impostazioni predefinite preconfigurate.</span><span class="sxs-lookup"><span data-stu-id="23c5f-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="23c5f-191">`CreateDefaultBuilder`chiama <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> quando<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> è :<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development></span><span class="sxs-lookup"><span data-stu-id="23c5f-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="23c5f-192">Quando `CreateDefaultBuilder` non viene chiamato, aggiungere l'origine di configurazione dei segreti utente in <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> modo esplicito chiamando nel `Startup` costruttore.</span><span class="sxs-lookup"><span data-stu-id="23c5f-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="23c5f-193">Chiamare `AddUserSecrets` solo quando l'app è in esecuzione nell'ambiente di sviluppo, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="23c5f-194">Installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="23c5f-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="23c5f-195">Aggiungere l'origine di configurazione dei segreti utente con una <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> chiamata a `Startup` nel costruttore:</span><span class="sxs-lookup"><span data-stu-id="23c5f-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="23c5f-196">I segreti utente possono essere recuperati `Configuration` tramite l'API:</span><span class="sxs-lookup"><span data-stu-id="23c5f-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="23c5f-197">Mappare i segreti a un POCO</span><span class="sxs-lookup"><span data-stu-id="23c5f-197">Map secrets to a POCO</span></span>

<span data-ttu-id="23c5f-198">Il mapping di un intero valore letterale di oggetto a un oggetto POCO (una classe .NET semplice con proprietà) è utile per l'aggregazione di proprietà correlate.</span><span class="sxs-lookup"><span data-stu-id="23c5f-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="23c5f-199">Per eseguire il mapping dei segreti precedenti a un poco, `Configuration` usare la funzionalità di [associazione dell'oggetto grafico](xref:fundamentals/configuration/index#bind-to-an-object-graph) dell'API.</span><span class="sxs-lookup"><span data-stu-id="23c5f-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="23c5f-200">Il codice seguente è associato a un oggetto `MovieSettings` poco personalizzato e accede al `ServiceApiKey` valore della proprietà:</span><span class="sxs-lookup"><span data-stu-id="23c5f-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="23c5f-201">I `Movies:ConnectionString` segreti `Movies:ServiceApiKey` e vengono mappati alle rispettive proprietà `MovieSettings`in:</span><span class="sxs-lookup"><span data-stu-id="23c5f-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="23c5f-202">Sostituzione di stringhe con segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-202">String replacement with secrets</span></span>

<span data-ttu-id="23c5f-203">L'archiviazione delle password in testo normale non è protetta.</span><span class="sxs-lookup"><span data-stu-id="23c5f-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="23c5f-204">Ad esempio, una stringa di connessione del database archiviata in *appSettings. JSON* può includere una password per l'utente specificato:</span><span class="sxs-lookup"><span data-stu-id="23c5f-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="23c5f-205">Un approccio più sicuro consiste nell'archiviare la password come segreto.</span><span class="sxs-lookup"><span data-stu-id="23c5f-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="23c5f-206">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23c5f-206">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="23c5f-207">Rimuovere la `Password` coppia chiave-valore dalla stringa di connessione in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="23c5f-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="23c5f-208">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="23c5f-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="23c5f-209">Il valore del segreto può essere impostato sulla <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> proprietà di un oggetto per completare la stringa di connessione:</span><span class="sxs-lookup"><span data-stu-id="23c5f-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="23c5f-210">Elenca i segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="23c5f-211">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="23c5f-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="23c5f-212">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="23c5f-213">Nell'esempio precedente, i due punti nei nomi delle chiavi indicano la gerarchia di oggetti all'interno di *Secrets. JSON*.</span><span class="sxs-lookup"><span data-stu-id="23c5f-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="23c5f-214">Rimuovere un singolo segreto</span><span class="sxs-lookup"><span data-stu-id="23c5f-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="23c5f-215">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="23c5f-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="23c5f-216">Il file *Secrets. JSON* dell'app è stato modificato per rimuovere la coppia chiave-valore associata `MoviesConnectionString` alla chiave:</span><span class="sxs-lookup"><span data-stu-id="23c5f-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="23c5f-217">In `dotnet user-secrets list` esecuzione viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="23c5f-218">Rimuovi tutti i segreti</span><span class="sxs-lookup"><span data-stu-id="23c5f-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="23c5f-219">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="23c5f-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="23c5f-220">Tutti i segreti utente per l'app sono stati eliminati dal file *Secrets. JSON* :</span><span class="sxs-lookup"><span data-stu-id="23c5f-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="23c5f-221">In `dotnet user-secrets list` esecuzione viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="23c5f-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="23c5f-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="23c5f-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
