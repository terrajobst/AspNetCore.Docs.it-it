---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni persistenti SignalR (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di protezione in un'applicazione di SignalR,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticazione e autorizzazione per le connessioni persistenti SignalR (SignalR 1. x)
====================
da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](index.md).


## <a name="enforce-authorization"></a>Applicare l'autorizzazione

Per applicare le regole di autorizzazione quando si utilizza un [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` metodo. Non è possibile utilizzare il `Authorize` attributo con connessioni permanenti. Il `AuthorizeRequest` metodo viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR. Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, autenticare l'utente tramite il meccanismo di autenticazione standard dell'applicazione.

Nell'esempio seguente viene illustrato come limitare le richieste agli utenti autenticati.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un ruolo specifico.
