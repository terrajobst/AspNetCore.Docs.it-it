---
title: Gestire utenti e gruppi in SignalR
author: rachelappel
description: Panoramica di ASP.NET Core SignalR utente e gruppo di gestione.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272081"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gestire utenti e gruppi in SignalR

Da [Brennan Conroy](https://github.com/BrennanConroy)

SignalR consente ai messaggi da inviare a tutte le connessioni associate a un utente specifico, nonché per gruppi di connessioni denominati.

[Consente di visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Utenti di SignalR

SignalR consente di inviare messaggi a tutte le connessioni associate a un utente specifico. Per impostazione predefinita SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come l'identificatore utente. Un singolo utente può avere più connessioni a un'applicazione di SignalR. Ad esempio, un utente potrebbe essere connesso sul desktop, nonché il proprio telefono. Ogni dispositivo presenta una connessione SignalR separata, ma sono tutti associate con lo stesso utente. Se un messaggio viene inviato all'utente, tutte le connessioni associate all'utente verrà visualizzato il messaggio.

Inviare un messaggio a un utente specifico passando l'identificatore utente per il `User` funzione al metodo dell'hub, come illustrato nell'esempio seguente:

> [!NOTE]
> L'identificatore utente è distinzione maiuscole/minuscole.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

L'identificatore utente può essere personalizzato tramite la creazione di un `IUserIdProvider`e registrandolo in `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR deve essere chiamato prima di registrare i servizi SignalR personalizzati.

## <a name="groups-in-signalr"></a>Gruppi di SignalR

Un gruppo è una raccolta di connessioni associate con un nome. Messaggi possono essere inviati a tutte le connessioni in un gruppo. I gruppi sono il modo consigliato per inviare una o più connessioni poiché i gruppi vengono gestiti dall'applicazione. Una connessione può essere un membro di più gruppi. In questo modo gruppi ideale per simile a un'applicazione di chat, in cui ogni stanza può essere rappresentato come un gruppo. Le connessioni possono essere aggiunto o rimosso dai gruppi tramite il `AddToGroupAsync` e `RemoveFromGroupAsync` metodi.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L'appartenenza al gruppo non viene mantenuto quando viene ristabilita una connessione. È necessario partecipare di nuovo il gruppo di quando viene ristabilita la connessione. Non è possibile contare i membri di un gruppo, poiché queste informazioni non sono disponibile se l'applicazione viene ridimensionato in base a più server.

> [!NOTE]
> I nomi dei gruppi sono distinzione maiuscole/minuscole.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
