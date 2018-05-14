# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Applicazione di esempio di Provider di configurazione dell'insieme di credenziali chiave (ASP.NET Core 2. x)

In questo esempio viene illustrato l'utilizzo del Provider di configurazione dell'insieme di credenziali chiave di Azure per ASP.NET Core 2. x. Per l'esempio di 1. x di ASP.NET Core, vedere [applicazione di esempio di Provider di configurazione dell'insieme di credenziali chiave (ASP.NET Core 1. x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x).

Per ulteriori informazioni sul funzionamento dell'esempio, vedere il [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration) argomento.

## <a name="using-the-sample"></a>Utilizzo dell'esempio
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
