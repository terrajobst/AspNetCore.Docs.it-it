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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="01c86-103">API di protezione dati di varie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01c86-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="01c86-104">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="01c86-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="01c86-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="01c86-105">ISecret</span></span>

<span data-ttu-id="01c86-106">Il `ISecret` interfaccia rappresenta un valore segreto, ad esempio chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="01c86-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="01c86-107">Contiene la superficie dell'API seguente:</span><span class="sxs-lookup"><span data-stu-id="01c86-107">It contains the following API surface:</span></span>

* <span data-ttu-id="01c86-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="01c86-108">`Length`: `int`</span></span>

* <span data-ttu-id="01c86-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="01c86-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="01c86-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="01c86-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="01c86-111">Il `WriteSecretIntoBuffer` metodo popola il buffer fornito con il valore non elaborato segreto.</span><span class="sxs-lookup"><span data-stu-id="01c86-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="01c86-112">Il motivo di questa API richiede il buffer come parametro e non restituiscono un `byte[]` direttamente è che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.</span><span class="sxs-lookup"><span data-stu-id="01c86-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="01c86-113">Il `Secret` tipo è un'implementazione concreta di `ISecret` in cui è archiviato il valore segreto in memoria nel processo.</span><span class="sxs-lookup"><span data-stu-id="01c86-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="01c86-114">Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="01c86-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
