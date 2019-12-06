---
title: Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare informazioni riservate come segreti dell'app durante lo sviluppo di un'app ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 0bbd6af01ce3a29d3931faa2853a50dc895490cc
ms.sourcegitcommit: fd2483f0a384b1c479c5b4af025ee46917db1919
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868030"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Archiviazione sicura dei segreti delle app in fase di sviluppo in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)e [Scott Addie](https://github.com/scottaddie)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Questo documento illustra le tecniche per l'archiviazione e il recupero di dati sensibili durante lo sviluppo di un'app ASP.NET Core. Non archiviare mai le password o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o il test. I segreti non devono essere distribuiti con l'app. Al contrario, i segreti devono essere resi disponibili nell'ambiente di produzione tramite un mezzo controllato, come le variabili di ambiente, Azure Key Vault e così via. È possibile archiviare e proteggere i segreti di test e di produzione di Azure con il [provider di configurazione Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variabili di ambiente

Le variabili di ambiente vengono usate per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locali. Le variabili di ambiente sostituiscono i valori di configurazione per tutte le origini di configurazione precedentemente specificate.

::: moniker range="<= aspnetcore-1.1"

Configurare la lettura dei valori delle variabili di ambiente chiamando <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> nel costruttore `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Si consideri un'app Web ASP.NET Core in cui è abilitata la sicurezza dei **singoli account utente** . Una stringa di connessione al database predefinita è inclusa nel file *appSettings. JSON* del progetto con la chiave `DefaultConnection`. La stringa di connessione predefinita è per il database locale, che viene eseguito in modalità utente e non richiede una password. Durante la distribuzione dell'app, è possibile eseguire l'override del valore della chiave di `DefaultConnection` con il valore di una variabile di ambiente. La variabile di ambiente può archiviare la stringa di connessione completa con credenziali riservate.

> [!WARNING]
> Le variabili di ambiente vengono in genere archiviate in testo normale e non crittografato. Se il computer o il processo è compromesso, le variabili di ambiente possono essere accessibili da entità non attendibili. Potrebbero essere necessarie misure aggiuntive per impedire la divulgazione di segreti utente.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Gestione segreta

Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core. In questo contesto, una parte di dati sensibili è un segreto dell'app. I segreti dell'app vengono archiviati in un percorso separato dall'albero del progetto. I segreti dell'app sono associati a un progetto specifico o condivisi tra più progetti. I segreti dell'app non vengono archiviati nel controllo del codice sorgente.

> [!WARNING]
> Lo strumento Secret Manager non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile. È solo a scopo di sviluppo. Le chiavi e i valori vengono archiviati in un file di configurazione JSON nella directory del profilo utente.

## <a name="how-the-secret-manager-tool-works"></a>Funzionamento dello strumento di gestione dei segreti

Lo strumento di gestione dei segreti estrae i dettagli di implementazione, ad esempio dove e come vengono archiviati i valori. È possibile utilizzare lo strumento senza conoscere i dettagli di implementazione. I valori vengono archiviati in un file di configurazione JSON in una cartella del profilo utente protetta dal sistema nel computer locale:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Percorso del file System:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Percorso del file System:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Nei percorsi di file precedenti sostituire `<user_secrets_id>` con il valore di `UserSecretsId` specificato nel file con *estensione csproj* .

Non scrivere codice che dipende dalla posizione o dal formato dei dati salvati con lo strumento di gestione dei segreti. Questi dettagli di implementazione possono cambiare. Ad esempio, i valori dei segreti non sono crittografati, ma potrebbero essere futuri.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Installare lo strumento di gestione dei segreti

Lo strumento di gestione dei segreti è integrato con il interfaccia della riga di comando di .NET Core in .NET Core SDK 2.1.300 o versione successiva. Per .NET Core SDK versioni precedenti a 2.1.300, è necessaria l'installazione dello strumento.

> [!TIP]
> Eseguire `dotnet --version` da una shell dei comandi per visualizzare il numero di versione .NET Core SDK installato.

Se il .NET Core SDK usato include lo strumento, viene visualizzato un avviso:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installare il pacchetto NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) nel progetto ASP.NET Core. Ad esempio:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:

```dotnetcli
dotnet user-secrets -h
```

Lo strumento Gestione segreta Visualizza l'utilizzo di esempio, le opzioni e la guida ai comandi:

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
> È necessario trovarsi nella stessa directory del file con *estensione csproj* per eseguire gli strumenti definiti negli elementi `DotNetCliToolReference` del file con estensione *csproj* .

::: moniker-end

## <a name="enable-secret-storage"></a>Abilita archiviazione segreta

Lo strumento Gestione Secret funziona sulle impostazioni di configurazione specifiche del progetto archiviate nel profilo utente.

::: moniker range=">= aspnetcore-3.0"

Lo strumento Gestione Secret include un comando `init` in .NET Core SDK 3.0.100 o versione successiva. Per usare i segreti utente, eseguire il comando seguente nella directory del progetto:

```dotnetcli
dotnet user-secrets init
```

Il comando precedente aggiunge un elemento `UserSecretsId` in un `PropertyGroup` del file con *estensione csproj* . Per impostazione predefinita, il testo interno del `UserSecretsId` è un GUID. Il testo interno è arbitrario, ma è univoco per il progetto.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Per usare i segreti utente, definire un elemento `UserSecretsId` in un `PropertyGroup` del file con *estensione csproj* . Il testo interno del `UserSecretsId` è arbitrario, ma è univoco per il progetto. Gli sviluppatori generano in genere un GUID per la `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> In Visual Studio fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Gestisci segreti utente** dal menu di scelta rapida. Questo movimento aggiunge un elemento `UserSecretsId`, popolato con un GUID, al file con *estensione csproj* .

## <a name="set-a-secret"></a>Imposta un segreto

Definire un segreto dell'app costituito da una chiave e dal relativo valore. Il segreto è associato al valore `UserSecretsId` del progetto. Ad esempio, eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Nell'esempio precedente, i due punti indicano che `Movies` è un valore letterale di oggetto con una proprietà `ServiceApiKey`.

Lo strumento Gestione Secret può essere usato anche da altre directory. Utilizzare l'opzione `--project` per specificare il percorso di file system in cui è presente il file con *estensione csproj* . Ad esempio:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Flat Structure JSON in Visual Studio

Il gesto di **gestione dei segreti utente** di Visual Studio apre un file *Secrets. JSON* nell'editor di testo. Sostituire il contenuto di *Secrets. JSON* con le coppie chiave-valore da archiviare. Ad esempio:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

La struttura JSON viene resa Flat dopo le modifiche tramite `dotnet user-secrets remove` o `dotnet user-secrets set`. Ad esempio, l'esecuzione di `dotnet user-secrets remove "Movies:ConnectionString"` comprime il valore letterale dell'oggetto `Movies`. Il file modificato è simile al seguente:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Impostare più segreti

È possibile impostare un batch di segreti inviando tramite pipe JSON al comando `set`. Nell'esempio seguente il contenuto del file *input. JSON* viene inviato tramite pipe al comando `set`.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Accedere a un segreto

L' [API di configurazione ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti di gestione segreti.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Se il progetto è destinato .NET Framework, installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

In ASP.NET Core 2,0 o versioni successive, l'origine di configurazione dei segreti utente viene aggiunta automaticamente in modalità di sviluppo quando il progetto chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> per inizializzare una nuova istanza dell'host con impostazioni predefinite preconfigurate. `CreateDefaultBuilder` chiama <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> quando il <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> è <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Quando non viene chiamato `CreateDefaultBuilder`, aggiungere l'origine di configurazione dei segreti utente in modo esplicito chiamando <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> nel costruttore di `Startup`. Chiamare `AddUserSecrets` solo quando l'app è in esecuzione nell'ambiente di sviluppo, come illustrato nell'esempio seguente:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Installare il pacchetto NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

Aggiungere l'origine di configurazione dei segreti utente con una chiamata a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> nel costruttore `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

I segreti utente possono essere recuperati tramite l'API `Configuration`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Mappare i segreti a un POCO

Il mapping di un intero valore letterale di oggetto a un oggetto POCO (una classe .NET semplice con proprietà) è utile per l'aggregazione di proprietà correlate.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Per eseguire il mapping dei segreti precedenti a un POCO, usare la funzionalità di [associazione dell'oggetto grafico](xref:fundamentals/configuration/index#bind-to-an-object-graph) dell'API `Configuration`. Il codice seguente esegue il binding a un oggetto personalizzato `MovieSettings` POCO e accede al valore della proprietà `ServiceApiKey`:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

Viene eseguito il mapping dei segreti `Movies:ConnectionString` e `Movies:ServiceApiKey` alle rispettive proprietà in `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Sostituzione di stringhe con segreti

L'archiviazione delle password in testo normale non è protetta. Ad esempio, una stringa di connessione del database archiviata in *appSettings. JSON* può includere una password per l'utente specificato:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un approccio più sicuro consiste nell'archiviare la password come segreto. Ad esempio:

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

Rimuovere la coppia chiave-valore `Password` dalla stringa di connessione in *appSettings. JSON*. Ad esempio:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Il valore del segreto può essere impostato sulla proprietà <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> di un oggetto <xref:System.Data.SqlClient.SqlConnectionStringBuilder> per completare la stringa di connessione:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Elenca i segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :

```dotnetcli
dotnet user-secrets list
```

Viene visualizzato l'output seguente:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Nell'esempio precedente, i due punti nei nomi delle chiavi indicano la gerarchia di oggetti all'interno di *Secrets. JSON*.

## <a name="remove-a-single-secret"></a>Rimuovere un singolo segreto

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

Il file *Secrets. JSON* dell'app è stato modificato per rimuovere la coppia chiave-valore associata alla chiave di `MoviesConnectionString`:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Rimuovi tutti i segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui è presente il file con *estensione csproj* :

```dotnetcli
dotnet user-secrets clear
```

Tutti i segreti utente per l'app sono stati eliminati dal file *Secrets. JSON* :

```json
{}
```

In esecuzione `dotnet user-secrets list` viene visualizzato il messaggio seguente:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
