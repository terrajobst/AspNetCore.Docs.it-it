---
title: Archiviazione sicura di segreti dell'app durante sviluppo in ASP.NET Core
author: rick-anderson
description: Viene illustrato come archiviare in modo sicuro i segreti durante lo sviluppo
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 99a1129549d6b9802315c7e5accfa22907994a41
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="b3272-104">Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3272-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<a name=security-app-secrets></a>

<span data-ttu-id="b3272-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b3272-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="b3272-106">Questo documento illustra come è possibile utilizzare lo strumento di gestione di segreto nello sviluppo per proteggere le informazioni riservate fuori dal codice.</span><span class="sxs-lookup"><span data-stu-id="b3272-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="b3272-107">L'aspetto più importante è evitare di archiviare le password o altri dati sensibili nel codice sorgente e non è consigliabile utilizzare i segreti di produzione in modalità di sviluppo e test.</span><span class="sxs-lookup"><span data-stu-id="b3272-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="b3272-108">È possibile utilizzare invece il [configurazione](../fundamentals/configuration.md) sistema per la lettura di questi valori dalle variabili di ambiente o strumento da valori archiviati utilizzando la chiave privata di gestione.</span><span class="sxs-lookup"><span data-stu-id="b3272-108">You can instead use the [configuration](../fundamentals/configuration.md) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="b3272-109">Lo strumento di gestione di segreto consente di impedire che i dati sensibili viene archiviato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b3272-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="b3272-110">Il [configurazione](../fundamentals/configuration.md) sistema può leggere i segreti memorizzati con lo strumento di gestione di segreto descritto in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="b3272-110">The [configuration](../fundamentals/configuration.md) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="b3272-111">Lo strumento di gestione segreto viene utilizzato solo in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b3272-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="b3272-112">È possibile proteggere i segreti di test e di produzione Azure con il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b3272-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="b3272-113">Vedere [provider di configurazione insieme credenziali chiavi Azure](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="b3272-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="b3272-114">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="b3272-114">Environment variables</span></span>

<span data-ttu-id="b3272-115">Per evitare di archiviare i segreti dell'app nel codice o nel file di configurazione locali, archiviare i segreti in variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b3272-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="b3272-116">È possibile configurare il [configurazione](../fundamentals/configuration.md) framework per leggere i valori dalle variabili di ambiente chiamando `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b3272-116">You can setup the [configuration](../fundamentals/configuration.md) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="b3272-117">È quindi possibile utilizzare le variabili di ambiente per eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3272-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="b3272-118">Ad esempio, se si crea una nuova app web ASP.NET Core con singoli account utente, aggiungerà una stringa di connessione predefinito per il *appSettings. JSON* file nel progetto con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="b3272-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="b3272-119">La stringa di connessione predefinito sia configurato per usare LocalDB, che viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="b3272-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="b3272-120">Quando si distribuisce un'applicazione a un server di prova o di produzione, è possibile eseguire l'override di `DefaultConnection` valore della chiave con un'impostazione di variabili di ambiente che contiene la stringa di connessione (potenzialmente con credenziali sensibili) per un database di test o produzione Server.</span><span class="sxs-lookup"><span data-stu-id="b3272-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="b3272-121">Le variabili di ambiente sono in genere archiviate in testo normale e non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="b3272-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="b3272-122">Se il computer o processi sono compromesso, le variabili di ambiente è possibile accedere da parti non attendibili.</span><span class="sxs-lookup"><span data-stu-id="b3272-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="b3272-123">Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessarie.</span><span class="sxs-lookup"><span data-stu-id="b3272-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="b3272-124">Segreto Manager</span><span class="sxs-lookup"><span data-stu-id="b3272-124">Secret Manager</span></span>

<span data-ttu-id="b3272-125">Lo strumento di gestione Secret archivia i dati sensibili per operazioni di sviluppo di fuori della struttura ad albero di progetto.</span><span class="sxs-lookup"><span data-stu-id="b3272-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="b3272-126">Lo strumento di gestione di chiave privata è uno strumento di progetto che può essere utilizzato per archiviare informazioni riservate per un [.NET Core](https://microsoft.com/net/core) progetto durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b3272-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="b3272-127">Con lo strumento di gestione di chiave privata, è possibile associare i segreti dell'app a un progetto specifico e condividerli tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="b3272-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="b3272-128">Lo strumento di gestione di chiave privata non consente di crittografare i segreti archiviati e non deve essere considerato un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="b3272-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="b3272-129">È solo a fini di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b3272-129">It is for development purposes only.</span></span> <span data-ttu-id="b3272-130">Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b3272-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

### <a name="visual-studio-2017-installing-the-secret-manager-tool"></a><span data-ttu-id="b3272-131">Visual Studio 2017: L'installazione lo strumento di gestione di segreto</span><span class="sxs-lookup"><span data-stu-id="b3272-131">Visual Studio 2017: Installing the Secret Manager tool</span></span>

<span data-ttu-id="b3272-132">Fare clic sul progetto in Esplora soluzioni e selezionare **modifica \<project_name\>csproj** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b3272-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="b3272-133">Aggiungere la riga evidenziata per il *csproj* e salvataggio dei file per ripristinare il pacchetto NuGet associato:</span><span class="sxs-lookup"><span data-stu-id="b3272-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

<span data-ttu-id="b3272-134">[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="b3272-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="b3272-135">Fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b3272-135">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b3272-136">Questa azione aggiunge un nuovo `UserSecretsId` nodo all'interno di un `PropertyGroup` del *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b3272-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="b3272-137">Inoltre, viene aperto un `secrets.json` file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b3272-137">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="b3272-138">Aggiungere quanto segue a `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="b3272-138">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-2015-installing-the-secret-manager-tool"></a><span data-ttu-id="b3272-139">Visual Studio 2015: Lo strumento di gestione di segreto l'installazione</span><span class="sxs-lookup"><span data-stu-id="b3272-139">Visual Studio 2015: Installing the Secret Manager tool</span></span>

<span data-ttu-id="b3272-140">Aprire il progetto `project.json` file.</span><span class="sxs-lookup"><span data-stu-id="b3272-140">Open the project's `project.json` file.</span></span> <span data-ttu-id="b3272-141">Aggiungere un riferimento a `Microsoft.Extensions.SecretManager.Tools` all'interno di `tools` , proprietà e salvataggio per ripristinare il pacchetto NuGet associato:</span><span class="sxs-lookup"><span data-stu-id="b3272-141">Add a reference to `Microsoft.Extensions.SecretManager.Tools` within the `tools` property, and save to restore the associated NuGet package:</span></span>

```json
"tools": {
    "Microsoft.Extensions.SecretManager.Tools": "1.0.0-preview2-final",
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
},
```

<span data-ttu-id="b3272-142">Fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b3272-142">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b3272-143">Questa azione aggiunge un nuovo `userSecretsId` proprietà `project.json`.</span><span class="sxs-lookup"><span data-stu-id="b3272-143">This gesture adds a new `userSecretsId` property to `project.json`.</span></span> <span data-ttu-id="b3272-144">Inoltre, viene aperto un `secrets.json` file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b3272-144">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="b3272-145">Aggiungere quanto segue a `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="b3272-145">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-code-or-command-line-installing-the-secret-manager-tool"></a><span data-ttu-id="b3272-146">Visual Studio Code o della riga di comando: installazione dello strumento di gestione segreto</span><span class="sxs-lookup"><span data-stu-id="b3272-146">Visual Studio Code or Command Line: Installing the Secret Manager tool</span></span>

<span data-ttu-id="b3272-147">Aggiungere `Microsoft.Extensions.SecretManager.Tools` per il *csproj* file ed eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="b3272-147">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span>

<span data-ttu-id="b3272-148">[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="b3272-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="b3272-149">Testare lo strumento di gestione Secret eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b3272-149">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="b3272-150">Utilizzo, opzioni della Guida in linea di comando, verrà visualizzato lo strumento di gestione di chiave privata.</span><span class="sxs-lookup"><span data-stu-id="b3272-150">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="b3272-151">È necessario essere nella stessa directory di *csproj* l'esecuzione di strumenti definiti nel file il *csproj* del file `DotNetCliToolReference` nodi.</span><span class="sxs-lookup"><span data-stu-id="b3272-151">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="b3272-152">Lo strumento di gestione di segreto opera sulle impostazioni di configurazione specifici del progetto che vengono archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="b3272-152">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="b3272-153">Per utilizzare informazioni riservate dell'utente, è necessario specificare il progetto un `UserSecretsId` valore nella relativa *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b3272-153">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="b3272-154">Il valore di `UserSecretsId` è arbitrario ma in genere univoco al progetto.</span><span class="sxs-lookup"><span data-stu-id="b3272-154">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="b3272-155">Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="b3272-155">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="b3272-156">Aggiungere un `UserSecretsId` per il progetto nel *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="b3272-156">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

<span data-ttu-id="b3272-157">[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="b3272-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span></span>

<span data-ttu-id="b3272-158">Utilizzare lo strumento di gestione di segreto per impostare un segreto.</span><span class="sxs-lookup"><span data-stu-id="b3272-158">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="b3272-159">Ad esempio, nella finestra di comando dalla directory del progetto, immettere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b3272-159">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="b3272-160">È possibile eseguire lo strumento di gestione di segreto da altre directory, ma è necessario utilizzare il `--project` opzione per passare il percorso per il *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="b3272-160">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="b3272-161">Inoltre, è possibile utilizzare lo strumento di gestione di segreto per elenco, rimuovere e cancellare i segreti dell'app.</span><span class="sxs-lookup"><span data-stu-id="b3272-161">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="b3272-162">L'accesso a informazioni riservate dell'utente tramite la configurazione</span><span class="sxs-lookup"><span data-stu-id="b3272-162">Accessing user secrets via configuration</span></span>

<span data-ttu-id="b3272-163">Segreto Manager segreti sono accessibili tramite il sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b3272-163">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="b3272-164">Aggiungere il `Microsoft.Extensions.Configuration.UserSecrets` pacchetto ed eseguire `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="b3272-164">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="b3272-165">Aggiungere l'origine di configurazione di segreti utente per il `Startup` metodo:</span><span class="sxs-lookup"><span data-stu-id="b3272-165">Add the user secrets configuration source to the `Startup` method:</span></span>

<span data-ttu-id="b3272-166">[!code-csharp[Principale](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span><span class="sxs-lookup"><span data-stu-id="b3272-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span></span>

<span data-ttu-id="b3272-167">È possibile accedere a informazioni riservate dell'utente tramite l'API di configurazione:</span><span class="sxs-lookup"><span data-stu-id="b3272-167">You can access user secrets via the configuration API:</span></span>

<span data-ttu-id="b3272-168">[!code-csharp[Principale](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="b3272-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="b3272-169">Il segreto Manager funzionamento dello strumento</span><span class="sxs-lookup"><span data-stu-id="b3272-169">How the Secret Manager tool works</span></span>

<span data-ttu-id="b3272-170">Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati.</span><span class="sxs-lookup"><span data-stu-id="b3272-170">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="b3272-171">È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b3272-171">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="b3272-172">Nella versione corrente, i valori vengono archiviati un [JSON](http://json.org/) file di configurazione nella directory di profilo dell'utente:</span><span class="sxs-lookup"><span data-stu-id="b3272-172">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="b3272-173">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b3272-173">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="b3272-174">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b3272-174">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="b3272-175">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b3272-175">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="b3272-176">Il valore di `userSecretsId` proviene dal valore specificato in *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b3272-176">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="b3272-177">È consigliabile non scrivere codice che dipende il percorso o il formato dei dati salvati con lo strumento di gestione di chiave privata, come è potrebbero modificare questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b3272-177">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="b3272-178">Ad esempio, i valori secret sono attualmente *non* crittografati oggi, ma può essere un giorno.</span><span class="sxs-lookup"><span data-stu-id="b3272-178">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b3272-179">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b3272-179">Additional Resources</span></span>

* [<span data-ttu-id="b3272-180">Configurazione</span><span class="sxs-lookup"><span data-stu-id="b3272-180">Configuration</span></span>](../fundamentals/configuration.md)
