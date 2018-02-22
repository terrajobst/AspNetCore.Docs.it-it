---
title: Introduzione alle autorizzazioni
author: rick-anderson
description: Questo documento fornisce una descrizione generale di autorizzazione e illustra le correlazioni tra autorizzazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a><span data-ttu-id="4356b-103">Introduzione</span><span class="sxs-lookup"><span data-stu-id="4356b-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="4356b-104">Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="4356b-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="4356b-105">Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina.</span><span class="sxs-lookup"><span data-stu-id="4356b-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="4356b-106">Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="4356b-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="4356b-107">L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente.</span><span class="sxs-lookup"><span data-stu-id="4356b-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="4356b-108">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="4356b-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="4356b-109">Tipi di autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="4356b-109">Authorization types</span></span>

<span data-ttu-id="4356b-110">Autorizzazione di ASP.NET Core offre un semplice dichiarativo [ruolo](roles.md) e potente [basata su criteri](policies.md) modello.</span><span class="sxs-lookup"><span data-stu-id="4356b-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="4356b-111">Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="4356b-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="4356b-112">Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.</span><span class="sxs-lookup"><span data-stu-id="4356b-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="4356b-113">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="4356b-113">Namespaces</span></span>

<span data-ttu-id="4356b-114">I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi, si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4356b-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="4356b-115">Consultare la documentazione su [autorizzazione semplice](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="4356b-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
