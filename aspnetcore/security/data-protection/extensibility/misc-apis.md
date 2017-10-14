---
title: Varie API
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a>Varie API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.

## <a name="isecret"></a>ISecret

L'interfaccia ISecret rappresenta un valore segreto, ad esempio chiavi crittografiche. Contiene la superficie dell'API seguente.

* Lunghezza: int

* Dispose (): void

* WriteSecretIntoBuffer (ArraySegment<byte> buffer): void

Il metodo WriteSecretIntoBuffer consente di popolare il buffer fornito con il valore non elaborato segreto. Il motivo di che questa API buffer accetta come parametro, anziché restituire che una matrice di byte sia direttamente che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.

Il tipo segreto è un'implementazione concreta di ISecret in cui è archiviato il valore segreto in memoria nel processo. Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
