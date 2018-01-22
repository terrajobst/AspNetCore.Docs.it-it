---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="29378-103">Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione</span><span class="sxs-lookup"><span data-stu-id="29378-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="29378-104">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="29378-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="29378-105">Introduzione alle Data Protection API</span><span class="sxs-lookup"><span data-stu-id="29378-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="29378-106">API utente</span><span class="sxs-lookup"><span data-stu-id="29378-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="29378-107">Panoramica delle API consumer</span><span class="sxs-lookup"><span data-stu-id="29378-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="29378-108">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="29378-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="29378-109">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="29378-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="29378-110">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="29378-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="29378-111">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="29378-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="29378-112">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="29378-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="29378-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="29378-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="29378-114">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="29378-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="29378-115">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="29378-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="29378-116">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="29378-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="29378-117">Scenari non compatibili con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="29378-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="29378-118">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="29378-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="29378-119">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="29378-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="29378-120">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="29378-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="29378-121">Varie API</span><span class="sxs-lookup"><span data-stu-id="29378-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="29378-122">Implementazione</span><span class="sxs-lookup"><span data-stu-id="29378-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="29378-123">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="29378-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="29378-124">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="29378-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="29378-125">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="29378-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="29378-126">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="29378-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="29378-127">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="29378-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="29378-128">Crittografia a chiave inattiva</span><span class="sxs-lookup"><span data-stu-id="29378-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="29378-129">Immutabilità delle chiavi e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="29378-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="29378-130">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="29378-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="29378-131">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="29378-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="29378-132">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="29378-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="29378-133">Condivisione di cookie tra app</span><span class="sxs-lookup"><span data-stu-id="29378-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="29378-134">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="29378-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
