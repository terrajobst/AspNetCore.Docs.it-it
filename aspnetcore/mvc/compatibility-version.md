---
title: Versione di compatibilità per ASP.NET Core MVC
author: rick-anderson
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 9/25/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b29e2ee49aaf0f557f1acd0cf03e9e82d5ea0105
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667125"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Versione di compatibilità per ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Il metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> è un no-op per le app ASP.NET Core 3,0. Ovvero la chiamata di `SetCompatibilityVersion` con qualsiasi valore di <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion> non ha alcun effetto sull'applicazione.

* La versione secondaria successiva di ASP.NET Core può fornire un nuovo valore `CompatibilityVersion`.
* i valori `CompatibilityVersion` `Version_2_0` tramite `Version_2_2` sono contrassegnati come `[Obsolete(...)]`.
* Vedere la pagina relativa [alle modifiche delle API in antifalsificazione, CORS, diagnostica, MVC e routing](https://github.com/aspnet/Announcements/issues/387). Questo elenco include modifiche di rilievo per le opzioni di compatibilità.

Per verificare il funzionamento di `SetCompatibilityVersion` con le app ASP.NET Core 2. x, selezionare la [versione ASP.NET Core 2,2 di questo articolo](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Il metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> consente a un'app ASP.NET Core 2. x di acconsentire o rifiutare esplicitamente le modifiche del comportamento potenzialmente indesiderate introdotte in ASP.NET Core MVC 2,1 o 2,2. Queste modifiche potenzialmente importanti del comportamento riguardano in genere il funzionamento del sottosistema MVC e la modalità con cui il **codice dell'utente** viene chiamato dal runtime. Se si acconsente esplicitamente, si ottiene il comportamento più recente e il comportamento a lungo termine di ASP.NET Core.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.2:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

È consigliabile testare l'app usando la versione più recente (`CompatibilityVersion.Latest`). Microsoft prevede che la maggior parte delle app non riscontrerà modifiche importanti del comportamento quando viene usata la versione più recente.

Le app che chiamano `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sono protette da modifiche potenzialmente indesiderate introdotte nelle versioni ASP.NET Core 2.1/2.2 MVC. La protezione:

* Non è valida per tutte le modifiche 2.1 e successive, ma è destinata alle modifiche potenzialmente importanti del comportamento del runtime ASP.NET Core nel sottosistema MVC.
* Non si estende a ASP.NET Core 3,0.

La compatibilità predefinita per le app ASP.NET Core 2,1 e 2,2 che **non** chiamano `SetCompatibilityVersion` è la compatibilità 2,0. In altri termini il fatto di non chiamare `SetCompatibilityVersion` equivale a chiamare `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.2, con l'eccezione dei comportamenti seguenti:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Per le app che riscontrano modifiche potenzialmente importanti del comportamento, l'uso delle opzioni di compatibilità appropriate:

* Consente di usare la versione più recente e di rifiutare esplicitamente modifiche importanti del comportamento specifiche.
* Garantisce il tempo necessario per l'aggiornamento dell'app, che in tal modo potrà funzionare con le modifiche più recenti.

La documentazione di <xref:Microsoft.AspNetCore.Mvc.MvcOptions> offre una spiegazione chiara delle modifiche e dei motivi per cui queste rappresentano un miglioramento per la maggior parte degli utenti.

Con ASP.NET Core 3,0, i comportamenti precedenti supportati dalle opzioni di compatibilità sono stati rimossi. Queste modifiche positive andranno a vantaggio della maggior parte degli utenti. Introducendo queste modifiche nei 2,1 e 2,2, la maggior parte delle app può trarre vantaggio, mentre altre hanno tempo per l'aggiornamento.
::: moniker-end
