---
title: Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core
author: rick-anderson
description: Informazioni su come archiviare e recuperare le informazioni riservate come segreti dell'app durante lo sviluppo di un'app di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Archiviazione sicura di segreti dell'app in fase di sviluppo in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Questo documento illustra le tecniche per archiviare e recuperare i dati sensibili durante lo sviluppo di un'app di ASP.NET Core. Evitare di archiviare le password o altri dati sensibili nel codice sorgente e non devono utilizzare i segreti di produzione in fase di sviluppo o dalla modalità. È possibile archiviare e proteggere i segreti Azure di test e produzione con la [provider di configurazione insieme credenziali chiavi Azure](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variabili di ambiente

Per evitare l'archiviazione di segreti dell'app nel codice o nei file di configurazione locale vengono utilizzate variabili di ambiente. Le variabili di ambiente di eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.

::: moniker range="<= aspnetcore-1.1"
Configurare la lettura dei valori di variabili di ambiente chiamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) nel `Startup` costruttore:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Si consideri un'applicazione web ASP.NET Core in cui **singoli account utente di** è protetta. Una stringa di connessione del database predefinito è incluso il progetto *appSettings* file con la chiave `DefaultConnection`. La stringa di connessione predefinita è per LocalDB, viene eseguito in modalità utente e non richiede una password. Durante la distribuzione di app, il `DefaultConnection` valore chiave può essere sostituito con il valore della variabile di ambiente. La variabile di ambiente può archiviare la stringa di connessione completa con credenziali sensibili.

> [!WARNING]
> Le variabili di ambiente vengono in genere archiviate in testo non crittografato. Se il computer o processi sono compromesso, le variabili di ambiente sono accessibili da parti non attendibili. Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessari.

## <a name="secret-manager"></a>Segreto Manager

Lo strumento Gestione Secret archivia i dati sensibili durante lo sviluppo di un progetto ASP.NET Core. In questo contesto, una parte dei dati sensibili è un segreto dell'app. App segreti sono archiviati in una posizione separata dalla struttura del progetto. I segreti dell'app sono associati a un progetto specifico o condivisa tra più progetti. I segreti dell'app non sono archiviati nel controllo del codice sorgente.

> [!WARNING]
> Lo strumento di gestione di segreto non crittografa i segreti archiviati e non deve essere considerato come un archivio attendibile. È solo a fini di sviluppo. Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.

## <a name="how-the-secret-manager-tool-works"></a>Il segreto Manager funzionamento dello strumento

Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati. È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione. I valori vengono archiviati un [JSON](https://json.org/) file di configurazione in una cartella del profilo utente sistema protetto nel computer locale:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Percorso del file system:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Percorso del file system:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Percorso del file system:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Nel passaggio precedente percorsi di file, sostituire `<user_secrets_id>` con il `UserSecretsId` specificato nel valore di *con estensione csproj* file.

Non scrivere codice che dipende dal percorso o formato di dati salvati con lo strumento di gestione di chiave privata. Questi dettagli di implementazione potrebbero cambiare. Ad esempio, i valori segreti non sono crittografati, ma potrebbe essere in futuro.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Installare lo strumento di gestione segreto

Lo strumento di gestione di chiave privata viene fornito con .NET Core CLI in .NET Core SDK 2.1. Per .NET Core SDK 2.0 e versioni precedenti, è necessario installazione dello strumento.

Installare il [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacchetto NuGet nel progetto ASP.NET Core:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Eseguire il comando seguente in una shell dei comandi per convalidare l'installazione dello strumento:

```console
dotnet user-secrets -h
```

Consente di visualizzare lo strumento Gestione segreto utilizzo dell'esempio, le opzioni e la Guida di comando:

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
> È necessario essere nella stessa directory il *csproj* l'esecuzione di strumenti definiti nel file il *con estensione csproj* file `DotNetCliToolReference` elementi.
::: moniker-end

## <a name="set-a-secret"></a>Impostare un segreto

Lo strumento Gestione segreto opera sulle impostazioni di configurazione specifici del progetto archiviate nel profilo utente. Per utilizzare informazioni riservate dell'utente, definire un `UserSecretsId` elemento all'interno di un `PropertyGroup` del *con estensione csproj* file. Il valore di `UserSecretsId` è arbitrario, ma è univoco per il progetto. Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> In Visual Studio, fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida. Questa azione aggiunge una `UserSecretsId` elemento, popolato con un GUID per il *csproj* file. Visual Studio apre una *secrets.json* file nell'editor di testo. Sostituire il contenuto di *secrets.json* con le coppie chiave-valore da archiviare. Ad esempio: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Definire un segreto dell'applicazione costituito da una chiave e il relativo valore. Il segreto è associato al progetto `UserSecretsId` valore. Ad esempio, eseguire il comando seguente dalla directory in cui il *csproj* file esistente:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Nell'esempio precedente, i due punti indica che `Movies` è un oggetto letterale con un `ServiceApiKey` proprietà.

Lo strumento di gestione di chiave privata utilizzabile da altre directory troppo. Usare la `--project` possibilità di fornire il percorso del file system in corrispondenza del quale il *csproj* file esista. Ad esempio:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Impostare più segreti

Un batch dei segreti può essere impostato tramite pipe JSON per il `set` comando. Nell'esempio seguente, il *input.json* contenuto del file viene inviati tramite pipe per il `set` comando.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Aprire una shell dei comandi ed eseguire il comando seguente:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Accedere a una chiave privata

Il [API di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) fornisce l'accesso ai segreti Manager segreto. Se la destinazione di .NET Core 1.x o .NET Framework, installare il [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacchetto NuGet.

::: moniker range="<= aspnetcore-1.1"
Aggiungere l'origine di configurazione di segreti utente per il `Startup` costruttore:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Informazioni riservate dell'utente possono essere recuperati tramite il `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Sostituire le stringhe dei segreti

L'archiviazione delle password in testo normale è rischiosa. Ad esempio, una stringa di connessione di database archiviati *appSettings* può includere una password per l'utente specificato:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un approccio più sicuro consiste nell'archiviare la password come una chiave privata. Ad esempio:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Sostituire la password nella *appSettings* con un segnaposto. Nell'esempio seguente, `{0}` viene utilizzato come segnaposto al form un [stringa di formato composita](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Valore di un segreto può essere inserite nel segnaposto per completare la stringa di connessione:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Elenco dei segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:

```console
dotnet user-secrets list
```

Viene visualizzato il seguente output:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

Nell'esempio precedente, i due punti nei nomi di chiave indica la gerarchia degli oggetti all'interno di *secrets.json*.

## <a name="remove-a-single-secret"></a>Rimuovere un singolo segreto

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

L'app *secrets.json* apportata per rimuovere la coppia chiave-valore associate al file il `MoviesConnectionString` chiave:

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

## <a name="remove-all-secrets"></a>Rimuovere tutti i segreti

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Eseguire il comando seguente dalla directory in cui il *csproj* file esistente:

```console
dotnet user-secrets clear
```

Tutte le informazioni riservate dell'utente per l'app sono state eliminate dal *secrets.json* file:

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