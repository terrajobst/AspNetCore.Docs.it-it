---
title: Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare informazioni riservate come segreti dell'app durante lo sviluppo di un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 0bbd6af01ce3a29d3931faa2853a50dc895490cc
ms.sourcegitcommit: fd2483f0a384b1c479c5b4af025ee46917db1919
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868030"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="508e7-103">Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="508e7-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="508e7-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="508e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="508e7-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="508e7-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="508e7-106">Questo documento illustra le tecniche per l'archiviazione e il recupero di dati sensibili durante lo sviluppo di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="508e7-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="508e7-107">Non archiviare mai le password o altri dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="508e7-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="508e7-108">I segreti di produzione non devono essere usati per lo sviluppo o il test.</span><span class="sxs-lookup"><span data-stu-id="508e7-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="508e7-109">I segreti non devono essere distribuiti con l'app.</span><span class="sxs-lookup"><span data-stu-id="508e7-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="508e7-110">Al contrario, i segreti devono essere resi disponibili nell'ambiente di produzione tramite un mezzo controllato, come le variabili di ambiente, Azure Key Vault e così via. È possibile archiviare e proteggere i segreti di test e di produzione di Azure con il [provider di configurazione Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="508e7-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="508e7-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="508e7-111">Environment variables</span></span>

<span data-ttu-id="508e7-112">Le variabili di ambiente vengono usate per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locali.</span><span class="sxs-lookup"><span data-stu-id="508e7-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="508e7-113">Le variabili di ambiente sostituiscono i valori di configurazione per tutte le origini di configurazione precedentemente specificate.</span><span class="sxs-lookup"><span data-stu-id="508e7-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="508e7-114">Configurare la lettura dei valori delle variabili di ambiente chiamando <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="508e7-114">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="508e7-115">Si consideri un'app Web ASP.NET Core in cui è abilitata la sicurezza dei **singoli account utente** .</span><span class="sxs-lookup"><span data-stu-id="508e7-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="508e7-116">Una stringa di connessione al database predefinita è inclusa nel file *appSettings. JSON* del progetto con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="508e7-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="508e7-117">La stringa di connessione predefinita è per il database locale, che viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="508e7-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="508e7-118">Durante la distribuzione dell'app, è possibile eseguire l'override del valore della chiave di `DefaultConnection` con il valore di una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="508e7-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="508e7-119">La variabile di ambiente può archiviare la stringa di connessione completa con credenziali riservate.</span><span class="sxs-lookup"><span data-stu-id="508e7-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="508e7-120">Le variabili di ambiente vengono in genere archiviate in testo normale e non crittografato.</span><span class="sxs-lookup"><span data-stu-id="508e7-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="508e7-121">Se il computer o il processo è compromesso, le variabili di ambiente possono essere accessibili da entità non attendibili.</span><span class="sxs-lookup"><span data-stu-id="508e7-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="508e7-122">Potrebbero essere necessarie misure aggiuntive per impedire la divulgazione di segreti utente.</span><span class="sxs-lookup"><span data-stu-id="508e7-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="508e7-123">Gestione segreta</span><span class="sxs-lookup"><span data-stu-id="508e7-123">Secret Manager</span></span>

<span data-ttu-id="508e7-124">Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="508e7-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="508e7-125">In questo contesto, una parte di dati sensibili è un segreto dell'app.</span><span class="sxs-lookup"><span data-stu-id="508e7-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="508e7-126">I segreti dell'app vengono archiviati in un percorso separato dall'albero del progetto.</span><span class="sxs-lookup"><span data-stu-id="508e7-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="508e7-127">I segreti dell'app sono associati a un progetto specifico o condivisi tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="508e7-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="508e7-128">I segreti dell'app non vengono archiviati nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="508e7-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="508e7-129">Lo strumento Secret Manager non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="508e7-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="508e7-130">È solo a scopo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="508e7-130">It's for development purposes only.</span></span> <span data-ttu-id="508e7-131">Le chiavi e i valori vengono archiviati in un file di configurazione JSON nella directory del profilo utente.</span><span class="sxs-lookup"><span data-stu-id="508e7-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="508e7-132">Funzionamento dello strumento di gestione dei segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="508e7-133">Lo strumento di gestione dei segreti estrae i dettagli di implementazione, ad esempio dove e come vengono archiviati i valori.</span><span class="sxs-lookup"><span data-stu-id="508e7-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="508e7-134">È possibile utilizzare lo strumento senza conoscere i dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="508e7-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="508e7-135">I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente protetta dal sistema nel computer locale:</span><span class="sxs-lookup"><span data-stu-id="508e7-135">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="508e7-136">Windows</span><span class="sxs-lookup"><span data-stu-id="508e7-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="508e7-137">Percorso del file System:</span><span class="sxs-lookup"><span data-stu-id="508e7-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="508e7-138">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="508e7-138">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="508e7-139">Percorso del file System:</span><span class="sxs-lookup"><span data-stu-id="508e7-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="508e7-140">Nei percorsi di file precedenti sostituire `<user_secrets_id>` con il valore di `UserSecretsId` specificato nel file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="508e7-141">Non scrivere codice che dipende dalla posizione o dal formato dei dati salvati con lo strumento di gestione dei segreti.</span><span class="sxs-lookup"><span data-stu-id="508e7-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="508e7-142">Questi dettagli di implementazione possono cambiare.</span><span class="sxs-lookup"><span data-stu-id="508e7-142">These implementation details may change.</span></span> <span data-ttu-id="508e7-143">Ad esempio, i valori dei segreti non sono crittografati, ma potrebbero essere futuri.</span><span class="sxs-lookup"><span data-stu-id="508e7-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="508e7-144">Installare lo strumento di gestione dei segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="508e7-145">Lo strumento di gestione dei segreti è integrato con il interfaccia della riga di comando di .NET Core in .NET Core SDK 2.1.300 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="508e7-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="508e7-146">Per .NET Core SDK versioni precedenti a 2.1.300, è necessaria l'installazione dello strumento.</span><span class="sxs-lookup"><span data-stu-id="508e7-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="508e7-147">Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione .NET Core SDK installato.</span><span class="sxs-lookup"><span data-stu-id="508e7-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="508e7-148">Se il .NET Core SDK usato include lo strumento, viene visualizzato un avviso:</span><span class="sxs-lookup"><span data-stu-id="508e7-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="508e7-149">Installare il pacchetto NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) nel progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="508e7-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="508e7-150">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="508e7-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="508e7-151">Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:</span><span class="sxs-lookup"><span data-stu-id="508e7-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="508e7-152">Lo strumento Gestione segreta Visualizza l'utilizzo di esempio, le opzioni e la guida ai comandi:</span><span class="sxs-lookup"><span data-stu-id="508e7-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="508e7-153">È necessario trovarsi nella stessa directory del file con *estensione csproj* per eseguire gli strumenti definiti negli elementi `DotNetCliToolReference` del file con estensione *csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="508e7-154">Abilita archiviazione segreta</span><span class="sxs-lookup"><span data-stu-id="508e7-154">Enable secret storage</span></span>

<span data-ttu-id="508e7-155">Lo strumento Gestione Secret funziona sulle impostazioni di configurazione specifiche del progetto archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="508e7-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="508e7-156">Lo strumento Gestione Secret include un comando `init` in .NET Core SDK 3.0.100 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="508e7-156">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="508e7-157">Per usare i segreti utente, eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="508e7-157">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="508e7-158">Il comando precedente aggiunge un elemento `UserSecretsId` in un `PropertyGroup` del file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-158">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="508e7-159">Per impostazione predefinita, il testo interno del `UserSecretsId` è un GUID.</span><span class="sxs-lookup"><span data-stu-id="508e7-159">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="508e7-160">Il testo interno è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="508e7-160">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="508e7-161">Per usare i segreti utente, definire un elemento `UserSecretsId` in un `PropertyGroup` del file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-161">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="508e7-162">Il testo interno del `UserSecretsId` è arbitrario, ma è univoco per il progetto.</span><span class="sxs-lookup"><span data-stu-id="508e7-162">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="508e7-163">Gli sviluppatori generano in genere un GUID per la `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="508e7-163">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="508e7-164">In Visual Studio fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci segreti utente** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="508e7-164">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="508e7-165">Questo movimento aggiunge un elemento `UserSecretsId`, popolato con un GUID, al file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-165">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="508e7-166">Imposta un segreto</span><span class="sxs-lookup"><span data-stu-id="508e7-166">Set a secret</span></span>

<span data-ttu-id="508e7-167">Definire un segreto dell'app costituito da una chiave e dal relativo valore.</span><span class="sxs-lookup"><span data-stu-id="508e7-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="508e7-168">Il segreto è associato al valore `UserSecretsId` del progetto.</span><span class="sxs-lookup"><span data-stu-id="508e7-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="508e7-169">Ad esempio, eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="508e7-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="508e7-170">Nell'esempio precedente, i due punti indicano che `Movies` è un valore letterale di oggetto con una proprietà `ServiceApiKey`.</span><span class="sxs-lookup"><span data-stu-id="508e7-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="508e7-171">Lo strumento Gestione Secret può essere usato anche da altre directory.</span><span class="sxs-lookup"><span data-stu-id="508e7-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="508e7-172">Utilizzare l'opzione `--project` per specificare il percorso di file system in cui è presente il file con *estensione csproj* .</span><span class="sxs-lookup"><span data-stu-id="508e7-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="508e7-173">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="508e7-173">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="508e7-174">Flat Structure JSON in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="508e7-174">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="508e7-175">Il gesto di **gestione dei segreti utente** di Visual Studio apre un file *Secrets. JSON* nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="508e7-175">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="508e7-176">Sostituire il contenuto di *Secrets. JSON* con le coppie chiave-valore da archiviare.</span><span class="sxs-lookup"><span data-stu-id="508e7-176">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="508e7-177">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="508e7-177">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="508e7-178">La struttura JSON viene resa Flat dopo le modifiche tramite `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="508e7-178">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="508e7-179">Ad esempio, l'esecuzione di `dotnet user-secrets remove "Movies:ConnectionString"` comprime il valore letterale dell'oggetto `Movies`.</span><span class="sxs-lookup"><span data-stu-id="508e7-179">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="508e7-180">Il file modificato è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-180">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="508e7-181">Impostare più segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-181">Set multiple secrets</span></span>

<span data-ttu-id="508e7-182">È possibile impostare un batch di segreti inviando tramite pipe JSON al comando `set`.</span><span class="sxs-lookup"><span data-stu-id="508e7-182">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="508e7-183">Nell'esempio seguente il contenuto del file *input. JSON* viene inviato tramite pipe al comando `set`.</span><span class="sxs-lookup"><span data-stu-id="508e7-183">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="508e7-184">Windows</span><span class="sxs-lookup"><span data-stu-id="508e7-184">Windows</span></span>](#tab/windows)

<span data-ttu-id="508e7-185">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-185">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="508e7-186">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="508e7-186">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="508e7-187">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-187">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="508e7-188">Accedere a un segreto</span><span class="sxs-lookup"><span data-stu-id="508e7-188">Access a secret</span></span>

<span data-ttu-id="508e7-189">L' [API di configurazione ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti di gestione segreti.</span><span class="sxs-lookup"><span data-stu-id="508e7-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="508e7-190">Se il progetto è destinato .NET Framework, installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="508e7-190">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="508e7-191">In ASP.NET Core 2,0 o versioni successive, l'origine di configurazione dei segreti utente viene aggiunta automaticamente in modalità di sviluppo quando il progetto chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> per inizializzare una nuova istanza dell'host con impostazioni predefinite preconfigurate.</span><span class="sxs-lookup"><span data-stu-id="508e7-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="508e7-192">`CreateDefaultBuilder` chiama <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> quando il <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> è <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="508e7-192">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="508e7-193">Quando non viene chiamato `CreateDefaultBuilder`, aggiungere l'origine di configurazione dei segreti utente in modo esplicito chiamando <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> nel costruttore di `Startup`.</span><span class="sxs-lookup"><span data-stu-id="508e7-193">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="508e7-194">Chiamare `AddUserSecrets` solo quando l'app è in esecuzione nell'ambiente di sviluppo, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-194">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="508e7-195">Installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="508e7-195">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="508e7-196">Aggiungere l'origine di configurazione dei segreti utente con una chiamata a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="508e7-196">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="508e7-197">I segreti utente possono essere recuperati tramite l'API `Configuration`:</span><span class="sxs-lookup"><span data-stu-id="508e7-197">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="508e7-198">Mappare i segreti a un POCO</span><span class="sxs-lookup"><span data-stu-id="508e7-198">Map secrets to a POCO</span></span>

<span data-ttu-id="508e7-199">Il mapping di un intero valore letterale di oggetto a un oggetto POCO (una classe .NET semplice con proprietà) è utile per l'aggregazione di proprietà correlate.</span><span class="sxs-lookup"><span data-stu-id="508e7-199">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="508e7-200">Per eseguire il mapping dei segreti precedenti a un POCO, usare la funzionalità di [associazione dell'oggetto grafico](xref:fundamentals/configuration/index#bind-to-an-object-graph) dell'API `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="508e7-200">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="508e7-201">Il codice seguente esegue il binding a un oggetto personalizzato `MovieSettings` POCO e accede al valore della proprietà `ServiceApiKey`:</span><span class="sxs-lookup"><span data-stu-id="508e7-201">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="508e7-202">Viene eseguito il mapping dei segreti `Movies:ConnectionString` e `Movies:ServiceApiKey` alle rispettive proprietà in `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="508e7-202">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="508e7-203">Sostituzione di stringhe con segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-203">String replacement with secrets</span></span>

<span data-ttu-id="508e7-204">L'archiviazione delle password in testo normale non è protetta.</span><span class="sxs-lookup"><span data-stu-id="508e7-204">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="508e7-205">Ad esempio, una stringa di connessione del database archiviata in *appSettings. JSON* può includere una password per l'utente specificato:</span><span class="sxs-lookup"><span data-stu-id="508e7-205">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="508e7-206">Un approccio più sicuro consiste nell'archiviare la password come segreto.</span><span class="sxs-lookup"><span data-stu-id="508e7-206">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="508e7-207">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="508e7-207">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="508e7-208">Rimuovere la coppia chiave-valore `Password` dalla stringa di connessione in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="508e7-208">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="508e7-209">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="508e7-209">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="508e7-210">Il valore del segreto può essere impostato sulla proprietà <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> di un oggetto <xref:System.Data.SqlClient.SqlConnectionStringBuilder> per completare la stringa di connessione:</span><span class="sxs-lookup"><span data-stu-id="508e7-210">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="508e7-211">Elenca i segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-211">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="508e7-212">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="508e7-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="508e7-213">Viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-213">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="508e7-214">Nell'esempio precedente, i due punti nei nomi delle chiavi indicano la gerarchia di oggetti all'interno di *Secrets. JSON*.</span><span class="sxs-lookup"><span data-stu-id="508e7-214">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="508e7-215">Rimuovere un singolo segreto</span><span class="sxs-lookup"><span data-stu-id="508e7-215">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="508e7-216">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="508e7-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="508e7-217">Il file *Secrets. JSON* dell'app è stato modificato per rimuovere la coppia chiave-valore associata alla chiave di `MoviesConnectionString`:</span><span class="sxs-lookup"><span data-stu-id="508e7-217">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="508e7-218">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="508e7-219">Rimuovi tutti i segreti</span><span class="sxs-lookup"><span data-stu-id="508e7-219">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="508e7-220">Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :</span><span class="sxs-lookup"><span data-stu-id="508e7-220">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="508e7-221">Tutti i segreti utente per l'app sono stati eliminati dal file *Secrets. JSON* :</span><span class="sxs-lookup"><span data-stu-id="508e7-221">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="508e7-222">In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="508e7-222">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="508e7-223">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="508e7-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
