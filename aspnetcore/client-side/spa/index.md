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
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="5df96-103">Usare i modelli di applicazione a pagina singola con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5df96-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="5df96-104">La versione rilasciata di .NET Core 2.0.x SDK include i modelli di progetto precedenti per Angular, React e React con Redux.</span><span class="sxs-lookup"><span data-stu-id="5df96-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="5df96-105">Questa documentazione non riguarda i modelli di progetto precedenti.</span><span class="sxs-lookup"><span data-stu-id="5df96-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="5df96-106">Questa documentazione si riferisce ai modelli Angular, React e React con Redux più recenti che possono essere installati manualmente in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5df96-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="5df96-107">I modelli sono inclusi per impostazione predefinita in ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5df96-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="5df96-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5df96-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="5df96-109">[Node.js](https://nodejs.org), versione 6 o successive</span><span class="sxs-lookup"><span data-stu-id="5df96-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="5df96-110">Installazione</span><span class="sxs-lookup"><span data-stu-id="5df96-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5df96-111">I modelli sono già installati con ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5df96-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5df96-112">Se è stato installato ASP.NET Core 2.0, eseguire il comando seguente per installare i modelli di ASP.NET Core aggiornati per Angular, React e React con Redux:</span><span class="sxs-lookup"><span data-stu-id="5df96-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="5df96-113">Usare i modelli</span><span class="sxs-lookup"><span data-stu-id="5df96-113">Use the templates</span></span>

* [<span data-ttu-id="5df96-114">Usare il modello di progetto per Angular</span><span class="sxs-lookup"><span data-stu-id="5df96-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="5df96-115">Usare il modello di progetto per React</span><span class="sxs-lookup"><span data-stu-id="5df96-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="5df96-116">Usare il modello di progetto per React con Redux</span><span class="sxs-lookup"><span data-stu-id="5df96-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
