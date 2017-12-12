---
title: Introduzione alle autorizzazioni
author: rick-anderson
description: Questo documento fornisce una descrizione generale di autorizzazione e illustra le correlazioni tra autorizzazione ASP.NET Core.
keywords: ASP.NET Core, autorizzazioni
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a><span data-ttu-id="415bf-104">Introduzione</span><span class="sxs-lookup"><span data-stu-id="415bf-104">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="415bf-105">Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="415bf-105">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="415bf-106">Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina.</span><span class="sxs-lookup"><span data-stu-id="415bf-106">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="415bf-107">Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="415bf-107">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="415bf-108">L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente.</span><span class="sxs-lookup"><span data-stu-id="415bf-108">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="415bf-109">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="415bf-109">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="415bf-110">Tipi di autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="415bf-110">Authorization Types</span></span>

<span data-ttu-id="415bf-111">Autorizzazione ASP.NET Core fornisce una semplice dichiarativa [ruolo](roles.md) e [basata su criteri avanzati](policies.md) modello.</span><span class="sxs-lookup"><span data-stu-id="415bf-111">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="415bf-112">Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="415bf-112">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="415bf-113">Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.</span><span class="sxs-lookup"><span data-stu-id="415bf-113">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="415bf-114">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="415bf-114">Namespaces</span></span>

<span data-ttu-id="415bf-115">I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="415bf-115">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
