---
title: Versione di compatibilità per ASP.NET Core MVC
author: rick-anderson
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 7c4189db435088e0803b35add82fa0eb9372e664
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410145"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>Versione di compatibilità per ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> consente a un'app di acconsentire o rifiutare esplicitamente modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core MVC 2.1 o versioni successive. Queste modifiche potenzialmente importanti del comportamento riguardano in genere il funzionamento del sottosistema MVC e la modalità con cui il **codice dell'utente** viene chiamato dal runtime. Se si acconsente esplicitamente, si ottiene il comportamento più recente e il comportamento a lungo termine di ASP.NET Core.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.2:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

È consigliabile testare l'app usando la versione più recente (`CompatibilityVersion.Version_2_2`). Microsoft prevede che la maggior parte delle app non riscontrerà modifiche importanti del comportamento quando viene usata la versione più recente.

Le app che chiamano `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sono protette dalle modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core 2.1 MVC e versioni 2.x successive. La protezione:

* Non è valida per tutte le modifiche 2.1 e successive, ma è destinata alle modifiche potenzialmente importanti del comportamento del runtime ASP.NET Core nel sottosistema MVC.
* Non viene estesa alla versione principale successiva.

La compatibilità predefinita per le app ASP.NET Core 2.1 e versioni 2.x successive che **non** chiamano `SetCompatibilityVersion` è la compatibilità 2.0. In altri termini il fatto di non chiamare `SetCompatibilityVersion` equivale a chiamare `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.2, con l'eccezione dei comportamenti seguenti:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Per le app che riscontrano modifiche potenzialmente importanti del comportamento, l'uso delle opzioni di compatibilità appropriate:

* Consente di usare la versione più recente e di rifiutare esplicitamente modifiche importanti del comportamento specifiche.
* Garantisce il tempo necessario per l'aggiornamento dell'app, che in tal modo potrà funzionare con le modifiche più recenti.

I commenti relativi alla classe di origine [MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) offrono una spiegazione chiara delle modifiche e dei motivi per cui queste rappresentano un miglioramento per la maggior parte degli utenti.

In una data futura verrà rilasciata la [versione ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap). I comportamenti precedenti supportati dalle opzioni di compatibilità verranno rimossi nella versione 3.0. Queste modifiche positive andranno a vantaggio della maggior parte degli utenti. Se si introducono ora queste modifiche, la maggior parte delle app può trarne vantaggio da subito e sarà disponibile tempo sufficiente per aggiornare le altre applicazioni.
