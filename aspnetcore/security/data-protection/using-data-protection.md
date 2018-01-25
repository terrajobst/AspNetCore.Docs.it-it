---
title: Guida introduttiva con le API di protezione dati
author: rick-anderson
description: Questo documento viene illustrato come utilizzare le API di protezione dati ASP.NET Core per la protezione e la rimozione della protezione dati in un'applicazione.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: c6a631b6dc4a7855b11031dfcef42b17906754b0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="9ff48-103">Guida introduttiva con le API di protezione dati</span><span class="sxs-lookup"><span data-stu-id="9ff48-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="9ff48-104">Nei dati più semplici, protezione prevede i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9ff48-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="9ff48-105">Creare un dati protezione da un provider di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="9ff48-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="9ff48-106">Chiamare il `Protect` (metodo) con i dati da proteggere.</span><span class="sxs-lookup"><span data-stu-id="9ff48-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="9ff48-107">Chiamare il `Unprotect` (metodo) con i dati a cui si desidera riconvertire in testo normale.</span><span class="sxs-lookup"><span data-stu-id="9ff48-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="9ff48-108">La maggior parte delle strutture e modelli di app, ad esempio ASP.NET o SignalR, configurare il sistema di protezione dati già e aggiungerlo a un contenitore del servizio che accedere tramite l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="9ff48-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="9ff48-109">L'esempio seguente illustra la configurazione di un contenitore del servizio per l'inserimento di dipendenze e lo stack di protezione dati di registrazione, il provider di protezione dati tramite DI ricezione, creazione di una protezione e la protezione, quindi la rimozione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="9ff48-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="9ff48-110">Quando si crea un programma di protezione è necessario fornire uno o più [scopo stringhe](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="9ff48-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="9ff48-111">Una stringa scopo garantisce l'isolamento tra i consumer.</span><span class="sxs-lookup"><span data-stu-id="9ff48-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="9ff48-112">Ad esempio, un programma di protezione creato con una stringa a scopo di "green" sarebbe in grado di rimuovere la protezione dei dati forniti da una protezione con lo scopo di "viola".</span><span class="sxs-lookup"><span data-stu-id="9ff48-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="9ff48-113">Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per più i chiamanti.</span><span class="sxs-lookup"><span data-stu-id="9ff48-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="9ff48-114">È destinata che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata a `CreateProtector`, tale riferimento verrà utilizzato per più chiamate a `Protect` e `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="9ff48-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="9ff48-115">Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto può essere verificato o decifrato.</span><span class="sxs-lookup"><span data-stu-id="9ff48-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="9ff48-116">Alcuni componenti preferibile ignorare gli errori durante rimuovere la protezione di operazioni. un componente che legge i cookie di autenticazione potrebbe gestire questo errore e gestire la richiesta, come se non contenesse alcun cookie affatto anziché rifiuteranno la richiesta di definitiva.</span><span class="sxs-lookup"><span data-stu-id="9ff48-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="9ff48-117">Componenti che questo comportamento devono in particolare intercettare CryptographicException anziché integrare tutte le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="9ff48-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
