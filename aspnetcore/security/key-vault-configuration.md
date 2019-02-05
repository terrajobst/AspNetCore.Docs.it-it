---
title: Azure Key Vault Configuration Provider in ASP.NET Core
author: guardrex
description: Informazioni su come usare il Provider di configurazione dell'insieme di credenziali chiave Azure per configurare un'app usando coppie nome-valore in fase di esecuzione.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 8e40c8308a692731e71fb8ebebfc64e606874290
ms.sourcegitcommit: 98e9c7187772d4ddefe6d8e85d0d206749dbd2ef
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55737655"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="e26fe-103">Azure Key Vault Configuration Provider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e26fe-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="e26fe-104">Dal [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="e26fe-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="e26fe-105">Questo documento illustra come usare il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Provider di configurazione per caricare i valori di configurazione di app da segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e26fe-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="e26fe-106">Azure Key Vault è un servizio basato sul cloud che semplifica la protezione delle chiavi crittografiche e segreti usati da App e servizi.</span><span class="sxs-lookup"><span data-stu-id="e26fe-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="e26fe-107">Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core sono:</span><span class="sxs-lookup"><span data-stu-id="e26fe-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="e26fe-108">Controllo dell'accesso ai dati di configurazione sensibili.</span><span class="sxs-lookup"><span data-stu-id="e26fe-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="e26fe-109">Soddisfa il requisito per FIPS 140-2 livello 2 convalidati i moduli di protezione Hardware (HSM) quando si archiviano i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="e26fe-110">Questo scenario è disponibile per le app destinate a ASP.NET Core 2.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="e26fe-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="e26fe-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e26fe-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="e26fe-112">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="e26fe-112">Packages</span></span>

<span data-ttu-id="e26fe-113">Per usare Azure Key Vault Configuration Provider, aggiungere un riferimento al pacchetto di [azurekeyvault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="e26fe-114">Per adottare lo scenario di identità del servizio gestito di Azure, aggiungere un riferimento al pacchetto di [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-114">To adopt the Azure Managed Service Identity scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="e26fe-115">Al momento della scrittura, la versione stabile più recente di `Microsoft.Azure.Services.AppAuthentication`, versione `1.0.3`, fornisce il supporto per [assegnato dal sistema gestito identità](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="e26fe-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="e26fe-116">Supporto per *assegnata dall'utente gestite delle identità* è disponibile nel `1.0.2-preview` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="e26fe-117">In questo argomento viene illustrato l'utilizzo delle identità gestite dal sistema e l'app di esempio fornito utilizza la versione `1.0.3` del `Microsoft.Azure.Services.AppAuthentication` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="e26fe-118">App di esempio</span><span class="sxs-lookup"><span data-stu-id="e26fe-118">Sample app</span></span>

<span data-ttu-id="e26fe-119">L'app di esempio viene eseguito in una delle due modalità di base di `#define` istruzione all'inizio del *Program.cs* file:</span><span class="sxs-lookup"><span data-stu-id="e26fe-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="e26fe-120">`Basic` &ndash; Illustra l'uso di un ID applicazione dell'insieme di credenziali chiave di Azure e una Password (segreto Client) per accedere ai segreti archiviati nell'insieme di credenziali chiave.</span><span class="sxs-lookup"><span data-stu-id="e26fe-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="e26fe-121">Distribuire il `Basic` versione dell'esempio in qualsiasi host in grado di servire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e26fe-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="e26fe-122">`Managed` &ndash; Illustra l'uso di Azure [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) eseguire l'autenticazione all'app di Azure Key Vault con l'autenticazione AD Azure senza le credenziali archiviate nel codice o nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-122">`Managed` &ndash; Demonstrates how to use Azure's [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="e26fe-123">Quando si usa l'identità del servizio gestito per l'autenticazione, non sono necessari un ID applicazione di Azure AD e una Password (segreto Client).</span><span class="sxs-lookup"><span data-stu-id="e26fe-123">When using MSI to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="e26fe-124">Il `Managed` versione dell'esempio deve essere distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-124">The `Managed` version of the sample must be deployed to Azure.</span></span>

<span data-ttu-id="e26fe-125">Per altre informazioni su come configurare un'app di esempio usando le direttive del preprocessore (`#define`), vedere <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="e26fe-125">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="e26fe-126">Archiviazione di segreti nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="e26fe-126">Secret storage in the Development environment</span></span>

<span data-ttu-id="e26fe-127">Impostare i segreti in locale usando il [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e26fe-127">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="e26fe-128">Quando l'app di esempio viene eseguito nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio segreto Manager locale.</span><span class="sxs-lookup"><span data-stu-id="e26fe-128">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="e26fe-129">Lo strumento Secret Manager richiede un `<UserSecretsId>` proprietà nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-129">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="e26fe-130">Impostare il valore della proprietà (`{GUID}`) a qualsiasi GUID univoco:</span><span class="sxs-lookup"><span data-stu-id="e26fe-130">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="e26fe-131">I segreti vengono creati come coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="e26fe-131">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="e26fe-132">I valori gerarchici (sezioni di configurazione) viene utilizzata una `:` (due punti) come separatore nella [configurazione di ASP.NET Core](xref:fundamentals/configuration/index) nomi chiave.</span><span class="sxs-lookup"><span data-stu-id="e26fe-132">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="e26fe-133">La gestione della chiave privata viene utilizzata da una shell dei comandi aperta alla radice del contenuto del progetto, in cui `{SECRET NAME}` è il nome e `{SECRET VALUE}` è il valore:</span><span class="sxs-lookup"><span data-stu-id="e26fe-133">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="e26fe-134">Eseguire i comandi seguenti in una shell dei comandi dalla radice del contenuto del progetto per impostare i segreti per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e26fe-134">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="e26fe-135">Quando questi segreti sono archiviati in Azure Key Vault nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il `_dev` suffisso viene modificato a `_prod`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-135">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="e26fe-136">Il suffisso fornisce un'indicazione visiva nell'output dell'app che indica l'origine dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-136">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="e26fe-137">Archiviazione di segreti nell'ambiente di produzione con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e26fe-137">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="e26fe-138">Le istruzioni fornite dal [Guida introduttiva: Impostare e recuperare un segreto da Azure Key Vault con Azure CLI](/azure/key-vault/quick-create-cli) argomento sono riepilogati di seguito per creare un Azure Key Vault e l'archiviazione dei segreti usati dall'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="e26fe-138">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="e26fe-139">Vedere l'argomento per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="e26fe-139">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="e26fe-140">Aprire Azure Cloud shell, usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="e26fe-140">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="e26fe-141">Selezionare **prova** nell'angolo superiore destro di un blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="e26fe-141">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="e26fe-142">Usare la stringa di ricerca "CLI di Azure" nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="e26fe-142">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="e26fe-143">Aprire Cloud Shell nel browser con il **avvia Cloud Shell** pulsante.</span><span class="sxs-lookup"><span data-stu-id="e26fe-143">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="e26fe-144">Selezionare il **Cloud Shell** pulsante menu in alto a destra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-144">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="e26fe-145">Per altre informazioni, vedere [interfaccia della riga di comando di Azure](/cli/azure/) e [Panoramica di Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="e26fe-145">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="e26fe-146">Se non sono già stati autenticati, accedere con il `az login` comando.</span><span class="sxs-lookup"><span data-stu-id="e26fe-146">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="e26fe-147">Creare un gruppo di risorse con il comando seguente, dove `{RESOURCE GROUP NAME}` è il nome del gruppo di risorse per il nuovo gruppo di risorse e `{LOCATION}` è l'area di Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="e26fe-147">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="e26fe-148">Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando seguente, dove `{KEY VAULT NAME}` è il nome per il nuovo insieme di credenziali delle chiavi e `{LOCATION}` è l'area di Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="e26fe-148">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="e26fe-149">Creare i segreti nell'insieme di credenziali chiave come coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="e26fe-149">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="e26fe-150">Azure Key Vault secret i nomi sono limitati da caratteri alfanumerici e trattini.</span><span class="sxs-lookup"><span data-stu-id="e26fe-150">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="e26fe-151">Usano i valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="e26fe-151">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="e26fe-152">I due punti, che vengono generalmente utilizzati per delimitare una sezione da una sottochiave [configurazione di ASP.NET Core](xref:fundamentals/configuration/index), non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave.</span><span class="sxs-lookup"><span data-stu-id="e26fe-152">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="e26fe-153">Pertanto, due trattini sono usati e scambiati per i due punti quando i segreti vengono caricati la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-153">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="e26fe-154">I segreti seguenti sono per l'uso con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="e26fe-154">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="e26fe-155">I valori includono un `_prod` suffisso per distinguerli dal `_dev` suffisso i valori caricati nell'ambiente di sviluppo da informazioni riservate dell'utente.</span><span class="sxs-lookup"><span data-stu-id="e26fe-155">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="e26fe-156">Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali chiave che è stato creato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="e26fe-156">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret"></a><span data-ttu-id="e26fe-157">Usare l'ID applicazione e il segreto Client</span><span class="sxs-lookup"><span data-stu-id="e26fe-157">Use Application ID and Client Secret</span></span>

<span data-ttu-id="e26fe-158">Configurare Azure AD, Azure Key Vault e l'app per usare un ID applicazione e una Password (segreto Client) per l'autenticazione a un insieme di credenziali delle chiavi quando l'app è ospitata all'esterno di Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-158">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault when the app is hosted outside of Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e26fe-159">Anche se con un ID applicazione e una Password (segreto Client) è supportato per le app ospitate in Azure, è consigliabile usare la [Managed Service Identity (MSI) Provider](#use-the-managed-service-identity-msi-provider) quando si ospita un'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-159">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using the [Managed Service Identity (MSI) Provider](#use-the-managed-service-identity-msi-provider) when hosting an app in Azure.</span></span> <span data-ttu-id="e26fe-160">Identità del servizio gestito non richiede l'archiviazione delle credenziali nell'applicazione o la configurazione, in modo che viene considerato come un approccio più sicuro a livello generale.</span><span class="sxs-lookup"><span data-stu-id="e26fe-160">MSI doesn't require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="e26fe-161">L'app di esempio Usa un ID applicazione e la Password (segreto Client) quando il `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Basic`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-161">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="e26fe-162">Registrare l'app con Azure AD e stabilire una Password (segreto Client) per l'identità dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-162">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="e26fe-163">Store il nome del key vault, ID applicazione e la Password e il segreto Client dell'app *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="e26fe-163">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="e26fe-164">Passare a **insiemi di credenziali della chiave** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-164">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="e26fe-165">Selezionare l'insieme di credenziali delle chiavi che è stato creato nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-165">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="e26fe-166">Selezionare **criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="e26fe-166">Select **Access policies**.</span></span>
1. <span data-ttu-id="e26fe-167">Selezionare **Aggiungi nuovo**.</span><span class="sxs-lookup"><span data-stu-id="e26fe-167">Select **Add new**.</span></span>
1. <span data-ttu-id="e26fe-168">Selezionare **selezionare un'entità** e selezionare l'app registrata in base al nome.</span><span class="sxs-lookup"><span data-stu-id="e26fe-168">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="e26fe-169">Selezionare il **seleziona** pulsante.</span><span class="sxs-lookup"><span data-stu-id="e26fe-169">Select the **Select** button.</span></span>
1. <span data-ttu-id="e26fe-170">Aprire **autorizzazioni segrete** e l'App disponga **ottenere** e **elenco** autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e26fe-170">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="e26fe-171">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="e26fe-171">Select **OK**.</span></span>
1. <span data-ttu-id="e26fe-172">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="e26fe-172">Select **Save**.</span></span>
1. <span data-ttu-id="e26fe-173">Distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-173">Deploy the app.</span></span>

<span data-ttu-id="e26fe-174">Il `Basic` app di esempio ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto:</span><span class="sxs-lookup"><span data-stu-id="e26fe-174">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="e26fe-175">Valori non gerarchici: Il valore per `SecretName` viene ottenuto con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-175">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="e26fe-176">Valori gerarchici (sezioni): Uso `:` notazione (due punti) o `GetSection` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-176">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="e26fe-177">Per ottenere il valore di configurazione, usare uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="e26fe-177">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="e26fe-178">Le chiamate dell'app `AddAzureKeyVault` con i valori forniti per il *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="e26fe-178">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="e26fe-179">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="e26fe-179">Example values:</span></span>

* <span data-ttu-id="e26fe-180">Nome del key vault: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="e26fe-180">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="e26fe-181">ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="e26fe-181">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="e26fe-182">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="e26fe-182">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="e26fe-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e26fe-183">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="e26fe-184">Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati.</span><span class="sxs-lookup"><span data-stu-id="e26fe-184">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="e26fe-185">Nell'ambiente di sviluppo, caricare i dati con i valori dei segreti di `_dev` suffisso.</span><span class="sxs-lookup"><span data-stu-id="e26fe-185">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="e26fe-186">Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso.</span><span class="sxs-lookup"><span data-stu-id="e26fe-186">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-the-managed-service-identity-msi-provider"></a><span data-ttu-id="e26fe-187">Usare il Provider di identità (MSI) del servizio gestito</span><span class="sxs-lookup"><span data-stu-id="e26fe-187">Use the Managed Service Identity (MSI) Provider</span></span>

<span data-ttu-id="e26fe-188">Un'app distribuita in Azure possa sfruttare i vantaggi di Managed Service Identity (MSI), che consente all'app per l'autenticazione con Azure Key Vault usando l'autenticazione di Azure AD senza credenziali (ID applicazione e la Password e il segreto Client) archiviati nell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-188">An app deployed to Azure can take advantage of Managed Service Identity (MSI), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="e26fe-189">L'app di esempio Usa identità del servizio gestito quando la `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Managed`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-189">The sample app uses MSI when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="e26fe-190">Immettere il nome dell'insieme di credenziali all'app *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="e26fe-190">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="e26fe-191">L'app di esempio non richiede un ID applicazione e una Password (segreto Client) se impostato sul `Managed` versione, pertanto è possibile ignorare tali voci di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-191">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="e26fe-192">L'app viene distribuita in Azure e Azure autentica l'app per accedere ad Azure Key Vault usando solo il nome dell'insieme di credenziali archiviata nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="e26fe-192">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="e26fe-193">Distribuire l'app di esempio in servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-193">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="e26fe-194">Un'app distribuita in servizio App di Azure viene automaticamente registrata con Azure AD quando viene creato il servizio.</span><span class="sxs-lookup"><span data-stu-id="e26fe-194">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="e26fe-195">Ottenere l'ID oggetto della distribuzione per l'uso nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="e26fe-195">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="e26fe-196">L'ID di oggetto viene visualizzato nel portale di Azure sul **identità** pannello del servizio App.</span><span class="sxs-lookup"><span data-stu-id="e26fe-196">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="e26fe-197">Uso della riga di comando di Azure e l'ID oggetto dell'app, specificare l'app con `list` e `get` delle autorizzazioni per accedere a key vault:</span><span class="sxs-lookup"><span data-stu-id="e26fe-197">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="e26fe-198">**Riavviare l'app** utilizzando CLI di Azure, PowerShell o il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-198">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="e26fe-199">L'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="e26fe-199">The sample app:</span></span>

* <span data-ttu-id="e26fe-200">Crea un'istanza di `AzureServiceTokenProvider` classe senza una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-200">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="e26fe-201">Quando una stringa di connessione non è specificato, il provider tenta di ottenere un token di accesso dall'identità del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="e26fe-201">When a connection string isn't provided, the provider attempts to obtain an access token from MSI.</span></span>
* <span data-ttu-id="e26fe-202">Una nuova `KeyVaultClient` viene creato con il `AzureServiceTokenProvider` callback token istanza.</span><span class="sxs-lookup"><span data-stu-id="e26fe-202">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="e26fe-203">Il `KeyVaultClient` istanza viene utilizzata con un'implementazione predefinita di `IKeyVaultSecretManager` che consente di caricare tutti i valori del segreto e la sostituisce doppia trattini (`--`) con i due punti (`:`) nei nomi delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e26fe-203">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="e26fe-204">Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati.</span><span class="sxs-lookup"><span data-stu-id="e26fe-204">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="e26fe-205">Nell'ambiente di sviluppo, i valori dei segreti hanno il `_dev` suffisso perché sta fornite dall'utente i segreti.</span><span class="sxs-lookup"><span data-stu-id="e26fe-205">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="e26fe-206">Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso perché sta fornite da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e26fe-206">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="e26fe-207">Se si riceve un `Access denied` errore, verificare che l'app viene registrata con Azure AD e fornito l'accesso all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e26fe-207">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="e26fe-208">Confermare che è stato riavviato il servizio in Azure.</span><span class="sxs-lookup"><span data-stu-id="e26fe-208">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="e26fe-209">Usare un prefisso del nome della chiave</span><span class="sxs-lookup"><span data-stu-id="e26fe-209">Use a key name prefix</span></span>

<span data-ttu-id="e26fe-210">`AddAzureKeyVault` fornisce un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare i segreti dell'insieme di credenziali chiave come vengono convertiti in chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-210">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="e26fe-211">Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti basati su un valore di prefisso specificato all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-211">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="e26fe-212">Ciò consente, ad esempio, per caricare i segreti in base alla versione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-212">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="e26fe-213">Non usare prefissi su segreti dell'insieme di credenziali chiave inserisca i segreti per più app nel medesimo insieme di credenziali chiave o inserire i segreti dell'ambiente (ad esempio, *development* rispetto *produzione* segreti) nella stessa insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="e26fe-213">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="e26fe-214">È consigliabile che diverse App e ambienti di sviluppo o di produzione usano insiemi di credenziali delle chiavi separati per isolare gli ambienti di app per il massimo livello di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="e26fe-214">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="e26fe-215">Nell'esempio seguente, viene stabilito un segreto della chiave dell'insieme di credenziali (e con lo strumento Secret Manager per l'ambiente di sviluppo) per `5000-AppSecret` (periodi non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave).</span><span class="sxs-lookup"><span data-stu-id="e26fe-215">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="e26fe-216">Questo valore rappresenta un segreto dell'app per la versione versione=5.0.0.0 dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-216">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="e26fe-217">Per un'altra versione dell'app, 5.1.0.0, una chiave privata viene aggiunta alla chiave dell'insieme di credenziali (e usare lo strumento Secret Manager) per `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-217">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="e26fe-218">Il valore del segreto con controllo delle versioni ogni versione dell'app carica la configurazione come `AppSecret`, stripping disattivata la versione durante il caricamento del segreto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-218">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="e26fe-219">`AddAzureKeyVault` viene chiamato con un oggetto personalizzato `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="e26fe-219">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="e26fe-220">I valori per nome del key vault, ID applicazione e la Password (segreto Client) vengono forniti per il *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="e26fe-220">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="e26fe-221">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="e26fe-221">Example values:</span></span>

* <span data-ttu-id="e26fe-222">Nome del key vault: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="e26fe-222">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="e26fe-223">ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="e26fe-223">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="e26fe-224">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="e26fe-224">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="e26fe-225">Il `IKeyVaultSecretManager` implementazione reagisca ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:</span><span class="sxs-lookup"><span data-stu-id="e26fe-225">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="e26fe-226">Il `Load` metodo viene chiamato da un algoritmo di provider che esegue l'iterazione attraverso i segreti dell'insieme di credenziali per individuare quelli che dispongono del prefisso della versione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-226">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="e26fe-227">Quando viene trovato un prefisso di versione con `Load`, l'algoritmo utilizza il `GetKey` per restituire il nome della configurazione del nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-227">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="e26fe-228">Questo rimuove il prefisso di versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nella configurazione dell'app per le coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="e26fe-228">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="e26fe-229">Quando viene implementato questo approccio:</span><span class="sxs-lookup"><span data-stu-id="e26fe-229">When this approach is implemented:</span></span>

1. <span data-ttu-id="e26fe-230">Versione dell'app specificata nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-230">The app's version specified in the app's project file.</span></span> <span data-ttu-id="e26fe-231">Nell'esempio seguente, la versione di app è impostata su `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="e26fe-231">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="e26fe-232">Verificare che un `<UserSecretsId>` proprietà è presente nel file di progetto dell'app, in cui `{GUID}` è un GUID fornito dall'utente:</span><span class="sxs-lookup"><span data-stu-id="e26fe-232">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="e26fe-233">Salvare i segreti seguenti in locale con il [strumento Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="e26fe-233">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="e26fe-234">I segreti vengono salvati in Azure Key Vault usando i comandi CLI di Azure seguenti:</span><span class="sxs-lookup"><span data-stu-id="e26fe-234">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="e26fe-235">Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali chiave.</span><span class="sxs-lookup"><span data-stu-id="e26fe-235">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="e26fe-236">Il segreto di stringa per `5000-AppSecret` corrisponde alla versione dell'app specificata nel file di progetto dell'app (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="e26fe-236">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="e26fe-237">La versione, `5000` (con il trattino), viene rimosso dal nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="e26fe-237">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="e26fe-238">In tutta l'app, la lettura di configurazione con la chiave `AppSecret` carica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-238">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="e26fe-239">Se la versione di app viene modificata nel file di progetto per `5.1.0.0` e l'app viene eseguita anche in questo caso, viene restituito il valore del segreto `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo e `5.1.0.0_secret_value_prod` nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-239">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="e26fe-240">È anche possibile fornire una propria `KeyVaultClient` implementazione di `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-240">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="e26fe-241">Un client personalizzato consente la condivisione di una singola istanza del client dell'app.</span><span class="sxs-lookup"><span data-stu-id="e26fe-241">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="e26fe-242">Eseguire l'autenticazione ad Azure Key Vault con un certificato X.509</span><span class="sxs-lookup"><span data-stu-id="e26fe-242">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="e26fe-243">Quando si sviluppa un'app .NET Framework in un ambiente che supporta i certificati, è possibile eseguire l'autenticazione ad Azure Key Vault con un certificato X.509.</span><span class="sxs-lookup"><span data-stu-id="e26fe-243">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="e26fe-244">Chiave privata del certificato X.509 viene gestita dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="e26fe-244">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="e26fe-245">Per altre informazioni, vedere [eseguire l'autenticazione con un certificato anziché un segreto Client](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="e26fe-245">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="e26fe-246">Usare la `AddAzureKeyVault` overload che accetta un `X509Certificate2` (`_env` nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e26fe-246">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="e26fe-247">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="e26fe-247">Bind an array to a class</span></span>

<span data-ttu-id="e26fe-248">Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.</span><span class="sxs-lookup"><span data-stu-id="e26fe-248">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="e26fe-249">Durante la lettura da un'origine di configurazione che consente di chiavi contenere i due punti (`:`) i separatori, un segmento della chiave numerico viene utilizzata per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="e26fe-249">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="e26fe-250">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="e26fe-250">`:{n}:`).</span></span> <span data-ttu-id="e26fe-251">Per altre informazioni, vedere [configurazione: Associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="e26fe-251">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="e26fe-252">Le chiavi di Azure Key Vault non è possibile usare i due punti come separatore.</span><span class="sxs-lookup"><span data-stu-id="e26fe-252">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="e26fe-253">L'approccio descritto in questo argomento utilizza i trattini double (`--`) come separatore per i valori gerarchici (sezioni).</span><span class="sxs-lookup"><span data-stu-id="e26fe-253">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="e26fe-254">Matrice chiavi vengono archiviate in Azure Key Vault con doppie trattini e segmenti di mercato principali numerici (`--0--`, `--1--`,...</span><span class="sxs-lookup"><span data-stu-id="e26fe-254">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="e26fe-255">`--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="e26fe-255">`--{n}--`).</span></span>

<span data-ttu-id="e26fe-256">Esaminare quanto segue [Serilog](https://serilog.net/) incluso in un file JSON di configurazione del provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e26fe-256">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="e26fe-257">Esistono due valori letterali definiti di oggetti di `WriteTo` matrice che riflettono Serilog due *sink*, che descrivono le destinazioni per l'output di registrazione:</span><span class="sxs-lookup"><span data-stu-id="e26fe-257">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="e26fe-258">La configurazione illustrata nel file JSON precedente viene archiviata in Azure Key Vault con trattino doppio (`--`) segmenti notazione e numerici:</span><span class="sxs-lookup"><span data-stu-id="e26fe-258">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="e26fe-259">Chiave</span><span class="sxs-lookup"><span data-stu-id="e26fe-259">Key</span></span> | <span data-ttu-id="e26fe-260">Value</span><span class="sxs-lookup"><span data-stu-id="e26fe-260">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="e26fe-261">Ricaricare i segreti</span><span class="sxs-lookup"><span data-stu-id="e26fe-261">Reload secrets</span></span>

<span data-ttu-id="e26fe-262">I segreti vengono memorizzati nella cache fino a `IConfigurationRoot.Reload()` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="e26fe-262">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="e26fe-263">Scaduto, disabilitato, e aggiornati i segreti nell'insieme di credenziali chiave non vengono rispettati dall'app fino al `Reload` viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="e26fe-263">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="e26fe-264">Segreti scaduti e disabilitati</span><span class="sxs-lookup"><span data-stu-id="e26fe-264">Disabled and expired secrets</span></span>

<span data-ttu-id="e26fe-265">I segreti scaduti e disabilitati generano un `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="e26fe-265">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="e26fe-266">Per evitare che l'app generi, sostituire l'app o aggiornare il segreto disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-266">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="e26fe-267">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="e26fe-267">Troubleshoot</span></span>

<span data-ttu-id="e26fe-268">L'app non riesce a caricare la configurazione utilizzando il provider, viene scritto un messaggio di errore per il [registrazione di ASP.NET Core infrastructure](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e26fe-268">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="e26fe-269">Configurazione del caricamento impedire che le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e26fe-269">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="e26fe-270">L'app non è configurato correttamente in Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e26fe-270">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="e26fe-271">L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e26fe-271">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="e26fe-272">L'app non è autorizzato ad accedere l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e26fe-272">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="e26fe-273">I criteri di accesso non includano `Get` e `List` autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="e26fe-273">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="e26fe-274">Nell'insieme di credenziali chiave, i dati di configurazione (coppia nome-valore) in modo non corretto denominati, mancante, disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-274">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="e26fe-275">L'app con il nome dell'insieme di credenziali chiave errata (`Vault`), Id di App di Azure AD (`ClientId`), o la chiave di Azure AD (`ClientSecret`).</span><span class="sxs-lookup"><span data-stu-id="e26fe-275">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="e26fe-276">La chiave di Azure AD (`ClientSecret`) è scaduto.</span><span class="sxs-lookup"><span data-stu-id="e26fe-276">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="e26fe-277">Non è corretta nell'app per il valore che si sta provando a caricare la chiave di configurazione (nome).</span><span class="sxs-lookup"><span data-stu-id="e26fe-277">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e26fe-278">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e26fe-278">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="e26fe-279">Microsoft Azure: Insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="e26fe-279">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="e26fe-280">Microsoft Azure: Documentazione su Key Vault</span><span class="sxs-lookup"><span data-stu-id="e26fe-280">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="e26fe-281">Come generare e trasferire chiavi HSM protette le chiavi SSH per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e26fe-281">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="e26fe-282">Classe KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="e26fe-282">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
