---
title: Scegliere tra ASP.NET 4.x e ASP.NET Core
author: rick-anderson
description: Vengono illustrati ASP.NET Core e ASP.NET 4.x e si spiega come scegliere tra le due soluzioni.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 454f1021520f8f22eb2b0417a958b78690f89cef
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64886966"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Scegliere tra ASP.NET 4.x e ASP.NET Core

ASP.NET Core è una riprogettazione di ASP.NET 4.x. Questo articolo elenca le differenze tra i due.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne app Web basate sul cloud in Windows, Mac OS o Linux.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x è un framework consolidato che offre i servizi necessari per la compilazione di app Web di livello aziendale basate su server in Windows.

## <a name="framework-selection"></a>Selezione del framework

La tabella seguente mette a confronto ASP.NET Core e ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Compilare per Windows, Mac OS o Linux|Compilare per Windows|
|[Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x. Vedere anche [MVC](xref:mvc/overview), [API Web](xref:tutorials/first-web-api) e [SignalR](xref:signalr/introduction).|Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Pagine Web](/aspnet/web-pages)|
|Più versioni per computer|Una versione per computer|
|Sviluppare con Visual Studio, [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#|Sviluppare con Visual Studio tramite C#, VB o F#|
|Prestazioni più elevate rispetto ad ASP.NET 4.x|Buone prestazioni|
|[Scegliere .NET Framework o Runtime di .NET Core](/dotnet/standard/choosing-core-framework-server)|Usare runtime .NET Framework|

Vedere [ASP.NET Core per .NET Framework](xref:index#target-framework) per informazioni sul supporto per ASP.NET Core 2.x in .NET Framework.

## <a name="aspnet-core-scenarios"></a>Scenari ASP.NET Core

* [Razor Pages](xref:razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web da ASP.NET Core 2.x.
* [Siti Web](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [In tempo reale](xref:signalr/index)
* [Distribuire un'app ASP.NET Core in Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>Scenari ASP.NET 4.x

* [Siti Web](/aspnet/mvc)
* [API](/aspnet/web-api)
* [In tempo reale](/aspnet/signalr)
* [Creare un'app Web ASP.NET 4.x in Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Introduzione ad ASP.NET](/aspnet/overview)
* [Introduzione a ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>
