---
title: Usare i modelli di applicazione a pagina singola con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come installare e iniziare a usare i modelli di progetto per applicazioni a pagina singola di ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291444"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Usare i modelli di applicazione a pagina singola con ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> La versione rilasciata di .NET Core 2.0.x SDK include i modelli di progetto precedenti per Angular, React e React con Redux. Questa documentazione non riguarda i modelli di progetto precedenti. Questa documentazione si riferisce ai modelli Angular, React e React con Redux più recenti che possono essere installati manualmente in ASP.NET Core 2.0. I modelli sono inclusi per impostazione predefinita in ASP.NET Core 2.1.

::: moniker-end

## <a name="prerequisites"></a>Prerequisiti

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), versione 6 o successive

## <a name="installation"></a>Installazione

::: moniker range=">= aspnetcore-2.1"

I modelli sono già installati con ASP.NET Core 2.1.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Se è stato installato ASP.NET Core 2.0, eseguire il comando seguente per installare i modelli di ASP.NET Core aggiornati per Angular, React e React con Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Usare i modelli

* [Usare il modello di progetto per Angular](xref:spa/angular)
* [Usare il modello di progetto per React](xref:spa/react)
* [Usare il modello di progetto per React con Redux](xref:spa/react-with-redux)
