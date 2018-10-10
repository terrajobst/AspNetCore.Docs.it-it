---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapping degli utenti di SignalR alle connessioni | Microsoft Docs
author: tfitzmac
description: In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni. Patrick Fletcher hanno contribuito alla scrittura di questo argomento. Versioni del software utilizzate in questo argomento...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: a3598eee30b29a95673cbc313adfb8f5b068ea24
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912579"
---
<a name="mapping-signalr-users-to-connections"></a>Mapping degli utenti di SignalR alle connessioni
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento viene illustrato come mantenere le informazioni sugli utenti e le relative connessioni.
>
> Patrick Fletcher hanno contribuito alla scrittura di questo argomento.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Introduzione

Ogni client che si connette a un hub passa un id univoco della connessione. È possibile recuperare questo valore nel `Context.ConnectionId` proprietà del contesto dell'hub. Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile usare uno dei seguenti:

- [Il Provider di ID utente (SignalR 2)](#IUserIdProvider)
- [Archiviazione in memoria](#inmemory), ad esempio un dizionario
- [Gruppo di SignalR per ogni utente](#groups)
- [Archiviazione esterna permanente](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure

Ognuna di queste implementazioni è illustrato in questo argomento. Si utilizza il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi del `Hub` classe tenere traccia dello stato di connessione utente.

L'approccio migliore per l'applicazione dipende:

- Il numero di server web che ospitano l'applicazione.
- Indica se è necessario ottenere un elenco di utenti attualmente connessi.
- Indica se è necessario rendere persistenti le informazioni di gruppo e utente quando l'applicazione o il server viene riavviato.
- Indica se la latenza della chiamata a un server esterno è un problema.

La tabella seguente illustra l'approccio funziona per queste considerazioni.

|  | Più di un server | Ottiene l'elenco di utenti attualmente connessi | Rendere persistenti le informazioni dopo il riavvio | Ottenere prestazioni ottimali |
| --- | --- | --- | --- | --- |
| Provider UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| In memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Gruppi utente singolo | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanenti, esterni | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Provider IUserID

Questa funzionalità consente agli utenti di specificare che cos'è l'ID utente basato su un IRequest tramite una nuova interfaccia IUserIdProvider.

**Il IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Per impostazione predefinita, vi sarà un'implementazione che usa l'utente `IPrincipal.Identity.Name` come nome utente. Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

All'interno di un hub, sarà in grado di inviare messaggi a questi utenti tramite l'API seguente:

**Invia un messaggio a un utente specifico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Archiviazione in memoria

Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un dizionario che viene archiviato in memoria. Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione SignalR. Ad esempio, un utente che è connessa tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.

Se l'applicazione viene arrestata, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti di ristabilire le connessioni. Archiviazione in memoria non funziona se l'ambiente include più di un server web, poiché ogni server avrà una raccolta separata di connessioni.

Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti alle connessioni. La chiave per il HashSet sarà il nome dell'utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

L'esempio successivo illustra come usare la classe di mapping di connessione da un hub. L'istanza della classe è archiviata in un nome di variabile `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Gruppi utente singolo

È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si vuole raggiungere solo tale utente. Il nome di ogni gruppo è il nome dell'utente. Se un utente ha più di una connessione, ogni id di connessione viene aggiunto al gruppo dell'utente.

È consigliabile non rimuovere manualmente l'utente dal gruppo quando l'utente si disconnette. Questa azione viene eseguita automaticamente dal framework di SignalR.

Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Archiviazione esterna permanente

In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione. Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati. Se i server web arresta lavoro o del riavvio dell'applicazione, il `OnDisconnected` non viene chiamato. Pertanto, è possibile che il repository dei dati avrà i record per ID di connessione che non sono più valide. Per eliminare tali record orfani, si consiglia di invalidare eventuali connessione creata di fuori di un intervallo di tempo è rilevante per l'applicazione. Gli esempi in questa sezione includono un valore per la registrazione quando la connessione è stata creata, ma non mostrano come pulire i record obsoleti poiché è possibile eseguire questa operazione come processo in background.

### <a name="database"></a>Database

Gli esempi seguenti illustrano come mantenere le informazioni di connessione e dell'utente in un database. È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, l'esempio seguente viene illustrato come definire i modelli usando Entity Framework. Questi modelli di entità corrispondono ai campi e tabelle di database. Struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione.

Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Quindi, dall'hub, è possibile monitorare lo stato di ogni connessione con il codice seguente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

L'esempio di archiviazione tabelle di Azure seguente è simile all'esempio del database. Non include tutte le informazioni che occorre per iniziare a usare il servizio di archiviazione tabelle di Azure. Per informazioni, vedere [come usare l'archiviazione tabelle da .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

L'esempio seguente illustra un'entità di tabella per archiviare le informazioni di connessione. Partiziona i dati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Nell'hub, tenere traccia dello stato di ogni connessione utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
