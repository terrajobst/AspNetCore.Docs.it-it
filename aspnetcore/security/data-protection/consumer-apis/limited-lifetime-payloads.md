---
title: Limitando la durata di payload protetto
author: rick-anderson
description: Questo documento viene illustrato come limitare la durata di un payload protetto utilizzando le API di protezione dati ASP.NET Core.
keywords: ASP.NET Core, protezione dei dati, la durata del payload
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="86379-104">Limitando la durata di payload protetto</span><span class="sxs-lookup"><span data-stu-id="86379-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="86379-105">Esistono scenari in cui lo sviluppatore dell'applicazione desidera creare un payload protetto che scade dopo un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="86379-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="86379-106">Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che devono solo essere valido per un'ora.</span><span class="sxs-lookup"><span data-stu-id="86379-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="86379-107">È certamente possibile per gli sviluppatori di creare i propri formato di payload che contiene una data di scadenza incorporati e gli sviluppatori avanzati si consiglia di eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestire queste scadenze può crescere noiosa.</span><span class="sxs-lookup"><span data-stu-id="86379-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="86379-108">Per semplificare questa operazione per il pubblico di sviluppatore, il pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API dell'utilità per la creazione di payload che scadrà automaticamente dopo un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="86379-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="86379-109">Queste API sporgere all'esterno del `ITimeLimitedDataProtector` tipo.</span><span class="sxs-lookup"><span data-stu-id="86379-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="86379-110">Utilizzo delle API</span><span class="sxs-lookup"><span data-stu-id="86379-110">API usage</span></span>

<span data-ttu-id="86379-111">Il `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e la rimozione della protezione di payload di tempo limitato / temporanee.</span><span class="sxs-lookup"><span data-stu-id="86379-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="86379-112">Per creare un'istanza di un `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di una normale [oggetto IDataProtector](overview.md) costruito con uno scopo specifico.</span><span class="sxs-lookup"><span data-stu-id="86379-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="86379-113">Una volta il `IDataProtector` istanza è disponibile, chiamare il `IDataProtector.ToTimeLimitedDataProtector` metodo di estensione per ottenere una protezione con le funzionalità di scadenza predefinita.</span><span class="sxs-lookup"><span data-stu-id="86379-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="86379-114">`ITimeLimitedDataProtector`espone i metodi di estensione e di area API seguenti:</span><span class="sxs-lookup"><span data-stu-id="86379-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="86379-115">CreateProtector (scopo stringa): ITimeLimitedDataProtector - questa API è simile al `IDataProtectionProvider.CreateProtector` in quanto può essere utilizzato per creare [allo scopo di catene](purpose-strings.md) da una protezione con durata limitata di radice.</span><span class="sxs-lookup"><span data-stu-id="86379-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="86379-116">Proteggere (byte [] testo normale, scadenza DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="86379-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="86379-117">Proteggere (testo normale di byte [], durata intervallo di tempo): byte]</span><span class="sxs-lookup"><span data-stu-id="86379-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="86379-118">Proteggere (testo crittografato [byte): byte]</span><span class="sxs-lookup"><span data-stu-id="86379-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="86379-119">Proteggere (stringa testo normale, scadenza DateTimeOffset): stringa</span><span class="sxs-lookup"><span data-stu-id="86379-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="86379-120">Proteggere (testo normale di stringa, durata intervallo di tempo): stringa</span><span class="sxs-lookup"><span data-stu-id="86379-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="86379-121">Proteggere (testo normale stringa): stringa</span><span class="sxs-lookup"><span data-stu-id="86379-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="86379-122">Oltre alle principali `Protect` metodi che accettano solo il testo non crittografato, sono disponibili nuovi overload che consentono di specificare la data di scadenza del payload.</span><span class="sxs-lookup"><span data-stu-id="86379-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="86379-123">La data di scadenza può essere specificata come una data assoluta (tramite un `DateTimeOffset`) o come un tempo relativi (dal sistema corrente time, tramite un `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="86379-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="86379-124">Se viene chiamato un overload che non avranno una scadenza, il payload presuppone scada mai.</span><span class="sxs-lookup"><span data-stu-id="86379-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="86379-125">Rimuovere la protezione (protectedData [] byte, out scadenza DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="86379-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="86379-126">Rimuovere la protezione (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="86379-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="86379-127">Rimuovere la protezione (stringa protectedData, out scadenza DateTimeOffset): stringa</span><span class="sxs-lookup"><span data-stu-id="86379-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="86379-128">Rimuovere la protezione (stringa protectedData): stringa</span><span class="sxs-lookup"><span data-stu-id="86379-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="86379-129">Il `Unprotect` i metodi restituiscono i dati originali non protetti.</span><span class="sxs-lookup"><span data-stu-id="86379-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="86379-130">Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro insieme ai dati protetti originali out facoltativa.</span><span class="sxs-lookup"><span data-stu-id="86379-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="86379-131">Se il payload è scaduto, tutti gli overload del metodo Unprotect genererà CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="86379-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="86379-132">È consigliabile non usare queste API per proteggere i payload che richiedono la persistenza a lungo termine o indefinita.</span><span class="sxs-lookup"><span data-stu-id="86379-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="86379-133">"È concedere per il payload protetto essere recuperati in modo permanente dopo un mese?"</span><span class="sxs-lookup"><span data-stu-id="86379-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="86379-134">può essere utilizzato come una buona regola pratica; Se la risposta non sviluppatori quindi considerare API alternative.</span><span class="sxs-lookup"><span data-stu-id="86379-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="86379-135">L'esempio seguente viene utilizzato il [i percorsi del codice non DI](../configuration/non-di-scenarios.md) per creare un'istanza di sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="86379-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="86379-136">Per eseguire questo esempio, verificare che è stato innanzitutto aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="86379-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
