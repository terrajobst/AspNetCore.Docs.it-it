---
title: Esempi di autenticazione per ASP.NET Core
author: rick-anderson
description: Vengono forniti collegamenti agli esempi di autenticazione nel repository di ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897158"
---
# <a name="authentication-samples-for-aspnet-core"></a>Esempi di autenticazione per ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il [repository di ASP.NET Core](https://github.com/aspnet/AspNetCore) contiene i seguenti esempi di autenticazione nella *AspNetCore/src/Security/samples* cartella:

* [Trasformazione delle attestazioni](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticazione tramite cookie](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Provider di criteri personalizzati - IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opzioni e gli schemi di autenticazione dinamica](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Attestazioni esterno](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Selezione di un altro schema di autenticazione basate sulla richiesta e senza cookie](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Limita l'accesso ai file statici](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Eseguire gli esempi

* Selezionare una [ramo](https://github.com/aspnet/AspNetCore). Ad esempio, `release/2.2`.
* Clonare o scaricare il [repository di ASP.NET Core](https://github.com/aspnet/AspNetCore).
* Verificare di aver installato il [.NET Core SDK](https://www.microsoft.com/net/download/all) versione che corrisponde il clone del repository ASP.NET Core.
* Passare a un esempio nella *AspNetCore/src/Security/samples* ed eseguire l'esempio con `dotnet run`.
