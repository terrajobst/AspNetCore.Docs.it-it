# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Applicazione di esempio di Provider di configurazione dell'insieme di credenziali chiave di prefisso (ASP.NET Core 2. x)

In questo esempio viene illustrato l'utilizzo del Provider di configurazione dell'insieme di credenziali chiave di Azure per ASP.NET Core 2. x utilizzando i prefissi dei nomi di chiave. Per l'esempio di 1. x di ASP.NET Core, vedere [applicazione di esempio di Provider di configurazione dell'insieme di credenziali chiave prefisso (ASP.NET Core 1. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x).

Per ulteriori informazioni sul funzionamento dell'esempio, vedere il [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration) argomento.

## <a name="using-the-sample"></a>Utilizzo dell'esempio
1. Creare un insieme di credenziali chiave e configurare Azure Active Directory (Azure AD) per l'applicazione seguendo le istruzioni in [introduzione insieme credenziali chiavi Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Aggiungere i segreti nell'insieme di credenziali chiave mediante il modulo PowerShell di Azure, l'API di gestione di Azure o il portale di Azure. I segreti vengono creati come *manuale* o *certificato* segreti. *Certificato* segreti sono certificati per l'utilizzo da applicazioni e servizi, ma non sono supportati dal provider di configurazione. È consigliabile utilizzare il *manuale* opzione per creare i segreti di coppia nome-valore per l'utilizzo con il provider di configurazione.
    * Utilizzano valori gerarchici (sezioni di configurazione) `--` (due trattini) come separatore.
    * Per l'app di esempio, creare due *manuale* segreti con le coppie nome-valore seguente:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Registrare l'app di esempio con Azure Active Directory.
  * Autorizzare l'app per l'insieme di credenziali chiave di accesso. Quando si utilizza il `Set-AzureRmKeyVaultAccessPolicy` fornisce cmdlet di PowerShell per autorizzare l'app per l'insieme di credenziali chiave di accesso `List` e `Get` accesso ai segreti con `-PermissionsToSecrets list,get`.
2. Aggiornare l'app *appSettings. JSON* file con i valori di `Vault`, `ClientId`, e `ClientSecret`.
3. Eseguire l'app di esempio, che ottiene i valori di configurazione da `IConfigurationRoot` con lo stesso nome come nome del segreto con prefisso. In questo esempio, il prefisso è la versione dell'app, che è fornito per il `PrefixKeyVaultSecretManager` quando è stato aggiunto il provider di configurazione insieme credenziali chiavi Azure. Il valore per `AppSecret` viene ottenuto con `config["AppSecret"]`.
4. Modificare la versione dell'assembly nel file di progetto da app `5.0.0.0` a `5.1.0.0` ed eseguire nuovamente l'app. Questa volta, il valore secret restituito è `5.1.0.0_secret_value`.
