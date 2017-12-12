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
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="cb809-104">Autenticazione e autorizzazione per le connessioni persistenti SignalR (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="cb809-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="cb809-105">da [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cb809-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cb809-106">In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente.</span><span class="sxs-lookup"><span data-stu-id="cb809-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="cb809-107">Per informazioni generali sull'integrazione di protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](index.md).</span><span class="sxs-lookup"><span data-stu-id="cb809-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="cb809-108">Applicare l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="cb809-108">Enforce authorization</span></span>

<span data-ttu-id="cb809-109">Per applicare le regole di autorizzazione quando si utilizza un [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` metodo.</span><span class="sxs-lookup"><span data-stu-id="cb809-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="cb809-110">Non è possibile utilizzare il `Authorize` attributo con connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="cb809-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="cb809-111">Il `AuthorizeRequest` metodo viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR.</span><span class="sxs-lookup"><span data-stu-id="cb809-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="cb809-112">Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, autenticare l'utente tramite il meccanismo di autenticazione standard dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cb809-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="cb809-113">Nell'esempio seguente viene illustrato come limitare le richieste agli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="cb809-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="cb809-114">È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un ruolo specifico.</span><span class="sxs-lookup"><span data-stu-id="cb809-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
