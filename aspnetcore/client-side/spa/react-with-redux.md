---
title: Usare il modello di progetto per React con Redux con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come iniziare a usare il modello di progetto per applicazioni a pagina singola di ASP.NET Core per React con Redux e create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291484"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="94b52-103">Usare il modello di progetto per React con Redux con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94b52-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="94b52-104">Questa documentazione non riguarda il modello di progetto per React con Redux incluso in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="94b52-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="94b52-105">È relativa al modello per React con Redux più recente, a cui è possibile eseguire l'aggiornamento manualmente.</span><span class="sxs-lookup"><span data-stu-id="94b52-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="94b52-106">Il modello è incluso in ASP.NET Core 2.1 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="94b52-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="94b52-107">Il modello di progetto aggiornato per React con Redux fornisce un ottimo punto di partenza per le app ASP.NET Core che usano le convenzioni React, Redux e [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) per implementare un'interfaccia utente avanzata sul lato client.</span><span class="sxs-lookup"><span data-stu-id="94b52-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="94b52-108">Fatta eccezione per il comando di creazione del progetto, tutte le informazioni relative al modello per React con Redux sono uguali a quelle relative al modello per React.</span><span class="sxs-lookup"><span data-stu-id="94b52-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="94b52-109">Per creare questo tipo di progetto, eseguire `dotnet new reactredux` anziché `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="94b52-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="94b52-110">Per altre informazioni sulle funzionalità comuni a entrambi i modelli basati su React, vedere la [documentazione del modello per React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="94b52-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
