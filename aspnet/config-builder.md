---
uid: config-builder
title: Generatori di configurazioni per ASP.NET
author: rick-anderson
description: Informazioni su come ottenere i dati di configurazione da origini diverse dal Web. config valori provenienti da origini esterne.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148837"
---
# <a name="configuration-builders-for-aspnet"></a>Generatori di configurazioni per ASP.NET

Dal [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)

I generatori di configurazione forniscono un meccanismo moderno e agile per le app ASP.NET ottenere i valori di configurazione dalle origini esterne.

Generatori di configurazione:

* Sono disponibili in .NET Framework 4.7.1 e versioni successive.
* Fornire un meccanismo flessibile per la lettura dei valori di configurazione.
* Risolvere alcuni delle esigenze di base dell'App quando si spostano in un contenitore e l'ambiente con lo stato attivo di cloud.
* Può essere utilizzato per migliorare la protezione dei dati di configurazione da disegno da origini non erano disponibili (ad esempio, le variabili di ambiente e Azure Key Vault) nel sistema di configurazione .NET.

## <a name="keyvalue-configuration-builders"></a>Generatori di configurazioni chiave-valore

Uno scenario comune che può essere gestito da generatori di configurazioni consiste nel fornire un meccanismo di sostituzione chiave/valore di base per le sezioni di configurazione che seguono uno schema chiave/valore. Il concetto di .NET Framework di ConfigurationBuilders non è limitato a modelli o le sezioni di configurazione specifico. Tuttavia, molti dei generatori di configurazione nel `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funzionano all'interno del criterio di chiave/valore.

## <a name="keyvalue-configuration-builders-settings"></a>Impostazioni di generatori di configurazione chiave/valore

Le impostazioni seguenti si applicano a tutti i generatori di configurazioni chiave-valore in `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modalità

I generatori di configurazione utilizzano un'origine esterna di informazioni chiave/valore per popolare gli elementi chiave/valore selezionati del sistema di configurazione. In particolare, il `<appSettings/>` e `<connectionStrings/>` sezioni ricevono un trattamento speciale da generatori di configurazione. I generatori di funzionano in tre modalità:

* `Strict` -La modalità predefinita. In questa modalità, il generatore di configurazione di agire esclusivamente sulla sezioni di configurazione di chiave/valore-centric noto. `Strict` modalità enumera ogni chiave nella sezione. Se viene trovata una chiave corrisponda nell'origine esterna:

   * I generatori di configurazioni sostituire il valore nella sezione di configurazione risultante con il valore dall'origine esterna.
* `Greedy` : Questa modalità è strettamente correlata al `Strict` modalità. Anziché essere vincolati alle chiavi già presenti nella configurazione originale:

  * I generatori di configurazione aggiunge tutte le coppie chiave/valore dall'origine esterna, nella sezione della configurazione risultante.

* `Expand` -Viene applicato il XML non elaborato prima che venga analizzato in un oggetto sezione di configurazione. Si può essere considerato come un'espansione dei token in una stringa. Qualsiasi parte della stringa XML non elaborata che corrisponde al modello `${token}` è un candidato per l'espansione del token. Se viene trovato alcun valore corrispondente nell'origine esterna, quindi il token non viene modificato. In questa modalità i generatori non sono limitati al `<appSettings/>` e `<connectionStrings/>` sezioni.

Il markup seguente dal *Web. config* consente la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` modalità:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` illustrato nel precedente *Web. config* file:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Il codice precedente imposterà i valori delle proprietà:

* I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.
* I valori dell'ambiente di variabile, se impostata.

Ad esempio, `ServiceID` conterrà:

* "Valore ServiceID da Web. config", se la variabile di ambiente `ServiceID` non è impostata.
* Il valore della `ServiceID` if variabile di ambiente impostata.

La figura seguente mostra le `<appSettings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:

![Editor dell'ambiente](config-builder/static/env.png)

Nota: Potrebbe essere necessario chiudere e riavviare Visual Studio per visualizzare le modifiche nelle variabili di ambiente.

### <a name="prefix-handling"></a>Gestione di prefisso

I prefissi di chiave possono semplificare le chiavi di impostazione perché:

* La configurazione di .NET Framework è complessi e nidificati.
* Le origini esterne chiave/valore sono comunemente basic e flat per natura. Ad esempio, le variabili di ambiente non sono annidate.

Usare uno degli approcci seguenti per inserire `<appSettings/>` e `<connectionStrings/>` nella configurazione tramite le variabili di ambiente:

* Con il `EnvironmentConfigBuilder` predefinita `Strict` modalità e i nomi delle chiavi appropriati nel file di configurazione. Il precedente codice e markup viene seguito questo approccio. Utilizzo di questo approccio è possibile **non** hanno lo stesso nome delle chiavi in entrambi `<appSettings/>` e `<connectionStrings/>`.
* Usare due `EnvironmentConfigBuilder`s presenti `Greedy` modalità con prefissi distinti e `stripPrefix`. Con questo approccio, l'app può leggere `<appSettings/>` e `<connectionStrings/>` senza la necessità di aggiornare il file di configurazione. La sezione successiva [stripPrefix](#stripprefix), viene illustrato come eseguire questa operazione.
* Usare due `EnvironmentConfigBuilder`s in `Greedy` modalità con prefissi distinti. Con questo approccio non è possibile avere nomi duplicati delle chiavi come i nomi delle chiavi deve essere diverso dal prefisso.  Ad esempio:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con il markup precedente, la stessa origine di chiave-valore flat è utilizzabile per inserire dati di configurazione per due sezioni diverse.

La figura seguente mostra le `<appSettings/>` e `<connectionStrings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel precedente *Web. config* file:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori delle proprietà:

* I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.
* I valori dell'ambiente di variabile, se impostata.

Ad esempio, utilizzando il precedente *Web. config* file, che sono impostati chiavi/valori in immagine editor ambiente precedente e il codice precedente, i valori seguenti:

|  Chiave              | Valore |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID dalle variabili di ambiente|
|    AppSetting_default            | Valore AppSetting_default env |
|       ConnStr_default         | Val ConnStr_default da env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: valore booleano, per impostazione predefinita `false`. 

Il markup XML precedente separa le impostazioni dell'app da stringhe di connessione, ma richiede che tutte le chiavi di *Web. config* file da utilizzare il prefisso specificato. Ad esempio, il prefisso `AppSetting` deve essere aggiunto al `ServiceID` chiave ("AppSetting_ServiceID"). Con `stripPrefix`, il prefisso non viene usato nel *Web. config* file. Il prefisso è necessaria l'origine del generatore di configurazione (ad esempio, nell'ambiente). Prevediamo che utilizzeranno la maggior parte degli sviluppatori `stripPrefix`.

Le applicazioni in genere rimuovono il prefisso. Quanto segue *Web. config* rimuove il prefisso:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Nel precedente *Web. config* file, il `default` chiave si trova in entrambi i `<appSettings/>` e `<connectionStrings/>`.

La figura seguente mostra le `<appSettings/>` e `<connectionStrings/>` chiavi/valori dal precedente *Web. config* set di file nell'editor di ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel precedente *Web. config* file:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori delle proprietà:

* I valori di *Web. config* file se le chiavi non sono impostate nelle variabili di ambiente.
* I valori dell'ambiente di variabile, se impostata.

Ad esempio, utilizzando il precedente *Web. config* file, che sono impostati chiavi/valori in immagine editor ambiente precedente e il codice precedente, i valori seguenti:

|  Chiave              | Valore |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID dalle variabili di ambiente|
|    default            | Valore AppSetting_default env |
|    default         | Val ConnStr_default da env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Stringa, il valore predefinito è `@"\$\{(\w+)\}"`

Il `Expand` comportamento dei generatori di ricerca XML non elaborati per i token che l'aspetto `${token}`. La ricerca viene eseguita con l'espressione regolare predefinita `@"\$\{(\w+)\}"`. Il set di caratteri che corrisponde a `\w` è più rigida rispetto a consentano il XML e molte origini di configurazione. Uso `tokenPattern` quando più caratteri rispetto a `@"\$\{(\w+)\}"` necessari nel nome del token.

`tokenPattern`: Stringa:

* Consente agli sviluppatori di modificare l'espressione regolare che viene usato per la corrispondenza di token.
* Viene eseguita alcuna convalida per assicurarsi che sia un'espressione regolare ben formata, non dannoso.
* Deve contenere un gruppo di acquisizione. L'intera espressione regolare deve corrispondere l'intero token. La prima acquisizione deve essere il nome del token per la ricerca nell'origine di configurazione.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generatori di configurazioni in Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

Il [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* È la più semplice dei generatori di configurazione.
* Legge i valori dall'ambiente.
* Non ha alcuna opzione di configurazione aggiuntive.
* Il `name` valore dell'attributo è arbitrario.

**Nota:** In un ambiente di contenitore di Windows, le variabili impostate in fase di esecuzione vengono inserite solo nell'ambiente dei processi di punto di ingresso. Le app che eseguono un servizio o un processo non EntryPoint non è possibile individuare queste variabili a meno che non vengono inseriti in caso contrario, tramite un meccanismo nel contenitore. Per la [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basato su contenitori, la versione corrente del [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) viene gestita nel *DefaultAppPool* solo. Potrebbe essere necessario sviluppare un proprio meccanismo di inserimento per i processi non EntryPoint altre varianti di contenitore basata su Windows.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Mai l'archiviazione delle password, stringhe di connessione riservate o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o test.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Questo generatore di configurazione offre una funzionalità simile a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).

Il [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) può essere usato nei progetti .NET Framework, ma è necessario specificare un file di segreti. In alternativa, è possibile definire il `UserSecretsId` proprietà nel progetto di file e crea il file non elaborati dei segreti nella posizione corretta per la lettura. Per mantenere le dipendenze esterne all'esterno del progetto, il file del segreto è in formato XML. La formattazione XML è un dettaglio di implementazione e il formato deve non è affidabile. Se è necessario condividere un *Secrets* file con i progetti .NET Core, è possibile usare i [SimpleJsonConfigBuilder](#simplejsonconfig). Il `SimpleJsonConfigBuilder` formattazione per .NET Core deve anche essere considerato un dettaglio di implementazione soggette a modifiche.

Configurazione degli attributi per `UserSecretsConfigBuilder`:

* `userSecretsId` -Questo è il metodo preferito per l'identificazione di un file di segreti XML. Ha un funzionamento simile a .NET Core, che usa un `UserSecretsId` proprietà per archiviare questo identificatore di progetto. La stringa deve essere univoca, non è necessario essere un GUID. Con questo attributo, il `UserSecretsConfigBuilder` Cerca in un percorso locale noto (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) per un file dei segreti che appartengono a questo identificatore.
* `userSecretsFile` : Attributo facoltativo che specifica il file che contiene i segreti. Il `~` carattere può essere utilizzato all'inizio per la radice dell'applicazione di riferimento. Questo attributo o `userSecretsId` attributo è obbligatorio. Se vengono specificati entrambi, `userSecretsFile` ha la precedenza.
* `optional`: valore booleano, predefinito `true` -impedisce un'eccezione se non viene trovato il file dei segreti. 
* Il `name` valore dell'attributo è arbitrario.

Il file dei segreti ha il formato seguente:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

Il [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) legge i valori archiviati nel [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` è necessario (il nome dell'insieme di credenziali) o un URI per l'insieme di credenziali. Gli altri attributi consentono il controllo sulla quale insieme di credenziali per connettersi a, ma sono necessari solo se l'applicazione non è in esecuzione in un ambiente che funziona con `Microsoft.Azure.Services.AppAuthentication`. La libreria di autenticazione di servizi di Azure consente di individuare automaticamente le informazioni di connessione dall'ambiente di esecuzione se possibile. È possibile sovrascrivere automaticamente prelevare di informazioni di connessione, fornendo una stringa di connessione.

* `vaultName` -Obbligatorio se `uri` in non specificato. Specifica il nome dell'insieme di credenziali nella sottoscrizione di Azure da cui eseguire la lettura di coppie chiave/valore.
* `connectionString` -Una stringa di connessione usata da [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Si connette ad altri provider di Key Vault con l'oggetto specificato `uri` valore. Se omesso, Azure (`vaultName`) è il provider dell'insieme di credenziali.
* `version` -Azure Key Vault offre una funzionalità di controllo delle versioni per i segreti. Se `version` viene specificato, il generatore recupera solo i segreti corrispondenti questa versione.
* `preloadSecretNames` -Per impostazione predefinita, questo querys generatore **tutti** nomi nell'insieme di credenziali chiave di chiave quando viene inizializzato. Per impedire la lettura di tutti i valori delle chiavi, impostare questo attributo su `false`. Se questa impostazione `false` legge i segreti uno alla volta. La lettura di segreti uno alla volta può essere utile anche se l'insieme di credenziali consente l'accesso "Get", ma non "List" di accesso. **Nota:** quando si usa `Greedy` modalità `preloadSecretNames` deve essere `true` (predefinito).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) è un generatore di configurazione di base che usa i file della directory come un'origine dei valori. Nome del file è la chiave e il contenuto è il valore. Questo generatore di configurazione può essere utile durante l'esecuzione in un ambiente del contenitore orchestrato senza interruzioni. I sistemi come Docker Swarm e Kubernetes forniscono `secrets` per i relativi contenitori di windows orchestrati in questo modo per ogni file di chiave.

Dettagli attributo:

* `directoryPath` - Obbligatorio. Specifica un percorso cui cercare i valori. Docker per i segreti vengono memorizzati in Windows il *C:\ProgramData\Docker\secrets* directory per impostazione predefinita.
* `ignorePrefix` -I file che iniziano con questo prefisso sono esclusi. Il valore predefinito è "ignore".
* `keyDelimiter` -Il valore predefinito è `null`. Se specificato, il generatore di configurazione consente di attraversare più livelli della directory, costruendo i nomi delle chiavi con questo delimitatore. Se questo valore è `null`, il generatore di configurazione Cerca solo il primo livello della directory.
* `optional` -Il valore predefinito è `false`. Specifica se il generatore di configurazione deve causare errori se la directory di origine non esiste.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Mai l'archiviazione delle password, stringhe di connessione riservate o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o test.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

I progetti .NET core usano spesso i file JSON per la configurazione. Il [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) generatore consente i file JSON di .NET Core da usare in .NET Framework. Questo generatore di configurazione è che fornisce un mapping di base da una fonte di chiave-valore flat in aree chiave/valore specifiche della configurazione di .NET Framework. Questo generatore di configurazione viene **non** fornire per le configurazioni gerarchiche. Il file JSON di sottostante è simile a un dizionario, non un oggetto gerarchico complesso. Un file gerarchico a più livelli può essere utilizzato. Questo provider `flatten`s aggiungendo il nome della proprietà a ogni livello utilizzando il livello di nidificazione `:` come delimitatore.

Dettagli attributo:

* `jsonFile` - Obbligatorio. Specifica il file JSON da cui leggere. Il `~` carattere può essere utilizzato all'avvio a cui fare riferimento radice dell'app.
* `optional` -Valore booleano, valore predefinito è `true`. Impedisce la generazione di eccezioni se non viene trovato il file JSON.
* `jsonMode` - `[Flat|Sectional]`. Il valore predefinito è `Flat`. Quando `jsonMode` è `Flat`, il file JSON è un'origine singola flat chiave/valore. Il `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` sono anche origini chiave-valore flat singolo. Quando la `SimpleJsonConfigBuilder` configurato in `Sectional` modalità:

  * Il file JSON è concettualmente diviso solo al primo livello in più dizionari.
  * Ognuno dei dizionari viene applicata solo alla sezione di configurazione che corrisponde al nome di proprietà di primo livello a cui è collegato. Ad esempio:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementazione di un generatore di configurazioni chiave/valore personalizzato

Se i generatori di configurazioni non soddisfano le proprie esigenze, è possibile scrivere una personalizzata. Il `KeyValueConfigBuilder` classe di base gestisce la maggior parte dei problemi di prefisso e le modalità di sostituzione. Un progetto di implementazione necessari solo:

* Ereditare dalla classe di base e implementare un'origine di base di coppie chiave/valore tramite il `GetValue` e `GetAllValues`:
* Aggiungere il [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al progetto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

Il `KeyValueConfigBuilder` classe di base di fornisce gran parte del comportamento aziendali e coerente tra chiave/valore generatori di configurazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Repository GitHub di generatori di configurazioni](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticazione da servizio a Azure Key Vault usando .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
