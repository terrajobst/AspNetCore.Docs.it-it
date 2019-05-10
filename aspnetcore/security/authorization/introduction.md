---
title: Introduzione alle autorizzazioni in ASP.NET Core
author: rick-anderson
description: Informazioni di base del funzionamento dell'autorizzazione nelle App ASP.NET Core e l'autorizzazione.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896978"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introduzione alle autorizzazioni in ASP.NET Core

<a name="security-authorization-introduction"></a>

Autorizzazione si riferisce al processo che determina ciò che un utente è in grado di eseguire. Ad esempio, un utente amministratore è consentito per creare una raccolta documenti aggiungere i documenti, modificare i documenti ed eliminarli. Un utente non amministratore, utilizzare la libreria solo è autorizzato a leggere i documenti.

L'autorizzazione è ortogonale e indipendente dall'autenticazione. Tuttavia, autorizzazione, è necessario un meccanismo di autenticazione. L'autenticazione è il processo di accerta l'identità è un utente. L'autenticazione può creare una o più identità per l'utente corrente.

## <a name="authorization-types"></a>Tipi di autorizzazione

Autorizzazione di ASP.NET Core fornisce un semplice e dichiarativo [ruolo](xref:security/authorization/roles) e un ricco [basata su criteri](xref:security/authorization/policies) modello. Autorizzazione viene espresso in requisiti e i gestori di valutare le attestazioni dell'utente in base ai requisiti. Controlli imperativi possono basarsi su simple o criteri di cui valutano l'identità dell'utente e le proprietà della risorsa a cui l'utente sta provando ad accedere.

## <a name="namespaces"></a>Spazi dei nomi

I componenti di autorizzazione, inclusi i `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi, si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.

Consultare la documentazione sul [autorizzazione semplice](xref:security/authorization/simple).
