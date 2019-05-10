---
title: API di protezione dei dati esterni ASP.NET Core
author: rick-anderson
description: Informazioni sull'interfaccia ISecret protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896618"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API di protezione dei dati esterni ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.

## <a name="isecret"></a>ISecret

Il `ISecret` interfaccia rappresenta un valore del segreto, ad esempio chiavi crittografiche. Contiene la superficie dell'API seguente:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Il `WriteSecretIntoBuffer` metodo consente di popolare il buffer fornito con il valore del segreto non elaborato. Il motivo di questa API richiede il buffer come parametro invece di restituire un `byte[]` direttamente è che in questo modo il chiamante l'opportunità di aggiungere l'oggetto del buffer, limitando l'esposizione di segreti gestiti garbage collector.

Il `Secret` il tipo è un'implementazione concreta di `ISecret` in cui è archiviato il valore del segreto in memoria nel processo. Su piattaforme Windows, il valore del segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
