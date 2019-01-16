---
title: Usare il modello di progetto per React con Redux con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come iniziare a usare il modello di progetto per applicazioni a pagina singola di ASP.NET Core per React con Redux e create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341620"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="2e24b-103">Usare il modello di progetto per React con Redux con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e24b-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="2e24b-104">Questa documentazione non sono relativi al modello di progetto React con Redux incluso in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2e24b-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="2e24b-105">Fa riferimento al modello di React con Redux più recente che è possibile aggiornare manualmente.</span><span class="sxs-lookup"><span data-stu-id="2e24b-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="2e24b-106">Il modello è disponibile in ASP.NET Core 2.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="2e24b-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="2e24b-107">Il modello di progetto aggiornato per React con Redux fornisce un ottimo punto di partenza per le app ASP.NET Core che usano le convenzioni React, Redux e [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) per implementare un'interfaccia utente avanzata sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2e24b-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="2e24b-108">Fatta eccezione per il comando di creazione del progetto, tutte le informazioni relative al modello per React con Redux sono uguali a quelle relative al modello per React.</span><span class="sxs-lookup"><span data-stu-id="2e24b-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="2e24b-109">Per creare questo tipo di progetto, eseguire `dotnet new reactredux` anziché `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="2e24b-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="2e24b-110">Per altre informazioni sulle funzionalità comuni a entrambi i modelli basati su React, vedere la [documentazione del modello per React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="2e24b-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="2e24b-111">Per informazioni sulla configurazione di una sotto-applicazione React con Redux in IIS, vedere [per ReactRedux modello 2.1: Non è possibile utilizzare SPA in IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span><span class="sxs-lookup"><span data-stu-id="2e24b-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
