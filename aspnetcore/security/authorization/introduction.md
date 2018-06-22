---
title: Introduzione alle autorizzazioni in ASP.NET Core
author: rick-anderson
description: Le nozioni di autorizzazione e come funziona l'autorizzazione in App ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276867"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="77afa-103">Introduzione alle autorizzazioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77afa-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="77afa-104">Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="77afa-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="77afa-105">Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina.</span><span class="sxs-lookup"><span data-stu-id="77afa-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="77afa-106">Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="77afa-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="77afa-107">L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="77afa-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="77afa-108">Autorizzazione richiede tuttavia un meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="77afa-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="77afa-109">L'autenticazione è il processo di individuazione che un utente.</span><span class="sxs-lookup"><span data-stu-id="77afa-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="77afa-110">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="77afa-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="77afa-111">Tipi di autorizzazioni</span><span class="sxs-lookup"><span data-stu-id="77afa-111">Authorization types</span></span>

<span data-ttu-id="77afa-112">Autorizzazione di ASP.NET Core offre un semplice dichiarativo [ruolo](xref:security/authorization/roles) e potente [basata su criteri](xref:security/authorization/policies) modello.</span><span class="sxs-lookup"><span data-stu-id="77afa-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="77afa-113">Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="77afa-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="77afa-114">Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.</span><span class="sxs-lookup"><span data-stu-id="77afa-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="77afa-115">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="77afa-115">Namespaces</span></span>

<span data-ttu-id="77afa-116">I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi, si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="77afa-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="77afa-117">Consultare la documentazione su [autorizzazione semplice](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="77afa-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
