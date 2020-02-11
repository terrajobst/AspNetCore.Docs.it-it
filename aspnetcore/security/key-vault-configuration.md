---
title: Provider di configurazione di Azure Key Vault in ASP.NET Core
author: guardrex
description: Informazioni su come usare il provider di configurazione Azure Key Vault per configurare un'app usando coppie nome-valore caricate in fase di esecuzione.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: security/key-vault-configuration
ms.openlocfilehash: 7eb8cf5dcd6b9f112a2ef30e694b6223a7d1f2fe
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114874"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="876c1-103">Provider di configurazione di Azure Key Vault in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="876c1-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="876c1-104">Di [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="876c1-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="876c1-105">Questo documento illustra come usare il provider di configurazione [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) per caricare i valori di configurazione dell'app da Azure Key Vault segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="876c1-106">Azure Key Vault è un servizio basato sul cloud che aiuta a proteggere le chiavi crittografiche e i segreti usati da app e servizi.</span><span class="sxs-lookup"><span data-stu-id="876c1-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="876c1-107">Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core includono:</span><span class="sxs-lookup"><span data-stu-id="876c1-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="876c1-108">Controllo dell'accesso ai dati di configurazione riservati.</span><span class="sxs-lookup"><span data-stu-id="876c1-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="876c1-109">Soddisfare i requisiti per i moduli di protezione hardware convalidati FIPS 140-2 Level 2 (HSM) quando si archiviano i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="876c1-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="876c1-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="876c1-111">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="876c1-111">Packages</span></span>

<span data-ttu-id="876c1-112">Aggiungere un riferimento al pacchetto [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="876c1-112">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="876c1-113">App di esempio</span><span class="sxs-lookup"><span data-stu-id="876c1-113">Sample app</span></span>

<span data-ttu-id="876c1-114">L'app di esempio viene eseguita in una delle due modalità determinate dall'istruzione `#define` nella parte superiore del file *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="876c1-114">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="876c1-115">`Certificate` &ndash; illustra l'uso di un ID client Azure Key Vault e di un certificato X. 509 per accedere ai segreti archiviati in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-115">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="876c1-116">Questa versione dell'esempio può essere eseguita da qualsiasi posizione, distribuita nel servizio app Azure o in qualsiasi host in grado di servire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="876c1-116">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="876c1-117">`Managed` &ndash; illustra come usare le [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) per autenticare l'app per Azure Key Vault con Azure ad autenticazione senza credenziali archiviate nel codice o nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-117">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="876c1-118">Quando si usano identità gestite per l'autenticazione, non è necessario un Azure AD ID applicazione e una password (segreto client).</span><span class="sxs-lookup"><span data-stu-id="876c1-118">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="876c1-119">La versione `Managed` dell'esempio deve essere distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-119">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="876c1-120">Seguire le istruzioni riportate nella sezione [usare le identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="876c1-120">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="876c1-121">Per altre informazioni su come configurare un'app di esempio usando le direttive per il preprocessore (`#define`), vedere <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="876c1-121">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="876c1-122">Archiviazione segreta nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="876c1-122">Secret storage in the Development environment</span></span>

<span data-ttu-id="876c1-123">Impostare i segreti in locale usando lo [strumento di gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="876c1-123">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="876c1-124">Quando l'app di esempio viene eseguita nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio di gestione dei segreti locali.</span><span class="sxs-lookup"><span data-stu-id="876c1-124">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="876c1-125">Lo strumento di gestione dei segreti richiede una proprietà `<UserSecretsId>` nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-125">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="876c1-126">Impostare il valore della proprietà (`{GUID}`) su un GUID univoco:</span><span class="sxs-lookup"><span data-stu-id="876c1-126">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="876c1-127">I segreti vengono creati come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="876c1-127">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="876c1-128">I valori gerarchici (sezioni di configurazione) usano un `:` (due punti) come separatore nei nomi delle chiavi di [configurazione ASP.NET Core](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="876c1-128">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="876c1-129">Il gestore del segreto viene usato da una shell dei comandi aperta alla [radice del contenuto](xref:fundamentals/index#content-root)del progetto, dove `{SECRET NAME}` è il nome e `{SECRET VALUE}` è il valore:</span><span class="sxs-lookup"><span data-stu-id="876c1-129">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="876c1-130">Eseguire i comandi seguenti in una shell dei comandi dalla radice del [contenuto](xref:fundamentals/index#content-root) del progetto per impostare i segreti per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-130">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="876c1-131">Quando questi segreti vengono archiviati in Azure Key Vault nell' [archiviazione segreta nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il suffisso `_dev` viene modificato in `_prod`.</span><span class="sxs-lookup"><span data-stu-id="876c1-131">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="876c1-132">Il suffisso fornisce un segnale visivo nell'output dell'app che indica l'origine dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-132">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="876c1-133">Archiviazione segreta nell'ambiente di produzione con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-133">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="876c1-134">Le istruzioni fornite nell'argomento [avvio rapido: impostare e recuperare un segreto da Azure Key Vault usando l'interfaccia](/azure/key-vault/quick-create-cli) della riga di comando di Azure sono riepilogate qui per la creazione di un Azure Key Vault e l'archiviazione di segreti usati dall'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="876c1-134">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="876c1-135">Per ulteriori informazioni, vedere l'argomento.</span><span class="sxs-lookup"><span data-stu-id="876c1-135">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="876c1-136">Aprire Azure cloud shell usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="876c1-136">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="876c1-137">Selezionare **Prova** nell'angolo superiore destro di un blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="876c1-137">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="876c1-138">Usare la stringa di ricerca "interfaccia della riga di comando di Azure" nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="876c1-138">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="876c1-139">Aprire Cloud Shell nel browser con il pulsante **avvia cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="876c1-139">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="876c1-140">Selezionare il pulsante **Cloud Shell** nel menu nell'angolo in alto a destra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-140">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="876c1-141">Per altre informazioni, vedere l'interfaccia della riga di comando di [Azure](/cli/azure/) e [Panoramica di Azure cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="876c1-141">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="876c1-142">Se non è già stato eseguito l'autenticazione, effettuare l'accesso con il comando `az login`.</span><span class="sxs-lookup"><span data-stu-id="876c1-142">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="876c1-143">Creare un gruppo di risorse con il comando seguente, dove `{RESOURCE GROUP NAME}` è il nome del gruppo di risorse per il nuovo gruppo di risorse e `{LOCATION}` è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="876c1-143">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="876c1-144">Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando seguente, dove `{KEY VAULT NAME}` è il nome del nuovo insieme di credenziali delle chiavi e `{LOCATION}` è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="876c1-144">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="876c1-145">Creare segreti nell'insieme di credenziali delle chiavi come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="876c1-145">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="876c1-146">Azure Key Vault nomi di segreto sono limitati a caratteri alfanumerici e trattini.</span><span class="sxs-lookup"><span data-stu-id="876c1-146">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="876c1-147">I valori gerarchici (sezioni di configurazione) usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="876c1-147">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="876c1-148">I due punti, che in genere vengono usati per delimitare una sezione da una sottochiave in [ASP.NET Core configurazione](xref:fundamentals/configuration/index), non sono consentiti nei nomi dei segreti di Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-148">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="876c1-149">Pertanto, vengono usati due trattini e scambiati per i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-149">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="876c1-150">I segreti seguenti sono da usare con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="876c1-150">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="876c1-151">I valori includono un suffisso di `_prod` per distinguerli dai valori dei suffissi `_dev` caricati nell'ambiente di sviluppo da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="876c1-151">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="876c1-152">Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali delle chiavi creato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="876c1-152">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="876c1-153">Usare l'ID applicazione e il certificato X. 509 per le app non ospitate in Azure</span><span class="sxs-lookup"><span data-stu-id="876c1-153">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="876c1-154">Configurare Azure AD, Azure Key Vault e l'app per usare un ID applicazione Azure Active Directory e un certificato X. 509 per l'autenticazione in un insieme di credenziali **delle chiavi quando l'app è ospitata all'esterno di Azure**.</span><span class="sxs-lookup"><span data-stu-id="876c1-154">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="876c1-155">Per i dettagli, vedere l'articolo relativo alle [informazioni su chiavi, segreti e certificati](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="876c1-155">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="876c1-156">Sebbene l'uso di un ID applicazione e di un certificato X. 509 sia supportato per le app ospitate in Azure, è consigliabile usare [identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) quando si ospita un'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-156">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="876c1-157">Le identità gestite non richiedono l'archiviazione di un certificato nell'app o nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="876c1-157">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="876c1-158">L'app di esempio usa un ID applicazione e un certificato X. 509 quando l'istruzione `#define` nella parte superiore del file *Program.cs* è impostata su `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="876c1-158">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="876c1-159">Creare un certificato di archivio PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="876c1-159">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="876c1-160">Le opzioni per la creazione di certificati includono [Makecert in Windows](/windows/desktop/seccrypto/makecert) e [openssl](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="876c1-160">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="876c1-161">Installare il certificato nell'archivio certificati personale dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="876c1-161">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="876c1-162">Contrassegnare la chiave come esportabile è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="876c1-162">Marking the key as exportable is optional.</span></span> <span data-ttu-id="876c1-163">Prendere nota dell'identificazione personale del certificato, che verrà usato più avanti in questo processo.</span><span class="sxs-lookup"><span data-stu-id="876c1-163">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="876c1-164">Esportare il certificato di archiviazione PKCS # 12 ( *. pfx*) come certificato con codifica der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="876c1-164">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="876c1-165">Registrare l'app con Azure AD (**registrazioni app**).</span><span class="sxs-lookup"><span data-stu-id="876c1-165">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="876c1-166">Caricare il certificato con codifica DER ( *. cer*) in Azure ad:</span><span class="sxs-lookup"><span data-stu-id="876c1-166">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="876c1-167">Selezionare l'app in Azure AD.</span><span class="sxs-lookup"><span data-stu-id="876c1-167">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="876c1-168">Passare a **certificati & segreti**.</span><span class="sxs-lookup"><span data-stu-id="876c1-168">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="876c1-169">Selezionare **Carica certificato** per caricare il certificato, che contiene la chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="876c1-169">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="876c1-170">Un certificato con *estensione cer*, *PEM*o *CRT* è accettabile.</span><span class="sxs-lookup"><span data-stu-id="876c1-170">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="876c1-171">Archiviare il nome dell'insieme di credenziali delle chiavi, l'ID applicazione e l'identificazione personale del certificato nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-171">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="876c1-172">Passare a insiemi di credenziali delle **chiavi** nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="876c1-173">Selezionare l'insieme di credenziali delle chiavi creato nell' [Archivio Secret nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.</span><span class="sxs-lookup"><span data-stu-id="876c1-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="876c1-174">Selezionare **Criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="876c1-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="876c1-175">Selezionare **Aggiungi criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="876c1-175">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="876c1-176">Aprire **autorizzazioni segrete** e fornire all'app le autorizzazioni **Get** ed **List** .</span><span class="sxs-lookup"><span data-stu-id="876c1-176">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="876c1-177">Selezionare **Seleziona entità** e selezionare l'app registrata in base al nome.</span><span class="sxs-lookup"><span data-stu-id="876c1-177">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="876c1-178">Fare clic sul pulsante **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="876c1-178">Select the **Select** button.</span></span>
1. <span data-ttu-id="876c1-179">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="876c1-179">Select **OK**.</span></span>
1. <span data-ttu-id="876c1-180">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="876c1-180">Select **Save**.</span></span>
1. <span data-ttu-id="876c1-181">Distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-181">Deploy the app.</span></span>

<span data-ttu-id="876c1-182">L'app di esempio `Certificate` ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome del nome del segreto:</span><span class="sxs-lookup"><span data-stu-id="876c1-182">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="876c1-183">Valori non gerarchici: il valore per `SecretName` viene ottenuto con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="876c1-183">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="876c1-184">Valori gerarchici (sezioni): usare la notazione `:` (due punti) o il metodo di estensione `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="876c1-184">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="876c1-185">Per ottenere il valore di configurazione, usare uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="876c1-185">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="876c1-186">Il certificato X. 509 è gestito dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="876c1-186">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="876c1-187">L'app chiama <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> con i valori forniti dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="876c1-187">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="876c1-188">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-188">Example values:</span></span>

* <span data-ttu-id="876c1-189">Nome dell'insieme di credenziali delle chiavi: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="876c1-189">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="876c1-190">ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="876c1-190">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="876c1-191">Identificazione personale del certificato: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="876c1-191">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="876c1-192">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="876c1-192">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/3.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="876c1-193">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="876c1-193">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="876c1-194">Nell'ambiente di sviluppo, i valori Secret vengono caricati con il suffisso `_dev`.</span><span class="sxs-lookup"><span data-stu-id="876c1-194">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="876c1-195">Nell'ambiente di produzione, i valori vengono caricati con il suffisso `_prod`.</span><span class="sxs-lookup"><span data-stu-id="876c1-195">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="876c1-196">Usare identità gestite per le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="876c1-196">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="876c1-197">**Un'app distribuita in Azure** può trarre vantaggio dalle [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview), che consente all'app di eseguire l'autenticazione con Azure Key Vault usando Azure ad autenticazione senza credenziali (ID applicazione e password/segreto client) archiviate nell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-197">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="876c1-198">L'app di esempio usa le identità gestite per le risorse di Azure quando l'istruzione `#define` nella parte superiore del file *Program.cs* è impostata su `Managed`.</span><span class="sxs-lookup"><span data-stu-id="876c1-198">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="876c1-199">Immettere il nome dell'insieme di credenziali nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-199">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="876c1-200">L'app di esempio non richiede un ID applicazione e una password (segreto client) quando è impostata sulla versione `Managed`, quindi è possibile ignorare tali voci di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-200">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="876c1-201">L'app viene distribuita in Azure e Azure autentica l'app per accedere Azure Key Vault solo usando il nome dell'insieme di credenziali archiviato nel file *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="876c1-201">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="876c1-202">Distribuire l'app di esempio nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-202">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="876c1-203">Un'app distribuita nel servizio app Azure viene registrata automaticamente con Azure AD quando viene creato il servizio.</span><span class="sxs-lookup"><span data-stu-id="876c1-203">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="876c1-204">Ottenere l'ID oggetto dalla distribuzione per utilizzarlo nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="876c1-204">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="876c1-205">L'ID oggetto viene visualizzato nella portale di Azure nel pannello **identità** del servizio app.</span><span class="sxs-lookup"><span data-stu-id="876c1-205">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="876c1-206">Usando l'interfaccia della riga di comando di Azure e l'ID oggetto dell'app, fornire all'app le autorizzazioni `list` e `get` per accedere all'insieme di credenziali delle chiavi:</span><span class="sxs-lookup"><span data-stu-id="876c1-206">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="876c1-207">**Riavviare l'app** usando l'interfaccia della riga di comando di Azure, PowerShell o la portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-207">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="876c1-208">App di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-208">The sample app:</span></span>

* <span data-ttu-id="876c1-209">Crea un'istanza della classe `AzureServiceTokenProvider` senza una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="876c1-209">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="876c1-210">Quando non viene specificata una stringa di connessione, il provider tenta di ottenere un token di accesso dalle identità gestite per le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-210">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="876c1-211">Viene creata una nuova <xref:Microsoft.Azure.KeyVault.KeyVaultClient> con il callback del token dell'istanza di `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="876c1-211">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="876c1-212">L'istanza di <xref:Microsoft.Azure.KeyVault.KeyVaultClient> viene utilizzata con un'implementazione predefinita di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> che carica tutti i valori del segreto e sostituisce i doppi trattini (`--`) con i due punti (`:`) nei nomi di chiave.</span><span class="sxs-lookup"><span data-stu-id="876c1-212">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/3.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="876c1-213">Esempio di nome dell'insieme di credenziali delle chiavi: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="876c1-213">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="876c1-214">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="876c1-214">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="876c1-215">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="876c1-215">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="876c1-216">Nell'ambiente di sviluppo i valori Secret hanno il suffisso `_dev` perché sono forniti da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="876c1-216">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="876c1-217">Nell'ambiente di produzione, i valori vengono caricati con il suffisso `_prod` perché sono forniti da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-217">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="876c1-218">Se viene visualizzato un errore di `Access denied`, verificare che l'app sia registrata con Azure AD e che l'accesso sia stato fornito all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-218">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="876c1-219">Confermare di aver riavviato il servizio in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-219">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="876c1-220">Per informazioni sull'uso del provider con un'identità gestita e una pipeline di Azure DevOps, vedere [creare una connessione del servizio Azure Resource Manager a una macchina virtuale con un'identità del servizio gestito](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="876c1-220">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="configuration-options"></a><span data-ttu-id="876c1-221">Opzioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="876c1-221">Configuration options</span></span>

<span data-ttu-id="876c1-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> possibile accettare una <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span><span class="sxs-lookup"><span data-stu-id="876c1-222"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> can accept an <xref:Microsoft.Extensions.Configuration.AzureKeyVault.AzureKeyVaultConfigurationOptions>:</span></span>

```csharp
config.AddAzureKeyVault(
    new AzureKeyVaultConfigurationOptions()
    {
        ...
    });
```

| <span data-ttu-id="876c1-223">Proprietà</span><span class="sxs-lookup"><span data-stu-id="876c1-223">Property</span></span>         | <span data-ttu-id="876c1-224">Descrizione</span><span class="sxs-lookup"><span data-stu-id="876c1-224">Description</span></span> |
| ---------------- | ----------- |
| `Client`         | <span data-ttu-id="876c1-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> da utilizzare per il recupero dei valori.</span><span class="sxs-lookup"><span data-stu-id="876c1-225"><xref:Microsoft.Azure.KeyVault.KeyVaultClient> to use for retrieving values.</span></span> |
| `Manager`        | <span data-ttu-id="876c1-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> istanza utilizzata per controllare il caricamento dei segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-226"><xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> instance used to control secret loading.</span></span> |
| `ReloadInterval` | <span data-ttu-id="876c1-227">`Timespan` di attesa tra i tentativi di polling dell'insieme di credenziali delle chiavi per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="876c1-227">`Timespan` to wait between attempts at polling the key vault for changes.</span></span> <span data-ttu-id="876c1-228">Il valore predefinito è `null` (la configurazione non viene ricaricata).</span><span class="sxs-lookup"><span data-stu-id="876c1-228">The default value is `null` (configuration isn't reloaded).</span></span> |
| `Vault`          | <span data-ttu-id="876c1-229">URI dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-229">Key vault URI.</span></span> |

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="876c1-230">Usa prefisso nome chiave</span><span class="sxs-lookup"><span data-stu-id="876c1-230">Use a key name prefix</span></span>

<span data-ttu-id="876c1-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> fornisce un overload che accetta un'implementazione di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, che consente di controllare la modalità di conversione dei segreti dell'insieme di credenziali delle chiavi in chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-231"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="876c1-232">Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti in base a un valore di prefisso fornito all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-232">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="876c1-233">Questo consente, ad esempio, di caricare i segreti in base alla versione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-233">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="876c1-234">Non usare i prefissi sui segreti dell'insieme di credenziali delle chiavi per collocare i segreti per più app nello stesso insieme di credenziali delle chiavi o per collocare i segreti ambientali (ad esempio, *sviluppo* rispetto ai segreti di *produzione* ) nello stesso insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="876c1-234">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="876c1-235">È consigliabile che app e ambienti di sviluppo/produzione diversi usino insiemi di credenziali delle chiavi distinti per isolare gli ambienti app per il massimo livello di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="876c1-235">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="876c1-236">Nell'esempio seguente viene stabilito un segreto nell'insieme di credenziali delle chiavi (e usando lo strumento di gestione dei segreti per l'ambiente di sviluppo) per `5000-AppSecret` (i periodi non sono consentiti nei nomi dei segreti dell'insieme di credenziali delle chiavi).</span><span class="sxs-lookup"><span data-stu-id="876c1-236">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="876c1-237">Questo segreto rappresenta un segreto app per la versione 5.0.0.0 dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-237">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="876c1-238">Per un'altra versione dell'app, 5.1.0.0, viene aggiunto un segreto all'insieme di credenziali delle chiavi (e usando lo strumento di gestione dei segreti) per `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="876c1-238">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="876c1-239">Ogni versione dell'app carica il valore del segreto con versione nella configurazione come `AppSecret`, rimuovendo la versione mentre carica il segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-239">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="876c1-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> viene chiamato con un <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>personalizzato:</span><span class="sxs-lookup"><span data-stu-id="876c1-240"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="876c1-241">L'implementazione di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> reagisce ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:</span><span class="sxs-lookup"><span data-stu-id="876c1-241">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="876c1-242">`Load` carica un segreto quando il nome inizia con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="876c1-242">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="876c1-243">Non vengono caricati altri segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-243">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="876c1-244">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="876c1-244">`GetKey`:</span></span>
  * <span data-ttu-id="876c1-245">Rimuove il prefisso dal nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-245">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="876c1-246">Sostituisce due trattini in qualsiasi nome con il `KeyDelimiter`, ovvero il delimitatore usato nella configurazione, in genere i due punti.</span><span class="sxs-lookup"><span data-stu-id="876c1-246">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="876c1-247">Azure Key Vault non consente i due punti nei nomi dei segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-247">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="876c1-248">Il metodo `Load` viene chiamato da un algoritmo del provider che scorre i segreti dell'insieme di credenziali per trovare quelli con il prefisso della versione.</span><span class="sxs-lookup"><span data-stu-id="876c1-248">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="876c1-249">Quando viene trovato un prefisso di versione con `Load`, l'algoritmo usa il metodo `GetKey` per restituire il nome della configurazione del nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-249">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="876c1-250">Rimuove il prefisso della versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nelle coppie nome-valore della configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-250">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="876c1-251">Quando viene implementato questo approccio:</span><span class="sxs-lookup"><span data-stu-id="876c1-251">When this approach is implemented:</span></span>

1. <span data-ttu-id="876c1-252">La versione dell'app specificata nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-252">The app's version specified in the app's project file.</span></span> <span data-ttu-id="876c1-253">Nell'esempio seguente la versione dell'app è impostata su `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="876c1-253">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="876c1-254">Verificare che nel file di progetto dell'app sia presente una proprietà `<UserSecretsId>`, dove `{GUID}` è un GUID fornito dall'utente:</span><span class="sxs-lookup"><span data-stu-id="876c1-254">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="876c1-255">Salvare i segreti seguenti localmente con lo [strumento di gestione dei segreti](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="876c1-255">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="876c1-256">I segreti vengono salvati in Azure Key Vault usando i comandi dell'interfaccia della riga di comando di Azure seguenti:</span><span class="sxs-lookup"><span data-stu-id="876c1-256">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="876c1-257">Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-257">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="876c1-258">La stringa segreta per `5000-AppSecret` viene confrontata con la versione dell'app specificata nel file di progetto dell'app (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="876c1-258">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="876c1-259">La versione `5000` (con il trattino) viene rimossa dal nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="876c1-259">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="876c1-260">In tutta l'app, la lettura della configurazione con la chiave `AppSecret` carica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-260">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="876c1-261">Se la versione dell'app viene modificata nel file di progetto per `5.1.0.0` e l'app viene nuovamente eseguita, il valore Secret restituito viene `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo e `5.1.0.0_secret_value_prod` in produzione.</span><span class="sxs-lookup"><span data-stu-id="876c1-261">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="876c1-262">È anche possibile fornire un'implementazione di <xref:Microsoft.Azure.KeyVault.KeyVaultClient> personalizzata per <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span><span class="sxs-lookup"><span data-stu-id="876c1-262">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="876c1-263">Un client personalizzato consente la condivisione di una singola istanza del client nell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-263">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="876c1-264">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="876c1-264">Bind an array to a class</span></span>

<span data-ttu-id="876c1-265">Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.</span><span class="sxs-lookup"><span data-stu-id="876c1-265">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="876c1-266">Quando si legge da un'origine di configurazione che consente alle chiavi di contenere separatori di due punti (`:`), viene usato un segmento di chiave numerico per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`...</span><span class="sxs-lookup"><span data-stu-id="876c1-266">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="876c1-267">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="876c1-267">`:{n}:`).</span></span> <span data-ttu-id="876c1-268">Per altre informazioni, vedere [Configuration: associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="876c1-268">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="876c1-269">Azure Key Vault chiavi non possono utilizzare i due punti come separatore.</span><span class="sxs-lookup"><span data-stu-id="876c1-269">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="876c1-270">L'approccio descritto in questo argomento USA trattini doppi (`--`) come separatore per i valori gerarchici (sezioni).</span><span class="sxs-lookup"><span data-stu-id="876c1-270">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="876c1-271">Le chiavi di matrice vengono archiviate in Azure Key Vault con trattini doppi e segmenti di chiave numerica (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="876c1-271">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="876c1-272">Esaminare la seguente configurazione del provider di registrazione [Serilog](https://serilog.net/) fornita da un file JSON.</span><span class="sxs-lookup"><span data-stu-id="876c1-272">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="876c1-273">Esistono due valori letterali di oggetto definiti nella matrice di `WriteTo` che riflettono due *sink*Serilog, che descrivono le destinazioni per la registrazione dell'output:</span><span class="sxs-lookup"><span data-stu-id="876c1-273">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="876c1-274">La configurazione mostrata nel file JSON precedente viene archiviata in Azure Key Vault usando la notazione a doppio trattino (`--`) e i segmenti numerici:</span><span class="sxs-lookup"><span data-stu-id="876c1-274">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="876c1-275">Chiave</span><span class="sxs-lookup"><span data-stu-id="876c1-275">Key</span></span> | <span data-ttu-id="876c1-276">valore</span><span class="sxs-lookup"><span data-stu-id="876c1-276">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="876c1-277">Ricarica segreti</span><span class="sxs-lookup"><span data-stu-id="876c1-277">Reload secrets</span></span>

<span data-ttu-id="876c1-278">I segreti vengono memorizzati nella cache fino a quando non viene chiamato `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="876c1-278">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="876c1-279">I segreti scaduti, disabilitati e aggiornati nell'insieme di credenziali delle chiavi non vengono rispettati dall'app fino a quando non viene eseguito `Reload`.</span><span class="sxs-lookup"><span data-stu-id="876c1-279">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="876c1-280">Segreti disabilitati e scaduti</span><span class="sxs-lookup"><span data-stu-id="876c1-280">Disabled and expired secrets</span></span>

<span data-ttu-id="876c1-281">I segreti disabilitati e scaduti generano una <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span><span class="sxs-lookup"><span data-stu-id="876c1-281">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="876c1-282">Per impedire che l'app venga generata, fornire la configurazione usando un provider di configurazione diverso o aggiornare il segreto disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="876c1-282">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="876c1-283">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="876c1-283">Troubleshoot</span></span>

<span data-ttu-id="876c1-284">Quando l'app non riesce a caricare la configurazione usando il provider, viene scritto un messaggio di errore nell' [infrastruttura di registrazione ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="876c1-284">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="876c1-285">Le condizioni seguenti impediranno il caricamento della configurazione:</span><span class="sxs-lookup"><span data-stu-id="876c1-285">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="876c1-286">L'app o il certificato non è configurato correttamente in Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="876c1-286">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="876c1-287">L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-287">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="876c1-288">L'app non è autorizzata ad accedere all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-288">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="876c1-289">Il criterio di accesso non include le autorizzazioni `Get` e `List`.</span><span class="sxs-lookup"><span data-stu-id="876c1-289">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="876c1-290">Nell'insieme di credenziali delle chiavi i dati di configurazione (coppia nome-valore) sono denominati, mancanti, disabilitati o scaduti in modo errato.</span><span class="sxs-lookup"><span data-stu-id="876c1-290">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="876c1-291">L'app presenta il nome dell'insieme di credenziali delle chiavi (`KeyVaultName`) errato, l'ID dell'applicazione Azure AD (`AzureADApplicationId`) o l'identificazione personale del certificato Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="876c1-291">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="876c1-292">La chiave di configurazione (nome) non è corretta nell'app per il valore che si sta provando a caricare.</span><span class="sxs-lookup"><span data-stu-id="876c1-292">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="876c1-293">Quando si aggiungono i criteri di accesso per l'app all'insieme di credenziali delle chiavi, il criterio è stato creato, ma il pulsante **Salva** non è stato selezionato nell'interfaccia utente dei **criteri di accesso** .</span><span class="sxs-lookup"><span data-stu-id="876c1-293">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="876c1-294">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="876c1-294">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="876c1-295">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-295">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="876c1-296">Microsoft Azure: Key Vault documentazione</span><span class="sxs-lookup"><span data-stu-id="876c1-296">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="876c1-297">Come generare e trasferire chiavi HSM protette per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-297">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="876c1-298">Classe KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="876c1-298">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="876c1-299">Guida introduttiva: impostare e recuperare un segreto da Azure Key Vault usando un'app Web .NET</span><span class="sxs-lookup"><span data-stu-id="876c1-299">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="876c1-300">Esercitazione: come usare Azure Key Vault con la macchina virtuale Windows di Azure in .NET</span><span class="sxs-lookup"><span data-stu-id="876c1-300">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="876c1-301">Questo documento illustra come usare il provider di configurazione [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) per caricare i valori di configurazione dell'app da Azure Key Vault segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-301">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="876c1-302">Azure Key Vault è un servizio basato sul cloud che aiuta a proteggere le chiavi crittografiche e i segreti usati da app e servizi.</span><span class="sxs-lookup"><span data-stu-id="876c1-302">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="876c1-303">Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core includono:</span><span class="sxs-lookup"><span data-stu-id="876c1-303">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="876c1-304">Controllo dell'accesso ai dati di configurazione riservati.</span><span class="sxs-lookup"><span data-stu-id="876c1-304">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="876c1-305">Soddisfare i requisiti per i moduli di protezione hardware convalidati FIPS 140-2 Level 2 (HSM) quando si archiviano i dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-305">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="876c1-306">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="876c1-306">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="876c1-307">Pacchetti</span><span class="sxs-lookup"><span data-stu-id="876c1-307">Packages</span></span>

<span data-ttu-id="876c1-308">Aggiungere un riferimento al pacchetto [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) .</span><span class="sxs-lookup"><span data-stu-id="876c1-308">Add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="876c1-309">App di esempio</span><span class="sxs-lookup"><span data-stu-id="876c1-309">Sample app</span></span>

<span data-ttu-id="876c1-310">L'app di esempio viene eseguita in una delle due modalità determinate dall'istruzione `#define` nella parte superiore del file *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="876c1-310">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="876c1-311">`Certificate` &ndash; illustra l'uso di un ID client Azure Key Vault e di un certificato X. 509 per accedere ai segreti archiviati in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-311">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="876c1-312">Questa versione dell'esempio può essere eseguita da qualsiasi posizione, distribuita nel servizio app Azure o in qualsiasi host in grado di servire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="876c1-312">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="876c1-313">`Managed` &ndash; illustra come usare le [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview) per autenticare l'app per Azure Key Vault con Azure ad autenticazione senza credenziali archiviate nel codice o nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-313">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="876c1-314">Quando si usano identità gestite per l'autenticazione, non è necessario un Azure AD ID applicazione e una password (segreto client).</span><span class="sxs-lookup"><span data-stu-id="876c1-314">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="876c1-315">La versione `Managed` dell'esempio deve essere distribuita in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-315">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="876c1-316">Seguire le istruzioni riportate nella sezione [usare le identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) .</span><span class="sxs-lookup"><span data-stu-id="876c1-316">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="876c1-317">Per altre informazioni su come configurare un'app di esempio usando le direttive per il preprocessore (`#define`), vedere <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="876c1-317">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="876c1-318">Archiviazione segreta nell'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="876c1-318">Secret storage in the Development environment</span></span>

<span data-ttu-id="876c1-319">Impostare i segreti in locale usando lo [strumento di gestione dei segreti](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="876c1-319">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="876c1-320">Quando l'app di esempio viene eseguita nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio di gestione dei segreti locali.</span><span class="sxs-lookup"><span data-stu-id="876c1-320">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="876c1-321">Lo strumento di gestione dei segreti richiede una proprietà `<UserSecretsId>` nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-321">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="876c1-322">Impostare il valore della proprietà (`{GUID}`) su un GUID univoco:</span><span class="sxs-lookup"><span data-stu-id="876c1-322">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="876c1-323">I segreti vengono creati come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="876c1-323">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="876c1-324">I valori gerarchici (sezioni di configurazione) usano un `:` (due punti) come separatore nei nomi delle chiavi di [configurazione ASP.NET Core](xref:fundamentals/configuration/index) .</span><span class="sxs-lookup"><span data-stu-id="876c1-324">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="876c1-325">Il gestore del segreto viene usato da una shell dei comandi aperta alla [radice del contenuto](xref:fundamentals/index#content-root)del progetto, dove `{SECRET NAME}` è il nome e `{SECRET VALUE}` è il valore:</span><span class="sxs-lookup"><span data-stu-id="876c1-325">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="876c1-326">Eseguire i comandi seguenti in una shell dei comandi dalla radice del [contenuto](xref:fundamentals/index#content-root) del progetto per impostare i segreti per l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-326">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="876c1-327">Quando questi segreti vengono archiviati in Azure Key Vault nell' [archiviazione segreta nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il suffisso `_dev` viene modificato in `_prod`.</span><span class="sxs-lookup"><span data-stu-id="876c1-327">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="876c1-328">Il suffisso fornisce un segnale visivo nell'output dell'app che indica l'origine dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-328">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="876c1-329">Archiviazione segreta nell'ambiente di produzione con Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-329">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="876c1-330">Le istruzioni fornite nell'argomento [avvio rapido: impostare e recuperare un segreto da Azure Key Vault usando l'interfaccia](/azure/key-vault/quick-create-cli) della riga di comando di Azure sono riepilogate qui per la creazione di un Azure Key Vault e l'archiviazione di segreti usati dall'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="876c1-330">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="876c1-331">Per ulteriori informazioni, vedere l'argomento.</span><span class="sxs-lookup"><span data-stu-id="876c1-331">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="876c1-332">Aprire Azure cloud shell usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="876c1-332">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="876c1-333">Selezionare **Prova** nell'angolo superiore destro di un blocco di codice.</span><span class="sxs-lookup"><span data-stu-id="876c1-333">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="876c1-334">Usare la stringa di ricerca "interfaccia della riga di comando di Azure" nella casella di testo.</span><span class="sxs-lookup"><span data-stu-id="876c1-334">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="876c1-335">Aprire Cloud Shell nel browser con il pulsante **avvia cloud Shell** .</span><span class="sxs-lookup"><span data-stu-id="876c1-335">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="876c1-336">Selezionare il pulsante **Cloud Shell** nel menu nell'angolo in alto a destra del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-336">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="876c1-337">Per altre informazioni, vedere l'interfaccia della riga di comando di [Azure](/cli/azure/) e [Panoramica di Azure cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="876c1-337">For more information, see [Azure CLI](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="876c1-338">Se non è già stato eseguito l'autenticazione, effettuare l'accesso con il comando `az login`.</span><span class="sxs-lookup"><span data-stu-id="876c1-338">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="876c1-339">Creare un gruppo di risorse con il comando seguente, dove `{RESOURCE GROUP NAME}` è il nome del gruppo di risorse per il nuovo gruppo di risorse e `{LOCATION}` è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="876c1-339">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="876c1-340">Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando seguente, dove `{KEY VAULT NAME}` è il nome del nuovo insieme di credenziali delle chiavi e `{LOCATION}` è l'area di Azure (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="876c1-340">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name {KEY VAULT NAME} --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="876c1-341">Creare segreti nell'insieme di credenziali delle chiavi come coppie nome/valore.</span><span class="sxs-lookup"><span data-stu-id="876c1-341">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="876c1-342">Azure Key Vault nomi di segreto sono limitati a caratteri alfanumerici e trattini.</span><span class="sxs-lookup"><span data-stu-id="876c1-342">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="876c1-343">I valori gerarchici (sezioni di configurazione) usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="876c1-343">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="876c1-344">I due punti, che in genere vengono usati per delimitare una sezione da una sottochiave in [ASP.NET Core configurazione](xref:fundamentals/configuration/index), non sono consentiti nei nomi dei segreti di Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-344">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="876c1-345">Pertanto, vengono usati due trattini e scambiati per i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-345">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="876c1-346">I segreti seguenti sono da usare con l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="876c1-346">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="876c1-347">I valori includono un suffisso di `_prod` per distinguerli dai valori dei suffissi `_dev` caricati nell'ambiente di sviluppo da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="876c1-347">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="876c1-348">Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali delle chiavi creato nel passaggio precedente:</span><span class="sxs-lookup"><span data-stu-id="876c1-348">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="876c1-349">Usare l'ID applicazione e il certificato X. 509 per le app non ospitate in Azure</span><span class="sxs-lookup"><span data-stu-id="876c1-349">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="876c1-350">Configurare Azure AD, Azure Key Vault e l'app per usare un ID applicazione Azure Active Directory e un certificato X. 509 per l'autenticazione in un insieme di credenziali **delle chiavi quando l'app è ospitata all'esterno di Azure**.</span><span class="sxs-lookup"><span data-stu-id="876c1-350">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="876c1-351">Per i dettagli, vedere l'articolo relativo alle [informazioni su chiavi, segreti e certificati](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="876c1-351">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="876c1-352">Sebbene l'uso di un ID applicazione e di un certificato X. 509 sia supportato per le app ospitate in Azure, è consigliabile usare [identità gestite per le risorse di Azure](#use-managed-identities-for-azure-resources) quando si ospita un'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-352">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="876c1-353">Le identità gestite non richiedono l'archiviazione di un certificato nell'app o nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="876c1-353">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="876c1-354">L'app di esempio usa un ID applicazione e un certificato X. 509 quando l'istruzione `#define` nella parte superiore del file *Program.cs* è impostata su `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="876c1-354">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="876c1-355">Creare un certificato di archivio PKCS # 12 ( *. pfx*).</span><span class="sxs-lookup"><span data-stu-id="876c1-355">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="876c1-356">Le opzioni per la creazione di certificati includono [Makecert in Windows](/windows/desktop/seccrypto/makecert) e [openssl](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="876c1-356">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="876c1-357">Installare il certificato nell'archivio certificati personale dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="876c1-357">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="876c1-358">Contrassegnare la chiave come esportabile è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="876c1-358">Marking the key as exportable is optional.</span></span> <span data-ttu-id="876c1-359">Prendere nota dell'identificazione personale del certificato, che verrà usato più avanti in questo processo.</span><span class="sxs-lookup"><span data-stu-id="876c1-359">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="876c1-360">Esportare il certificato di archiviazione PKCS # 12 ( *. pfx*) come certificato con codifica der ( *. cer*).</span><span class="sxs-lookup"><span data-stu-id="876c1-360">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="876c1-361">Registrare l'app con Azure AD (**registrazioni app**).</span><span class="sxs-lookup"><span data-stu-id="876c1-361">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="876c1-362">Caricare il certificato con codifica DER ( *. cer*) in Azure ad:</span><span class="sxs-lookup"><span data-stu-id="876c1-362">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="876c1-363">Selezionare l'app in Azure AD.</span><span class="sxs-lookup"><span data-stu-id="876c1-363">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="876c1-364">Passare a **certificati & segreti**.</span><span class="sxs-lookup"><span data-stu-id="876c1-364">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="876c1-365">Selezionare **Carica certificato** per caricare il certificato, che contiene la chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="876c1-365">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="876c1-366">Un certificato con *estensione cer*, *PEM*o *CRT* è accettabile.</span><span class="sxs-lookup"><span data-stu-id="876c1-366">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="876c1-367">Archiviare il nome dell'insieme di credenziali delle chiavi, l'ID applicazione e l'identificazione personale del certificato nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-367">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="876c1-368">Passare a insiemi di credenziali delle **chiavi** nella portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-368">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="876c1-369">Selezionare l'insieme di credenziali delle chiavi creato nell' [Archivio Secret nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.</span><span class="sxs-lookup"><span data-stu-id="876c1-369">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="876c1-370">Selezionare **Criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="876c1-370">Select **Access policies**.</span></span>
1. <span data-ttu-id="876c1-371">Selezionare **Aggiungi criteri di accesso**.</span><span class="sxs-lookup"><span data-stu-id="876c1-371">Select **Add Access Policy**.</span></span>
1. <span data-ttu-id="876c1-372">Aprire **autorizzazioni segrete** e fornire all'app le autorizzazioni **Get** ed **List** .</span><span class="sxs-lookup"><span data-stu-id="876c1-372">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="876c1-373">Selezionare **Seleziona entità** e selezionare l'app registrata in base al nome.</span><span class="sxs-lookup"><span data-stu-id="876c1-373">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="876c1-374">Fare clic sul pulsante **Seleziona**.</span><span class="sxs-lookup"><span data-stu-id="876c1-374">Select the **Select** button.</span></span>
1. <span data-ttu-id="876c1-375">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="876c1-375">Select **OK**.</span></span>
1. <span data-ttu-id="876c1-376">Selezionare **Salva**.</span><span class="sxs-lookup"><span data-stu-id="876c1-376">Select **Save**.</span></span>
1. <span data-ttu-id="876c1-377">Distribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-377">Deploy the app.</span></span>

<span data-ttu-id="876c1-378">L'app di esempio `Certificate` ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome del nome del segreto:</span><span class="sxs-lookup"><span data-stu-id="876c1-378">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="876c1-379">Valori non gerarchici: il valore per `SecretName` viene ottenuto con `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="876c1-379">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="876c1-380">Valori gerarchici (sezioni): usare la notazione `:` (due punti) o il metodo di estensione `GetSection`.</span><span class="sxs-lookup"><span data-stu-id="876c1-380">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="876c1-381">Per ottenere il valore di configurazione, usare uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="876c1-381">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="876c1-382">Il certificato X. 509 è gestito dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="876c1-382">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="876c1-383">L'app chiama <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> con i valori forniti dal file *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="876c1-383">The app calls <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="876c1-384">Valori di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-384">Example values:</span></span>

* <span data-ttu-id="876c1-385">Nome dell'insieme di credenziali delle chiavi: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="876c1-385">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="876c1-386">ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="876c1-386">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="876c1-387">Identificazione personale del certificato: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="876c1-387">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="876c1-388">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="876c1-388">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/samples/2.x/SampleApp/appsettings.json?highlight=10-12)]

<span data-ttu-id="876c1-389">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="876c1-389">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="876c1-390">Nell'ambiente di sviluppo, i valori Secret vengono caricati con il suffisso `_dev`.</span><span class="sxs-lookup"><span data-stu-id="876c1-390">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="876c1-391">Nell'ambiente di produzione, i valori vengono caricati con il suffisso `_prod`.</span><span class="sxs-lookup"><span data-stu-id="876c1-391">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="876c1-392">Usare identità gestite per le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="876c1-392">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="876c1-393">**Un'app distribuita in Azure** può trarre vantaggio dalle [identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview), che consente all'app di eseguire l'autenticazione con Azure Key Vault usando Azure ad autenticazione senza credenziali (ID applicazione e password/segreto client) archiviate nell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-393">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="876c1-394">L'app di esempio usa le identità gestite per le risorse di Azure quando l'istruzione `#define` nella parte superiore del file *Program.cs* è impostata su `Managed`.</span><span class="sxs-lookup"><span data-stu-id="876c1-394">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="876c1-395">Immettere il nome dell'insieme di credenziali nel file *appSettings. JSON* dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-395">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="876c1-396">L'app di esempio non richiede un ID applicazione e una password (segreto client) quando è impostata sulla versione `Managed`, quindi è possibile ignorare tali voci di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-396">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="876c1-397">L'app viene distribuita in Azure e Azure autentica l'app per accedere Azure Key Vault solo usando il nome dell'insieme di credenziali archiviato nel file *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="876c1-397">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="876c1-398">Distribuire l'app di esempio nel servizio app Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-398">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="876c1-399">Un'app distribuita nel servizio app Azure viene registrata automaticamente con Azure AD quando viene creato il servizio.</span><span class="sxs-lookup"><span data-stu-id="876c1-399">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="876c1-400">Ottenere l'ID oggetto dalla distribuzione per utilizzarlo nel comando seguente.</span><span class="sxs-lookup"><span data-stu-id="876c1-400">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="876c1-401">L'ID oggetto viene visualizzato nella portale di Azure nel pannello **identità** del servizio app.</span><span class="sxs-lookup"><span data-stu-id="876c1-401">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="876c1-402">Usando l'interfaccia della riga di comando di Azure e l'ID oggetto dell'app, fornire all'app le autorizzazioni `list` e `get` per accedere all'insieme di credenziali delle chiavi:</span><span class="sxs-lookup"><span data-stu-id="876c1-402">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name {KEY VAULT NAME} --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="876c1-403">**Riavviare l'app** usando l'interfaccia della riga di comando di Azure, PowerShell o la portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-403">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="876c1-404">App di esempio:</span><span class="sxs-lookup"><span data-stu-id="876c1-404">The sample app:</span></span>

* <span data-ttu-id="876c1-405">Crea un'istanza della classe `AzureServiceTokenProvider` senza una stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="876c1-405">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="876c1-406">Quando non viene specificata una stringa di connessione, il provider tenta di ottenere un token di accesso dalle identità gestite per le risorse di Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-406">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="876c1-407">Viene creata una nuova <xref:Microsoft.Azure.KeyVault.KeyVaultClient> con il callback del token dell'istanza di `AzureServiceTokenProvider`.</span><span class="sxs-lookup"><span data-stu-id="876c1-407">A new <xref:Microsoft.Azure.KeyVault.KeyVaultClient> is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="876c1-408">L'istanza di <xref:Microsoft.Azure.KeyVault.KeyVaultClient> viene utilizzata con un'implementazione predefinita di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> che carica tutti i valori del segreto e sostituisce i doppi trattini (`--`) con i due punti (`:`) nei nomi di chiave.</span><span class="sxs-lookup"><span data-stu-id="876c1-408">The <xref:Microsoft.Azure.KeyVault.KeyVaultClient> instance is used with a default implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/samples/2.x/SampleApp/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="876c1-409">Esempio di nome dell'insieme di credenziali delle chiavi: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="876c1-409">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="876c1-410">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="876c1-410">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="876c1-411">Quando si esegue l'app, in una pagina Web vengono visualizzati i valori dei segreti caricati.</span><span class="sxs-lookup"><span data-stu-id="876c1-411">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="876c1-412">Nell'ambiente di sviluppo i valori Secret hanno il suffisso `_dev` perché sono forniti da segreti utente.</span><span class="sxs-lookup"><span data-stu-id="876c1-412">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="876c1-413">Nell'ambiente di produzione, i valori vengono caricati con il suffisso `_prod` perché sono forniti da Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-413">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="876c1-414">Se viene visualizzato un errore di `Access denied`, verificare che l'app sia registrata con Azure AD e che l'accesso sia stato fornito all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-414">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="876c1-415">Confermare di aver riavviato il servizio in Azure.</span><span class="sxs-lookup"><span data-stu-id="876c1-415">Confirm that you've restarted the service in Azure.</span></span>

<span data-ttu-id="876c1-416">Per informazioni sull'uso del provider con un'identità gestita e una pipeline di Azure DevOps, vedere [creare una connessione del servizio Azure Resource Manager a una macchina virtuale con un'identità del servizio gestito](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span><span class="sxs-lookup"><span data-stu-id="876c1-416">For information on using the provider with a managed identity and an Azure DevOps pipeline, see [Create an Azure Resource Manager service connection to a VM with a managed service identity](/azure/devops/pipelines/library/connect-to-azure#create-an-azure-resource-manager-service-connection-to-a-vm-with-a-managed-service-identity).</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="876c1-417">Usa prefisso nome chiave</span><span class="sxs-lookup"><span data-stu-id="876c1-417">Use a key name prefix</span></span>

<span data-ttu-id="876c1-418"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> fornisce un overload che accetta un'implementazione di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, che consente di controllare la modalità di conversione dei segreti dell'insieme di credenziali delle chiavi in chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="876c1-418"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> provides an overload that accepts an implementation of <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="876c1-419">Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti in base a un valore di prefisso fornito all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-419">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="876c1-420">Questo consente, ad esempio, di caricare i segreti in base alla versione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-420">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="876c1-421">Non usare i prefissi sui segreti dell'insieme di credenziali delle chiavi per collocare i segreti per più app nello stesso insieme di credenziali delle chiavi o per collocare i segreti ambientali (ad esempio, *sviluppo* rispetto ai segreti di *produzione* ) nello stesso insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="876c1-421">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="876c1-422">È consigliabile che app e ambienti di sviluppo/produzione diversi usino insiemi di credenziali delle chiavi distinti per isolare gli ambienti app per il massimo livello di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="876c1-422">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="876c1-423">Nell'esempio seguente viene stabilito un segreto nell'insieme di credenziali delle chiavi (e usando lo strumento di gestione dei segreti per l'ambiente di sviluppo) per `5000-AppSecret` (i periodi non sono consentiti nei nomi dei segreti dell'insieme di credenziali delle chiavi).</span><span class="sxs-lookup"><span data-stu-id="876c1-423">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="876c1-424">Questo segreto rappresenta un segreto app per la versione 5.0.0.0 dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-424">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="876c1-425">Per un'altra versione dell'app, 5.1.0.0, viene aggiunto un segreto all'insieme di credenziali delle chiavi (e usando lo strumento di gestione dei segreti) per `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="876c1-425">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="876c1-426">Ogni versione dell'app carica il valore del segreto con versione nella configurazione come `AppSecret`, rimuovendo la versione mentre carica il segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-426">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="876c1-427"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> viene chiamato con un <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>personalizzato:</span><span class="sxs-lookup"><span data-stu-id="876c1-427"><xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*> is called with a custom <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager>:</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Program.cs)]

<span data-ttu-id="876c1-428">L'implementazione di <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> reagisce ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:</span><span class="sxs-lookup"><span data-stu-id="876c1-428">The <xref:Microsoft.Extensions.Configuration.AzureKeyVault.IKeyVaultSecretManager> implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

* <span data-ttu-id="876c1-429">`Load` carica un segreto quando il nome inizia con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="876c1-429">`Load` loads a secret when its name starts with the prefix.</span></span> <span data-ttu-id="876c1-430">Non vengono caricati altri segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-430">Other secrets aren't loaded.</span></span>
* <span data-ttu-id="876c1-431">`GetKey`:</span><span class="sxs-lookup"><span data-stu-id="876c1-431">`GetKey`:</span></span>
  * <span data-ttu-id="876c1-432">Rimuove il prefisso dal nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-432">Removes the prefix from the secret name.</span></span>
  * <span data-ttu-id="876c1-433">Sostituisce due trattini in qualsiasi nome con il `KeyDelimiter`, ovvero il delimitatore usato nella configurazione, in genere i due punti.</span><span class="sxs-lookup"><span data-stu-id="876c1-433">Replaces two dashes in any name with the `KeyDelimiter`, which is the delimiter used in configuration (usually a colon).</span></span> <span data-ttu-id="876c1-434">Azure Key Vault non consente i due punti nei nomi dei segreti.</span><span class="sxs-lookup"><span data-stu-id="876c1-434">Azure Key Vault doesn't allow a colon in secret names.</span></span>

[!code-csharp[](key-vault-configuration/samples_snapshot/Startup.cs)]

<span data-ttu-id="876c1-435">Il metodo `Load` viene chiamato da un algoritmo del provider che scorre i segreti dell'insieme di credenziali per trovare quelli con il prefisso della versione.</span><span class="sxs-lookup"><span data-stu-id="876c1-435">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="876c1-436">Quando viene trovato un prefisso di versione con `Load`, l'algoritmo usa il metodo `GetKey` per restituire il nome della configurazione del nome del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-436">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="876c1-437">Rimuove il prefisso della versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nelle coppie nome-valore della configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-437">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="876c1-438">Quando viene implementato questo approccio:</span><span class="sxs-lookup"><span data-stu-id="876c1-438">When this approach is implemented:</span></span>

1. <span data-ttu-id="876c1-439">La versione dell'app specificata nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-439">The app's version specified in the app's project file.</span></span> <span data-ttu-id="876c1-440">Nell'esempio seguente la versione dell'app è impostata su `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="876c1-440">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="876c1-441">Verificare che nel file di progetto dell'app sia presente una proprietà `<UserSecretsId>`, dove `{GUID}` è un GUID fornito dall'utente:</span><span class="sxs-lookup"><span data-stu-id="876c1-441">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="876c1-442">Salvare i segreti seguenti localmente con lo [strumento di gestione dei segreti](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="876c1-442">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="876c1-443">I segreti vengono salvati in Azure Key Vault usando i comandi dell'interfaccia della riga di comando di Azure seguenti:</span><span class="sxs-lookup"><span data-stu-id="876c1-443">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name {KEY VAULT NAME} --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="876c1-444">Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-444">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="876c1-445">La stringa segreta per `5000-AppSecret` viene confrontata con la versione dell'app specificata nel file di progetto dell'app (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="876c1-445">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="876c1-446">La versione `5000` (con il trattino) viene rimossa dal nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="876c1-446">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="876c1-447">In tutta l'app, la lettura della configurazione con la chiave `AppSecret` carica il valore del segreto.</span><span class="sxs-lookup"><span data-stu-id="876c1-447">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="876c1-448">Se la versione dell'app viene modificata nel file di progetto per `5.1.0.0` e l'app viene nuovamente eseguita, il valore Secret restituito viene `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo e `5.1.0.0_secret_value_prod` in produzione.</span><span class="sxs-lookup"><span data-stu-id="876c1-448">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="876c1-449">È anche possibile fornire un'implementazione di <xref:Microsoft.Azure.KeyVault.KeyVaultClient> personalizzata per <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span><span class="sxs-lookup"><span data-stu-id="876c1-449">You can also provide your own <xref:Microsoft.Azure.KeyVault.KeyVaultClient> implementation to <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault*>.</span></span> <span data-ttu-id="876c1-450">Un client personalizzato consente la condivisione di una singola istanza del client nell'app.</span><span class="sxs-lookup"><span data-stu-id="876c1-450">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="876c1-451">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="876c1-451">Bind an array to a class</span></span>

<span data-ttu-id="876c1-452">Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.</span><span class="sxs-lookup"><span data-stu-id="876c1-452">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="876c1-453">Quando si legge da un'origine di configurazione che consente alle chiavi di contenere separatori di due punti (`:`), viene usato un segmento di chiave numerico per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`...</span><span class="sxs-lookup"><span data-stu-id="876c1-453">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="876c1-454">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="876c1-454">`:{n}:`).</span></span> <span data-ttu-id="876c1-455">Per altre informazioni, vedere [Configuration: associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="876c1-455">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="876c1-456">Azure Key Vault chiavi non possono utilizzare i due punti come separatore.</span><span class="sxs-lookup"><span data-stu-id="876c1-456">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="876c1-457">L'approccio descritto in questo argomento USA trattini doppi (`--`) come separatore per i valori gerarchici (sezioni).</span><span class="sxs-lookup"><span data-stu-id="876c1-457">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="876c1-458">Le chiavi di matrice vengono archiviate in Azure Key Vault con trattini doppi e segmenti di chiave numerica (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="876c1-458">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="876c1-459">Esaminare la seguente configurazione del provider di registrazione [Serilog](https://serilog.net/) fornita da un file JSON.</span><span class="sxs-lookup"><span data-stu-id="876c1-459">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="876c1-460">Esistono due valori letterali di oggetto definiti nella matrice di `WriteTo` che riflettono due *sink*Serilog, che descrivono le destinazioni per la registrazione dell'output:</span><span class="sxs-lookup"><span data-stu-id="876c1-460">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="876c1-461">La configurazione mostrata nel file JSON precedente viene archiviata in Azure Key Vault usando la notazione a doppio trattino (`--`) e i segmenti numerici:</span><span class="sxs-lookup"><span data-stu-id="876c1-461">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="876c1-462">Chiave</span><span class="sxs-lookup"><span data-stu-id="876c1-462">Key</span></span> | <span data-ttu-id="876c1-463">valore</span><span class="sxs-lookup"><span data-stu-id="876c1-463">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="876c1-464">Ricarica segreti</span><span class="sxs-lookup"><span data-stu-id="876c1-464">Reload secrets</span></span>

<span data-ttu-id="876c1-465">I segreti vengono memorizzati nella cache fino a quando non viene chiamato `IConfigurationRoot.Reload()`.</span><span class="sxs-lookup"><span data-stu-id="876c1-465">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="876c1-466">I segreti scaduti, disabilitati e aggiornati nell'insieme di credenziali delle chiavi non vengono rispettati dall'app fino a quando non viene eseguito `Reload`.</span><span class="sxs-lookup"><span data-stu-id="876c1-466">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="876c1-467">Segreti disabilitati e scaduti</span><span class="sxs-lookup"><span data-stu-id="876c1-467">Disabled and expired secrets</span></span>

<span data-ttu-id="876c1-468">I segreti disabilitati e scaduti generano una <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span><span class="sxs-lookup"><span data-stu-id="876c1-468">Disabled and expired secrets throw a <xref:Microsoft.Azure.KeyVault.Models.KeyVaultErrorException>.</span></span> <span data-ttu-id="876c1-469">Per impedire che l'app venga generata, fornire la configurazione usando un provider di configurazione diverso o aggiornare il segreto disabilitato o scaduto.</span><span class="sxs-lookup"><span data-stu-id="876c1-469">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="876c1-470">Risolvere problemi</span><span class="sxs-lookup"><span data-stu-id="876c1-470">Troubleshoot</span></span>

<span data-ttu-id="876c1-471">Quando l'app non riesce a caricare la configurazione usando il provider, viene scritto un messaggio di errore nell' [infrastruttura di registrazione ASP.NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="876c1-471">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="876c1-472">Le condizioni seguenti impediranno il caricamento della configurazione:</span><span class="sxs-lookup"><span data-stu-id="876c1-472">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="876c1-473">L'app o il certificato non è configurato correttamente in Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="876c1-473">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="876c1-474">L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="876c1-474">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="876c1-475">L'app non è autorizzata ad accedere all'insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="876c1-475">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="876c1-476">Il criterio di accesso non include le autorizzazioni `Get` e `List`.</span><span class="sxs-lookup"><span data-stu-id="876c1-476">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="876c1-477">Nell'insieme di credenziali delle chiavi i dati di configurazione (coppia nome-valore) sono denominati, mancanti, disabilitati o scaduti in modo errato.</span><span class="sxs-lookup"><span data-stu-id="876c1-477">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="876c1-478">L'app presenta il nome dell'insieme di credenziali delle chiavi (`KeyVaultName`) errato, l'ID dell'applicazione Azure AD (`AzureADApplicationId`) o l'identificazione personale del certificato Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="876c1-478">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="876c1-479">La chiave di configurazione (nome) non è corretta nell'app per il valore che si sta provando a caricare.</span><span class="sxs-lookup"><span data-stu-id="876c1-479">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>
* <span data-ttu-id="876c1-480">Quando si aggiungono i criteri di accesso per l'app all'insieme di credenziali delle chiavi, il criterio è stato creato, ma il pulsante **Salva** non è stato selezionato nell'interfaccia utente dei **criteri di accesso** .</span><span class="sxs-lookup"><span data-stu-id="876c1-480">When adding the access policy for the app to the key vault, the policy was created, but the **Save** button wasn't selected in the **Access policies** UI.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="876c1-481">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="876c1-481">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="876c1-482">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-482">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="876c1-483">Microsoft Azure: Key Vault documentazione</span><span class="sxs-lookup"><span data-stu-id="876c1-483">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="876c1-484">Come generare e trasferire chiavi HSM protette per Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="876c1-484">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="876c1-485">Classe KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="876c1-485">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="876c1-486">Guida introduttiva: impostare e recuperare un segreto da Azure Key Vault usando un'app Web .NET</span><span class="sxs-lookup"><span data-stu-id="876c1-486">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="876c1-487">Esercitazione: come usare Azure Key Vault con la macchina virtuale Windows di Azure in .NET</span><span class="sxs-lookup"><span data-stu-id="876c1-487">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)

::: moniker-end

