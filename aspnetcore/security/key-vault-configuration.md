---
title: Provider di configurazione di Azure Key Vault in ASP.NET Core
author: guardrex
description: Informazioni su come usare il Provider di configurazione dell'insieme di credenziali chiave Azure per configurare un'app usando coppie nome-valore in fase di esecuzione.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: c6dc8b9c462841351b3ada72deeae727da356a6c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207888"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Provider di configurazione di Azure Key Vault in ASP.NET Core

Dal [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-Nurse](https://github.com/anurse)

Questo documento illustra come usare il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provider di configurazione per caricare i valori di configurazione di app da segreti di Azure Key Vault. Azure Key Vault è un servizio basato sul cloud che consente di proteggere le chiavi crittografiche e segreti usati da App e servizi. Gli scenari comuni includono il controllo dell'accesso ai dati di configurazione sensibili e che soddisfano il requisito per FIPS 140-2 livello 2 convalidati i moduli di protezione Hardware (HSM) quando si archiviano i dati di configurazione. Questa funzionalità è disponibile per le app destinate a ASP.NET Core 1.1 o versione successiva.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="package"></a>Pacchetto

Per usare il provider, aggiungere un riferimento per la [azurekeyvault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacchetto.

## <a name="app-configuration"></a>Configurazione delle App

È possibile esplorare il provider con il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Dopo aver stabilito un insieme di credenziali chiave e creare i segreti nell'insieme di credenziali, caricare i valori del segreto nelle relative configurazioni in modo sicuro le app di esempio e visualizzati nelle pagine Web.

Il provider viene aggiunto alla configurazione dell'app con il `AddAzureKeyVault` estensione. Nelle app di esempio, l'estensione utilizza tre valori di configurazione caricati dal *appSettings. JSON* file.

| Impostazione dell'App    | Descrizione                    | Esempio                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nome dell'insieme di credenziali chiave Azure           | contosovault                                 |
| `ClientId`     | Id App di Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Chiave dell'App Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Creare i segreti dell'insieme di credenziali chiave e caricare i valori di configurazione (esempio di basic)

1. Creare un insieme di credenziali delle chiavi e configurare Azure Active Directory (Azure AD) per l'app seguendo le indicazioni fornite in [Introduzione a Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Aggiungere i segreti all'insieme di credenziali delle chiavi utilizzando il [modulo di PowerShell insieme di credenziali delle chiavi di AzureRM](/powershell/module/azurerm.keyvault) disponibile dal [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), il [API REST di Azure Key Vault](/rest/api/keyvault/), o il [Portale di azure](https://portal.azure.com/). I segreti vengono creati come *manuali* oppure *certificato* i segreti. *Certificato* i segreti sono certificati da usare per le App e servizi, ma non sono supportati dal provider di configurazione. È consigliabile usare la *manuale* opzione per creare i segreti di coppia nome-valore per l'uso con il provider di configurazione.
     * I segreti semplici vengono creati come coppie nome-valore. Azure Key Vault secret i nomi sono limitati da caratteri alfanumerici e trattini.
     * Usano i valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore dell'esempio. I due punti, che vengono generalmente utilizzati per delimitare una sezione da una sottochiave [configurazione di ASP.NET Core](xref:fundamentals/configuration/index), non sono consentiti nei nomi di segreto. Pertanto, due trattini sono usati e scambiati per i due punti quando i segreti vengono caricati la configurazione dell'app.
     * Creare due *manuale* segreti con le seguenti coppie nome-valore. La chiave privata prima un nome semplice e un valore e il segreto secondo crea un valore del segreto con una sezione e una sottochiave nel nome del segreto:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registrare l'app di esempio con Azure Active Directory.
   * Autorizzare l'app per l'insieme di credenziali delle chiavi di accesso. Quando si usa la `Set-AzureRmKeyVaultAccessPolicy` cmdlet di PowerShell per autorizzare l'app per accedere a key vault, forniscono `List` e `Get` l'accesso ai segreti con `-PermissionsToSecrets list,get`.

2. Aggiornare l'app *appSettings. JSON* file con valori di `Vault`, `ClientId`, e `ClientSecret`.
3. Eseguire l'app di esempio che ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto.
   * I valori non gerarchico: il valore per `SecretName` viene ottenuto con `config["SecretName"]`.
   * I valori gerarchici (sezioni): usare `:` notazione (due punti) o `GetSection` metodo di estensione. Per ottenere il valore di configurazione, usare uno di questi approcci:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Quando si esegue l'app, una pagina Web Mostra i valori del segreto caricati:

![Finestra del browser con i valori dei segreti caricati tramite Azure Key Vault Configuration Provider](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>Associare una matrice a una classe

Il provider è in grado di leggere i valori di configurazione in una matrice per l'associazione a una matrice POCO.

Durante la lettura da un'origine di configurazione che consente di chiavi contenere i due punti (`:`) i separatori, un segmento della chiave numerico viene utilizzata per distinguere le chiavi che costituiscono una matrice (`:0:`, `:1:`,... `:{n}:`). Per altre informazioni, vedere [configurazione: associare una matrice a una classe](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

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

| Chiave | Valore |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Creare i segreti con prefisso insieme di credenziali delle chiavi e caricare i valori di configurazione (key-name-prefisso-esempio)

`AddAzureKeyVault` fornisce anche un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare i segreti dell'insieme di credenziali chiave come vengono convertiti in chiavi di configurazione. Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti basati su un valore di prefisso specificato all'avvio dell'app. Ciò consente, ad esempio, per caricare i segreti in base alla versione dell'app.

> [!WARNING]
> Non usare prefissi su segreti dell'insieme di credenziali chiave inserisca i segreti per più app nel medesimo insieme di credenziali chiave o inserire i segreti dell'ambiente (ad esempio, *development* rispetto *produzione* segreti) nella stessa insieme di credenziali. È consigliabile che diverse App e ambienti di sviluppo o di produzione usano insiemi di credenziali delle chiavi separati per isolare gli ambienti di app per il massimo livello di sicurezza.

Si usa la seconda app di esempio, creare un segreto nell'insieme di credenziali chiave per `5000-AppSecret` (periodi non sono consentiti nei nomi di segreto dell'insieme di credenziali chiave) che rappresenta un segreto dell'app per la versione versione=5.0.0.0 dell'app. Per un'altra versione, 5.1.0.0, si crea un segreto per `5100-AppSecret`. Ogni versione dell'app carica il proprio valore del segreto in sua configurazione come `AppSecret`, stripping disattivata la versione durante il caricamento del segreto. L'implementazione dell'esempio è illustrato di seguito:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

Il `Load` metodo viene chiamato da un algoritmo di provider che esegue l'iterazione attraverso i segreti dell'insieme di credenziali per individuare quelli che dispongono del prefisso della versione. Quando viene trovato un prefisso di versione con `Load`, l'algoritmo utilizza il `GetKey` per restituire il nome della configurazione del nome del segreto. Questo rimuove il prefisso di versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nella configurazione dell'app per le coppie nome-valore.

Quando si implementa questo approccio:

1. Vengono caricati i segreti dell'insieme di credenziali chiave.
2. Il segreto di stringa per `5000-AppSecret` esiste una corrispondenza.
3. La versione `5000` (con il trattino), viene rimosso dal nome della chiave lasciando `AppSecret` per caricare la configurazione dell'app con il valore del segreto.

> [!NOTE]
> È anche possibile fornire una propria `KeyVaultClient` implementazione di `AddAzureKeyVault`. Fornisce un client personalizzato consente di condividere una singola istanza del client tra il provider di configurazione e altre parti dell'app.

1. Creare un insieme di credenziali delle chiavi e configurare Azure Active Directory (Azure AD) per l'app seguendo le indicazioni fornite in [Introduzione a Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Aggiungere i segreti all'insieme di credenziali delle chiavi utilizzando il [modulo di PowerShell insieme di credenziali delle chiavi di AzureRM](/powershell/module/azurerm.keyvault) disponibile dal [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), il [API REST di Azure Key Vault](/rest/api/keyvault/), o il [Portale di azure](https://portal.azure.com/). I segreti vengono creati come *manuali* oppure *certificato* i segreti. *Certificato* i segreti sono certificati da usare per le App e servizi, ma non sono supportati dal provider di configurazione. È consigliabile usare la *manuale* opzione per creare i segreti di coppia nome-valore per l'uso con il provider di configurazione.
     * Usano i valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore.
     * Creare due *manuale* segreti con le seguenti coppie nome-valore:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrare l'app di esempio con Azure Active Directory.
   * Autorizzare l'app per l'insieme di credenziali delle chiavi di accesso. Quando si usa la `Set-AzureRmKeyVaultAccessPolicy` cmdlet di PowerShell per autorizzare l'app per accedere a key vault, forniscono `List` e `Get` l'accesso ai segreti con `-PermissionsToSecrets list,get`.

2. Aggiornare l'app *appSettings. JSON* file con valori di `Vault`, `ClientId`, e `ClientSecret`.
3. Eseguire l'app di esempio che ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto con prefisso. In questo esempio, il prefisso è la versione dell'app, che è fornito per il `PrefixKeyVaultSecretManager` quando è stato aggiunto il provider di configurazione di Azure Key Vault. Il valore per `AppSecret` viene ottenuto con `config["AppSecret"]`. La pagina Web generata dall'app Mostra il valore caricato:

   ![Finestra del browser con un valore del segreto caricato tramite Azure Key Vault Configuration Provider quando la versione dell'app è versione=5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Modificare la versione di assembly dell'app nel file di progetto dal `5.0.0.0` a `5.1.0.0` ed eseguire nuovamente l'app. Questa volta, viene restituito il valore del segreto `5.1.0.0_secret_value`. La pagina Web generata dall'app Mostra il valore caricato:

   ![Finestra del browser con un valore del segreto caricato tramite Azure Key Vault Configuration Provider quando la versione dell'app è 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Controllare l'accesso per il segreto client

Usare la [strumento Secret Manager](xref:security/app-secrets) mantenere il `ClientSecret` di fuori dell'albero di origine del progetto. Con Secret Manager, associare i segreti dell'app a un progetto specifico e condividerli tra più progetti.

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
