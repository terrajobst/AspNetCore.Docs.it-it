---
title: Provider di configurazione di Azure Key Vault in ASP.NET Core
author: guardrex
description: Informazioni su come usare il provider di configurazione Azure Key Vault per configurare un'app usando coppie nome-valore caricate in fase di esecuzione.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 0d0b6e20a1901d4a2630ce263b5fd0cd7bcca8fe
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776658"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="c8493-103">Provider di configurazione di Azure Key Vault in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8493-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="c8493-104">Di [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c8493-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="c8493-105">Questo documento illustra come usare il provider di configurazione [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) per caricare i valori di configurazione dell'app da Azure Key Vault segreti.</span><span class="sxs-lookup"><span data-stu-id="c8493-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="c8493-106">Azure Key Vault è un servizio basato sul cloud che aiuta a proteggere le chiavi crittografiche e i segreti usati da app e servizi.</span><span class="sxs-lookup"><span data-stu-id="c8493-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="c8493-107">Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core includono:</span><span class="sxs-lookup"><span data-stu-id="c8493-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="c8493-108">Controllo dell'accesso ai dati di configurazione riservati.</span><span class="sxs-lookup"><span data-stu-id="c8493-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="c8493-109">Soddisfare i requisiti per i moduli di protezione hardware convalidati FIPS 140-2 Level 2 (HSM) quando si archiviano i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c8493-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="c8493-110">Questo scenario è disponibile per le app destinate a ASP.NET Core 2,1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c8493-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="c8493-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8493-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="c8493-112">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="c8493-112">Packages</span></span>

<span data-ttu-id="c8493-113">Per usare il provider di configurazione Azure Key Vault, aggiungere un riferimento al pacchetto [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="c8493-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="c8493-114">Per adottare lo scenario delle [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) , aggiungere un riferimento al pacchetto [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .</span><span class="sxs-lookup"><span data-stu-id="c8493-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="c8493-115">Al momento della stesura di questo articolo, la versione `Microsoft.Azure.Services.AppAuthentication` `1.0.3`stabile più recente di, fornisce il supporto per le [identità gestite assegnate dal sistema](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span><span class="sxs-lookup"><span data-stu-id="c8493-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="c8493-116">Il supporto per le `1.2.0-preview2` *identità gestite assegnate dall'utente* è disponibile nel pacchetto.</span><span class="sxs-lookup"><span data-stu-id="c8493-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="c8493-117">Questo argomento illustra l'uso delle identità gestite dal sistema e l'app `1.0.3` `Microsoft.Azure.Services.AppAuthentication` di esempio fornita usa la versione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="c8493-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="c8493-118">App di esempio</span><span class="sxs-lookup"><span data-stu-id="c8493-118">Sample app</span></span>

<span data-ttu-id="c8493-119">L'app di esempio viene eseguita in una delle due modalità determinate `#define` dall'istruzione all'inizio del file *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="c8493-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="c8493-120">`Certificate`&ndash; Viene illustrato l'uso di un ID client Azure Key Vault e di un certificato X. 509 per accedere ai segreti archiviati nel Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c8493-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="c8493-121">Questa versione dell'esempio può essere eseguita da qualsiasi posizione, distribuita nel servizio app Azure o in qualsiasi host in grado di servire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8493-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="c8493-122">`Managed`Illustra come usare le [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) per autenticare l'app Azure Key Vault con Azure ad l'autenticazione senza credenziali archiviate nel codice o nella configurazione dell'app. &ndash;</span><span class="sxs-lookup"><span data-stu-id="c8493-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="c8493-123">Quando si usano identità gestite per l'autenticazione, non è necessario un Azure AD ID applicazione e una password (segreto client).</span><span class="sxs-lookup"><span data-stu-id="c8493-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="c8493-124">La `Managed` versione dell'esempio deve essere distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="c8493-125">Seguire le istruzioni riportate nella sezione [usare le identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="c8493-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="c8493-126">Per ulteriori informazioni su come configurare un'app di esempio utilizzando le direttive per il preprocessore`#define`( <xref:index#preprocessor-directives-in-sample-code>), vedere.</span><span class="sxs-lookup"><span data-stu-id="c8493-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="c8493-127">Archiviazione segreta nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="c8493-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="c8493-128">Impostare i segreti in locale usando lo [strumento di gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c8493-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="c8493-129">Quando l'app di esempio viene eseguita nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio di gestione dei segreti locali.</span><span class="sxs-lookup"><span data-stu-id="c8493-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="c8493-130">Lo strumento di gestione dei segreti `<UserSecretsId>` richiede una proprietà nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="c8493-131">Impostare il valore della proprietà`{GUID}`() su un GUID univoco:</span><span class="sxs-lookup"><span data-stu-id="c8493-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="c8493-132">I segreti vengono creati come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="c8493-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="c8493-133">I valori gerarchici (sezioni di configurazione) `:` utilizzano un (due punti) come separatore nei nomi delle chiavi di [configurazione ASP.NET Core](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="c8493-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="c8493-134">Il gestore del segreto viene usato da una shell dei comandi aperta alla radice del contenuto del progetto `{SECRET NAME}` , dove è il `{SECRET VALUE}` nome e è il valore:</span><span class="sxs-lookup"><span data-stu-id="c8493-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="c8493-135">Eseguire i comandi seguenti in una shell dei comandi dalla radice del contenuto del progetto per impostare i segreti per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="c8493-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="c8493-136">Quando questi segreti vengono archiviati in Azure Key Vault nell' [archiviazione segreta nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il `_dev` suffisso viene modificato in. `_prod`</span><span class="sxs-lookup"><span data-stu-id="c8493-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="c8493-137">Il suffisso fornisce un segnale visivo nell'output dell'app che indica l'origine dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c8493-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="c8493-138">Archiviazione segreta nell'ambiente di produzione con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8493-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="c8493-139">Le istruzioni fornite dalla [Guida introduttiva: Impostare e recuperare un segreto da Azure Key Vault usando l'interfaccia](/azure/key-vault/quick-create-cli) della riga di comando di Azure sono riepilogati di seguito per la creazione di un Azure Key Vault e l'archiviazione di segreti usati dall'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="c8493-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="c8493-140">Per ulteriori informazioni, vedere l'argomento.</span><span class="sxs-lookup"><span data-stu-id="c8493-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="c8493-141">Aprire Azure cloud shell usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="c8493-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="c8493-142">Selezionare **try it** nell'angolo superiore destro di un blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="c8493-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="c8493-143">Usare la stringa di ricerca "interfaccia della riga di comando di Azure" nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="c8493-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="c8493-144">Aprire Cloud Shell nel browser con il pulsante **avvia cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="c8493-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="c8493-145">Selezionare il pulsante **cloud Shell** nel menu nell'angolo in alto a destra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="c8493-146">Per altre informazioni, vedere [interfaccia della riga di comando di Azure](/cli/azure/) e [Panoramica di Azure cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="c8493-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="c8493-147">Se non è già stato eseguito l'autenticazione, effettuare l' `az login` accesso con il comando.</span><span class="sxs-lookup"><span data-stu-id="c8493-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="c8493-148">Creare un gruppo di risorse con il comando seguente, `{RESOURCE GROUP NAME}` dove è il nome del gruppo di risorse per il nuovo `{LOCATION}` gruppo di risorse e è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="c8493-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="c8493-149">Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando `{KEY VAULT NAME}` seguente, dove è il nome del nuovo insieme `{LOCATION}` di credenziali delle chiavi e è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="c8493-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="c8493-150">Creare segreti nell'insieme di credenziali delle chiavi come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="c8493-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="c8493-151">Azure Key Vault nomi di segreto sono limitati a caratteri alfanumerici e trattini.</span><span class="sxs-lookup"><span data-stu-id="c8493-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="c8493-152">I valori gerarchici (sezioni di configurazione `--` ) usano (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="c8493-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="c8493-153">I due punti, che in genere vengono usati per delimitare una sezione da una sottochiave in [ASP.NET Core configurazione](xref:fundamentals/configuration/index), non sono consentiti nei nomi dei segreti di Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c8493-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="c8493-154">Pertanto, vengono usati due trattini e scambiati per i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="c8493-155">I segreti seguenti sono da usare con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="c8493-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="c8493-156">I valori includono un `_prod` suffisso per distinguerli `_dev` dai valori dei suffissi caricati nell'ambiente di sviluppo da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="c8493-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="c8493-157">Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali delle chiavi creato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="c8493-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="c8493-158">Usare l'ID applicazione e il certificato X. 509 per le app non ospitate in Azure</span><span class="sxs-lookup"><span data-stu-id="c8493-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="c8493-159">Configurare Azure AD, Azure Key Vault e l'app per usare un ID applicazione Azure Active Directory e un certificato X. 509 per l'autenticazione in un insieme di credenziali **delle chiavi quando l'app è ospitata all'esterno di Azure**.</span><span class="sxs-lookup"><span data-stu-id="c8493-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="c8493-160">Per ulteriori informazioni, vedere [informazioni su chiavi, segreti e certificati](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="c8493-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="c8493-161">Sebbene l'uso di un ID applicazione e di un certificato X. 509 sia supportato per le app ospitate in Azure, è consigliabile usare [identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) quando si ospita un'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="c8493-162">Le identità gestite non richiedono l'archiviazione di un certificato nell'app o nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c8493-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="c8493-163">L'app di esempio usa un ID applicazione e un certificato X. 509 `#define` quando l'istruzione all'inizio del file *Program.cs* è impostata su `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="c8493-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="c8493-164">Creare un certificato di archivio PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="c8493-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="c8493-165">Le opzioni per la creazione di certificati includono [Makecert in Windows](/windows/desktop/seccrypto/makecert) e [openssl](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="c8493-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="c8493-166">Installare il certificato nell'archivio certificati personale dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="c8493-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="c8493-167">Contrassegnare la chiave come esportabile è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="c8493-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="c8493-168">Prendere nota dell'identificazione personale del certificato, che verrà usato più avanti in questo processo.</span><span class="sxs-lookup"><span data-stu-id="c8493-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="c8493-169">Esportare il certificato di archiviazione PKCS # 12 ( *. pfx*) come certificato con codifica der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="c8493-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="c8493-170">Registrare l'app con Azure AD (**registrazioni app**).</span><span class="sxs-lookup"><span data-stu-id="c8493-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="c8493-171">Caricare il certificato con codifica DER ( *. cer*) in Azure ad:</span><span class="sxs-lookup"><span data-stu-id="c8493-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="c8493-172">Selezionare l'app in Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8493-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="c8493-173">Passare a **certificati & segreti**.</span><span class="sxs-lookup"><span data-stu-id="c8493-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="c8493-174">Selezionare **Carica certificato** per caricare il certificato, che contiene la chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="c8493-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="c8493-175">Un certificato con *estensione cer*, *PEM*o *CRT* è accettabile.</span><span class="sxs-lookup"><span data-stu-id="c8493-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="c8493-176">Archiviare il nome dell'insieme di credenziali delle chiavi, l'ID applicazione e l'identificazione personale del certificato nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="c8493-177">Passare a insiemi di credenziali delle **chiavi** nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="c8493-178">Selezionare l'insieme di credenziali delle chiavi creato nell' [Archivio Secret nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.</span><span class="sxs-lookup"><span data-stu-id="c8493-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="c8493-179">Selezionare **criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="c8493-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="c8493-180">Selezionare **Aggiungi nuovo**.</span><span class="sxs-lookup"><span data-stu-id="c8493-180">Select **Add new**.</span></span>
1. <span data-ttu-id="c8493-181">Selezionare **Seleziona entità** e selezionare l'app registrata in base al nome.</span><span class="sxs-lookup"><span data-stu-id="c8493-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="c8493-182">Selezionare il pulsante **Seleziona** .</span><span class="sxs-lookup"><span data-stu-id="c8493-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="c8493-183">Aprire **autorizzazioni segrete** e fornire all'app le autorizzazioni **Get** ed **List** .</span><span class="sxs-lookup"><span data-stu-id="c8493-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="c8493-184">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8493-184">Select **OK**.</span></span>
1. <span data-ttu-id="c8493-185">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="c8493-185">Select **Save**.</span></span>
1. <span data-ttu-id="c8493-186">Distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-186">Deploy the app.</span></span>

<span data-ttu-id="c8493-187">L' `Certificate` app di esempio ottiene i valori di configurazione `IConfigurationRoot` da con lo stesso nome del nome del segreto:</span><span class="sxs-lookup"><span data-stu-id="c8493-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="c8493-188">Valori non gerarchici: Il valore per `SecretName` viene ottenuto con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="c8493-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="c8493-189">Valori gerarchici (sezioni): Usare `:` la notazione (due punti) `GetSection` o il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="c8493-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="c8493-190">Per ottenere il valore di configurazione, usare uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="c8493-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="c8493-191">Il certificato X. 509 è gestito dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="c8493-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="c8493-192">L'app chiama `AddAzureKeyVault` con i valori forniti dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="c8493-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="c8493-193">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="c8493-193">Example values:</span></span>

* <span data-ttu-id="c8493-194">Nome dell'insieme di credenziali delle chiavi:`contosovault`</span><span class="sxs-lookup"><span data-stu-id="c8493-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="c8493-195">ID applicazione:`627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="c8493-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="c8493-196">Identificazione personale del certificato:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="c8493-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="c8493-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c8493-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="c8493-198">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="c8493-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="c8493-199">Nell'ambiente di sviluppo, i valori Secret vengono caricati `_dev` con il suffisso.</span><span class="sxs-lookup"><span data-stu-id="c8493-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="c8493-200">Nell'ambiente di produzione, i valori vengono caricati con `_prod` il suffisso.</span><span class="sxs-lookup"><span data-stu-id="c8493-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="c8493-201">Usare identità gestite per le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="c8493-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="c8493-202">**Un'app distribuita in Azure** può sfruttare le [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview), che consentono all'app di eseguire l'autenticazione con Azure Key Vault usando l'autenticazione Azure ad senza credenziali (ID applicazione e password/segreto client) archiviati nell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="c8493-203">L'app di esempio usa le identità gestite per le risorse di `#define` Azure quando l'istruzione all'inizio del file *Program.cs* è impostata `Managed`su.</span><span class="sxs-lookup"><span data-stu-id="c8493-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="c8493-204">Immettere il nome dell'insieme di credenziali nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="c8493-205">L'app di esempio non richiede un ID applicazione e una password (segreto client) quando è `Managed` impostata sulla versione, in modo che sia possibile ignorare tali voci di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c8493-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="c8493-206">L'app viene distribuita in Azure e Azure autentica l'app per accedere Azure Key Vault solo usando il nome dell'insieme di credenziali archiviato nel file *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="c8493-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="c8493-207">Distribuire l'app di esempio nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="c8493-208">Un'app distribuita nel servizio app Azure viene registrata automaticamente con Azure AD quando viene creato il servizio.</span><span class="sxs-lookup"><span data-stu-id="c8493-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="c8493-209">Ottenere l'ID oggetto dalla distribuzione per utilizzarlo nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="c8493-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="c8493-210">L'ID oggetto viene visualizzato nella portale di Azure nel pannello **identità** del servizio app.</span><span class="sxs-lookup"><span data-stu-id="c8493-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="c8493-211">Usando l'interfaccia della riga di comando di Azure e l'ID oggetto dell' `list` app `get` , fornire all'app le autorizzazioni e per accedere all'insieme di credenziali delle chiavi:</span><span class="sxs-lookup"><span data-stu-id="c8493-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="c8493-212">**Riavviare l'app** usando l'interfaccia della riga di comando di Azure, PowerShell o la portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="c8493-213">App di esempio:</span><span class="sxs-lookup"><span data-stu-id="c8493-213">The sample app:</span></span>

* <span data-ttu-id="c8493-214">Crea un'istanza della `AzureServiceTokenProvider` classe senza una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8493-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="c8493-215">Quando non viene specificata una stringa di connessione, il provider tenta di ottenere un token di accesso dalle identità gestite per le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="c8493-216">Viene creato `KeyVaultClient` un nuovo oggetto con `AzureServiceTokenProvider` il callback del token dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="c8493-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="c8493-217">L' `KeyVaultClient` istanza viene utilizzata con un'implementazione predefinita di `IKeyVaultSecretManager` che carica tutti i valori del segreto e sostituisce i doppi trattini (`--`) con i`:`due punti () nei nomi delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="c8493-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="c8493-218">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="c8493-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="c8493-219">Nell'ambiente di sviluppo i valori Secret hanno il `_dev` suffisso perché sono forniti da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="c8493-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="c8493-220">Nell'ambiente di produzione, i valori vengono caricati con `_prod` il suffisso perché sono forniti da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c8493-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="c8493-221">Se viene visualizzato un `Access denied` errore, verificare che l'app sia registrata con Azure ad e che l'accesso sia stato fornito all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="c8493-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="c8493-222">Confermare di aver riavviato il servizio in Azure.</span><span class="sxs-lookup"><span data-stu-id="c8493-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="c8493-223">Usa prefisso nome chiave</span><span class="sxs-lookup"><span data-stu-id="c8493-223">Use a key name prefix</span></span>

<span data-ttu-id="c8493-224">`AddAzureKeyVault`fornisce un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare la modalità di conversione dei segreti dell'insieme di credenziali delle chiavi in chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c8493-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="c8493-225">Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti in base a un valore di prefisso fornito all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="c8493-226">Questo consente, ad esempio, di caricare i segreti in base alla versione dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="c8493-227">Non usare i prefissi sui segreti dell'insieme di credenziali delle chiavi per collocare i segreti per più app nello stesso insieme di credenziali delle chiavi o per collocare i segreti ambientali (ad esempio, *sviluppo* rispetto ai segreti di *produzione* ) nello stesso insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="c8493-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="c8493-228">È consigliabile che app e ambienti di sviluppo/produzione diversi usino insiemi di credenziali delle chiavi distinti per isolare gli ambienti app per il massimo livello di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="c8493-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="c8493-229">Nell'esempio seguente viene stabilito un segreto nell'insieme di credenziali delle chiavi (e usando lo strumento di gestione dei segreti per l'ambiente di `5000-AppSecret` sviluppo) per (i periodi non sono consentiti nei nomi dei segreti dell'insieme di credenziali delle chiavi).</span><span class="sxs-lookup"><span data-stu-id="c8493-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="c8493-230">Questo segreto rappresenta un segreto app per la versione 5.0.0.0 dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="c8493-231">Per un'altra versione dell'app, 5.1.0.0, viene aggiunto un segreto all'insieme di credenziali delle chiavi (e usando lo strumento di gestione `5100-AppSecret`dei segreti) per.</span><span class="sxs-lookup"><span data-stu-id="c8493-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="c8493-232">Ogni versione dell'app carica il valore del segreto con versione nella configurazione `AppSecret`come, rimuovendo la versione durante il caricamento del segreto.</span><span class="sxs-lookup"><span data-stu-id="c8493-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="c8493-233">`AddAzureKeyVault`viene chiamato con un oggetto `IKeyVaultSecretManager`personalizzato:</span><span class="sxs-lookup"><span data-stu-id="c8493-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="c8493-234">L' `IKeyVaultSecretManager` implementazione reagisce ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:</span><span class="sxs-lookup"><span data-stu-id="c8493-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="c8493-235">Il `Load` metodo viene chiamato da un algoritmo del provider che scorre i segreti dell'insieme di credenziali per trovare quelli con il prefisso della versione.</span><span class="sxs-lookup"><span data-stu-id="c8493-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="c8493-236">Quando viene trovato un prefisso di versione `Load`con, l'algoritmo usa `GetKey` il metodo per restituire il nome della configurazione del nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="c8493-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="c8493-237">Rimuove il prefisso della versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nelle coppie nome-valore della configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="c8493-238">Quando viene implementato questo approccio:</span><span class="sxs-lookup"><span data-stu-id="c8493-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="c8493-239">La versione dell'app specificata nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="c8493-240">Nell'esempio seguente la versione dell'app è impostata su `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="c8493-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="c8493-241">Verificare che sia `<UserSecretsId>` presente una proprietà nel file di progetto dell'app, dove `{GUID}` è un GUID fornito dall'utente:</span><span class="sxs-lookup"><span data-stu-id="c8493-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="c8493-242">Salvare i segreti seguenti localmente con lo [strumento di gestione dei segreti](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="c8493-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="c8493-243">I segreti vengono salvati in Azure Key Vault usando i comandi dell'interfaccia della riga di comando di Azure seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8493-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="c8493-244">Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="c8493-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="c8493-245">La stringa segreta per `5000-AppSecret` viene confrontata con la versione dell'app specificata nel file di progetto dell'app`5.0.0.0`().</span><span class="sxs-lookup"><span data-stu-id="c8493-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="c8493-246">La versione `5000` (con il trattino) viene rimossa dal nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="c8493-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="c8493-247">In tutta l'app, la lettura della configurazione `AppSecret` con la chiave carica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="c8493-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="c8493-248">Se la versione dell'app viene modificata nel file di `5.1.0.0` progetto in e l'app viene nuovamente eseguita, il valore del segreto restituito è `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo `5.1.0.0_secret_value_prod` e in produzione.</span><span class="sxs-lookup"><span data-stu-id="c8493-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="c8493-249">È anche possibile fornire un'implementazione `KeyVaultClient` personalizzata a `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="c8493-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="c8493-250">Un client personalizzato consente la condivisione di una singola istanza del client nell'app.</span><span class="sxs-lookup"><span data-stu-id="c8493-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="c8493-251">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="c8493-251">Bind an array to a class</span></span>

<span data-ttu-id="c8493-252">Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.</span><span class="sxs-lookup"><span data-stu-id="c8493-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="c8493-253">Quando si legge da un'origine di configurazione che consente alle chiavi di`:`contenere separatori di due punti (), viene usato un segmento di chiave numerico per distinguere le chiavi`:0:`che `:1:`costituiscono una matrice (,,...</span><span class="sxs-lookup"><span data-stu-id="c8493-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="c8493-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="c8493-254">`:{n}:`).</span></span> <span data-ttu-id="c8493-255">Per ulteriori informazioni, vedere [la pagina relativa alla configurazione. Associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="c8493-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="c8493-256">Azure Key Vault chiavi non possono utilizzare i due punti come separatore.</span><span class="sxs-lookup"><span data-stu-id="c8493-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="c8493-257">L'approccio descritto in questo argomento USA trattini doppi (`--`) come separatore per i valori gerarchici (sezioni).</span><span class="sxs-lookup"><span data-stu-id="c8493-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="c8493-258">Le chiavi di matrice vengono archiviate in Azure Key Vault con trattini doppi e segmenti`--0--`di `--1--`chiave &hellip; numerica (,, `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="c8493-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="c8493-259">Esaminare la seguente configurazione del provider di registrazione [Serilog](https://serilog.net/) fornita da un file JSON.</span><span class="sxs-lookup"><span data-stu-id="c8493-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="c8493-260">Nella matrice sono definiti due valori letterali di `WriteTo` oggetto che riflettono due *sink*Serilog, che descrivono le destinazioni per la registrazione dell'output:</span><span class="sxs-lookup"><span data-stu-id="c8493-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="c8493-261">La configurazione mostrata nel file JSON precedente viene archiviata in Azure Key Vault usando la notazione`--`a doppio trattino () e i segmenti numerici:</span><span class="sxs-lookup"><span data-stu-id="c8493-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="c8493-262">Chiave</span><span class="sxs-lookup"><span data-stu-id="c8493-262">Key</span></span> | <span data-ttu-id="c8493-263">Value</span><span class="sxs-lookup"><span data-stu-id="c8493-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="c8493-264">Ricarica segreti</span><span class="sxs-lookup"><span data-stu-id="c8493-264">Reload secrets</span></span>

<span data-ttu-id="c8493-265">I segreti vengono memorizzati `IConfigurationRoot.Reload()` nella cache finché non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c8493-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="c8493-266">I segreti scaduti, disabilitati e aggiornati nell'insieme di credenziali delle chiavi non vengono rispettati `Reload` dall'app fino a quando non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="c8493-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="c8493-267">Segreti disabilitati e scaduti</span><span class="sxs-lookup"><span data-stu-id="c8493-267">Disabled and expired secrets</span></span>

<span data-ttu-id="c8493-268">I segreti disabilitati e scaduti `KeyVaultClientException` generano un oggetto in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c8493-268">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="c8493-269">Per impedire che l'app venga generata, fornire la configurazione usando un provider di configurazione diverso o aggiornare il segreto disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="c8493-269">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="c8493-270">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="c8493-270">Troubleshoot</span></span>

<span data-ttu-id="c8493-271">Quando l'app non riesce a caricare la configurazione usando il provider, viene scritto un messaggio di errore nell' [infrastruttura di registrazione ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c8493-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="c8493-272">Le condizioni seguenti impediranno il caricamento della configurazione:</span><span class="sxs-lookup"><span data-stu-id="c8493-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="c8493-273">L'app o il certificato non è configurato correttamente in Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c8493-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="c8493-274">L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c8493-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="c8493-275">L'app non è autorizzata ad accedere all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="c8493-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="c8493-276">Il criterio di accesso non `Get` include `List` le autorizzazioni e.</span><span class="sxs-lookup"><span data-stu-id="c8493-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="c8493-277">Nell'insieme di credenziali delle chiavi i dati di configurazione (coppia nome-valore) sono denominati, mancanti, disabilitati o scaduti in modo errato.</span><span class="sxs-lookup"><span data-stu-id="c8493-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="c8493-278">L'app presenta il nome dell'insieme di credenziali`KeyVaultName`delle chiavi errato (),`AzureADApplicationId`Azure ad ID applicazione () o l'`AzureADCertThumbprint`identificazione personale del certificato Azure ad ().</span><span class="sxs-lookup"><span data-stu-id="c8493-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="c8493-279">La chiave di configurazione (nome) non è corretta nell'app per il valore che si sta provando a caricare.</span><span class="sxs-lookup"><span data-stu-id="c8493-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8493-280">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c8493-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="c8493-281">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8493-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="c8493-282">Microsoft Azure: Documentazione di Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8493-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="c8493-283">Come generare e trasferire chiavi HSM protette per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c8493-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="c8493-284">Classe KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="c8493-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="c8493-285">Avvio rapido: Impostare e recuperare un segreto da Azure Key Vault usando un'app Web .NET</span><span class="sxs-lookup"><span data-stu-id="c8493-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="c8493-286">Esercitazione: Come usare Azure Key Vault con la macchina virtuale Windows di Azure in .NET</span><span class="sxs-lookup"><span data-stu-id="c8493-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
