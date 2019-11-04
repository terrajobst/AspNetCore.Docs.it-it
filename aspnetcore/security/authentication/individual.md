---
title: Articoli basati su progetti ASP.NET Core creati con account utente singoli
author: rick-anderson
description: Scopri gli articoli in base ai progetti ASP.NET Core creati con singoli account utente.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463028"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articoli basati su progetti ASP.NET Core creati con account utente singoli

ASP.NET Core identità è inclusa nei modelli di progetto di Visual Studio con l'opzione "singoli account utente".

I modelli di autenticazione sono disponibili in interfaccia della riga di comando di .NET Core con `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore/issues/5833) per l'autenticazione dell'API Web.

<a name="no"></a>

## <a name="no-authentication"></a>Nessuna autenticazione

L'autenticazione viene specificata nella interfaccia della riga di comando di .NET Core con l'opzione `-au`. In Visual Studio la finestra di dialogo **Modifica autenticazione** è disponibile per le nuove applicazioni Web. Il valore predefinito per le nuove app Web in Visual Studio **non è Authentication**.

Progetti creati senza autenticazione:

* Non contenere pagine Web e interfaccia utente per l'accesso e la disconnessione.
* Non contiene codice di autenticazione.

<a name="win"></a>

## <a name="windows-authentication"></a>Autenticazione di Windows

Viene specificata l'autenticazione di Windows per le nuove app Web nella interfaccia della riga di comando di .NET Core con l'opzione `-au Windows`. In Visual Studio la finestra di dialogo **Cambia autenticazione** fornisce le opzioni di **autenticazione di Windows** .

Se è selezionata l'autenticazione di Windows, l'app viene configurata per l'uso del [modulo IIS](xref:host-and-deploy/iis/modules)per l'autenticazione di Windows. L'autenticazione di Windows è destinata ai siti Web Intranet.

## <a name="additional-resources"></a>Risorse aggiuntive

Gli articoli seguenti illustrano come usare il codice generato in ASP.NET Core modelli che usano singoli account utente:

* [Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)
* [Creare un'app ASP.NET Core con i dati utente protetti dall'autorizzazione](xref:security/authorization/secure-data)
