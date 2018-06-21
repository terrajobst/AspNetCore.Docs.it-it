---
title: Protezione dati in ASP.NET Core
author: rick-anderson
description: Questo documento funge da sommario per vari argomenti sulla protezione dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277124"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="eafde-103">Protezione dati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eafde-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="eafde-104">Introduzione alla protezione dati</span><span class="sxs-lookup"><span data-stu-id="eafde-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="eafde-105">Introduzione alle Data Protection API</span><span class="sxs-lookup"><span data-stu-id="eafde-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="eafde-106">API utente</span><span class="sxs-lookup"><span data-stu-id="eafde-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="eafde-107">Panoramica delle API consumer</span><span class="sxs-lookup"><span data-stu-id="eafde-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="eafde-108">Stringhe di scopi</span><span class="sxs-lookup"><span data-stu-id="eafde-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="eafde-109">Gerarchia di scopi e multi-tenancy</span><span class="sxs-lookup"><span data-stu-id="eafde-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="eafde-110">Password con hash</span><span class="sxs-lookup"><span data-stu-id="eafde-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="eafde-111">Limitare la durata di payload protetti</span><span class="sxs-lookup"><span data-stu-id="eafde-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="eafde-112">Payload non protetti le cui chiavi sono state revocate</span><span class="sxs-lookup"><span data-stu-id="eafde-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="eafde-113">Configurazione</span><span class="sxs-lookup"><span data-stu-id="eafde-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="eafde-114">Configurare la protezione dati di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eafde-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="eafde-115">Impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="eafde-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="eafde-116">Criteri a livello di computer</span><span class="sxs-lookup"><span data-stu-id="eafde-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="eafde-117">Scenari non compatibili con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="eafde-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="eafde-118">API di estendibilità</span><span class="sxs-lookup"><span data-stu-id="eafde-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="eafde-119">Estendibilità della crittografia Core</span><span class="sxs-lookup"><span data-stu-id="eafde-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="eafde-120">Estendibilità della gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="eafde-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="eafde-121">Varie API</span><span class="sxs-lookup"><span data-stu-id="eafde-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="eafde-122">Implementazione</span><span class="sxs-lookup"><span data-stu-id="eafde-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="eafde-123">Dettagli di crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="eafde-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="eafde-124">Derivazione di sottochiavi e crittografia autenticata</span><span class="sxs-lookup"><span data-stu-id="eafde-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="eafde-125">Intestazioni di contesto</span><span class="sxs-lookup"><span data-stu-id="eafde-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="eafde-126">Gestione delle chiavi</span><span class="sxs-lookup"><span data-stu-id="eafde-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="eafde-127">Provider di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="eafde-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="eafde-128">Crittografia a chiave inattiva</span><span class="sxs-lookup"><span data-stu-id="eafde-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="eafde-129">Immutabilità delle chiavi e impostazioni</span><span class="sxs-lookup"><span data-stu-id="eafde-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="eafde-130">Formato di archiviazione chiavi</span><span class="sxs-lookup"><span data-stu-id="eafde-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="eafde-131">Provider di protezione dati temporanea</span><span class="sxs-lookup"><span data-stu-id="eafde-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="eafde-132">Compatibilità</span><span class="sxs-lookup"><span data-stu-id="eafde-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="eafde-133">Sostituzione di ASP.NET <machineKey> in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eafde-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
