---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="b81bc-103">Protezione dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione</span><span class="sxs-lookup"><span data-stu-id="b81bc-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="b81bc-104">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="b81bc-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="b81bc-105">Introduzione alle API di protezione dati</span><span class="sxs-lookup"><span data-stu-id="b81bc-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="b81bc-106">API utente</span><span class="sxs-lookup"><span data-stu-id="b81bc-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="b81bc-107">Panoramica di API utente</span><span class="sxs-lookup"><span data-stu-id="b81bc-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="b81bc-108">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="b81bc-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="b81bc-109">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="b81bc-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="b81bc-110">Hashing della password</span><span class="sxs-lookup"><span data-stu-id="b81bc-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="b81bc-111">Limitazione della durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="b81bc-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="b81bc-112">Rimozione della protezione di payload le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="b81bc-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="b81bc-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="b81bc-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="b81bc-114">Configurazione della protezione dati</span><span class="sxs-lookup"><span data-stu-id="b81bc-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="b81bc-115">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="b81bc-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="b81bc-116">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="b81bc-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="b81bc-117">Scenari non compatibili con DI</span><span class="sxs-lookup"><span data-stu-id="b81bc-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="b81bc-118">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="b81bc-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="b81bc-119">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="b81bc-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="b81bc-120">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="b81bc-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="b81bc-121">Varie API</span><span class="sxs-lookup"><span data-stu-id="b81bc-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="b81bc-122">Implementazione</span><span class="sxs-lookup"><span data-stu-id="b81bc-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="b81bc-123">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="b81bc-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="b81bc-124">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="b81bc-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="b81bc-125">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="b81bc-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="b81bc-126">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="b81bc-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="b81bc-127">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="b81bc-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="b81bc-128">Crittografia chiavi inattive</span><span class="sxs-lookup"><span data-stu-id="b81bc-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="b81bc-129">Immutabilità della chiave e modifica delle impostazioni</span><span class="sxs-lookup"><span data-stu-id="b81bc-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="b81bc-130">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="b81bc-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="b81bc-131">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="b81bc-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="b81bc-132">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="b81bc-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="b81bc-133">Condivisione dei cookie tra applicazioni</span><span class="sxs-lookup"><span data-stu-id="b81bc-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="b81bc-134">Sostituzione di <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b81bc-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
