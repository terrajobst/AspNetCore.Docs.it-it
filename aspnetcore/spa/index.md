---
title: Usare i modelli di applicazione a pagina singola con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come installare e iniziare a usare i modelli di progetto per applicazioni a pagina singola di ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="e20ef-103">Usare i modelli di applicazione a pagina singola con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e20ef-103">Use the Single Page Application templates with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="e20ef-104">La versione rilasciata di .NET Core 2.0.x SDK include i modelli di progetto precedenti per Angular, React e React con Redux.</span><span class="sxs-lookup"><span data-stu-id="e20ef-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="e20ef-105">Questa documentazione non riguarda i modelli di progetto precedenti.</span><span class="sxs-lookup"><span data-stu-id="e20ef-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="e20ef-106">Questa documentazione si riferisce ai modelli Angular, React e React con Redux più recenti che possono essere installati manualmente in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e20ef-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="e20ef-107">I modelli sono inclusi per impostazione predefinita in ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e20ef-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e20ef-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e20ef-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="e20ef-109">[Node.js](https://nodejs.org), versione 6 o successive</span><span class="sxs-lookup"><span data-stu-id="e20ef-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="e20ef-110">Installazione</span><span class="sxs-lookup"><span data-stu-id="e20ef-110">Installation</span></span>

<span data-ttu-id="e20ef-111">Se è stato installato ASP.NET Core 2.0, eseguire il comando seguente per installare i modelli di ASP.NET Core aggiornati per Angular, React e React con Redux:</span><span class="sxs-lookup"><span data-stu-id="e20ef-111">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="e20ef-112">Usare i modelli</span><span class="sxs-lookup"><span data-stu-id="e20ef-112">Use the templates</span></span>

- [<span data-ttu-id="e20ef-113">Usare il modello di progetto per Angular</span><span class="sxs-lookup"><span data-stu-id="e20ef-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="e20ef-114">Usare il modello di progetto per React</span><span class="sxs-lookup"><span data-stu-id="e20ef-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="e20ef-115">Usare il modello di progetto per React con Redux</span><span class="sxs-lookup"><span data-stu-id="e20ef-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
