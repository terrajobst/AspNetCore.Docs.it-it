---
title: Introduzione
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a><span data-ttu-id="e6dd7-103">Introduzione</span><span class="sxs-lookup"><span data-stu-id="e6dd7-103">Introduction</span></span>

<a name=security-authorization-introduction></a>

<span data-ttu-id="e6dd7-104">Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="e6dd7-105">Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="e6dd7-106">Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="e6dd7-107">L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="e6dd7-108">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="e6dd7-109">Tipi di autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="e6dd7-109">Authorization Types</span></span>

<span data-ttu-id="e6dd7-110">Autorizzazione ASP.NET Core fornisce una semplice dichiarativa [ruolo](roles.md#security-authorization-role-based) e [basata su criteri avanzati](policies.md#security-authorization-policies-based) modello.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="e6dd7-111">Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="e6dd7-112">Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="e6dd7-113">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="e6dd7-113">Namespaces</span></span>

<span data-ttu-id="e6dd7-114">I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e6dd7-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
