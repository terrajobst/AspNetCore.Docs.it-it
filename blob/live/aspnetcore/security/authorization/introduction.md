---
title: Introduzione alle autorizzazioni
author: rick-anderson
description: Questo documento fornisce una descrizione generale di autorizzazione e illustra le correlazioni tra autorizzazione ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 6f4f1fb4f2776db10a1640049885e31e9a54011a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="introduction"></a><span data-ttu-id="b7243-103">Introduzione</span><span class="sxs-lookup"><span data-stu-id="b7243-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="b7243-104">Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="b7243-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="b7243-105">Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina.</span><span class="sxs-lookup"><span data-stu-id="b7243-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="b7243-106">Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="b7243-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="b7243-107">L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente.</span><span class="sxs-lookup"><span data-stu-id="b7243-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="b7243-108">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="b7243-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="b7243-109">Tipi di autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="b7243-109">Authorization Types</span></span>

<span data-ttu-id="b7243-110">Autorizzazione ASP.NET Core fornisce una semplice dichiarativa [ruolo](roles.md) e [basata su criteri avanzati](policies.md) modello.</span><span class="sxs-lookup"><span data-stu-id="b7243-110">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="b7243-111">Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="b7243-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="b7243-112">Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.</span><span class="sxs-lookup"><span data-stu-id="b7243-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="b7243-113">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="b7243-113">Namespaces</span></span>

<span data-ttu-id="b7243-114">I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b7243-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
