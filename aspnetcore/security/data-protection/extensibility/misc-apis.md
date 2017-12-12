---
title: Varie API
author: rick-anderson
description: Questo documento descrive l'interfaccia ASP.NET Core ISecret protezione dei dati.
keywords: ASP.NET Core, protezione dei dati, ISecret
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="bcfad-104">Varie API</span><span class="sxs-lookup"><span data-stu-id="bcfad-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="bcfad-105">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="bcfad-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="bcfad-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="bcfad-106">ISecret</span></span>

<span data-ttu-id="bcfad-107">Il `ISecret` interfaccia rappresenta un valore segreto, ad esempio chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="bcfad-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="bcfad-108">Contiene la superficie dell'API seguente:</span><span class="sxs-lookup"><span data-stu-id="bcfad-108">It contains the following API surface:</span></span>

* <span data-ttu-id="bcfad-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="bcfad-109">`Length`: `int`</span></span>

* <span data-ttu-id="bcfad-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="bcfad-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="bcfad-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="bcfad-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="bcfad-112">Il `WriteSecretIntoBuffer` metodo popola il buffer fornito con il valore non elaborato segreto.</span><span class="sxs-lookup"><span data-stu-id="bcfad-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="bcfad-113">Il motivo di questa API richiede il buffer come parametro e non restituiscono un `byte[]` direttamente è che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.</span><span class="sxs-lookup"><span data-stu-id="bcfad-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="bcfad-114">Il `Secret` tipo è un'implementazione concreta di `ISecret` in cui è archiviato il valore segreto in memoria nel processo.</span><span class="sxs-lookup"><span data-stu-id="bcfad-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="bcfad-115">Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="bcfad-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
