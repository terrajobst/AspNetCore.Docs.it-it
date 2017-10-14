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
ms.openlocfilehash: fa6dcbc0627181cd1aca0926fa008f3db907742f
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="introduction"></a>Introduzione

<a name="security-authorization-introduction"></a>

Autorizzazione si intende il processo che determina a quali un utente è in grado di eseguire. Ad esempio, un utente amministratore è possibile creare una raccolta documenti, aggiungere documenti, modificare i documenti e li elimina. Un utente senza privilegi di amministratore che utilizza con la libreria è autorizzato solo leggere i documenti.

L'autorizzazione è ortogonali e indipendenti dal tipo di autenticazione, ovvero il processo di individuazione che un utente. L'autenticazione può creare una o più identità per l'utente corrente.

## <a name="authorization-types"></a>Tipi di autorizzazioni

Autorizzazione ASP.NET Core fornisce una semplice dichiarativa [ruolo](roles.md#security-authorization-role-based) e [basata su criteri avanzati](policies.md#security-authorization-policies-based) modello. Autorizzazione è espresso in requisiti e i gestori di valutare le attestazioni dell'utente rispetto ai requisiti. Controlli imperativi possono essere basati su criteri semplici o criteri di cui valutare l'identità dell'utente e le proprietà della risorsa che l'utente sta tentando di accedere.

## <a name="namespaces"></a>Spazi dei nomi

I componenti di autorizzazione, inclusi il `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.
