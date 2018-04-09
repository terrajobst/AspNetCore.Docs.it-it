---
title: Articoli basati su ASP.NET Core i progetti creati con singoli account utente
author: rick-anderson
description: Individuare articoli basati su ASP.NET Core i progetti creati con singoli account utente.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="ffef7-103">Articoli basati su ASP.NET Core i progetti creati con singoli account utente</span><span class="sxs-lookup"><span data-stu-id="ffef7-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="ffef7-104">Identità di ASP.NET Core è incluso nei modelli di progetto in Visual Studio con l'opzione "Singoli account utente".</span><span class="sxs-lookup"><span data-stu-id="ffef7-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="ffef7-105">I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="ffef7-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="ffef7-106">Gli articoli seguenti viene illustrato come utilizzare il codice generato nei modelli ASP.NET di base che utilizzano i singoli account utente:</span><span class="sxs-lookup"><span data-stu-id="ffef7-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="ffef7-107">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="ffef7-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* <span data-ttu-id="ffef7-108">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ffef7-108">[Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm)</span></span>
* [<span data-ttu-id="ffef7-109">Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="ffef7-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
