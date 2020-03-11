---
title: Gestire i riferimenti Protobuf con dotnet-grpc
author: juntaoluo
description: Informazioni sull'aggiunta, l'aggiornamento, la rimozione e l'elenco di riferimenti protobuf con lo strumento globale DotNet-grpc.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667335"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>Gestire i riferimenti Protobuf con dotnet-grpc

A cura di [John Luo](https://github.com/juntaoluo)

`dotnet-grpc` è uno strumento globale .NET Core per la gestione dei riferimenti [protobuf ( *. proto*)](xref:grpc/basics#proto-file) in un progetto gRPC .NET. Lo strumento può essere usato per aggiungere, aggiornare, rimuovere ed elencare i riferimenti a protobuf.

## <a name="installation"></a>Installazione

Per installare lo [strumento globale di `dotnet-grpc` .NET Core](/dotnet/core/tools/global-tools), eseguire il comando seguente:

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>Aggiungere riferimenti

`dotnet-grpc` può essere usato per aggiungere riferimenti protobuf come elementi di `<Protobuf />` al file con *estensione csproj* :

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

I riferimenti protobuf vengono usati per generare le C# risorse del client e/o del server. Lo strumento `dotnet-grpc` può:

* Creare un riferimento protobuf da file locali su disco.
* Creare un riferimento protobuf da un file remoto specificato da un URL.
* Verificare che al progetto siano state aggiunte le dipendenze del pacchetto gRPC corrette.

Ad esempio, il pacchetto di `Grpc.AspNetCore` viene aggiunto a un'app Web. `Grpc.AspNetCore` contiene le librerie client e di server gRPC e il supporto degli strumenti. In alternativa, i pacchetti `Grpc.Net.Client`, `Grpc.Tools` e `Google.Protobuf`, che contengono solo le librerie client gRPC e il supporto degli strumenti, vengono aggiunti a un'app console.

### <a name="add-file"></a>Aggiungere file

Il comando `add-file` viene usato per aggiungere file locali su disco come riferimenti a protobuf. I percorsi dei file forniti:

* Può essere relativo alla directory corrente o ai percorsi assoluti.
* Può contenere caratteri jolly per i file basati su pattern [glob](https://wikipedia.org/wiki/Glob_(programming)).

Se i file si trovano all'esterno della directory del progetto, viene aggiunto un elemento `Link` per visualizzare il file nella cartella `Protos` in Visual Studio.

### <a name="usage"></a>Uso

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>Argomenti

| Argomento | Descrizione |
|-|-|
| files | Il file protobuf fa riferimento a. Può trattarsi di un percorso di glob per i file protobuf locali. |

#### <a name="options"></a>Opzioni

| Short-opzione | Opzione Long | Descrizione |
|-|-|-|
| -p | --progetto | Percorso del file di progetto su cui operare. Se non si specifica un file, il comando Cerca una directory corrente.
| -S | --Servizi | Tipo di servizi gRPC da generare. Se viene specificato `Default`, `Both` viene utilizzato per i progetti Web e `Client` viene utilizzato per i progetti non Web. I valori accettati sono `Both`, `Client`, `Default`, `None``Server`.
| -i | --Additional-Import-directory | Directory aggiuntive da usare quando si risolvono le importazioni per i file protobuf. Si tratta di un elenco di percorsi separati da punto e virgola.
| | --accesso | Modificatore di accesso da utilizzare per le C# classi generate. Il valore predefinito è `Public`. I valori accettati sono `Internal` e `Public`.

### <a name="add-url"></a>Aggiungere URL

Il comando `add-url` viene usato per aggiungere un file remoto specificato da un URL di origine come riferimento protobuf. È necessario fornire un percorso di file per specificare dove scaricare il file remoto. Il percorso del file può essere relativo alla directory corrente o a un percorso assoluto. Se il percorso del file è esterno alla directory del progetto, viene aggiunto un elemento `Link` per visualizzare il file nella cartella virtuale `Protos` in Visual Studio.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>Argomenti

| Argomento | Descrizione |
|-|-|
| url | URL di un file protobuf remoto. |

#### <a name="options"></a>Opzioni

| Short-opzione | Opzione Long | Descrizione |
|-|-|-|
| -o | --output | Specifica il percorso di download per il file protobuf remoto. Si tratta di un'opzione obbligatoria.
| -p | --progetto | Percorso del file di progetto su cui operare. Se non si specifica un file, il comando Cerca una directory corrente.
| -S | --Servizi | Tipo di servizi gRPC da generare. Se viene specificato `Default`, `Both` viene utilizzato per i progetti Web e `Client` viene utilizzato per i progetti non Web. I valori accettati sono `Both`, `Client`, `Default`, `None``Server`.
| -i | --Additional-Import-directory | Directory aggiuntive da usare quando si risolvono le importazioni per i file protobuf. Si tratta di un elenco di percorsi separati da punto e virgola.
| | --accesso | Modificatore di accesso da utilizzare per le C# classi generate. Il valore predefinito è `Public`. I valori accettati sono `Internal` e `Public`.

## <a name="remove"></a>Rimuovere

Il comando `remove` viene usato per rimuovere i riferimenti protobuf dal file con *estensione csproj* . Il comando accetta gli argomenti del percorso e gli URL di origine come argomenti. Lo strumento:

* Rimuove solo il riferimento protobuf.
* Non elimina il file *. proto* , neanche se è stato originariamente scaricato da un URL remoto.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>Argomenti

| Argomento | Descrizione |
|-|-|
| riferimenti | URL o percorsi di file dei riferimenti protobuf da rimuovere. |

### <a name="options"></a>Opzioni

| Short-opzione | Opzione Long | Descrizione |
|-|-|-|
| -p | --progetto | Percorso del file di progetto su cui operare. Se non si specifica un file, il comando Cerca una directory corrente.

## <a name="refresh"></a>Aggiorna

Il comando `refresh` viene usato per aggiornare un riferimento remoto con il contenuto più recente dall'URL di origine. È possibile usare sia il percorso del file di download che l'URL di origine per specificare il riferimento da aggiornare. Nota:

* Gli hash del contenuto del file vengono confrontati per determinare se è necessario aggiornare il file locale.
* Nessuna informazione sul timestamp viene confrontata.

Se è necessario un aggiornamento, lo strumento sostituisce sempre il file locale con il file remoto.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>Argomenti

| Argomento | Descrizione |
|-|-|
| riferimenti | Gli URL o i percorsi di file per i riferimenti protobuf remoti che devono essere aggiornati. Lasciare vuoto questo argomento per aggiornare tutti i riferimenti remoti. |

### <a name="options"></a>Opzioni

| Short-opzione | Opzione Long | Descrizione |
|-|-|-|
| -p | --progetto | Percorso del file di progetto su cui operare. Se non si specifica un file, il comando Cerca una directory corrente.
| | --esecuzione a secco | Restituisce un elenco di file che verrebbero aggiornati senza scaricare alcun nuovo contenuto.

## <a name="list"></a>Elenco

Il comando `list` viene usato per visualizzare tutti i riferimenti protobuf nel file di progetto. Se tutti i valori di una colonna sono valori predefiniti, è possibile omettere la colonna.

### <a name="usage"></a>Uso

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>Opzioni

| Short-opzione | Opzione Long | Descrizione |
|-|-|-|
| -p | --progetto | Percorso del file di progetto su cui operare. Se non si specifica un file, il comando Cerca una directory corrente.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
