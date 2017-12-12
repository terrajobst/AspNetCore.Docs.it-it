---
title: Articoli basati su progetti creati con singoli account utente
author: rick-anderson
description: Questo documento elenca articoli basati su progetti creati con singoli account utente.
keywords: ASP.NET Core, autorizzazione, IAuthorizationService
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="be645-104">Articoli basati su progetti creati con singoli account utente</span><span class="sxs-lookup"><span data-stu-id="be645-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="be645-105">Identità di ASP.NET Core è incluso nei modelli di progetto in Visual Studio con l'opzione "Singoli account utente".</span><span class="sxs-lookup"><span data-stu-id="be645-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="be645-106">I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="be645-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="be645-107">Gli articoli seguenti viene illustrato come utilizzare il codice generato nei modelli ASP.NET di base che utilizzano i singoli account utente:</span><span class="sxs-lookup"><span data-stu-id="be645-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="be645-108">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="be645-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* <span data-ttu-id="be645-109">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="be645-109">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm)</span></span>
* [<span data-ttu-id="be645-110">Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="be645-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)