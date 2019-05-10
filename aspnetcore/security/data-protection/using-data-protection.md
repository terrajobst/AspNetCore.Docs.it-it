---
title: Introduzione a Data Protection API in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare le API di protezione dati ASP.NET Core per la protezione e rimozione della protezione dei dati in un'app.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087643"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="dc931-103">Introduzione a Data Protection API in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc931-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="dc931-104">Nei più semplici, la protezione dei dati prevede i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dc931-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="dc931-105">Creare un dati protezione da un provider di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="dc931-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="dc931-106">Chiamare il `Protect` metodo con i dati da proteggere.</span><span class="sxs-lookup"><span data-stu-id="dc931-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="dc931-107">Chiamare il `Unprotect` metodo con i dati a cui si desidera riconvertire in testo normale.</span><span class="sxs-lookup"><span data-stu-id="dc931-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="dc931-108">La maggior parte dei Framework e modelli di app, ad esempio ASP.NET Core o SignalR, configurare il sistema di protezione dati già e aggiungerlo a un contenitore del servizio che accedere tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dc931-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="dc931-109">L'esempio seguente viene illustrata la configurazione di un contenitore del servizio per l'inserimento delle dipendenze e la registrazione lo stack di protezione dati, riceve il provider di protezione dati tramite l'inserimento delle dipendenze, creazione di una protezione e rimozione della protezione e la protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="dc931-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="dc931-110">Quando si crea un programma di protezione è necessario fornire uno o più [stringhe di scopi](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="dc931-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="dc931-111">Una stringa scopo fornisce isolamento tra i consumer.</span><span class="sxs-lookup"><span data-stu-id="dc931-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="dc931-112">Ad esempio, una protezione creata con una stringa di utilizzo generico di "green" sarebbe in grado di rimuovere la protezione dei dati forniti da una protezione con uno scopo della "viola".</span><span class="sxs-lookup"><span data-stu-id="dc931-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="dc931-113">Le istanze di `IDataProtectionProvider` e `IDataProtector` sono thread-safe per i chiamanti di più.</span><span class="sxs-lookup"><span data-stu-id="dc931-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="dc931-114">È destinato che una volta che un componente ottiene un riferimento a un `IDataProtector` tramite una chiamata verso `CreateProtector`, che fanno riferimento a verranno utilizzate per più chiamate a `Protect` e `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="dc931-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="dc931-115">Una chiamata a `Unprotect` genererà CryptographicException se il payload protetto non può essere verificato o decifrato.</span><span class="sxs-lookup"><span data-stu-id="dc931-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="dc931-116">Alcuni componenti può essere utile ignorare gli errori durante rimuovere la protezione di operazioni; un componente che legge i cookie di autenticazione può gestire questo errore e trattare la richiesta come se non avesse alcun cookie affatto anziché Nega la richiesta direttamente.</span><span class="sxs-lookup"><span data-stu-id="dc931-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="dc931-117">I componenti che questo comportamento devono in particolare intercettare CryptographicException invece di assorbimento di tutte le eccezioni.</span><span class="sxs-lookup"><span data-stu-id="dc931-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
