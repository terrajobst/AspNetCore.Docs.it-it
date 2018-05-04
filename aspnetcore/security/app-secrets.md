---
title: Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Viene illustrato come archiviare in modo sicuro i segreti durante lo sviluppo
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 0a04f5762a35426f342b58b8b60288c66c057ae7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="b774c-103">Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b774c-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="b774c-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b774c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="b774c-105">Questo documento illustra come è possibile utilizzare lo strumento di gestione di segreto nello sviluppo per proteggere le informazioni riservate fuori dal codice.</span><span class="sxs-lookup"><span data-stu-id="b774c-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="b774c-106">L'aspetto più importante è evitare di archiviare le password o altri dati sensibili nel codice sorgente e non è consigliabile utilizzare i segreti di produzione in modalità di sviluppo e test.</span><span class="sxs-lookup"><span data-stu-id="b774c-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="b774c-107">È possibile utilizzare invece il [configurazione](xref:fundamentals/configuration/index) sistema per la lettura di questi valori dalle variabili di ambiente o strumento da valori archiviati utilizzando la chiave privata di gestione.</span><span class="sxs-lookup"><span data-stu-id="b774c-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="b774c-108">Lo strumento di gestione di segreto consente di impedire che i dati sensibili viene archiviato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b774c-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="b774c-109">Il [configurazione](xref:fundamentals/configuration/index) sistema può leggere i segreti memorizzati con lo strumento di gestione di segreto descritto in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="b774c-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="b774c-110">Lo strumento di gestione segreto viene utilizzato solo in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b774c-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="b774c-111">È possibile proteggere i segreti di test e di produzione Azure con il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b774c-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="b774c-112">Vedere [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="b774c-112">See [Azure Key Vault configuration provider](xref:security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="b774c-113">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="b774c-113">Environment variables</span></span>

<span data-ttu-id="b774c-114">Per evitare di archiviare i segreti dell'app nel codice o nel file di configurazione locali, archiviare i segreti in variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="b774c-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="b774c-115">È possibile configurare il [configurazione](xref:fundamentals/configuration/index) framework per leggere i valori dalle variabili di ambiente chiamando `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b774c-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="b774c-116">È quindi possibile utilizzare le variabili di ambiente per eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b774c-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="b774c-117">Ad esempio, se si crea una nuova app web ASP.NET Core con singoli account utente, aggiungerà una stringa di connessione predefinito per il *appSettings. JSON* file nel progetto con la chiave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="b774c-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="b774c-118">La stringa di connessione predefinito sia configurato per usare LocalDB, che viene eseguito in modalità utente e non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="b774c-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="b774c-119">Quando si distribuisce un'applicazione a un server di prova o di produzione, è possibile eseguire l'override di `DefaultConnection` valore della chiave con un'impostazione di variabili di ambiente che contiene la stringa di connessione (potenzialmente con credenziali sensibili) per un database di test o produzione Server.</span><span class="sxs-lookup"><span data-stu-id="b774c-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="b774c-120">Le variabili di ambiente sono in genere archiviate in testo normale e non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="b774c-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="b774c-121">Se il computer o processi sono compromesso, le variabili di ambiente è possibile accedere da parti non attendibili.</span><span class="sxs-lookup"><span data-stu-id="b774c-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="b774c-122">Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessarie.</span><span class="sxs-lookup"><span data-stu-id="b774c-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="b774c-123">Segreto Manager</span><span class="sxs-lookup"><span data-stu-id="b774c-123">Secret Manager</span></span>

<span data-ttu-id="b774c-124">Lo strumento di gestione Secret archivia i dati sensibili per operazioni di sviluppo di fuori della struttura ad albero di progetto.</span><span class="sxs-lookup"><span data-stu-id="b774c-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="b774c-125">Lo strumento di gestione di chiave privata è uno strumento di progetto che può essere utilizzato per archiviare informazioni riservate per un progetto .NET Core durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b774c-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="b774c-126">Con lo strumento di gestione di chiave privata, è possibile associare i segreti dell'app a un progetto specifico e condividerli tra più progetti.</span><span class="sxs-lookup"><span data-stu-id="b774c-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="b774c-127">Lo strumento di gestione di segreto non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile.</span><span class="sxs-lookup"><span data-stu-id="b774c-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="b774c-128">È solo a fini di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b774c-128">It's for development purposes only.</span></span> <span data-ttu-id="b774c-129">Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b774c-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="b774c-130">Installazione dello strumento di gestione segreto</span><span class="sxs-lookup"><span data-stu-id="b774c-130">Installing the Secret Manager tool</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b774c-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b774c-131">Visual Studio</span></span>](#tab/visual-studio/)
<span data-ttu-id="b774c-132">Fare clic sul progetto in Esplora soluzioni e selezionare **modifica \<project_name\>csproj** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b774c-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="b774c-133">Aggiungere la riga evidenziata per il *csproj* e salvataggio dei file per ripristinare il pacchetto NuGet associato:</span><span class="sxs-lookup"><span data-stu-id="b774c-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="b774c-134">Fare di nuovo il progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="b774c-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="b774c-135">Questa azione aggiunge un nuovo `UserSecretsId` nodo all'interno di un `PropertyGroup` del *csproj* file, come mostrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="b774c-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="b774c-136">Salvataggio modificato *csproj* anche file viene aperto un `secrets.json` file nell'editor di testo.</span><span class="sxs-lookup"><span data-stu-id="b774c-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="b774c-137">Sostituire il contenuto del `secrets.json` file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b774c-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b774c-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b774c-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)
<span data-ttu-id="b774c-139">Aggiungere `Microsoft.Extensions.SecretManager.Tools` per il *csproj* file ed eseguire [ripristino dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="b774c-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="b774c-140">È possibile utilizzare gli stessi passaggi per installare lo strumento di gestione di segreto tramite riga di comando.</span><span class="sxs-lookup"><span data-stu-id="b774c-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="b774c-141">Testare lo strumento di gestione Secret eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b774c-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="b774c-142">Utilizzo, opzioni della Guida in linea di comando, verrà visualizzato lo strumento di gestione di chiave privata.</span><span class="sxs-lookup"><span data-stu-id="b774c-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="b774c-143">È necessario essere nella stessa directory di *csproj* l'esecuzione di strumenti definiti nel file il *csproj* del file `DotNetCliToolReference` nodi.</span><span class="sxs-lookup"><span data-stu-id="b774c-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="b774c-144">Lo strumento di gestione di segreto opera sulle impostazioni di configurazione specifici del progetto che vengono archiviate nel profilo utente.</span><span class="sxs-lookup"><span data-stu-id="b774c-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="b774c-145">Per utilizzare informazioni riservate dell'utente, è necessario specificare il progetto un `UserSecretsId` valore nella relativa *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b774c-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="b774c-146">Il valore di `UserSecretsId` è arbitrario ma in genere univoco al progetto.</span><span class="sxs-lookup"><span data-stu-id="b774c-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="b774c-147">Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="b774c-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="b774c-148">Aggiungere un `UserSecretsId` per il progetto nel *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="b774c-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="b774c-149">Utilizzare lo strumento di gestione di segreto per impostare un segreto.</span><span class="sxs-lookup"><span data-stu-id="b774c-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="b774c-150">Ad esempio, nella finestra di comando dalla directory del progetto, immettere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b774c-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="b774c-151">È possibile eseguire lo strumento di gestione di segreto da altre directory, ma è necessario utilizzare il `--project` opzione per passare il percorso per il *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="b774c-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="b774c-152">Inoltre, è possibile utilizzare lo strumento di gestione di segreto per elenco, rimuovere e cancellare i segreti dell'app.</span><span class="sxs-lookup"><span data-stu-id="b774c-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

* * *
## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="b774c-153">L'accesso a informazioni riservate dell'utente tramite la configurazione</span><span class="sxs-lookup"><span data-stu-id="b774c-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="b774c-154">Segreto Manager segreti sono accessibili tramite il sistema di configurazione.</span><span class="sxs-lookup"><span data-stu-id="b774c-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="b774c-155">Aggiungere il `Microsoft.Extensions.Configuration.UserSecrets` pacchetto ed eseguire [ripristino dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="b774c-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="b774c-156">Aggiungere l'origine di configurazione di segreti utente per il `Startup` metodo:</span><span class="sxs-lookup"><span data-stu-id="b774c-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="b774c-157">È possibile accedere a informazioni riservate dell'utente tramite l'API di configurazione:</span><span class="sxs-lookup"><span data-stu-id="b774c-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="b774c-158">Il segreto Manager funzionamento dello strumento</span><span class="sxs-lookup"><span data-stu-id="b774c-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="b774c-159">Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati.</span><span class="sxs-lookup"><span data-stu-id="b774c-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="b774c-160">È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b774c-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="b774c-161">Nella versione corrente, i valori vengono archiviati un [JSON](http://json.org/) file di configurazione nella directory di profilo dell'utente:</span><span class="sxs-lookup"><span data-stu-id="b774c-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="b774c-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b774c-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="b774c-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b774c-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="b774c-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="b774c-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="b774c-165">Il valore di `userSecretsId` proviene dal valore specificato in *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="b774c-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="b774c-166">Non deve scrivere codice che dipende il percorso o il formato dei dati salvati con lo strumento di gestione di chiave privata, come è potrebbero modificare questi dettagli di implementazione.</span><span class="sxs-lookup"><span data-stu-id="b774c-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="b774c-167">Ad esempio, i valori secret sono attualmente *non* crittografati oggi, ma può essere un giorno.</span><span class="sxs-lookup"><span data-stu-id="b774c-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b774c-168">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b774c-168">Additional resources</span></span>

* [<span data-ttu-id="b774c-169">Configurazione</span><span class="sxs-lookup"><span data-stu-id="b774c-169">Configuration</span></span>](xref:fundamentals/configuration/index)
