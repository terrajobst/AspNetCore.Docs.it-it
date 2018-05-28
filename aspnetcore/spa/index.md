---
title: Usare i modelli di applicazione a pagina singola con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come installare e iniziare a usare i modelli di progetto per applicazioni a pagina singola di ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="7eb1c-103">Usare i modelli di applicazione a pagina singola con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7eb1c-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="7eb1c-104">La versione rilasciata di .NET Core 2.0.x SDK include i modelli di progetto precedenti per Angular, React e React con Redux.</span><span class="sxs-lookup"><span data-stu-id="7eb1c-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="7eb1c-105">Questa documentazione non riguarda i modelli di progetto precedenti.</span><span class="sxs-lookup"><span data-stu-id="7eb1c-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="7eb1c-106">Questa documentazione si riferisce ai modelli Angular, React e React con Redux più recenti che possono essere installati manualmente in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7eb1c-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="7eb1c-107">I modelli sono inclusi per impostazione predefinita in ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7eb1c-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="7eb1c-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7eb1c-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="7eb1c-109">[Node.js](https://nodejs.org), versione 6 o successive</span><span class="sxs-lookup"><span data-stu-id="7eb1c-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="7eb1c-110">Installazione</span><span class="sxs-lookup"><span data-stu-id="7eb1c-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7eb1c-111">I modelli sono già installati con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7eb1c-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7eb1c-112">Se è stato installato ASP.NET Core 2.0, eseguire il comando seguente per installare i modelli di ASP.NET Core aggiornati per Angular, React e React con Redux:</span><span class="sxs-lookup"><span data-stu-id="7eb1c-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="7eb1c-113">Usare i modelli</span><span class="sxs-lookup"><span data-stu-id="7eb1c-113">Use the templates</span></span>

* [<span data-ttu-id="7eb1c-114">Usare il modello di progetto per Angular</span><span class="sxs-lookup"><span data-stu-id="7eb1c-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="7eb1c-115">Usare il modello di progetto per React</span><span class="sxs-lookup"><span data-stu-id="7eb1c-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="7eb1c-116">Usare il modello di progetto per React con Redux</span><span class="sxs-lookup"><span data-stu-id="7eb1c-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
