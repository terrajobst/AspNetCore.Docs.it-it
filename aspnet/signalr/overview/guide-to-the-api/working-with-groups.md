---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Utilizzo dei gruppi in SignalR | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come rendere persistenti le informazioni di appartenenza al gruppo con l'API di Hub.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 11f5be1ac4e74b692f0db3daac971a2c9d74a64c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042222"
---
<a name="working-with-groups-in-signalr"></a>Utilizzo dei gruppi in SignalR
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

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
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

Gruppi di SignalR forniscono un metodo per la trasmissione di messaggi a un subset specificato di client connessi. Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi. Non è necessario creare in modo esplicito gruppi. In effetti, la prima volta, che specificare il nome in una chiamata a Groups.Add viene automaticamente creato un gruppo e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza in essa contenuti. Per un'introduzione all'uso di gruppi, vedere [come gestire l'appartenenza al gruppo dalla classe dell'Hub](hubs-api-guide-server.md#groupsfromhub) nell'API di hub - Guida di Server.

Non vi è alcuna API per ottenere un elenco di appartenenze di gruppo oppure un elenco di gruppi. SignalR invia messaggi al client e i gruppi basati su un modello di pubblicazione/sottoscrizione e il server non gestisce gli elenchi di gruppi o appartenenza al gruppo. Ciò consente di ottimizzare la scalabilità, poiché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.

Quando si aggiunge un utente a un gruppo con il `Groups.Add` (metodo), l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente in tale gruppo non viene mantenuto oltre la connessione corrente. Se si desidera mantenere in modo permanente le informazioni sui gruppi e appartenenza al gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure. Quindi, ogni volta che un utente si connette all'applicazione, recuperare dal repository quali gruppi a cui appartiene l'utente e aggiungere manualmente l'utente a tali gruppi.

Quando la riconnessione dopo un'interruzione temporanea, l'utente aggiunge automaticamente anche nuovamente i gruppi assegnate in precedenza. Automaticamente nuova partecipazione a un gruppo si applica solo quando la riconnessione, non per stabilire una nuova connessione. Un token di firma digitale viene passato dal client che contiene l'elenco dei gruppi assegnate in precedenza. Se si desidera verificare se l'utente appartiene al gruppo richiesto, è possibile eseguire l'override del comportamento predefinito.

Questo argomento include le sezioni seguenti:

- [Aggiunta e rimozione di utenti](#add)
- [La chiamata di membri di un gruppo](#call)
- [L'appartenenza al gruppo di archiviazione in un database](#storedatabase)
- [L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure](#storeazuretable)
- [Verifica dell'appartenenza al gruppo durante la riconnessione](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Aggiunta e rimozione di utenti

Per aggiungere o rimuovere utenti da un gruppo, si chiama il [Aggiungi](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi e passare l'id connessione dell'utente e il nome del gruppo come parametri. Non è necessario rimuovere manualmente un utente da un gruppo, al termine della connessione.

Nell'esempio seguente il `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Il `Groups.Add` e `Groups.Remove` metodi eseguire in modo asincrono.

Se si desidera aggiungere un client a un gruppo e invia immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo Groups.Add termina per prima. Gli esempi di codice seguente viene illustrato come eseguire questa operazione.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

In generale, non è necessario includere `await` quando si chiama il `Groups.Remove` metodo perché l'id di connessione che si sta tentando di rimuovere potrebbe non essere più disponibile. In tal caso, `TaskCanceledException` dopo la richiesta scade, viene generata un'eccezione. Se l'applicazione deve verificare che l'utente è stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima Groups.Remove e quindi catch il `TaskCanceledException` eccezione che potrebbe essere generata.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>La chiamata di membri di un gruppo

È possibile inviare messaggi a tutti i membri di un gruppo o solo i membri specificati del gruppo, come illustrato negli esempi seguenti.

- **Tutti** connessi i client in un gruppo specificato. 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Tutti i client in un gruppo specificato connessi **tranne client specificati**, identificato dall'ID connessione. 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Tutti i client in un gruppo specificato connessi **eccetto il client chiamante**. 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>L'appartenenza al gruppo di archiviazione in un database

Nell'esempio seguente viene illustrato come mantenere le informazioni utente e di gruppo in un database. È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework. Questi modelli di entità corrispondono ai campi e tabelle di database. La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione. In questo esempio include una classe denominata `ConversationRoom` che sono univoci a un'applicazione che consente agli utenti di partecipare a conversazioni relativi ad argomenti diversi, ad esempio sports o garden. In questo esempio include anche una classe per le connessioni. La classe di connessione non è assolutamente necessaria per l'appartenenza al gruppo di rilevamento, ma spesso è parte della soluzione efficace per tenere traccia degli utenti.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Quindi, nell'hub, è possibile recuperare le informazioni di gruppo e utente dal database e aggiungere manualmente l'utente ai gruppi appropriati. L'esempio non include codice per tenere traccia delle connessioni utente. In questo esempio, il `await` parola chiave non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo. Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo l'aggiunta del nuovo membro, si desidera applicare il `await` (parola chiave) per assicurarsi che l'operazione asincrona è stata completata.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>L'appartenenza al gruppo di archiviazione nell'archiviazione tabelle di Azure

Utilizzo di archiviazione tabelle di Azure per archiviare informazioni utente e gruppo è simile all'utilizzo di un database. L'esempio seguente mostra un'entità di tabella che archivia il nome utente e il nome del gruppo.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

Nell'hub, recuperare i gruppi assegnati quando l'utente si connette.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verifica dell'appartenenza al gruppo durante la riconnessione

Per impostazione predefinita, SignalR nuovamente assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e nuovamente stabilita prima del timeout della connessione. Informazioni di gruppo dell'utente vengono passate in un token durante la riconnessione e tale token viene verificato sul server. Per informazioni sul processo di verifica per la nuova partecipazione a gruppi di utenti, vedere [nuova partecipazione a gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).

In generale, utilizzare il comportamento predefinito di automaticamente nuova partecipazione a gruppi in riconnessione. Gruppi di SignalR non servono come meccanismo di sicurezza per limitare l'accesso ai dati sensibili. Tuttavia, se l'applicazione deve controllare l'appartenenza al gruppo un utente durante la riconnessione, è possibile ignorare il comportamento predefinito. Modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché l'appartenenza al gruppo dell'utente deve essere recuperato per ogni riconnessione anziché solo quando l'utente si connette.

Se è necessario verificare l'appartenenza al gruppo in riconnettersi, creare un nuovo modulo di pipeline di hub che restituisce un elenco di gruppi assegnati, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Aggiungere quindi tale modulo alla pipeline dell'hub, come evidenziato qui sotto.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
