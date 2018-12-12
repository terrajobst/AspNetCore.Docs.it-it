---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di sicurezza in un'applicazione di SignalR,...
ms.author: riande
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4973cb9bd03088106fe9502e0e706c6c28d2d46d
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286857"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticazione e autorizzazione per le connessioni persistenti di SignalR (SignalR 1.x)
====================
dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione della protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](index.md).


## <a name="enforce-authorization"></a>Imporre l'autorizzazione

Per applicare le regole di autorizzazione quando si usa un' [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` (metodo). Non è possibile usare il `Authorize` attributo con le connessioni permanenti. Il `AuthorizeRequest` viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR. Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.

L'esempio seguente viene illustrato come limitare le richieste per gli utenti autenticati.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un particolare ruolo.
