---
title: Considerazioni sulla progettazione di API SignalR
author: anurse
description: Informazioni su come progettare APIs SignalR per garantire la compatibilità tra versioni dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571553"
---
# <a name="signalr-api-design-considerations"></a><span data-ttu-id="69d88-103">Considerazioni sulla progettazione di API SignalR</span><span class="sxs-lookup"><span data-stu-id="69d88-103">SignalR API design considerations</span></span>

<span data-ttu-id="69d88-104">Da [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="69d88-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="69d88-105">Questo articolo fornisce indicazioni per la compilazione di API basate su SignalR.</span><span class="sxs-lookup"><span data-stu-id="69d88-105">This article provides guidance for building SignalR-based APIs.</span></span>

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a><span data-ttu-id="69d88-106">Usare i parametri dell'oggetto personalizzato per garantire la compatibilità</span><span class="sxs-lookup"><span data-stu-id="69d88-106">Use custom object parameters to ensure backwards-compatibility</span></span>

<span data-ttu-id="69d88-107">Aggiunta di parametri a un metodo dell'hub di SignalR (sul client o server) è un *modifica di rilievo*.</span><span class="sxs-lookup"><span data-stu-id="69d88-107">Adding parameters to a SignalR hub method (on either the client or the server) is a *breaking change*.</span></span> <span data-ttu-id="69d88-108">Ciò significa che i client/server meno recenti verranno generati errori quando si tenta di richiamare il metodo senza il numero appropriato di parametri.</span><span class="sxs-lookup"><span data-stu-id="69d88-108">This means older clients/servers will get errors when they try to invoke the method without the appropriate number of parameters.</span></span> <span data-ttu-id="69d88-109">Aggiunta di proprietà a un parametro di oggetto personalizzato è tuttavia **non** una modifica sostanziale.</span><span class="sxs-lookup"><span data-stu-id="69d88-109">However, adding properties to a custom object parameter is **not** a breaking change.</span></span> <span data-ttu-id="69d88-110">Utilizzabile per la progettazione di API compatibili che siano resilienti alle modifiche apportate sul client o server.</span><span class="sxs-lookup"><span data-stu-id="69d88-110">This can be used to design compatible APIs that are resilient to changes on the client or the server.</span></span>

<span data-ttu-id="69d88-111">Si consideri ad esempio un'API lato server simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="69d88-111">For example, consider a server-side API like the following:</span></span>

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

<span data-ttu-id="69d88-112">Il client JavaScript chiama questo metodo usando `invoke` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="69d88-112">The JavaScript client calls this method using `invoke` as follows:</span></span>

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

<span data-ttu-id="69d88-113">Se in un secondo momento si aggiunge un secondo parametro al metodo del server, client meno recenti non specificare questo valore di parametro.</span><span class="sxs-lookup"><span data-stu-id="69d88-113">If you later add a second parameter to the server method, older clients won't provide this parameter value.</span></span> <span data-ttu-id="69d88-114">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="69d88-114">For example:</span></span>

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

<span data-ttu-id="69d88-115">Quando il client precedente tenta di richiamare questo metodo, otterrà un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="69d88-115">When the old client tries to invoke this method, it will get an error like this:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

<span data-ttu-id="69d88-116">Nel server, si verrà visualizzato un messaggio di log simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="69d88-116">On the server, you'll see a log message like this:</span></span>

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

<span data-ttu-id="69d88-117">Client precedente inviato solo un parametro, ma l'API del server più recenti necessari due parametri.</span><span class="sxs-lookup"><span data-stu-id="69d88-117">The old client only sent one parameter, but the newer server API required two parameters.</span></span> <span data-ttu-id="69d88-118">Utilizzo di oggetti personalizzati come parametri offre maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="69d88-118">Using custom objects as parameters gives you more flexibility.</span></span> <span data-ttu-id="69d88-119">È possibile riprogettare l'API originale per l'uso di un oggetto personalizzato:</span><span class="sxs-lookup"><span data-stu-id="69d88-119">Let's redesign the original API to use a custom object:</span></span>

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

<span data-ttu-id="69d88-120">A questo punto, il client usa un oggetto per chiamare il metodo:</span><span class="sxs-lookup"><span data-stu-id="69d88-120">Now, the client uses an object to call the method:</span></span>

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

<span data-ttu-id="69d88-121">Anziché aggiungere un parametro, aggiungere una proprietà di `TotalLengthRequest` oggetto:</span><span class="sxs-lookup"><span data-stu-id="69d88-121">Instead of adding a parameter, add a property to the `TotalLengthRequest` object:</span></span>

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

<span data-ttu-id="69d88-122">Quando il client precedente invia un singolo parametro, extra `Param2` verrà lasciata proprietà `null`.</span><span class="sxs-lookup"><span data-stu-id="69d88-122">When the old client sends a single parameter, the extra `Param2` property will be left `null`.</span></span> <span data-ttu-id="69d88-123">È possibile rilevare un messaggio inviato da un client precedente controllando il `Param2` per `null` e applicare un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="69d88-123">You can detect a message sent by an older client by checking the `Param2` for `null` and apply a default value.</span></span> <span data-ttu-id="69d88-124">Un nuovo client può inviare entrambi i parametri.</span><span class="sxs-lookup"><span data-stu-id="69d88-124">A new client can send both parameters.</span></span>

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

<span data-ttu-id="69d88-125">La stessa tecnica funziona per i metodi definiti nel client.</span><span class="sxs-lookup"><span data-stu-id="69d88-125">The same technique works for methods defined on the client.</span></span> <span data-ttu-id="69d88-126">È possibile inviare un oggetto personalizzato sul lato server:</span><span class="sxs-lookup"><span data-stu-id="69d88-126">You can send a custom object from the server side:</span></span>

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

<span data-ttu-id="69d88-127">Sul lato client, è accedere il `Message` proprietà anziché con un parametro:</span><span class="sxs-lookup"><span data-stu-id="69d88-127">On the client side, you access the `Message` property rather than using a parameter:</span></span>

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

<span data-ttu-id="69d88-128">Se successivamente si decide di aggiungere il mittente del messaggio al payload, aggiungere una proprietà nell'oggetto:</span><span class="sxs-lookup"><span data-stu-id="69d88-128">If you later decide to add the sender of the message to the payload, add a property to the object:</span></span>

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

<span data-ttu-id="69d88-129">I client meno recenti non aspetta il `Sender` valore, quindi verrà ignorata.</span><span class="sxs-lookup"><span data-stu-id="69d88-129">The older clients won't be expecting the `Sender` value, so they'll ignore it.</span></span> <span data-ttu-id="69d88-130">Un nuovo client può accettarlo aggiornando in modo che la nuova proprietà di lettura:</span><span class="sxs-lookup"><span data-stu-id="69d88-130">A new client can accept it by updating to read the new property:</span></span>

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

<span data-ttu-id="69d88-131">In questo caso, il nuovo client è anche tolleranza di errore di un server precedente che non forniscono il `Sender` valore.</span><span class="sxs-lookup"><span data-stu-id="69d88-131">In this case, the new client is also tolerant of an old server that doesn't provide the `Sender` value.</span></span> <span data-ttu-id="69d88-132">Dal momento che il vecchio server non fornisce il `Sender` valore, il client controlla per verificare se è presente prima dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="69d88-132">Since the old server won't provide the `Sender` value, the client checks to see if it exists before accessing it.</span></span>
