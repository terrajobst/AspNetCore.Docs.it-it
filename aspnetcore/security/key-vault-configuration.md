---
title: Azure Key Vault Configuration Provider in ASP.NET Core
author: guardrex
description: Informazioni su come usare il Provider di configurazione dell'insieme di credenziali chiave Azure per configurare un'app usando coppie nome-valore in fase di esecuzione.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: be176ed612be0773c4a5b52607c023da3856ac14
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815321"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="8c6d7-103">Azure Key Vault Configuration Provider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c6d7-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="8c6d7-104">Dal [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="8c6d7-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="8c6d7-105">Questo documento illustra come usare il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Provider di configurazione per caricare i valori di configurazione di app da segreti di Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="8c6d7-106">Azure Key Vault è un servizio basato sul cloud che semplifica la protezione delle chiavi crittografiche e segreti usati da App e servizi.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="8c6d7-107">Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core sono:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="8c6d7-108">Controllo dell'accesso ai dati di configurazione sensibili.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="8c6d7-109">Soddisfa il requisito per FIPS 140-2 livello 2 convalidati i moduli di protezione Hardware (HSM) quando si archiviano i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="8c6d7-110">Questo scenario è disponibile per le app destinate a ASP.NET Core 2.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="8c6d7-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c6d7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="8c6d7-112">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="8c6d7-112">Packages</span></span>

<span data-ttu-id="8c6d7-113">Per usare Azure Key Vault Configuration Provider, aggiungere un riferimento al pacchetto di [azurekeyvault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="8c6d7-114">Adottare il [gestite le identità per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) scenario, aggiungere un riferimento al pacchetto le [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="8c6d7-115">Al momento della scrittura, la versione stabile più recente di `Microsoft.Azure.Services.AppAuthentication`, versione `1.0.3`, fornisce il supporto per [assegnato dal sistema gestito identità](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="8c6d7-116">Supporto per *assegnata dall'utente gestite delle identità* è disponibile nel `1.2.0-preview2` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="8c6d7-117">In questo argomento viene illustrato l'utilizzo delle identità gestite dal sistema e l'app di esempio fornito utilizza la versione `1.0.3` del `Microsoft.Azure.Services.AppAuthentication` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="8c6d7-118">App di esempio</span><span class="sxs-lookup"><span data-stu-id="8c6d7-118">Sample app</span></span>

<span data-ttu-id="8c6d7-119">L'app di esempio viene eseguito in una delle due modalità di base di `#define` istruzione all'inizio del *Program.cs* file:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="8c6d7-120">`Certificate` &ndash; Illustra l'uso di un certificato X.509 e ID Client dell'insieme di credenziali chiave di Azure per i segreti di accesso archiviato in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="8c6d7-121">Questa versione dell'esempio può essere eseguita da qualsiasi posizione, distribuiti in Azure App Service o qualsiasi host in grado di servire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="8c6d7-122">`Managed` &ndash; Viene illustrato come utilizzare [gestite le identità per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) eseguire l'autenticazione all'app di Azure Key Vault con l'autenticazione AD Azure senza le credenziali archiviate nel codice o nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="8c6d7-123">Quando si usa identità gestite per l'autenticazione, non sono necessari un ID applicazione di Azure AD e una Password (segreto Client).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="8c6d7-124">Il `Managed` versione dell'esempio deve essere distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="8c6d7-125">Seguire le indicazioni fornite nel [usare le identità gestita per le risorse di Azure](#use-managed-identities-for-azure-resources) sezione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="8c6d7-126">Per altre informazioni su come configurare un'app di esempio usando le direttive del preprocessore (`#define`), vedere <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="8c6d7-127">Archiviazione di segreti nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="8c6d7-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="8c6d7-128">Impostare i segreti in locale usando il [strumento Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="8c6d7-129">Quando l'app di esempio viene eseguito nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio segreto Manager locale.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="8c6d7-130">Lo strumento Secret Manager richiede un `<UserSecretsId>` proprietà nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="8c6d7-131">Impostare il valore della proprietà (`{GUID}`) a qualsiasi GUID univoco:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="8c6d7-132">I segreti vengono creati come coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="8c6d7-133">I valori gerarchici (sezioni di configurazione) viene utilizzata una `:` (due punti) come separatore nella [configurazione di ASP.NET Core](xref:fundamentals/configuration/index) nomi chiave.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="8c6d7-134">La gestione della chiave privata viene utilizzata da una shell dei comandi aperta alla radice del contenuto del progetto, in cui `{SECRET NAME}` è il nome e `{SECRET VALUE}` è il valore:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="8c6d7-135">Eseguire i comandi seguenti in una shell dei comandi dalla radice del contenuto del progetto per impostare i segreti per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="8c6d7-136">Quando questi segreti sono archiviati in Azure Key Vault nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il `_dev` suffisso viene modificato a `_prod`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="8c6d7-137">Il suffisso fornisce un'indicazione visiva nell'output dell'app che indica l'origine dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="8c6d7-138">Archiviazione di segreti nell'ambiente di produzione con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8c6d7-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="8c6d7-139">Le istruzioni fornite dal [Guida introduttiva: Impostare e recuperare un segreto da Azure Key Vault con Azure CLI](/azure/key-vault/quick-create-cli) argomento sono riepilogati di seguito per creare un Azure Key Vault e l'archiviazione dei segreti usati dall'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="8c6d7-140">Vedere l'argomento per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="8c6d7-141">Aprire Azure Cloud shell, usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="8c6d7-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="8c6d7-142">Selezionare **prova** nell'angolo superiore destro di un blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="8c6d7-143">Usare la stringa di ricerca "CLI di Azure" nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="8c6d7-144">Aprire Cloud Shell nel browser con il **avvia Cloud Shell** pulsante.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="8c6d7-145">Selezionare il **Cloud Shell** pulsante menu in alto a destra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="8c6d7-146">Per altre informazioni, vedere [interfaccia della riga di comando di Azure](/cli/azure/) e [Panoramica di Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="8c6d7-147">Se non sono già stati autenticati, accedere con il `az login` comando.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="8c6d7-148">Creare un gruppo di risorse con il comando seguente, dove `{RESOURCE GROUP NAME}` è il nome del gruppo di risorse per il nuovo gruppo di risorse e `{LOCATION}` è l'area di Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="8c6d7-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="8c6d7-149">Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando seguente, dove `{KEY VAULT NAME}` è il nome per il nuovo insieme di credenziali delle chiavi e `{LOCATION}` è l'area di Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="8c6d7-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="8c6d7-150">Creare i segreti nell'insieme di credenziali chiave come coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="8c6d7-151">Azure Key Vault secret i nomi sono limitati da caratteri alfanumerici e trattini.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="8c6d7-152">Usano i valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="8c6d7-153">I due punti, che vengono generalmente utilizzati per delimitare una sezione da una sottochiave [configurazione di ASP.NET Core](xref:fundamentals/configuration/index), non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="8c6d7-154">Pertanto, due trattini sono usati e scambiati per i due punti quando i segreti vengono caricati la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="8c6d7-155">I segreti seguenti sono per l'uso con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="8c6d7-156">I valori includono un `_prod` suffisso per distinguerli dal `_dev` suffisso i valori caricati nell'ambiente di sviluppo da informazioni riservate dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="8c6d7-157">Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali chiave che è stato creato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="8c6d7-158">Usare certificati X.509 e ID dell'applicazione per le app non--ospitate in Azure</span><span class="sxs-lookup"><span data-stu-id="8c6d7-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="8c6d7-159">Configurare Azure AD, Azure Key Vault e l'app per usare un ID di applicazione di Azure Active Directory e x. 509 del certificato per l'autenticazione a un insieme di credenziali delle chiavi **quando l'app è ospitata all'esterno di Azure**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="8c6d7-160">Per altre informazioni, vedere [sulle chiavi, segreti e certificati](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="8c6d7-161">Anche se con un certificato X.509 e ID applicazione è supportata per le app ospitate in Azure, è consigliabile usare [gestite le identità per le risorse di Azure](#use-managed-identities-for-azure-resources) quando si ospita un'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="8c6d7-162">Identità gestite e non richiedono l'archiviazione di un certificato nell'app o nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="8c6d7-163">L'app di esempio Usa un ID applicazione e certificati X.509 quando le `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="8c6d7-164">Creare un archivio di PKCS #12 (*PFX*) certificato.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="8c6d7-165">Le opzioni per la creazione di certificati includono [MakeCert in Windows](/windows/desktop/seccrypto/makecert) e [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="8c6d7-166">Installare il certificato nell'archivio certificati personali dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="8c6d7-167">Contrassegnare la chiave come esportabile è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="8c6d7-168">Si noti identificazione personale del certificato, che viene usato più avanti in questo processo.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="8c6d7-169">Esportare l'archivio di PKCS #12 (*PFX*) certificato sotto forma di un certificato con codifica DER (*CER*).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="8c6d7-170">Registrare l'app con Azure AD (**registrazioni per l'App**).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="8c6d7-171">Caricare il certificato con codifica DER (*CER*) ad Azure AD:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="8c6d7-172">Selezionare l'app di Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="8c6d7-173">Passare a **certificati e i segreti**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="8c6d7-174">Selezionare **carica certificato** per caricare il certificato che contiene la chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="8c6d7-175">Oggetto *CER*, *PEM*, o *CRT* certificato è accettabile.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="8c6d7-176">Store il nome del key vault, ID applicazione e identificazione personale del certificato dell'app *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="8c6d7-177">Passare a **insiemi di credenziali della chiave** nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="8c6d7-178">Selezionare l'insieme di credenziali delle chiavi che è stato creato nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="8c6d7-179">Selezionare **criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="8c6d7-180">Selezionare **Aggiungi nuovo**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-180">Select **Add new**.</span></span>
1. <span data-ttu-id="8c6d7-181">Selezionare **selezionare un'entità** e selezionare l'app registrata in base al nome.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="8c6d7-182">Selezionare il **seleziona** pulsante.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="8c6d7-183">Aprire **autorizzazioni segrete** e l'App disponga **ottenere** e **elenco** autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="8c6d7-184">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-184">Select **OK**.</span></span>
1. <span data-ttu-id="8c6d7-185">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-185">Select **Save**.</span></span>
1. <span data-ttu-id="8c6d7-186">Distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-186">Deploy the app.</span></span>

<span data-ttu-id="8c6d7-187">Il `Certificate` app di esempio ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="8c6d7-188">Valori non gerarchici: Il valore per `SecretName` viene ottenuto con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="8c6d7-189">Valori gerarchici (sezioni): Uso `:` notazione (due punti) o `GetSection` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="8c6d7-190">Per ottenere il valore di configurazione, usare uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="8c6d7-191">Il certificato X.509 viene gestito dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="8c6d7-192">Le chiamate dell'app `AddAzureKeyVault` con i valori forniti per il *appSettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="8c6d7-193">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-193">Example values:</span></span>

* <span data-ttu-id="8c6d7-194">Nome del key vault: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="8c6d7-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="8c6d7-195">ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="8c6d7-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="8c6d7-196">Identificazione personale del certificato: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="8c6d7-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="8c6d7-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="8c6d7-198">Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="8c6d7-199">Nell'ambiente di sviluppo, caricare i dati con i valori dei segreti di `_dev` suffisso.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="8c6d7-200">Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="8c6d7-201">Usare le identità gestita per le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="8c6d7-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="8c6d7-202">**Un'app distribuita in Azure** possono sfruttare [gestite le identità per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview), che consente all'app di eseguire l'autenticazione con Azure Key Vault usando l'autenticazione di Azure AD senza credenziali (ID applicazione e Segreto Password/Client) archiviati nell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="8c6d7-203">L'app di esempio Usa identità gestite per le risorse di Azure quando la `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Managed`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="8c6d7-204">Immettere il nome dell'insieme di credenziali all'app *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="8c6d7-205">L'app di esempio non richiede un ID applicazione e una Password (segreto Client) se impostato sul `Managed` versione, pertanto è possibile ignorare tali voci di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="8c6d7-206">L'app viene distribuita in Azure e Azure autentica l'app per accedere ad Azure Key Vault usando solo il nome dell'insieme di credenziali archiviata nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="8c6d7-207">Distribuire l'app di esempio in servizio App di Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="8c6d7-208">Un'app distribuita in servizio App di Azure viene automaticamente registrata con Azure AD quando viene creato il servizio.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="8c6d7-209">Ottenere l'ID oggetto della distribuzione per l'uso nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="8c6d7-210">L'ID di oggetto viene visualizzato nel portale di Azure sul **identità** pannello del servizio App.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="8c6d7-211">Uso della riga di comando di Azure e l'ID oggetto dell'app, specificare l'app con `list` e `get` delle autorizzazioni per accedere a key vault:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="8c6d7-212">**Riavviare l'app** utilizzando CLI di Azure, PowerShell o il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="8c6d7-213">L'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-213">The sample app:</span></span>

* <span data-ttu-id="8c6d7-214">Crea un'istanza di `AzureServiceTokenProvider` classe senza una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="8c6d7-215">Quando non viene fornita una stringa di connessione, il provider tenta di ottenere un token di accesso dall'identità gestita per le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="8c6d7-216">Una nuova `KeyVaultClient` viene creato con il `AzureServiceTokenProvider` callback token istanza.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="8c6d7-217">Il `KeyVaultClient` istanza viene utilizzata con un'implementazione predefinita di `IKeyVaultSecretManager` che consente di caricare tutti i valori del segreto e la sostituisce doppia trattini (`--`) con i due punti (`:`) nei nomi delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="8c6d7-218">Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="8c6d7-219">Nell'ambiente di sviluppo, i valori dei segreti hanno il `_dev` suffisso perché sta fornite dall'utente i segreti.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="8c6d7-220">Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso perché sta fornite da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="8c6d7-221">Se si riceve un `Access denied` errore, verificare che l'app viene registrata con Azure AD e fornito l'accesso all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="8c6d7-222">Confermare che è stato riavviato il servizio in Azure.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="8c6d7-223">Usare un prefisso del nome della chiave</span><span class="sxs-lookup"><span data-stu-id="8c6d7-223">Use a key name prefix</span></span>

<span data-ttu-id="8c6d7-224">`AddAzureKeyVault` fornisce un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare i segreti dell'insieme di credenziali chiave come vengono convertiti in chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="8c6d7-225">Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti basati su un valore di prefisso specificato all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="8c6d7-226">Ciò consente, ad esempio, per caricare i segreti in base alla versione dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="8c6d7-227">Non usare prefissi su segreti dell'insieme di credenziali chiave inserisca i segreti per più app nel medesimo insieme di credenziali chiave o inserire i segreti dell'ambiente (ad esempio, *development* rispetto *produzione* segreti) nella stessa insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="8c6d7-228">È consigliabile che diverse App e ambienti di sviluppo o di produzione usano insiemi di credenziali delle chiavi separati per isolare gli ambienti di app per il massimo livello di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="8c6d7-229">Nell'esempio seguente, viene stabilito un segreto della chiave dell'insieme di credenziali (e con lo strumento Secret Manager per l'ambiente di sviluppo) per `5000-AppSecret` (periodi non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="8c6d7-230">Questo valore rappresenta un segreto dell'app per la versione versione=5.0.0.0 dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="8c6d7-231">Per un'altra versione dell'app, 5.1.0.0, una chiave privata viene aggiunta alla chiave dell'insieme di credenziali (e usare lo strumento Secret Manager) per `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="8c6d7-232">Il valore del segreto con controllo delle versioni ogni versione dell'app carica la configurazione come `AppSecret`, stripping disattivata la versione durante il caricamento del segreto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="8c6d7-233">`AddAzureKeyVault` viene chiamato con un oggetto personalizzato `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="8c6d7-234">Il `IKeyVaultSecretManager` implementazione reagisca ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="8c6d7-235">Il `Load` metodo viene chiamato da un algoritmo di provider che esegue l'iterazione attraverso i segreti dell'insieme di credenziali per individuare quelli che dispongono del prefisso della versione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="8c6d7-236">Quando viene trovato un prefisso di versione con `Load`, l'algoritmo utilizza il `GetKey` per restituire il nome della configurazione del nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="8c6d7-237">Questo rimuove il prefisso di versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nella configurazione dell'app per le coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="8c6d7-238">Quando viene implementato questo approccio:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="8c6d7-239">Versione dell'app specificata nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="8c6d7-240">Nell'esempio seguente, la versione di app è impostata su `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="8c6d7-241">Verificare che un `<UserSecretsId>` proprietà è presente nel file di progetto dell'app, in cui `{GUID}` è un GUID fornito dall'utente:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="8c6d7-242">Salvare i segreti seguenti in locale con il [strumento Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="8c6d7-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="8c6d7-243">I segreti vengono salvati in Azure Key Vault usando i comandi CLI di Azure seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="8c6d7-244">Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali chiave.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="8c6d7-245">Il segreto di stringa per `5000-AppSecret` corrisponde alla versione dell'app specificata nel file di progetto dell'app (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="8c6d7-246">La versione, `5000` (con il trattino), viene rimosso dal nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="8c6d7-247">In tutta l'app, la lettura di configurazione con la chiave `AppSecret` carica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="8c6d7-248">Se la versione di app viene modificata nel file di progetto per `5.1.0.0` e l'app viene eseguita anche in questo caso, viene restituito il valore del segreto `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo e `5.1.0.0_secret_value_prod` nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="8c6d7-249">È anche possibile fornire una propria `KeyVaultClient` implementazione di `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="8c6d7-250">Un client personalizzato consente la condivisione di una singola istanza del client dell'app.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="8c6d7-251">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="8c6d7-251">Bind an array to a class</span></span>

<span data-ttu-id="8c6d7-252">Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="8c6d7-253">Durante la lettura da un'origine di configurazione che consente di chiavi contenere i due punti (`:`) i separatori, un segmento della chiave numerico viene utilizzata per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="8c6d7-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="8c6d7-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-254">`:{n}:`).</span></span> <span data-ttu-id="8c6d7-255">Per altre informazioni, vedere [configurazione: Associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="8c6d7-256">Le chiavi di Azure Key Vault non è possibile usare i due punti come separatore.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="8c6d7-257">L'approccio descritto in questo argomento utilizza i trattini double (`--`) come separatore per i valori gerarchici (sezioni).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="8c6d7-258">Matrice chiavi vengono archiviate in Azure Key Vault con doppie trattini e segmenti di mercato principali numerici (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="8c6d7-259">Esaminare quanto segue [Serilog](https://serilog.net/) incluso in un file JSON di configurazione del provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="8c6d7-260">Esistono due valori letterali definiti di oggetti di `WriteTo` matrice che riflettono Serilog due *sink*, che descrivono le destinazioni per l'output di registrazione:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="8c6d7-261">La configurazione illustrata nel file JSON precedente viene archiviata in Azure Key Vault con trattino doppio (`--`) segmenti notazione e numerici:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="8c6d7-262">Chiave</span><span class="sxs-lookup"><span data-stu-id="8c6d7-262">Key</span></span> | <span data-ttu-id="8c6d7-263">Value</span><span class="sxs-lookup"><span data-stu-id="8c6d7-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="8c6d7-264">Ricaricare i segreti</span><span class="sxs-lookup"><span data-stu-id="8c6d7-264">Reload secrets</span></span>

<span data-ttu-id="8c6d7-265">I segreti vengono memorizzati nella cache fino a `IConfigurationRoot.Reload()` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="8c6d7-266">Scaduto, disabilitato, e aggiornati i segreti nell'insieme di credenziali chiave non vengono rispettati dall'app fino al `Reload` viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="8c6d7-267">Segreti scaduti e disabilitati</span><span class="sxs-lookup"><span data-stu-id="8c6d7-267">Disabled and expired secrets</span></span>

<span data-ttu-id="8c6d7-268">I segreti scaduti e disabilitati generano un `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-268">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="8c6d7-269">Per evitare che l'app generi, sostituire l'app o aggiornare il segreto disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-269">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="8c6d7-270">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="8c6d7-270">Troubleshoot</span></span>

<span data-ttu-id="8c6d7-271">L'app non riesce a caricare la configurazione utilizzando il provider, viene scritto un messaggio di errore per il [registrazione di ASP.NET Core infrastructure](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="8c6d7-272">Configurazione del caricamento impedire che le condizioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c6d7-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="8c6d7-273">L'app o il certificato non è configurato correttamente in Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="8c6d7-274">L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="8c6d7-275">L'app non è autorizzato ad accedere l'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="8c6d7-276">I criteri di accesso non includano `Get` e `List` autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="8c6d7-277">Nell'insieme di credenziali chiave, i dati di configurazione (coppia nome-valore) in modo non corretto denominati, mancante, disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="8c6d7-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="8c6d7-278">L'app con il nome dell'insieme di credenziali chiave errata (`KeyVaultName`), Id di applicazione Azure AD (`AzureADApplicationId`), o l'identificazione personale del certificato di Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="8c6d7-279">Non è corretta nell'app per il valore che si sta provando a caricare la chiave di configurazione (nome).</span><span class="sxs-lookup"><span data-stu-id="8c6d7-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c6d7-280">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8c6d7-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="8c6d7-281">Microsoft Azure: Insieme di credenziali delle chiavi</span><span class="sxs-lookup"><span data-stu-id="8c6d7-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="8c6d7-282">Microsoft Azure: Documentazione su Key Vault</span><span class="sxs-lookup"><span data-stu-id="8c6d7-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="8c6d7-283">Come generare e trasferire chiavi HSM protette le chiavi SSH per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8c6d7-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="8c6d7-284">Classe KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="8c6d7-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="8c6d7-285">Avvio rapido: Impostare e recuperare un segreto da Azure Key Vault usando un'app web .NET</span><span class="sxs-lookup"><span data-stu-id="8c6d7-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="8c6d7-286">Esercitazione: Come usare Azure Key Vault con Windows macchina virtuale di Azure in .NET</span><span class="sxs-lookup"><span data-stu-id="8c6d7-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
