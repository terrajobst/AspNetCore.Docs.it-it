---
title: Provider di configurazione di Azure insieme di credenziali chiave in ASP.NET Core
author: guardrex
description: Informazioni su come utilizzare il Provider di configurazione dell'insieme di credenziali chiave di Azure per configurare un'applicazione che utilizza coppie nome-valore in fase di esecuzione.
ms.author: riande
ms.date: 08/09/2017
uid: security/key-vault-configuration
ms.openlocfilehash: 433a9cee917a36ff7aa950dbe2a631d127c74821
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273437"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Provider di configurazione di Azure insieme di credenziali chiave in ASP.NET Core

Da [Luke Latham](https://github.com/guardrex) e [Andrew Stanton-infermiera](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Consente di visualizzare o scaricare il codice di esempio per 2. x:

* [Esempio di base](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))-legge i valori dei segreti in un'app.
* [Esempio di prefisso di nome della chiave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) - legge i valori dei segreti mediante un prefisso del nome della chiave che rappresenta la versione di un'app, che consente di caricare un set diverso di valori segreti per ogni versione di app.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Consente di visualizzare o scaricare il codice di esempio per 1. x:

* [Esempio di base](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))-legge i valori dei segreti in un'app.
* [Esempio di prefisso di nome della chiave](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)) - legge i valori dei segreti mediante un prefisso del nome della chiave che rappresenta la versione di un'app, che consente di caricare un set diverso di valori segreti per ogni versione di app.

---

Questo documento viene illustrato come utilizzare il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provider di configurazione da caricare i valori di configurazione dell'applicazione da segreti insieme credenziali chiavi Azure. Insieme di credenziali chiave di Azure è un servizio basato su cloud che consente di proteggere le chiavi di crittografia e i segreti utilizzati dall'App e servizi. Gli scenari comuni includono il controllo dell'accesso ai dati di configurazione sensibili e soddisfare il requisito per FIPS 140-2 livello 2 convalidata moduli di protezione Hardware (HSM) per l'archiviazione dei dati di configurazione. Questa funzionalità è disponibile per le applicazioni che utilizzano ASP.NET Core 1.1 o successiva.

## <a name="package"></a>Pacchetto

Per utilizzare il provider, aggiungere un riferimento di [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pacchetto.

## <a name="application-configuration"></a>Configurazione dell'applicazione

È possibile esplorare il provider con il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Dopo un insieme di credenziali chiave di stabilire e creare i segreti nell'insieme di credenziali, le app di esempio vengono caricati i valori secret le relative configurazioni in modo sicuro e visualizzano nelle pagine Web.

Il provider viene aggiunto per il `ConfigurationBuilder` con il `AddAzureKeyVault` estensione. Nelle app di esempio, l'estensione utilizza tre valori di configurazione caricati dal *appSettings. JSON* file.

| Impostazione dell'App    | Descrizione                    | Esempio                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Nome insieme di credenziali chiave di Azure           | contosovault                                 |
| `ClientId`     | Id di App di Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Chiave dell'applicazione Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Creazione di segreti dell'insieme di credenziali chiave e il caricamento dei valori di configurazione (esempio di base)

1. Creare un insieme di credenziali chiave e configurare Azure Active Directory (Azure AD) per l'applicazione seguendo le istruzioni in [introduzione insieme credenziali chiavi Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Aggiungere i segreti alla chiave dell'insieme di credenziali utilizzando il [modulo PowerShell di insieme di credenziali chiave di Azure Resource Manager](/powershell/module/azurerm.keyvault) disponibile dal [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), il [API REST dell'insieme di credenziali chiave di Azure](/rest/api/keyvault/), o il [Portale di azure](https://portal.azure.com/). I segreti vengono creati come *manuale* o *certificato* segreti. *Certificato* segreti sono certificati per l'utilizzo da applicazioni e servizi, ma non sono supportati dal provider di configurazione. È consigliabile utilizzare il *manuale* opzione per creare i segreti di coppia nome-valore per l'utilizzo con il provider di configurazione.
     * I segreti semplici vengono creati come coppie nome-valore. Azure insieme di credenziali chiave segreta i nomi sono limitati a caratteri alfanumerici e trattini.
     * Utilizzano valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore nell'esempio. I due punti, che vengono generalmente utilizzati per la delimitazione di una sezione di una sottochiave [configurazione di ASP.NET Core](xref:fundamentals/configuration/index), non sono consentiti nei nomi dei segreti. Pertanto, due trattini vengono utilizzati e scambiati per i due punti, quando i segreti vengono caricati nella configurazione dell'applicazione.
     * Creare due *manuale* segreti con le seguenti coppie nome-valore. Il segreto primo è un nome semplice e un valore e il segreto secondo crea un valore segreto con una sezione e una sottochiave nel nome del segreto:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registrare l'app di esempio con Azure Active Directory.
   * Autorizzare l'app per l'insieme di credenziali chiave di accesso. Quando si utilizza il `Set-AzureRmKeyVaultAccessPolicy` fornisce cmdlet di PowerShell per autorizzare l'app per l'insieme di credenziali chiave di accesso `List` e `Get` accesso ai segreti con `-PermissionsToSecrets list,get`.

2. Aggiornare l'app *appSettings. JSON* file con i valori di `Vault`, `ClientId`, e `ClientSecret`.
3. Eseguire l'app di esempio, che ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto.
   * I valori non gerarchico: il valore per `SecretName` viene ottenuto con `config["SecretName"]`.
   * I valori gerarchici (sezioni): utilizzare `:` notazione (virgola) o `GetSection` metodo di estensione. Per ottenere il valore di configurazione, utilizzare uno di questi approcci:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Quando si esegue l'app, una pagina Web Mostra i valori caricati segreti:

![Finestra del browser che mostra i valori dei segreti caricati tramite il Provider di configurazione dell'insieme di credenziali chiave di Azure](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Creazione di segreti con prefisso insieme di credenziali chiave e il caricamento dei valori di configurazione (tasto-nome-prefisso-esempio)

`AddAzureKeyVault` fornisce anche un overload che accetta un'implementazione di `IKeyVaultSecretManager`, che consente di controllare i segreti dell'insieme di credenziali chiave vengono convertiti in chiavi di configurazione. Ad esempio, è possibile implementare l'interfaccia per caricare i valori dei segreti basati su un valore di prefisso che è fornire all'avvio dell'app. Questo consente, ad esempio, per caricare i segreti in base alla versione dell'app.

> [!WARNING]
> Non utilizzare i prefissi su segreti dell'insieme di credenziali chiave inserire stesso insieme di credenziali chiave di segreti per più applicazioni o di posizionare i segreti dell'ambiente (ad esempio, *sviluppo* e *produzione* segreti) nella stessa insieme di credenziali. È consigliabile che diverse App e gli ambienti di sviluppo/produzione usare gli insiemi di credenziali chiave separati per isolare gli ambienti di app per il massimo livello di sicurezza.

Utilizza la seconda applicazione di esempio, creare un segreto nell'insieme di credenziali chiave per `5000-AppSecret` (periodi non sono consentiti nei nomi di insieme di credenziali chiave privata) che rappresenta un segreto dell'app per la versione 5.0.0.0 dell'app. Per un'altra versione, 5.1.0.0, si crea una chiave privata per `5100-AppSecret`. Ogni versione di app carica il proprio valore segreto nella relativa configurazione come `AppSecret`, stripping la versione durante il caricamento del segreto. Implementazione dell'esempio è illustrato di seguito:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=20)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

Il `Load` metodo viene chiamato da un algoritmo di provider che scorre i segreti dell'insieme di credenziali per individuare quelli che dispongono del prefisso di versione. Quando un prefisso di versione è stato trovato con `Load`, l'algoritmo utilizza il `GetKey` per restituire il nome di configurazione del nome del segreto. Rimuove il prefisso di versione dal nome del segreto e restituisce il resto del nome del segreto per il caricamento nella configurazione dell'app coppie nome-valore.

Quando si implementa questo approccio:

1. I segreti dell'insieme di credenziali chiave vengono caricati.
2. Il segreto di stringa per `5000-AppSecret` viene trovata una corrispondenza.
3. La versione, `5000` (con il trattino), viene rimossa da lasciare il nome della chiave `AppSecret` per caricare con valore segreto nella configurazione dell'applicazione.

> [!NOTE]
> È anche possibile fornire una propria `KeyVaultClient` implementazione `AddAzureKeyVault`. Fornisce un client personalizzato consente di condividere una singola istanza del client tra il provider di configurazione e altre parti dell'app.

1. Creare un insieme di credenziali chiave e configurare Azure Active Directory (Azure AD) per l'applicazione seguendo le istruzioni in [introduzione insieme credenziali chiavi Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Aggiungere i segreti alla chiave dell'insieme di credenziali utilizzando il [modulo PowerShell di insieme di credenziali chiave di Azure Resource Manager](/powershell/module/azurerm.keyvault) disponibile dal [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM.KeyVault), il [API REST dell'insieme di credenziali chiave di Azure](/rest/api/keyvault/), o il [Portale di azure](https://portal.azure.com/). I segreti vengono creati come *manuale* o *certificato* segreti. *Certificato* segreti sono certificati per l'utilizzo da applicazioni e servizi, ma non sono supportati dal provider di configurazione. È consigliabile utilizzare il *manuale* opzione per creare i segreti di coppia nome-valore per l'utilizzo con il provider di configurazione.
     * Utilizzano valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore.
     * Creare due *manuale* segreti con le coppie nome-valore seguente:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrare l'app di esempio con Azure Active Directory.
   * Autorizzare l'app per l'insieme di credenziali chiave di accesso. Quando si utilizza il `Set-AzureRmKeyVaultAccessPolicy` fornisce cmdlet di PowerShell per autorizzare l'app per l'insieme di credenziali chiave di accesso `List` e `Get` accesso ai segreti con `-PermissionsToSecrets list,get`.

2. Aggiornare l'app *appSettings. JSON* file con i valori di `Vault`, `ClientId`, e `ClientSecret`.
3. Eseguire l'app di esempio, che ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto con prefisso. In questo esempio, il prefisso è la versione dell'app, che è fornito per il `PrefixKeyVaultSecretManager` quando è stato aggiunto il provider di configurazione insieme credenziali chiavi Azure. Il valore per `AppSecret` viene ottenuto con `config["AppSecret"]`. La pagina Web generata dall'app Mostra il valore caricato:

   ![Finestra del browser che mostra un valore segreto caricato tramite il Provider di configurazione dell'insieme di credenziali chiave di Azure quando la versione dell'app è 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Modificare la versione dell'assembly nel file di progetto da app `5.0.0.0` a `5.1.0.0` ed eseguire nuovamente l'app. Questa volta, il valore secret restituito è `5.1.0.0_secret_value`. La pagina Web generata dall'app Mostra il valore caricato:

   ![Finestra del browser che mostra un valore segreto caricato tramite il Provider di configurazione dell'insieme di credenziali chiave di Azure quando la versione dell'app è 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Controllo dell'accesso per il ClientSecret

Utilizzare il [lo strumento Gestione segreto](xref:security/app-secrets) per mantenere il `ClientSecret` di fuori di struttura di origine del progetto. Con gestione segreto, associare i segreti dell'app a un progetto specifico e condividerli tra più progetti.

Quando si sviluppa un'app .NET Framework in un ambiente che supporta i certificati, è possibile autenticare l'insieme di credenziali chiave di Azure con un certificato x. 509. Chiave privata del certificato x. 509 è gestita dal sistema operativo. Per ulteriori informazioni, vedere [eseguire l'autenticazione con un certificato anziché un segreto Client](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Utilizzare il `AddAzureKeyVault` overload che accetta un `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Ricaricare i segreti

I segreti vengono memorizzati nella cache fino a quando `IConfigurationRoot.Reload()` viene chiamato. Scaduto, disabilitato, e per l'applicazione fino a quando non viene rispettati aggiornato i segreti nell'insieme di credenziali chiave `Reload` viene eseguita.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Segreti disabilitati e scaduti

Generano i segreti disabilitati e scaduti un `KeyVaultClientException`. Per impedire la generazione dell'app, sostituire l'app o aggiorna il segreto disabilitato o scaduto.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Quando l'applicazione non riesce a caricare la configurazione utilizzando il provider, viene scritto un messaggio di errore di [dell'infrastruttura di registrazione di ASP.NET](xref:fundamentals/logging/index). Le condizioni seguenti impedirà la configurazione da caricare:

* L'app non è configurato correttamente in Azure Active Directory.
* L'insieme di credenziali chiave non esiste nell'insieme di credenziali chiave di Azure.
* L'app non è autorizzato ad accedere l'insieme di credenziali chiave.
* Non includono i criteri di accesso `Get` e `List` le autorizzazioni.
* Nell'insieme di credenziali chiave, i dati di configurazione (coppia nome-valore) in modo corretto il nome sono mancante, disabilitato o scaduto.
* L'app con il nome dell'insieme di credenziali chiave errata (`Vault`), Id di App di Azure Active Directory (`ClientId`), o la chiave di Azure AD (`ClientSecret`).
* La chiave di Azure Active Directory (`ClientSecret`) è scaduto.
* Non è corretta nell'app per il valore di cui che si sta provando a caricare la chiave di configurazione (nome).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione](xref:fundamentals/configuration/index)
* [Microsoft Azure: Insieme di credenziali chiave](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Documentazione dell'insieme di credenziali chiave](https://docs.microsoft.com/azure/key-vault/)
* [Come generare e trasferire HSM protetta di chiavi per l'insieme di credenziali chiave di Azure](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [Classe KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
