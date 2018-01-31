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
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="cc910-103">Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione</span><span class="sxs-lookup"><span data-stu-id="cc910-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="cc910-104">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="cc910-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="cc910-105">Introduzione alle Data Protection API</span><span class="sxs-lookup"><span data-stu-id="cc910-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="cc910-106">API utente</span><span class="sxs-lookup"><span data-stu-id="cc910-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="cc910-107">Panoramica delle API consumer</span><span class="sxs-lookup"><span data-stu-id="cc910-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="cc910-108">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="cc910-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="cc910-109">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="cc910-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="cc910-110">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="cc910-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="cc910-111">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="cc910-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="cc910-112">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="cc910-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="cc910-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="cc910-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="cc910-114">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="cc910-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="cc910-115">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="cc910-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="cc910-116">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="cc910-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="cc910-117">Scenari non compatibili con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="cc910-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="cc910-118">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="cc910-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="cc910-119">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="cc910-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="cc910-120">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="cc910-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="cc910-121">Varie API</span><span class="sxs-lookup"><span data-stu-id="cc910-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="cc910-122">Implementazione</span><span class="sxs-lookup"><span data-stu-id="cc910-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="cc910-123">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="cc910-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="cc910-124">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="cc910-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="cc910-125">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="cc910-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="cc910-126">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="cc910-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="cc910-127">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="cc910-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="cc910-128">Crittografia a chiave inattiva</span><span class="sxs-lookup"><span data-stu-id="cc910-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="cc910-129">Immutabilità delle chiavi e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="cc910-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="cc910-130">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="cc910-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="cc910-131">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="cc910-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="cc910-132">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="cc910-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="cc910-133">Condivisione di cookie tra le app</span><span class="sxs-lookup"><span data-stu-id="cc910-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="cc910-134">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cc910-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
