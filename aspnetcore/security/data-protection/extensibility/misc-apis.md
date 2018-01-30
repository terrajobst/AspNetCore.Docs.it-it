---
title: Varie API
author: rick-anderson
description: Questo documento descrive l'interfaccia ASP.NET Core ISecret protezione dei dati.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="30e13-103">Varie API</span><span class="sxs-lookup"><span data-stu-id="30e13-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="30e13-104">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="30e13-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="30e13-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="30e13-105">ISecret</span></span>

<span data-ttu-id="30e13-106">Il `ISecret` interfaccia rappresenta un valore segreto, ad esempio chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="30e13-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="30e13-107">Contiene la superficie dell'API seguente:</span><span class="sxs-lookup"><span data-stu-id="30e13-107">It contains the following API surface:</span></span>

* <span data-ttu-id="30e13-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="30e13-108">`Length`: `int`</span></span>

* <span data-ttu-id="30e13-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="30e13-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="30e13-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="30e13-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="30e13-111">Il `WriteSecretIntoBuffer` metodo popola il buffer fornito con il valore non elaborato segreto.</span><span class="sxs-lookup"><span data-stu-id="30e13-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="30e13-112">Il motivo di questa API richiede il buffer come parametro e non restituiscono un `byte[]` direttamente è che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.</span><span class="sxs-lookup"><span data-stu-id="30e13-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="30e13-113">Il `Secret` tipo è un'implementazione concreta di `ISecret` in cui è archiviato il valore segreto in memoria nel processo.</span><span class="sxs-lookup"><span data-stu-id="30e13-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="30e13-114">Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="30e13-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
