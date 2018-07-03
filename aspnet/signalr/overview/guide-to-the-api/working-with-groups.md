---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Uso dei gruppi in SignalR | Microsoft Docs
author: pfletcher
description: In questo argomento viene descritto come rendere persistenti le informazioni di appartenenza al gruppo con l'API dell'Hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: b4ac3053a5b324de11d69865c92c6783aadab7f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378258"
---
<a name="working-with-groups-in-signalr"></a>Uso dei gruppi in SignalR
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento viene descritto come aggiungere utenti ai gruppi e rendere persistenti le informazioni di appartenenza al gruppo. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
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


## <a name="overview"></a>Panoramica

Gruppi in SignalR forniscono un metodo per la trasmissione di messaggi per sottoinsiemi specifici di client connessi. Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi. Non è necessario creare in modo esplicito gruppi. In effetti, la prima volta che specificarne il nome in una chiamata a Groups.Add viene automaticamente creato un gruppo, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza al suo interno. Per un'introduzione all'uso dei gruppi, vedere [come gestire l'appartenenza al gruppo dalla classe Hub](hubs-api-guide-server.md#groupsfromhub) nell'API di hub - Guida di Server.

Non è disponibile alcuna API per ottenere un elenco di appartenenza al gruppo o un elenco di gruppi. SignalR invia messaggi al client e i gruppi basati su un modello pub/sub e il server non venga mantenuto un elenco di gruppi o appartenenze ai gruppi. Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.

Quando si aggiunge un utente a un gruppo usando il `Groups.Add` metodo, l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente in tale gruppo non viene mantenuto oltre la connessione corrente. Se si desidera mantenere in modo permanente informazioni sui gruppi e appartenenza al gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure. Quindi, ogni volta che un utente si connette all'applicazione, recuperare dal repository di quali gruppi a cui appartiene l'utente e aggiungere manualmente l'utente a tali gruppi.

Quando si riconnette dopo un'interruzione temporanea, l'utente nuovamente unisce automaticamente i gruppi assegnati in precedenza. Automaticamente la nuova partecipazione a un gruppo si applica solo quando la riconnessione, non quando si stabilisce una nuova connessione. Un token di firma digitale viene passato dal client che contiene l'elenco dei gruppi assegnati in precedenza. Se si desidera verificare se l'utente appartiene a gruppi di richiesta, è possibile eseguire l'override del comportamento predefinito.

Questo argomento include le sezioni seguenti:

- [Aggiunta e rimozione di utenti](#add)
- [La chiamata a membri di un gruppo](#call)
- [L'appartenenza al gruppo di archiviazione in un database](#storedatabase)
- [L'archiviazione l'appartenenza al gruppo di nell'archiviazione tabelle di Azure](#storeazuretable)
- [Verifica per determinare se l'appartenenza al gruppo durante la riconnessione](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Aggiunta e rimozione di utenti

Per aggiungere o rimuovere utenti da un gruppo, si chiama il [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) oppure [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi e passare il nome del gruppo come parametri e l'id di connessione dell'utente. Non devi rimuovere manualmente un utente da un gruppo, al termine della connessione.

L'esempio seguente mostra le `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Il `Groups.Add` e `Groups.Remove` metodi eseguiti in modo asincrono.

Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo Groups.Add termina per prima. Esempi di codice seguenti illustrano come eseguire questa operazione.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

In generale, non è necessario includere `await` quando si chiama il `Groups.Remove` metodo perché l'id di connessione che si sta tentando di rimuovere potrebbe non essere più disponibile. In tal caso, `TaskCanceledException` viene generata dopo che la richiesta scade. Se l'applicazione deve verificare che l'utente è stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima di Groups.Remove e quindi catch il `TaskCanceledException` eccezioni che potrebbero essere generate.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>La chiamata a membri di un gruppo

È possibile inviare messaggi a tutti i membri di un gruppo o solo i membri specificati del gruppo, come illustrato negli esempi seguenti.

- **Tutti i** connessi i client in un gruppo specificato. 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Tutti i client in un gruppo specificato connessi **eccetto il client specificati**, identificato dall'ID di connessione. 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Tutti i client in un gruppo specificato connessi **eccetto il client chiamante**. 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>L'appartenenza al gruppo di archiviazione in un database

Gli esempi seguenti illustrano come mantenere le informazioni utente e gruppo in un database. È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, l'esempio seguente viene illustrato come definire i modelli usando Entity Framework. Questi modelli di entità corrispondono ai campi e tabelle di database. Struttura dei dati potrebbe variare notevolmente a seconda dei requisiti dell'applicazione. In questo esempio include una classe denominata `ConversationRoom` che potrebbe essere univoco per un'applicazione che consente agli utenti di aggiungersi conversazioni relativi ad argomenti diversi, ad esempio sportivo o garden. Questo esempio include anche una classe per le connessioni. La classe di connessione non è assolutamente necessaria per l'appartenenza al gruppo di rilevamento, ma è spesso parte di una soluzione efficace per tenere traccia degli utenti.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Quindi, nell'hub, è possibile recuperare le informazioni di gruppo e utente dal database e aggiungere manualmente l'utente ai gruppi appropriati. L'esempio non include codice per tenere traccia delle connessioni utente. In questo esempio, il `await` parola chiave non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo. Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo aver aggiunto il nuovo membro, si desidera applicare il `await` (parola chiave) per assicurarsi che l'operazione asincrona è stata completata.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>L'archiviazione l'appartenenza al gruppo di nell'archiviazione tabelle di Azure

Utilizzo dell'archiviazione tabelle di Azure per archiviare le informazioni utente e gruppo è simile all'uso di un database. L'esempio seguente illustra un'entità di tabella che archivia il nome utente e il nome del gruppo.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

Nell'hub, recuperare i gruppi assegnati quando l'utente si connette.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verifica per determinare se l'appartenenza al gruppo durante la riconnessione

Per impostazione predefinita, SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati durante la riconnessione dopo un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Informazioni sui gruppi dell'utente viene passati un token durante la riconnessione, e tale token viene verificato nel server. Per informazioni sul processo di verifica per la nuova partecipazione a gruppi di utenti, vedere [la nuova partecipazione a gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).

In generale, si dovrebbe usare il comportamento predefinito di automaticamente la nuova partecipazione a gruppi in riconnessione. SignalR gruppi non sono intesi come meccanismo di sicurezza per limitare l'accesso ai dati sensibili. Tuttavia, se l'applicazione deve controllare l'appartenenza al gruppo dell'utente durante la riconnessione, è possibile eseguire l'override del comportamento predefinito. Modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché l'appartenenza al gruppo dell'utente deve essere recuperato per ciascun riconnessione anziché solo quando l'utente si connette.

Se è necessario verificare l'appartenenza al gruppo in riconnessione, creare un nuovo modulo di pipeline dell'hub che restituisce un elenco dei gruppi assegnati, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Aggiungere quindi tale modulo alla pipeline dell'hub, come evidenziato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
