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
ms.openlocfilehash: a6bd81a4e5796c1d0a0033c2b8d5a6ba9282564c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction"></a>Introduzione

<a name="security-authorization-introduction"></a>

Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire. Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina. Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.

L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente. L'autenticazione può creare una o più identità per l'utente corrente.

## <a name="authorization-types"></a>Tipi di autorizzazioni

Autorizzazione ASP.NET Core fornisce una semplice dichiarativa [ruolo](roles.md) e [basata su criteri avanzati](policies.md) modello. Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti. Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.

## <a name="namespaces"></a>Spazi dei nomi

I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.
