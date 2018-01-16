---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
keywords: ASP.NET Core,protezione dati
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="85df5-104">Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione</span><span class="sxs-lookup"><span data-stu-id="85df5-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="85df5-105">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="85df5-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="85df5-106">Introduzione alle Data Protection API</span><span class="sxs-lookup"><span data-stu-id="85df5-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="85df5-107">API utente</span><span class="sxs-lookup"><span data-stu-id="85df5-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="85df5-108">Panoramica delle API consumer</span><span class="sxs-lookup"><span data-stu-id="85df5-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="85df5-109">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="85df5-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="85df5-110">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="85df5-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="85df5-111">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="85df5-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="85df5-112">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="85df5-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="85df5-113">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="85df5-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="85df5-114">Configurazione</span><span class="sxs-lookup"><span data-stu-id="85df5-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="85df5-115">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="85df5-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="85df5-116">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="85df5-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="85df5-117">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="85df5-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="85df5-118">Scenari non compatibili con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="85df5-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="85df5-119">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="85df5-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="85df5-120">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="85df5-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="85df5-121">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="85df5-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="85df5-122">Varie API</span><span class="sxs-lookup"><span data-stu-id="85df5-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="85df5-123">Implementazione</span><span class="sxs-lookup"><span data-stu-id="85df5-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="85df5-124">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="85df5-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="85df5-125">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="85df5-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="85df5-126">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="85df5-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="85df5-127">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="85df5-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="85df5-128">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="85df5-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="85df5-129">Crittografia a chiave inattiva</span><span class="sxs-lookup"><span data-stu-id="85df5-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="85df5-130">Immutabilità delle chiavi e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="85df5-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="85df5-131">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="85df5-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="85df5-132">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="85df5-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="85df5-133">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="85df5-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="85df5-134">Condivisione di cookie tra app</span><span class="sxs-lookup"><span data-stu-id="85df5-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="85df5-135">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="85df5-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
