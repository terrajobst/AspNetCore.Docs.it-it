---
title: Articoli basati su ASP.NET Core i progetti creati con singoli account utente
author: rick-anderson
description: Scopri articoli basati su ASP.NET Core i progetti creati con singoli account utente.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f9c1be16386da935382275815bb5fd5c72894b1c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892528"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articoli basati su ASP.NET Core i progetti creati con singoli account utente

ASP.NET Core Identity è incluso nei modelli di progetto in Visual Studio con l'opzione "Account utente individuali".

I modelli di autenticazione sono disponibili in .NET Core CLI con `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Visualizzare [questo problema su GitHub](https://github.com/aspnet/AspNetCore/issues/5833) per l'autenticazione di API web.

<a name="no"></a>

## <a name="no-authentication"></a>Nessuna autenticazione

Viene specificata l'autenticazione in .NET Core CLI con il `-au` opzione. In Visual Studio, il **Modifica autenticazione** finestra di dialogo è disponibile per le nuove applicazioni web. Il valore predefinito per la nuova App web in Visual Studio viene **Nessuna autenticazione**.

Progetti creati con nessuna autenticazione:

* Non contengono le pagine web e dell'interfaccia utente per accedere e disconnettersi.
* Non contengono codice di autenticazione.

<a name="win"></a>

## <a name="windows-authentication"></a>Autenticazione di Windows

Viene specificata l'autenticazione di Windows per le nuove app web in .NET Core CLI con il `-au Windows` opzione. In Visual Studio, il **Modifica autenticazione** finestra di dialogo offre la **l'autenticazione di Windows** opzioni.

Se si seleziona l'autenticazione di Windows, l'app è configurata per usare la [modulo di Windows autenticazione IIS](xref:host-and-deploy/iis/modules). L'autenticazione di Windows è progettato per i siti web Intranet.

## <a name="additional-resources"></a>Risorse aggiuntive

Gli articoli seguenti illustrano come usare il codice generato nei modelli di ASP.NET Core che usano singoli account utente:

* [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core](xref:security/authentication/accconfirm) (Conferma dell'account e recupero della password in ASP.NET Core)
* [Creare un'app ASP.NET Core con i dati utente protetti da autorizzazione](xref:security/authorization/secure-data)
