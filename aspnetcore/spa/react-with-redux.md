---
title: Utilizzare il modello di progetto React-con-il ritorno con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come iniziare con il modello di progetto ASP.NET Core singolo pagina applicazione (SPA) per React con Redux e creare app di react.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="0446d-103">Utilizzare il modello di progetto React-con-il ritorno con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0446d-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="0446d-104">Sul modello di progetto React-con-il ritorno della presente documentazione non è incluso in ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="0446d-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="0446d-105">È sul modello di reazione-con-il ritorno più recente a cui è possibile aggiornare manualmente.</span><span class="sxs-lookup"><span data-stu-id="0446d-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="0446d-106">Per impostazione predefinita, il modello è incluso in ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0446d-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="0446d-107">Il modello di progetto aggiornato React-con-Redux fornisce un punto di partenza ideale per le applicazioni ASP.NET Core utilizzando reagiscono, Redux, e [creare app di reazione](https://github.com/facebookincubator/create-react-app) convenzioni (CRA) per implementare un'interfaccia utente avanzata, sul lato client (UI).</span><span class="sxs-lookup"><span data-stu-id="0446d-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="0446d-108">Fatta eccezione per il comando di creazione del progetto, tutte le informazioni relative al modello React-con-il ritorno sono lo stesso come il modello React.</span><span class="sxs-lookup"><span data-stu-id="0446d-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="0446d-109">Per creare questo tipo di progetto, eseguire `dotnet new reactredux` anziché `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="0446d-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="0446d-110">Per ulteriori informazioni sulle funzionalità comuni a entrambi i modelli basati su React, vedere [reagire la documentazione del modello](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="0446d-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
