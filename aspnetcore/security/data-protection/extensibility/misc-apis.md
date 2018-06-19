---
title: API di protezione dati di varie ASP.NET Core
author: rick-anderson
description: Informazioni sull'interfaccia ISecret di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072764"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API di protezione dati di varie ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.

## <a name="isecret"></a>ISecret

Il `ISecret` interfaccia rappresenta un valore segreto, ad esempio chiavi crittografiche. Contiene la superficie dell'API seguente:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Il `WriteSecretIntoBuffer` metodo popola il buffer fornito con il valore non elaborato segreto. Il motivo di questa API richiede il buffer come parametro e non restituiscono un `byte[]` direttamente è che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.

Il `Secret` tipo è un'implementazione concreta di `ISecret` in cui è archiviato il valore segreto in memoria nel processo. Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
