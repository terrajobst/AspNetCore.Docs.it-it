---
title: Archiviazione sicura dei segreti delle app durante lo sviluppo di ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare informazioni riservate come segreti dell'app durante lo sviluppo di un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 18313f8284e81d196cbe786f494a607ee97a299f
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750978"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Archiviazione sicura dei segreti delle app durante lo sviluppo di ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Questo documento illustra le tecniche per archiviare e recuperare i dati sensibili durante lo sviluppo di un'app ASP.NET Core. Non archiviare mai le password o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o test. È possibile memorizzare e proteggere i segreti relativi al test e alla produzione di Azure usando il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variabili di ambiente

Le variabili di ambiente vengono utilizzate per evitare l'archiviazione dei segreti delle app nel codice o nei file di configurazione locale. Le variabili di ambiente di eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.

::: moniker range="<= aspnetcore-1.1"

Configurare la lettura dei valori di variabili di ambiente tramite una chiamata <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> nella `Startup` costruttore:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Si consideri un'applicazione web ASP.NET Core in cui **singoli account utente di** viene abilitata la sicurezza. Una stringa di connessione del database predefinito è incluso del progetto *appSettings. JSON* file con la chiave `DefaultConnection`. La stringa di connessione predefinita è per LocalDB, che viene eseguito in modalità utente e non richiede una password. Durante la distribuzione di app, il `DefaultConnection` valore chiave può essere sostituito con il valore della variabile di ambiente. La variabile di ambiente può archiviare la stringa di connessione completa con le credenziali sensibili.

> [!WARNING]
> Le variabili di ambiente vengono in genere archiviate in testo normale e non crittografato. Se il computer o il processo è compromesso, le variabili di ambiente sono accessibili da parti non attendibili. Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessari.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>SECRET Manager

Lo strumento Secret Manager archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core. In questo contesto, una porzione di dati sensibili è un segreto dell'app. I segreti dell'App vengono archiviati in una posizione separata dall'albero di progetto. I segreti dell'app sono associati a un progetto specifico o condivisa tra più progetti. I segreti dell'app non vengono controllati nel controllo del codice sorgente.

> [!WARNING]
> Lo strumento Secret Manager non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile. È solo a fini di sviluppo. Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory del profilo utente.

## <a name="how-the-secret-manager-tool-works"></a>Come funziona lo strumento Secret Manager

Lo strumento Secret Manager estrae i dettagli di implementazione, ad esempio come e dove vengono archiviati i valori. È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione. I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente sistema protetto nel computer locale:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Percorso del file system:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Percorso del file system:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Nel precedente i percorsi di file, sostituire `<user_secrets_id>` con il `UserSecretsId` valore specificato nelle *csproj* file.

Non scrivere codice che dipende dal percorso o formato di dati salvati con lo strumento Secret Manager. Questi dettagli di implementazione possono cambiare. Ad esempio, i valori del segreto non vengono crittografati, ma potrebbe essere in futuro.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Installare lo strumento Secret Manager

Lo strumento Secret Manager è in bundle con la CLI di .NET Core in .NET Core SDK 2.1.300 o versione successiva. Per le versioni di .NET Core SDK 2.1.300 prima di, è necessario installazione dello strumento.

> [!TIP]
> Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione di .NET Core SDK installato.

Se .NET Core SDK in uso include lo strumento, viene visualizzato un avviso:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installare il [Microsoft](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacchetto NuGet nel progetto ASP.NET Core. Ad esempio:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione degli strumenti:

```console
dotnet user-secrets -h
```

Lo strumento Secret Manager consente di visualizzare esempio di utilizzo, opzioni e Guida dei comandi:

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> È necessario essere nella stessa directory il *file con estensione csproj* l'esecuzione di strumenti definiti nel file la *csproj* del file `DotNetCliToolReference` elementi.

::: moniker-end

## <a name="enable-secret-storage"></a>Abilita l'archiviazione privata

Lo strumento Secret Manager opera sulle impostazioni di configurazione specifici del progetto archiviate nel profilo utente.

::: moniker range=">= aspnetcore-3.0"

Lo strumento Secret Manager include un `init` comando di .NET Core SDK 3.0.100 o versione successiva. Per usare i segreti utente, eseguire il comando seguente nella directory del progetto:

```console
dotnet user-secrets init
```

Il comando precedente aggiunge un `UserSecretsId` elemento all'interno di un `PropertyGroup` del *csproj* file. Per impostazione predefinita, il testo interno del `UserSecretsId` è un GUID. Il testo interno è arbitrario, ma è univoco per il progetto.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Per usare i segreti utente, definire un `UserSecretsId` elemento all'interno di un `PropertyGroup` delle *csproj* file. Il testo interno del `UserSecretsId` è arbitrario, ma è univoco per il progetto. Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> In Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **Gestisci segreti utente** dal menu di scelta rapida. Questa azione aggiunge un `UserSecretsId` elemento, popolato con un GUID al *csproj* file.

## <a name="set-a-secret"></a>Impostare un segreto

Definire un segreto dell'app costituito da una chiave e il relativo valore. Il segreto è associato al progetto `UserSecretsId` valore. Ad esempio, eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Nell'esempio precedente, i due punti indica che `Movies` è un oggetto letterale con una `ServiceApiKey` proprietà.

Lo strumento Secret Manager può essere utilizzato da altre directory troppo. Usare la `--project` opzione per specificare il percorso del file system in corrispondenza del quale il *csproj* file esista. Ad esempio:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Struttura JSON flat nel Visual Studio

Visual Studio **Gestisci segreti utente** movimenti si apre un *Secrets* file nell'editor di testo. Sostituire il contenuto del *Secrets* con le coppie chiave-valore da archiviare. Ad esempio:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

La struttura JSON viene reso flat seguito alle modifiche apportate tramite `dotnet user-secrets remove` o `dotnet user-secrets set`. Ad esempio, in esecuzione `dotnet user-secrets remove "Movies:ConnectionString"` comprime il `Movies` valore letterale di oggetto. Il file modificato simile al seguente:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Impostare più chiavi private

Un batch dei segreti può essere impostato da inviare tramite pipe da JSON al `set` comando. Nell'esempio seguente, il *input. JSON* contenuto del file viene inviati tramite pipe al `set` comando.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Accedere a un segreto

Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Secret Manager.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Se il progetto è destinato a .NET Framework, installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

In ASP.NET Core 2.0 o versione successiva, l'origine di configurazione di segreti utente viene aggiunto automaticamente in modalità di sviluppo se il progetto chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> per inizializzare una nuova istanza dell'host con i valori predefiniti preconfigurati. `CreateDefaultBuilder` le chiamate <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> quando il <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> è <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Quando `CreateDefaultBuilder` non viene chiamato, aggiungere l'origine di configurazione di segreti utente in modo esplicito chiamando <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> nel `Startup` costruttore. Chiamare `AddUserSecrets` solo quando l'app viene eseguita nell'ambiente di sviluppo, come illustrato nell'esempio seguente:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.

Aggiungere l'origine di configurazione di segreti utente con una chiamata a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> nella `Startup` costruttore:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Informazioni riservate dell'utente possono essere recuperati tramite il `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Eseguire il mapping di segreti da un oggetto POCO

Mapping di un intero valore letterale di oggetto a un oggetto POCO (una classe .NET semplice con proprietà) è utile per l'aggregazione di proprietà correlate.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Per mappare i segreti precedenti a un oggetto POCO, usare il `Configuration` dell'API [oggetto associazione di graph](xref:fundamentals/configuration/index#bind-to-an-object-graph) funzionalità. Il codice seguente viene associata a una classe personalizzata `MovieSettings` POCO e gli accessi di `ServiceApiKey` valore proprietà:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

Il `Movies:ConnectionString` e `Movies:ServiceApiKey` segreti vengono eseguito il mapping nelle rispettive proprietà `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Sostituire le stringhe con segreti

L'archiviazione delle password in testo normale non è sicura. Ad esempio, una stringa di connessione di database archiviati nelle *appSettings. JSON* può includere una password per l'utente specificato:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un approccio più sicuro consiste nell'archiviare la password come segreto. Ad esempio:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Rimuovere il `Password` coppia chiave-valore dalla stringa di connessione nel *appSettings. JSON*. Ad esempio:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Valore del segreto può essere impostato su un <xref:System.Data.SqlClient.SqlConnectionStringBuilder> dell'oggetto <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> proprietà per completare la stringa di connessione:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Elenca i segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:

```console
dotnet user-secrets list
```

Viene visualizzato l'output seguente:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Nell'esempio precedente, i due punti nei nomi delle chiavi denota la gerarchia di oggetti all'interno *Secrets*.

## <a name="remove-a-single-secret"></a>Rimuovere un singolo segreto

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

L'app *Secrets* apportata per rimuovere la coppia chiave-valore associate al file il `MoviesConnectionString` chiave:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Rimuovere tutti i segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *file con estensione csproj* file esistente:

```console
dotnet user-secrets clear
```

Tutti i segreti utente dell'App sono stati eliminati dal *Secrets* file:

```json
{}
```

Esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
