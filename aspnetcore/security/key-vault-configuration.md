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
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Azure Key Vault Configuration Provider in ASP.NET Core

Dal [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)

Questo documento illustra come usare il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Provider di configurazione per caricare i valori di configurazione di app da segreti di Azure Key Vault. Azure Key Vault è un servizio basato sul cloud che semplifica la protezione delle chiavi crittografiche e segreti usati da App e servizi. Gli scenari comuni per l'uso di Azure Key Vault con le app ASP.NET Core sono:

* Controllo dell'accesso ai dati di configurazione sensibili.
* Soddisfa il requisito per FIPS 140-2 livello 2 convalidati i moduli di protezione Hardware (HSM) quando si archiviano i dati di configurazione.

Questo scenario è disponibile per le app destinate a ASP.NET Core 2.1 o versione successiva.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Pacchetti

Per usare Azure Key Vault Configuration Provider, aggiungere un riferimento al pacchetto di [azurekeyvault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacchetto.

Per adottare lo scenario di identità del servizio gestito di Azure, aggiungere un riferimento al pacchetto di [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pacchetto.

> [!NOTE]
> Al momento della scrittura, la versione stabile più recente di `Microsoft.Azure.Services.AppAuthentication`, versione `1.0.3`, fornisce il supporto per [assegnato dal sistema gestito identità](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka). Supporto per *assegnata dall'utente gestite delle identità* è disponibile nel `1.0.2-preview` pacchetto. In questo argomento viene illustrato l'utilizzo delle identità gestite dal sistema e l'app di esempio fornito utilizza la versione `1.0.3` del `Microsoft.Azure.Services.AppAuthentication` pacchetto.

## <a name="sample-app"></a>App di esempio

L'app di esempio viene eseguito in una delle due modalità di base di `#define` istruzione all'inizio del *Program.cs* file:

* `Basic` &ndash; Illustra l'uso di un ID applicazione dell'insieme di credenziali chiave di Azure e una Password (segreto Client) per accedere ai segreti archiviati nell'insieme di credenziali chiave. Distribuire il `Basic` versione dell'esempio in qualsiasi host in grado di servire un'app ASP.NET Core.
* `Managed` &ndash; Illustra l'uso di Azure [Managed Service Identity (MSI)](/azure/active-directory/managed-identities-azure-resources/overview) eseguire l'autenticazione all'app di Azure Key Vault con l'autenticazione AD Azure senza le credenziali archiviate nel codice o nella configurazione dell'app. Quando si usa l'identità del servizio gestito per l'autenticazione, non sono necessari un ID applicazione di Azure AD e una Password (segreto Client). Il `Managed` versione dell'esempio deve essere distribuita in Azure.

Per altre informazioni su come configurare un'app di esempio usando le direttive del preprocessore (`#define`), vedere <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Archiviazione di segreti nell'ambiente di sviluppo

Impostare i segreti in locale usando il [strumento Secret Manager](xref:security/app-secrets). Quando l'app di esempio viene eseguito nel computer locale nell'ambiente di sviluppo, i segreti vengono caricati dall'archivio segreto Manager locale.

Lo strumento Secret Manager richiede un `<UserSecretsId>` proprietà nel file di progetto dell'app. Impostare il valore della proprietà (`{GUID}`) a qualsiasi GUID univoco:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

I segreti vengono creati come coppie nome-valore. I valori gerarchici (sezioni di configurazione) viene utilizzata una `:` (due punti) come separatore nella [configurazione di ASP.NET Core](xref:fundamentals/configuration/index) nomi chiave.

La gestione della chiave privata viene utilizzata da una shell dei comandi aperta alla radice del contenuto del progetto, in cui `{SECRET NAME}` è il nome e `{SECRET VALUE}` è il valore:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Eseguire i comandi seguenti in una shell dei comandi dalla radice del contenuto del progetto per impostare i segreti per l'app di esempio:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Quando questi segreti sono archiviati in Azure Key Vault nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione, il `_dev` suffisso viene modificato a `_prod`. Il suffisso fornisce un'indicazione visiva nell'output dell'app che indica l'origine dei valori di configurazione.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Archiviazione di segreti nell'ambiente di produzione con Azure Key Vault

Le istruzioni fornite dal [Guida introduttiva: Impostare e recuperare un segreto da Azure Key Vault con Azure CLI](/azure/key-vault/quick-create-cli) argomento sono riepilogati di seguito per creare un Azure Key Vault e l'archiviazione dei segreti usati dall'app di esempio. Vedere l'argomento per altri dettagli.

1. Aprire Azure Cloud shell, usando uno dei metodi seguenti nel [portale di Azure](https://portal.azure.com/):

   * Selezionare **prova** nell'angolo superiore destro di un blocco di codice. Usare la stringa di ricerca "CLI di Azure" nella casella di testo.
   * Aprire Cloud Shell nel browser con il **avvia Cloud Shell** pulsante.
   * Selezionare il **Cloud Shell** pulsante menu in alto a destra del portale di Azure.

   Per altre informazioni, vedere [interfaccia della riga di comando di Azure](/cli/azure/) e [Panoramica di Azure Cloud Shell](/azure/cloud-shell/overview).

1. Se non sono già stati autenticati, accedere con il `az login` comando.

1. Creare un gruppo di risorse con il comando seguente, dove `{RESOURCE GROUP NAME}` è il nome del gruppo di risorse per il nuovo gruppo di risorse e `{LOCATION}` è l'area di Azure (datacenter):

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Creare un insieme di credenziali delle chiavi nel gruppo di risorse con il comando seguente, dove `{KEY VAULT NAME}` è il nome per il nuovo insieme di credenziali delle chiavi e `{LOCATION}` è l'area di Azure (datacenter):

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Creare i segreti nell'insieme di credenziali chiave come coppie nome-valore.

   Azure Key Vault secret i nomi sono limitati da caratteri alfanumerici e trattini. Usano i valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore. I due punti, che vengono generalmente utilizzati per delimitare una sezione da una sottochiave [configurazione di ASP.NET Core](xref:fundamentals/configuration/index), non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave. Pertanto, due trattini sono usati e scambiati per i due punti quando i segreti vengono caricati la configurazione dell'app.

   I segreti seguenti sono per l'uso con l'app di esempio. I valori includono un `_prod` suffisso per distinguerli dal `_dev` suffisso i valori caricati nell'ambiente di sviluppo da informazioni riservate dell'utente. Sostituire `{KEY VAULT NAME}` con il nome dell'insieme di credenziali chiave che è stato creato nel passaggio precedente:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret"></a>Usare l'ID applicazione e il segreto Client

Configurare Azure AD, Azure Key Vault e l'app per usare un ID applicazione e una Password (segreto Client) per l'autenticazione a un insieme di credenziali delle chiavi quando l'app è ospitata all'esterno di Azure.

> [!NOTE]
> Anche se con un ID applicazione e una Password (segreto Client) è supportato per le app ospitate in Azure, è consigliabile usare la [Managed Service Identity (MSI) Provider](#use-the-managed-service-identity-msi-provider) quando si ospita un'app in Azure. Identità del servizio gestito non richiede l'archiviazione delle credenziali nell'applicazione o la configurazione, in modo che viene considerato come un approccio più sicuro a livello generale.

L'app di esempio Usa un ID applicazione e la Password (segreto Client) quando il `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Basic`.

1. Registrare l'app con Azure AD e stabilire una Password (segreto Client) per l'identità dell'app.
1. Store il nome del key vault, ID applicazione e la Password e il segreto Client dell'app *appSettings. JSON* file.
1. Passare a **insiemi di credenziali della chiave** nel portale di Azure.
1. Selezionare l'insieme di credenziali delle chiavi che è stato creato nel [archiviazione di segreti nell'ambiente di produzione con Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sezione.
1. Selezionare **criteri di accesso**.
1. Selezionare **Aggiungi nuovo**.
1. Selezionare **selezionare un'entità** e selezionare l'app registrata in base al nome. Selezionare il **seleziona** pulsante.
1. Aprire **autorizzazioni segrete** e l'App disponga **ottenere** e **elenco** autorizzazioni.
1. Scegliere **OK**.
1. Selezionare **Salva**.
1. Distribuire l'app.

Il `Basic` app di esempio ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto:

* Valori non gerarchici: Il valore per `SecretName` viene ottenuto con `config["SecretName"]`.
* Valori gerarchici (sezioni): Uso `:` notazione (due punti) o `GetSection` metodo di estensione. Per ottenere il valore di configurazione, usare uno di questi approcci:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

Le chiamate dell'app `AddAzureKeyVault` con i valori forniti per il *appSettings. JSON* file:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

Valori di esempio:

* Nome del key vault: `contosovault`
* ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`
* Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

*appsettings.json*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati. Nell'ambiente di sviluppo, caricare i dati con i valori dei segreti di `_dev` suffisso. Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso.

## <a name="use-the-managed-service-identity-msi-provider"></a>Usare il Provider di identità (MSI) del servizio gestito

Un'app distribuita in Azure possa sfruttare i vantaggi di Managed Service Identity (MSI), che consente all'app per l'autenticazione con Azure Key Vault usando l'autenticazione di Azure AD senza credenziali (ID applicazione e la Password e il segreto Client) archiviati nell'app.

L'app di esempio Usa identità del servizio gestito quando la `#define` istruzione all'inizio del *Program.cs* file sia impostato su `Managed`.

Immettere il nome dell'insieme di credenziali all'app *appSettings. JSON* file. L'app di esempio non richiede un ID applicazione e una Password (segreto Client) se impostato sul `Managed` versione, pertanto è possibile ignorare tali voci di configurazione. L'app viene distribuita in Azure e Azure autentica l'app per accedere ad Azure Key Vault usando solo il nome dell'insieme di credenziali archiviata nel *appSettings. JSON* file.

Distribuire l'app di esempio in servizio App di Azure.

Un'app distribuita in servizio App di Azure viene automaticamente registrata con Azure AD quando viene creato il servizio. Ottenere l'ID oggetto della distribuzione per l'uso nel comando seguente. L'ID di oggetto viene visualizzato nel portale di Azure sul **identità** pannello del servizio App.

Uso della riga di comando di Azure e l'ID oggetto dell'app, specificare l'app con `list` e `get` delle autorizzazioni per accedere a key vault:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Riavviare l'app** utilizzando CLI di Azure, PowerShell o il portale di Azure.

L'app di esempio:

* Crea un'istanza di `AzureServiceTokenProvider` classe senza una stringa di connessione. Quando una stringa di connessione non è specificato, il provider tenta di ottenere un token di accesso dall'identità del servizio gestito.
* Una nuova `KeyVaultClient` viene creato con il `AzureServiceTokenProvider` callback token istanza.
* Il `KeyVaultClient` istanza viene utilizzata con un'implementazione predefinita di `IKeyVaultSecretManager` che consente di caricare tutti i valori del segreto e la sostituisce doppia trattini (`--`) con i due punti (`:`) nei nomi delle chiavi.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati. Nell'ambiente di sviluppo, i valori dei segreti hanno il `_dev` suffisso perché sta fornite dall'utente i segreti. Nell'ambiente di produzione, caricare i dati con i valori di `_prod` suffisso perché sta fornite da Azure Key Vault.

Se si riceve un `Access denied` errore, verificare che l'app viene registrata con Azure AD e fornito l'accesso all'insieme di credenziali delle chiavi. Confermare che è stato riavviato il servizio in Azure.

## <a name="use-a-key-name-prefix"></a>Usare un prefisso del nome della chiave

`AddAzureKeyVault` fornisce un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare i segreti dell'insieme di credenziali chiave come vengono convertiti in chiavi di configurazione. Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti basati su un valore di prefisso specificato all'avvio dell'app. Ciò consente, ad esempio, per caricare i segreti in base alla versione dell'app.

> [!WARNING]
> Non usare prefissi su segreti dell'insieme di credenziali chiave inserisca i segreti per più app nel medesimo insieme di credenziali chiave o inserire i segreti dell'ambiente (ad esempio, *development* rispetto *produzione* segreti) nella stessa insieme di credenziali. È consigliabile che diverse App e ambienti di sviluppo o di produzione usano insiemi di credenziali delle chiavi separati per isolare gli ambienti di app per il massimo livello di sicurezza.

Nell'esempio seguente, viene stabilito un segreto della chiave dell'insieme di credenziali (e con lo strumento Secret Manager per l'ambiente di sviluppo) per `5000-AppSecret` (periodi non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave). Questo valore rappresenta un segreto dell'app per la versione versione=5.0.0.0 dell'app. Per un'altra versione dell'app, 5.1.0.0, una chiave privata viene aggiunta alla chiave dell'insieme di credenziali (e usare lo strumento Secret Manager) per `5100-AppSecret`. Il valore del segreto con controllo delle versioni ogni versione dell'app carica la configurazione come `AppSecret`, stripping disattivata la versione durante il caricamento del segreto.

`AddAzureKeyVault` viene chiamato con un oggetto personalizzato `IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

I valori per nome del key vault, ID applicazione e la Password (segreto Client) vengono forniti per il *appSettings. JSON* file:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Valori di esempio:

* Nome del key vault: `contosovault`
* ID applicazione: `627e911e-43cc-61d4-992e-12db9c81b413`
* Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`

Il `IKeyVaultSecretManager` implementazione reagisca ai prefissi di versione dei segreti per caricare il segreto appropriato nella configurazione:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

Il `Load` metodo viene chiamato da un algoritmo di provider che esegue l'iterazione attraverso i segreti dell'insieme di credenziali per individuare quelli che dispongono del prefisso della versione. Quando viene trovato un prefisso di versione con `Load`, l'algoritmo utilizza il `GetKey` per restituire il nome della configurazione del nome del segreto. Questo rimuove il prefisso di versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nella configurazione dell'app per le coppie nome-valore.

Quando viene implementato questo approccio:

1. Versione dell'app specificata nel file di progetto dell'app. Nell'esempio seguente, la versione di app è impostata su `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Verificare che un `<UserSecretsId>` proprietà è presente nel file di progetto dell'app, in cui `{GUID}` è un GUID fornito dall'utente:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Salvare i segreti seguenti in locale con il [strumento Secret Manager](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. I segreti vengono salvati in Azure Key Vault usando i comandi CLI di Azure seguenti:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Quando l'app viene eseguita, vengono caricati i segreti dell'insieme di credenziali chiave. Il segreto di stringa per `5000-AppSecret` corrisponde alla versione dell'app specificata nel file di progetto dell'app (`5.0.0.0`).

1. La versione, `5000` (con il trattino), viene rimosso dal nome della chiave. In tutta l'app, la lettura di configurazione con la chiave `AppSecret` carica il valore del segreto.

1. Se la versione di app viene modificata nel file di progetto per `5.1.0.0` e l'app viene eseguita anche in questo caso, viene restituito il valore del segreto `5.1.0.0_secret_value_dev` nell'ambiente di sviluppo e `5.1.0.0_secret_value_prod` nell'ambiente di produzione.

> [!NOTE]
> È anche possibile fornire una propria `KeyVaultClient` implementazione di `AddAzureKeyVault`. Un client personalizzato consente la condivisione di una singola istanza del client dell'app.

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a>Eseguire l'autenticazione ad Azure Key Vault con un certificato X.509

Quando si sviluppa un'app .NET Framework in un ambiente che supporta i certificati, è possibile eseguire l'autenticazione ad Azure Key Vault con un certificato X.509. Chiave privata del certificato X.509 viene gestita dal sistema operativo. Per altre informazioni, vedere [eseguire l'autenticazione con un certificato anziché un segreto Client](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Usare la `AddAzureKeyVault` overload che accetta un `X509Certificate2` (`_env` nell'esempio seguente:

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

## <a name="bind-an-array-to-a-class"></a>Associare una matrice a una classe

Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.

Durante la lettura da un'origine di configurazione che consente di chiavi contenere i due punti (`:`) i separatori, un segmento della chiave numerico viene utilizzata per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`,... `:{n}:`). Per altre informazioni, vedere [configurazione: Associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Le chiavi di Azure Key Vault non è possibile usare i due punti come separatore. L'approccio descritto in questo argomento utilizza i trattini double (`--`) come separatore per i valori gerarchici (sezioni). Matrice chiavi vengono archiviate in Azure Key Vault con doppie trattini e segmenti di mercato principali numerici (`--0--`, `--1--`,... `--{n}--`).

Esaminare quanto segue [Serilog](https://serilog.net/) incluso in un file JSON di configurazione del provider di registrazione. Esistono due valori letterali definiti di oggetti di `WriteTo` matrice che riflettono Serilog due *sink*, che descrivono le destinazioni per l'output di registrazione:

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

La configurazione illustrata nel file JSON precedente viene archiviata in Azure Key Vault con trattino doppio (`--`) segmenti notazione e numerici:

| Chiave | Value |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Ricaricare i segreti

I segreti vengono memorizzati nella cache fino a `IConfigurationRoot.Reload()` viene chiamato. Scaduto, disabilitato, e aggiornati i segreti nell'insieme di credenziali chiave non vengono rispettati dall'app fino al `Reload` viene eseguita.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Segreti scaduti e disabilitati

I segreti scaduti e disabilitati generano un `KeyVaultClientException`. Per evitare che l'app generi, sostituire l'app o aggiornare il segreto disabilitato o scaduto.

## <a name="troubleshoot"></a>Risolvere i problemi

L'app non riesce a caricare la configurazione utilizzando il provider, viene scritto un messaggio di errore per il [registrazione di ASP.NET Core infrastructure](xref:fundamentals/logging/index). Configurazione del caricamento impedire che le condizioni seguenti:

* L'app non è configurato correttamente in Azure Active Directory.
* L'insieme di credenziali delle chiavi non esiste in Azure Key Vault.
* L'app non è autorizzato ad accedere l'insieme di credenziali delle chiavi.
* I criteri di accesso non includano `Get` e `List` autorizzazioni.
* Nell'insieme di credenziali chiave, i dati di configurazione (coppia nome-valore) in modo non corretto denominati, mancante, disabilitato o scaduto.
* L'app con il nome dell'insieme di credenziali chiave errata (`Vault`), Id di App di Azure AD (`ClientId`), o la chiave di Azure AD (`ClientSecret`).
* La chiave di Azure AD (`ClientSecret`) è scaduto.
* Non è corretta nell'app per il valore che si sta provando a caricare la chiave di configurazione (nome).

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Insieme di credenziali delle chiavi](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentazione su Key Vault](/azure/key-vault/)
* [Come generare e trasferire chiavi HSM protette le chiavi SSH per Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
