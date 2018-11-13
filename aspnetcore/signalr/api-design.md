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
# <a name="signalr-api-design-considerations"></a>Considerazioni sulla progettazione di API SignalR

Da [Andrew Stanton-Nurse](https://twitter.com/anurse)

Questo articolo fornisce indicazioni per la compilazione di API basate su SignalR.

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>Usare i parametri dell'oggetto personalizzato per garantire la compatibilità

Aggiunta di parametri a un metodo dell'hub di SignalR (sul client o server) è un *modifica di rilievo*. Ciò significa che i client/server meno recenti verranno generati errori quando si tenta di richiamare il metodo senza il numero appropriato di parametri. Aggiunta di proprietà a un parametro di oggetto personalizzato è tuttavia **non** una modifica sostanziale. Utilizzabile per la progettazione di API compatibili che siano resilienti alle modifiche apportate sul client o server.

Si consideri ad esempio un'API lato server simile al seguente:

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

Il client JavaScript chiama questo metodo usando `invoke` come indicato di seguito:

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

Se in un secondo momento si aggiunge un secondo parametro al metodo del server, client meno recenti non specificare questo valore di parametro. Ad esempio:

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

Quando il client precedente tenta di richiamare questo metodo, otterrà un errore simile al seguente:

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

Nel server, si verrà visualizzato un messaggio di log simile al seguente:

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

Client precedente inviato solo un parametro, ma l'API del server più recenti necessari due parametri. Utilizzo di oggetti personalizzati come parametri offre maggiore flessibilità. È possibile riprogettare l'API originale per l'uso di un oggetto personalizzato:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

A questo punto, il client usa un oggetto per chiamare il metodo:

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

Anziché aggiungere un parametro, aggiungere una proprietà di `TotalLengthRequest` oggetto:

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

Quando il client precedente invia un singolo parametro, extra `Param2` verrà lasciata proprietà `null`. È possibile rilevare un messaggio inviato da un client precedente controllando il `Param2` per `null` e applicare un valore predefinito. Un nuovo client può inviare entrambi i parametri.

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

La stessa tecnica funziona per i metodi definiti nel client. È possibile inviare un oggetto personalizzato sul lato server:

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

Sul lato client, è accedere il `Message` proprietà anziché con un parametro:

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

Se successivamente si decide di aggiungere il mittente del messaggio al payload, aggiungere una proprietà nell'oggetto:

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

I client meno recenti non aspetta il `Sender` valore, quindi verrà ignorata. Un nuovo client può accettarlo aggiornando in modo che la nuova proprietà di lettura:

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

In questo caso, il nuovo client è anche tolleranza di errore di un server precedente che non forniscono il `Sender` valore. Dal momento che il vecchio server non fornisce il `Sender` valore, il client controlla per verificare se è presente prima dell'accesso.
