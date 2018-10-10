---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticazione e autorizzazione per le connessioni persistenti di SignalR | Microsoft Docs
author: pfletcher
description: In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente. Per informazioni generali sull'integrazione di sicurezza in un'applicazione di SignalR,...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 0df0ced803af04c4dc537aff6f1db60278aae578
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911214"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="86beb-104">Autenticazione e autorizzazione per le connessioni persistenti di SignalR</span><span class="sxs-lookup"><span data-stu-id="86beb-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="86beb-105">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="86beb-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="86beb-106">In questo argomento viene descritto come applicare l'autorizzazione in una connessione permanente.</span><span class="sxs-lookup"><span data-stu-id="86beb-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="86beb-107">Per informazioni generali sull'integrazione della protezione in un'applicazione di SignalR, vedere [Introduzione alla sicurezza](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="86beb-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="86beb-108">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="86beb-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="86beb-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="86beb-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="86beb-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="86beb-110">.NET 4.5</span></span>
> - <span data-ttu-id="86beb-111">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="86beb-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="86beb-112">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="86beb-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="86beb-113">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="86beb-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="86beb-114">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="86beb-114">Questions and comments</span></span>
>
> <span data-ttu-id="86beb-115">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="86beb-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="86beb-116">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="86beb-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="86beb-117">Imporre l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="86beb-117">Enforce authorization</span></span>

<span data-ttu-id="86beb-118">Per applicare le regole di autorizzazione quando si usa un' [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) è necessario eseguire l'override di `AuthorizeRequest` (metodo).</span><span class="sxs-lookup"><span data-stu-id="86beb-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="86beb-119">Non è possibile usare il `Authorize` attributo con le connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="86beb-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="86beb-120">Il `AuthorizeRequest` viene chiamato dal Framework prima di ogni richiesta per verificare che l'utente è autorizzato a eseguire l'azione richiesta SignalR.</span><span class="sxs-lookup"><span data-stu-id="86beb-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="86beb-121">Il `AuthorizeRequest` metodo non viene chiamato dal client; in alternativa, si autentica l'utente tramite il meccanismo di autenticazione standard dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="86beb-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="86beb-122">L'esempio seguente viene illustrato come limitare le richieste per gli utenti autenticati.</span><span class="sxs-lookup"><span data-stu-id="86beb-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="86beb-123">È possibile aggiungere qualsiasi logica di autorizzazione personalizzato nel metodo AuthorizeRequest; ad esempio, verifica se un utente appartiene a un particolare ruolo.</span><span class="sxs-lookup"><span data-stu-id="86beb-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
