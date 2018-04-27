---
title: Introduzione alle autorizzazioni in ASP.NET Core
author: rick-anderson
description: Le nozioni di autorizzazione e come funziona l'autorizzazione in App ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introduzione alle autorizzazioni in ASP.NET Core

<a name="security-authorization-introduction"></a>

Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire. Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina. Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.

L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione. Autorizzazione richiede tuttavia un meccanismo di autenticazione. L'autenticazione è il processo di individuazione che un utente. L'autenticazione può creare una o più identità per l'utente corrente.

## <a name="authorization-types"></a>Tipi di autorizzazioni

Autorizzazione di ASP.NET Core offre un semplice dichiarativo [ruolo](xref:security/authorization/roles) e potente [basata su criteri](xref:security/authorization/policies) modello. Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti. Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.

## <a name="namespaces"></a>Spazi dei nomi

I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi, si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.

Consultare la documentazione su [autorizzazione semplice](xref:security/authorization/simple).
