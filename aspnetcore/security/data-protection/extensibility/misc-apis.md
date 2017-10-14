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
# <a name="miscellaneous-apis"></a><span data-ttu-id="2f237-103">Varie API</span><span class="sxs-lookup"><span data-stu-id="2f237-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="2f237-104">Tipi che implementano le interfacce seguenti devono essere thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="2f237-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="2f237-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="2f237-105">ISecret</span></span>

<span data-ttu-id="2f237-106">L'interfaccia ISecret rappresenta un valore segreto, ad esempio chiavi crittografiche.</span><span class="sxs-lookup"><span data-stu-id="2f237-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="2f237-107">Contiene la superficie dell'API seguente.</span><span class="sxs-lookup"><span data-stu-id="2f237-107">It contains the following API surface.</span></span>

* <span data-ttu-id="2f237-108">Lunghezza: int</span><span class="sxs-lookup"><span data-stu-id="2f237-108">Length : int</span></span>

* <span data-ttu-id="2f237-109">Dispose (): void</span><span class="sxs-lookup"><span data-stu-id="2f237-109">Dispose() : void</span></span>

* <span data-ttu-id="2f237-110">WriteSecretIntoBuffer (ArraySegment<byte> buffer): void</span><span class="sxs-lookup"><span data-stu-id="2f237-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="2f237-111">Il metodo WriteSecretIntoBuffer consente di popolare il buffer fornito con il valore non elaborato segreto.</span><span class="sxs-lookup"><span data-stu-id="2f237-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="2f237-112">Il motivo di che questa API buffer accetta come parametro, anziché restituire che una matrice di byte sia direttamente che in questo modo il chiamante l'opportunità di aggiungere l'oggetto buffer, limita l'esposizione dei segreti gestiti garbage collector.</span><span class="sxs-lookup"><span data-stu-id="2f237-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="2f237-113">Il tipo segreto è un'implementazione concreta di ISecret in cui è archiviato il valore segreto in memoria nel processo.</span><span class="sxs-lookup"><span data-stu-id="2f237-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="2f237-114">Su piattaforme Windows, il valore segreto viene crittografato tramite [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="2f237-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
