---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071696"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="bb589-103">Protezione dati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb589-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="bb589-104">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="bb589-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="bb589-105">Introduzione alle Data Protection API</span><span class="sxs-lookup"><span data-stu-id="bb589-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="bb589-106">API utente</span><span class="sxs-lookup"><span data-stu-id="bb589-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="bb589-107">Panoramica delle API consumer</span><span class="sxs-lookup"><span data-stu-id="bb589-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="bb589-108">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="bb589-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="bb589-109">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="bb589-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="bb589-110">Password con hash</span><span class="sxs-lookup"><span data-stu-id="bb589-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="bb589-111">Limitare la durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="bb589-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="bb589-112">Payload non protetti le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="bb589-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="bb589-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="bb589-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="bb589-114">Configurare la protezione dati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb589-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="bb589-115">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="bb589-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="bb589-116">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="bb589-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="bb589-117">Scenari non compatibili con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="bb589-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="bb589-118">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="bb589-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="bb589-119">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="bb589-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="bb589-120">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="bb589-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="bb589-121">Varie API</span><span class="sxs-lookup"><span data-stu-id="bb589-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="bb589-122">Implementazione</span><span class="sxs-lookup"><span data-stu-id="bb589-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="bb589-123">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="bb589-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="bb589-124">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="bb589-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="bb589-125">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="bb589-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="bb589-126">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="bb589-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="bb589-127">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="bb589-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="bb589-128">Crittografia a chiave inattiva</span><span class="sxs-lookup"><span data-stu-id="bb589-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="bb589-129">Immutabilità delle chiavi e impostazioni</span><span class="sxs-lookup"><span data-stu-id="bb589-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="bb589-130">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="bb589-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="bb589-131">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="bb589-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="bb589-132">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="bb589-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="bb589-133">Sostituzione di ASP.NET <machineKey> in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb589-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
