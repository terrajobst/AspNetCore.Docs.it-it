---
title: Rimozione della protezione payload le cui chiavi sono stati revocati.
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 44f21f380b994f46a8bb7368bca0cfc6e438ec4d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="039b9-103">Rimozione della protezione payload le cui chiavi sono stati revocati.</span><span class="sxs-lookup"><span data-stu-id="039b9-103">Unprotecting payloads whose keys have been revoked</span></span>

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

<span data-ttu-id="039b9-104">Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservato.</span><span class="sxs-lookup"><span data-stu-id="039b9-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="039b9-105">Altre tecnologie come [DPAPI CNG di Windows](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) sono più adatti per lo scenario di memorizzazione indefinito, e hanno funzionalità di gestione delle chiavi conseguentemente sicuro.</span><span class="sxs-lookup"><span data-stu-id="039b9-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="039b9-106">Ciò premesso, non c'è niente che uno sviluppatore di utilizzare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati.</span><span class="sxs-lookup"><span data-stu-id="039b9-106">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="039b9-107">Le chiavi non vengono mai rimosse dalla sequenza di chiave, in modo IDataProtector.Unprotect poter ripristinare sempre i payload esistenti, purché le chiavi sono disponibili e validi.</span><span class="sxs-lookup"><span data-stu-id="039b9-107">Keys are never removed from the key ring, so IDataProtector.Unprotect can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="039b9-108">Tuttavia, un problema si verifica quando lo sviluppatore tenta di rimuovere la protezione dati che sono stato protetto con una chiave revocata, come IDataProtector.Unprotect genererà un'eccezione in questo caso.</span><span class="sxs-lookup"><span data-stu-id="039b9-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as IDataProtector.Unprotect will throw an exception in this case.</span></span> <span data-ttu-id="039b9-109">Potrebbe essere appropriato per payload di breve durato o temporanee (ad esempio, il token di autenticazione), questi tipi di payload possono facilmente essere ricreati mediante il sistema e nel peggiore dei casi i visitatori del sito potrebbe essere necessario eseguire nuovamente l'accesso.</span><span class="sxs-lookup"><span data-stu-id="039b9-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="039b9-110">Ma per il payload persistente, con Unprotect generare può causare la perdita di dati accettabile.</span><span class="sxs-lookup"><span data-stu-id="039b9-110">But for persisted payloads, having Unprotect throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="039b9-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="039b9-111">IPersistedDataProtector</span></span>

<span data-ttu-id="039b9-112">Per supportare lo scenario di consentire il payload essere protetto anche in caso di chiavi revocate, il sistema di protezione dei dati contiene un tipo IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="039b9-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an IPersistedDataProtector type.</span></span> <span data-ttu-id="039b9-113">Per ottenere un'istanza di IPersistedDataProtector, ottenere un'istanza dell'oggetto IDataProtector in modo normale e provare a eseguire il cast l'oggetto IDataProtector per IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="039b9-113">To get an instance of IPersistedDataProtector, simply get an instance of IDataProtector in the normal fashion and try casting the IDataProtector to IPersistedDataProtector.</span></span>

> [!NOTE]
> <span data-ttu-id="039b9-114">Non tutte le istanze di oggetto IDataProtector possono essere convertite in IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="039b9-114">Not all IDataProtector instances can be cast to IPersistedDataProtector.</span></span> <span data-ttu-id="039b9-115">Gli sviluppatori devono usare c# come operatore o simili per evitare eccezioni di runtime dovuto a cast non valido e deve essere preparato a gestire il caso di errore in modo appropriato.</span><span class="sxs-lookup"><span data-stu-id="039b9-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="039b9-116">IPersistedDataProtector espone la superficie dell'API seguente:</span><span class="sxs-lookup"><span data-stu-id="039b9-116">IPersistedDataProtector exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

<span data-ttu-id="039b9-117">Questa API richiede il payload protetto (come una matrice di byte) e restituisce il payload non protetto.</span><span class="sxs-lookup"><span data-stu-id="039b9-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="039b9-118">Non vi è alcun overload basato su stringa.</span><span class="sxs-lookup"><span data-stu-id="039b9-118">There is no string-based overload.</span></span> <span data-ttu-id="039b9-119">Di seguito sono riportati i due parametri out.</span><span class="sxs-lookup"><span data-stu-id="039b9-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="039b9-120">requiresMigration: verrà impostato su true se la chiave utilizzata per proteggere il payload non è più la chiave predefinita attiva, ad esempio, la chiave utilizzata per proteggere il payload è precedente ed è una chiave di sequenza di operazione eseguita sul posto.</span><span class="sxs-lookup"><span data-stu-id="039b9-120">requiresMigration: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="039b9-121">Il chiamante potrebbe essere necessario prendere in considerazione proteggere di nuovo il payload a seconda delle esigenze aziendali.</span><span class="sxs-lookup"><span data-stu-id="039b9-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="039b9-122">wasRevoked: verrà impostato su true se la chiave utilizzata per proteggere il payload è stata revocata.</span><span class="sxs-lookup"><span data-stu-id="039b9-122">wasRevoked: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="039b9-123">Prestare molta attenzione quando si passa ignoreRevocationErrors: true se il metodo DangerousUnprotect.</span><span class="sxs-lookup"><span data-stu-id="039b9-123">Exercise extreme caution when passing ignoreRevocationErrors: true to the DangerousUnprotect method.</span></span> <span data-ttu-id="039b9-124">Se dopo aver chiamato questo metodo, il valore di wasRevoked è true, quindi la chiave utilizzata per proteggere il payload è stata revocata e autenticità del payload devono essere considerate come sospetto.</span><span class="sxs-lookup"><span data-stu-id="039b9-124">If after calling this method the wasRevoked value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="039b9-125">In questo caso solo smettere di funzionare nel payload di non protetti, se si dispone di una discreta garanzia di separata che è autentico, ad esempio, che non del provenienti da un database protetto anziché inviati da un client web non attendibili.</span><span class="sxs-lookup"><span data-stu-id="039b9-125">In this case only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

<span data-ttu-id="039b9-126">[!code-none[Principale](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span><span class="sxs-lookup"><span data-stu-id="039b9-126">[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span></span>
