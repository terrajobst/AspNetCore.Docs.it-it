---
title: API di protezione dati di varie ASP.NET Core
author: rick-anderson
description: Informazioni sull'interfaccia ISecret di protezione dei dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279155"
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
