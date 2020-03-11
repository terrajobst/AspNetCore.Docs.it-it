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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="b7672-103">API per la protezione dei dati ASP.NET Core varie</span><span class="sxs-lookup"><span data-stu-id="b7672-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="b7672-104">I tipi che implementano una delle interfacce seguenti devono essere thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="b7672-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="b7672-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="b7672-105">ISecret</span></span>

<span data-ttu-id="b7672-106">L'interfaccia `ISecret` rappresenta un valore segreto, ad esempio materiale della chiave crittografica.</span><span class="sxs-lookup"><span data-stu-id="b7672-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="b7672-107">Contiene la seguente superficie API:</span><span class="sxs-lookup"><span data-stu-id="b7672-107">It contains the following API surface:</span></span>

* <span data-ttu-id="b7672-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="b7672-108">`Length`: `int`</span></span>

* <span data-ttu-id="b7672-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="b7672-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="b7672-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="b7672-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="b7672-111">Il metodo `WriteSecretIntoBuffer` popola il buffer fornito con il valore Secret non elaborato.</span><span class="sxs-lookup"><span data-stu-id="b7672-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="b7672-112">Il motivo per cui questa API accetta il buffer come parametro piuttosto che restituire un `byte[]` direttamente è che questo fornisce al chiamante la possibilità di aggiungere l'oggetto buffer, limitando l'esposizione segreta al Garbage Collector gestito.</span><span class="sxs-lookup"><span data-stu-id="b7672-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="b7672-113">Il tipo di `Secret` è un'implementazione concreta di `ISecret` in cui il valore del segreto viene archiviato nella memoria in-process.</span><span class="sxs-lookup"><span data-stu-id="b7672-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="b7672-114">Nelle piattaforme Windows il valore Secret viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="b7672-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
