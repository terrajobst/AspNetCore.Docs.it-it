---
title: Articoli basati su ASP.NET Core i progetti creati con singoli account utente
author: rick-anderson
description: Individuare articoli basati su ASP.NET Core i progetti creati con singoli account utente.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274427"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articoli basati su ASP.NET Core i progetti creati con singoli account utente

Identità di ASP.NET Core è incluso nei modelli di progetto in Visual Studio con l'opzione "Singoli account utente".

I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Gli articoli seguenti viene illustrato come utilizzare il codice generato nei modelli ASP.NET di base che utilizzano i singoli account utente:

* [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)
* [Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione](xref:security/authorization/secure-data)
