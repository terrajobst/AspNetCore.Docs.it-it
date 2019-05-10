---
title: Gestire utenti e gruppi in SignalR
author: bradygaster
description: Panoramica di ASP.NET Core SignalR utente e gruppo di gestione.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 180f8b4551eea39cc340bf1d250f4575cb5f71ed
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087426"
---
# <a name="manage-users-and-groups-in-signalr"></a>Gestire utenti e gruppi in SignalR

Da [Brennan Conroy](https://github.com/BrennanConroy)

SignalR consente ai messaggi da inviare a tutte le connessioni associate a un utente specifico, nonché per gruppi di connessioni con nome.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(come scaricare)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Utenti di SignalR

SignalR consente di inviare messaggi a tutte le connessioni associate a un utente specifico. Per impostazione predefinita, SignalR utilizza il `ClaimTypes.NameIdentifier` dal `ClaimsPrincipal` associato alla connessione come identificatore dell'utente. Un singolo utente può avere più connessioni a un'app per SignalR. Ad esempio, un utente potrebbe essere connesso sul desktop, nonché il telefono. Ogni dispositivo ha una connessione SignalR separata, ma sono tutte associate con lo stesso utente. Se un messaggio viene inviato all'utente, tutte le connessioni associate all'utente di ricevere il messaggio. L'identificatore utente per una connessione sono accessibili dal `Context.UserIdentifier` proprietà nell'hub.

Inviare un messaggio a un utente specifico passando l'identificatore utente per il `User` funzionare nel metodo dell'hub, come illustrato nell'esempio seguente:

> [!NOTE]
> L'identificatore utente è tra maiuscole e minuscole.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Gruppi in SignalR

Un gruppo è una raccolta di connessioni associate a un nome. I messaggi possono essere inviati a tutte le connessioni in un gruppo. I gruppi sono il metodo consigliato per l'invio di una connessione o più connessioni poiché i gruppi vengono gestiti dall'applicazione. Una connessione può essere un membro di più gruppi. In questo modo gruppi ideale per App come un'applicazione di chat, in cui ogni chat può essere rappresentato come un gruppo. Le connessioni possono essere aggiunti o rimossi dai gruppi tramite il `AddToGroupAsync` e `RemoveFromGroupAsync` metodi.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

L'appartenenza al gruppo non viene mantenuta quando si riconnette una connessione. Per partecipare di nuovo il gruppo quando viene ristabilita la connessione. Non è possibile contare i membri di un gruppo, poiché queste informazioni non sono disponibile se l'applicazione viene ridimensionata a più server.

Per proteggere l'accesso alle risorse durante l'uso di gruppi, usare [autenticazione e autorizzazione](xref:signalr/authn-and-authz) funzionalità in ASP.NET Core. Se si aggiungono solo gli utenti a un gruppo quando le credenziali sono valide per il gruppo, i messaggi inviati a tale gruppo passerà solo agli utenti autorizzati. Tuttavia, i gruppi non sono una funzionalità di sicurezza. Le attestazioni di autenticazione hanno funzionalità che i gruppi non lo consentono, ad esempio la scadenza e revoca. Se viene revocata l'autorizzazione di un utente per accedere al gruppo, è necessario rilevare che manualmente e rimuoverli dal gruppo.

> [!NOTE]
> I nomi dei gruppi sono tra maiuscole e minuscole.

## <a name="related-resources"></a>Risorse correlate

* [Introduzione](xref:tutorials/signalr)
* [Hub](xref:signalr/hubs)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
