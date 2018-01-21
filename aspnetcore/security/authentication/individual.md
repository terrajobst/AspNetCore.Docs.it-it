---
title: Articoli basati su progetti creati con singoli account utente
author: rick-anderson
description: Questo documento elenca articoli basati su progetti creati con singoli account utente.
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Articoli basati su progetti creati con singoli account utente

Identità di ASP.NET Core è incluso nei modelli di progetto in Visual Studio con l'opzione "Singoli account utente".

I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Gli articoli seguenti viene illustrato come utilizzare il codice generato nei modelli ASP.NET di base che utilizzano i singoli account utente:

* [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)
* [Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione](xref:security/authorization/secure-data)
