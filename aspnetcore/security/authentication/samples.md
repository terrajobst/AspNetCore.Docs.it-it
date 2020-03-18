---
title: Esempi di autenticazione per ASP.NET Core
author: rick-anderson
description: Fornisce collegamenti agli esempi di autenticazione nel repository ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511613"
---
# <a name="authentication-samples-for-aspnet-core"></a>Esempi di autenticazione per ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Il [repository ASP.NET Core](https://github.com/dotnet/AspNetCore) contiene gli esempi di autenticazione seguenti nella cartella *AspNetCore/SRC/Security/Samples* :

* [Trasformazione delle attestazioni](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Autenticazione cookie](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Provider di criteri personalizzati-IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Opzioni e schemi di autenticazione dinamici](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Attestazioni esterne](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [Selezione tra cookie e un altro schema di autenticazione in base alla richiesta](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Limita l'accesso ai file statici](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Eseguire gli esempi

* Selezionare un [ramo](https://github.com/dotnet/AspNetCore). Ad esempio, usare `Tag:v3.0.0`
* Clonare o scaricare il [repository ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Verificare di aver installato la versione di [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) corrispondente al clone del repository di ASP.NET Core.
* Passare a un esempio in *AspNetCore/SRC/Security/Samples* ed eseguire l'esempio con `dotnet run`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Il [repository ASP.NET Core](https://github.com/dotnet/AspNetCore) contiene gli esempi di autenticazione seguenti nella cartella *AspNetCore/SRC/Security/Samples* :

* [Trasformazione delle attestazioni](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticazione cookie](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Provider di criteri personalizzati-IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opzioni e schemi di autenticazione dinamici](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Attestazioni esterne](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Selezione tra cookie e un altro schema di autenticazione in base alla richiesta](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Limita l'accesso ai file statici](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Eseguire gli esempi

* Selezionare un [ramo](https://github.com/dotnet/AspNetCore). Ad esempio, usare `release/2.2`
* Clonare o scaricare il [repository ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Verificare di aver installato la versione di [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) corrispondente al clone del repository di ASP.NET Core.
* Passare a un esempio in *AspNetCore/SRC/Security/Samples* ed eseguire l'esempio con `dotnet run`.

::: moniker-end
