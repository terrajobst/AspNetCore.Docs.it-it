---
title: API per la protezione dei dati ASP.NET Core varie
author: rick-anderson
description: Informazioni sull'interfaccia di ISecret per la protezione dei dati ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666082"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API per la protezione dei dati ASP.NET Core varie

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.

## <a name="isecret"></a>ISecret

L'interfaccia `ISecret` rappresenta un valore segreto, ad esempio materiale della chiave crittografica. Contiene la seguente superficie API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Il metodo `WriteSecretIntoBuffer` popola il buffer fornito con il valore Secret non elaborato. Il motivo per cui questa API accetta il buffer come parametro piuttosto che restituire un `byte[]` direttamente è che questo fornisce al chiamante la possibilità di aggiungere l'oggetto buffer, limitando l'esposizione segreta al Garbage Collector gestito.

Il tipo di `Secret` è un'implementazione concreta di `ISecret` in cui il valore del segreto viene archiviato nella memoria in-process. Nelle piattaforme Windows il valore Secret viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
