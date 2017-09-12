---
title: Archiviazione sicura di segreti dell'app durante sviluppo in ASP.NET Core
author: rick-anderson
description: Viene illustrato come archiviare in modo sicuro i segreti durante lo sviluppo
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 56214c2fbdca84591c5c1a6b7f2451f33ee64ef0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>Archiviazione sicura di segreti dell'app durante lo sviluppo di ASP.NET Core

<a name=security-app-secrets></a>

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com) 

Questo documento illustra come è possibile utilizzare lo strumento di gestione di segreto nello sviluppo per proteggere le informazioni riservate fuori dal codice. L'aspetto più importante è evitare di archiviare le password o altri dati sensibili nel codice sorgente e non è consigliabile utilizzare i segreti di produzione in modalità di sviluppo e test. È possibile utilizzare invece il [configurazione](../fundamentals/configuration.md) sistema per la lettura di questi valori dalle variabili di ambiente o strumento da valori archiviati utilizzando la chiave privata di gestione. Lo strumento di gestione di segreto consente di impedire che i dati sensibili viene archiviato nel controllo del codice sorgente. Il [configurazione](../fundamentals/configuration.md) sistema può leggere i segreti memorizzati con lo strumento di gestione di segreto descritto in questo articolo.

Lo strumento di gestione segreto viene utilizzato solo in fase di sviluppo. È possibile proteggere i segreti di test e di produzione Azure con il [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provider di configurazione. Vedere [provider di configurazione insieme credenziali chiavi Azure](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) per ulteriori informazioni.

## <a name="environment-variables"></a>Variabili di ambiente

Per evitare di archiviare i segreti dell'app nel codice o nel file di configurazione locali, archiviare i segreti in variabili di ambiente. È possibile configurare il [configurazione](../fundamentals/configuration.md) framework per leggere i valori dalle variabili di ambiente chiamando `AddEnvironmentVariables`. È quindi possibile utilizzare le variabili di ambiente per eseguire l'override dei valori di configurazione per tutte le origini di configurazione specificato in precedenza.

Ad esempio, se si crea una nuova app web ASP.NET Core con singoli account utente, aggiungerà una stringa di connessione predefinito per il *appSettings. JSON* file nel progetto con la chiave `DefaultConnection`. La stringa di connessione predefinito sia configurato per usare LocalDB, che viene eseguito in modalità utente e non richiede una password. Quando si distribuisce un'applicazione a un server di prova o di produzione, è possibile eseguire l'override di `DefaultConnection` valore della chiave con un'impostazione di variabili di ambiente che contiene la stringa di connessione (potenzialmente con credenziali sensibili) per un database di test o produzione Server.

>[!WARNING]
> Le variabili di ambiente sono in genere archiviate in testo normale e non vengono crittografate. Se il computer o processi sono compromesso, le variabili di ambiente è possibile accedere da parti non attendibili. Misure aggiuntive per impedire la divulgazione di informazioni riservate dell'utente potrebbero essere necessarie.

## <a name="secret-manager"></a>Segreto Manager

Lo strumento di gestione Secret archivia i dati sensibili per operazioni di sviluppo di fuori della struttura ad albero di progetto. Lo strumento di gestione di chiave privata è uno strumento di progetto che può essere utilizzato per archiviare informazioni riservate per un [.NET Core](https://www.microsoft.com/net/core) progetto durante lo sviluppo. Con lo strumento di gestione di chiave privata, è possibile associare i segreti dell'app a un progetto specifico e condividerli tra più progetti.

>[!WARNING]
> Lo strumento di gestione di chiave privata non consente di crittografare i segreti archiviati e non deve essere considerato un archivio attendibile. È solo a fini di sviluppo. Le chiavi e valori vengono archiviati in un file di configurazione JSON nella directory di profilo dell'utente.

### <a name="visual-studio-2017-installing-the-secret-manager-tool"></a>Visual Studio 2017: L'installazione lo strumento di gestione di segreto

Fare clic sul progetto in Esplora soluzioni e selezionare **modifica \<project_name\>csproj** dal menu di scelta rapida. Aggiungere la riga evidenziata per il *csproj* e salvataggio dei file per ripristinare il pacchetto NuGet associato:

[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]

Fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida. Questa azione aggiunge un nuovo `UserSecretsId` nodo all'interno di un `PropertyGroup` del *csproj* file. Inoltre, viene aperto un `secrets.json` file nell'editor di testo.

Aggiungere quanto segue a `secrets.json`:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-2015-installing-the-secret-manager-tool"></a>Visual Studio 2015: Lo strumento di gestione di segreto l'installazione

Aprire il progetto `project.json` file. Aggiungere un riferimento a `Microsoft.Extensions.SecretManager.Tools` all'interno di `tools` , proprietà e salvataggio per ripristinare il pacchetto NuGet associato:

```json
"tools": {
    "Microsoft.Extensions.SecretManager.Tools": "1.0.0-preview2-final",
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
},
```

Fare clic sul progetto in Esplora soluzioni e selezionare **informazioni riservate dell'utente gestire** dal menu di scelta rapida. Questa azione aggiunge un nuovo `userSecretsId` proprietà `project.json`. Inoltre, viene aperto un `secrets.json` file nell'editor di testo.

Aggiungere quanto segue a `secrets.json`:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-code-or-command-line-installing-the-secret-manager-tool"></a>Visual Studio Code o della riga di comando: installazione dello strumento di gestione segreto

Aggiungere `Microsoft.Extensions.SecretManager.Tools` per il *csproj* file ed eseguire `dotnet restore`.

[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]

Testare lo strumento di gestione Secret eseguendo il comando seguente:

```console
dotnet user-secrets -h
```

Utilizzo, opzioni della Guida in linea di comando, verrà visualizzato lo strumento di gestione di chiave privata.

> [!NOTE]
> È necessario essere nella stessa directory di *csproj* l'esecuzione di strumenti definiti nel file il *csproj* del file `DotNetCliToolReference` nodi.

Lo strumento di gestione di segreto opera sulle impostazioni di configurazione specifici del progetto che vengono archiviate nel profilo utente. Per utilizzare informazioni riservate dell'utente, è necessario specificare il progetto un `UserSecretsId` valore nella relativa *csproj* file. Il valore di `UserSecretsId` è arbitrario ma in genere univoco al progetto. Gli sviluppatori di generano in genere un GUID per il `UserSecretsId`.

Aggiungere un `UserSecretsId` per il progetto nel *csproj* file:

[!code-xml[Principale](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]

Utilizzare lo strumento di gestione di segreto per impostare un segreto. Ad esempio, nella finestra di comando dalla directory del progetto, immettere quanto segue:

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

È possibile eseguire lo strumento di gestione di segreto da altre directory, ma è necessario utilizzare il `--project` opzione per passare il percorso per il *csproj* file:
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Inoltre, è possibile utilizzare lo strumento di gestione di segreto per elenco, rimuovere e cancellare i segreti dell'app.

## <a name="accessing-user-secrets-via-configuration"></a>L'accesso a informazioni riservate dell'utente tramite la configurazione

Segreto Manager segreti sono accessibili tramite il sistema di configurazione. Aggiungere il `Microsoft.Extensions.Configuration.UserSecrets` pacchetto ed eseguire `dotnet restore`.

Aggiungere l'origine di configurazione di segreti utente per il `Startup` metodo:

[!code-csharp[Principale](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

È possibile accedere a informazioni riservate dell'utente tramite l'API di configurazione:

[!code-csharp[Principale](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Il segreto Manager funzionamento dello strumento

Lo strumento di gestione di segreto estrae i dettagli di implementazione, ad esempio dove e come i valori vengono archiviati. È possibile utilizzare lo strumento senza conoscere questi dettagli di implementazione. Nella versione corrente, i valori vengono archiviati un [JSON](http://json.org/) file di configurazione nella directory di profilo dell'utente:

* Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

Il valore di `userSecretsId` proviene dal valore specificato in *csproj* file.

È consigliabile non scrivere codice che dipende il percorso o il formato dei dati salvati con lo strumento di gestione di chiave privata, come è potrebbero modificare questi dettagli di implementazione. Ad esempio, i valori secret sono attualmente *non* crittografati oggi, ma può essere un giorno.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Configurazione](../fundamentals/configuration.md)
