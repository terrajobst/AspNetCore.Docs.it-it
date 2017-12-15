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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="5effd-104">Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione</span><span class="sxs-lookup"><span data-stu-id="5effd-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="5effd-105">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="5effd-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="5effd-106">Introduzione alle API di protezione dati</span><span class="sxs-lookup"><span data-stu-id="5effd-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="5effd-107">API utente</span><span class="sxs-lookup"><span data-stu-id="5effd-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="5effd-108">Panoramica di API utente</span><span class="sxs-lookup"><span data-stu-id="5effd-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="5effd-109">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="5effd-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="5effd-110">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="5effd-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="5effd-111">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="5effd-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="5effd-112">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="5effd-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="5effd-113">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="5effd-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="5effd-114">Configurazione</span><span class="sxs-lookup"><span data-stu-id="5effd-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="5effd-115">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="5effd-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="5effd-116">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="5effd-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="5effd-117">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="5effd-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="5effd-118">Scenari non compatibili con DI</span><span class="sxs-lookup"><span data-stu-id="5effd-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="5effd-119">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="5effd-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="5effd-120">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="5effd-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="5effd-121">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="5effd-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="5effd-122">Varie API</span><span class="sxs-lookup"><span data-stu-id="5effd-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="5effd-123">Implementazione</span><span class="sxs-lookup"><span data-stu-id="5effd-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="5effd-124">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="5effd-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="5effd-125">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="5effd-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="5effd-126">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="5effd-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="5effd-127">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="5effd-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="5effd-128">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="5effd-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="5effd-129">Crittografia chiavi inattive</span><span class="sxs-lookup"><span data-stu-id="5effd-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="5effd-130">Immutabilità della chiave e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="5effd-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="5effd-131">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="5effd-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="5effd-132">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="5effd-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="5effd-133">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="5effd-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="5effd-134">Condivisione dei cookie tra applicazioni</span><span class="sxs-lookup"><span data-stu-id="5effd-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="5effd-135">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5effd-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
