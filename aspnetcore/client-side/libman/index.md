---
title: Acquisizione di una libreria lato client in ASP.NET Core con LibMan
author: scottaddie
description: Informazioni su come installare asset di una libreria lato client in un progetto ASP.NET Core tramite Gestione librerie (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312359"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="f4943-103">Acquisizione di una libreria lato client in ASP.NET Core con LibMan</span><span class="sxs-lookup"><span data-stu-id="f4943-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="f4943-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f4943-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f4943-105">Gestione librerie (LibMan) è uno strumento per l'acquisizione di librerie lato client leggere.</span><span class="sxs-lookup"><span data-stu-id="f4943-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="f4943-106">LibMan scarica librerie e framework popolari dal file system o da una [rete per la distribuzione di contenuti (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="f4943-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="f4943-107">Le reti CDN supportate includono [CDNJS](https://cdnjs.com/) e [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="f4943-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="f4943-108">I file della libreria selezionata vengono recuperati e inseriti nella posizione appropriata all'interno del progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4943-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="f4943-109">Casi d'uso di LibMan</span><span class="sxs-lookup"><span data-stu-id="f4943-109">LibMan use cases</span></span>

<span data-ttu-id="f4943-110">LibMan offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f4943-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="f4943-111">Vengono scaricati solo i file di libreria necessari.</span><span class="sxs-lookup"><span data-stu-id="f4943-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="f4943-112">Non sono necessari strumenti aggiuntivi, come [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) e [WebPack](https://webpack.js.org) per acquisire un subset di file in una libreria.</span><span class="sxs-lookup"><span data-stu-id="f4943-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="f4943-113">I file possono essere collocati in una posizione specifica senza dovere usare alcuna attività di compilazione o ricorrere alla copia dei file manuale.</span><span class="sxs-lookup"><span data-stu-id="f4943-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="f4943-114">Per altre informazioni sui vantaggi del LibMan, guardare [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s) (Sviluppo di Web front-end moderni con Visual Studio 2017: segmento LibMan).</span><span class="sxs-lookup"><span data-stu-id="f4943-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="f4943-115">LibMan non è un sistema di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="f4943-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="f4943-116">Se si usa già una gestione pacchetti, ad esempio npm oppure [yarn](https://yarnpkg.com), continuare a farlo.</span><span class="sxs-lookup"><span data-stu-id="f4943-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="f4943-117">LibMan non è stato sviluppato per sostituire questi strumenti.</span><span class="sxs-lookup"><span data-stu-id="f4943-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4943-118">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f4943-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="f4943-119">Repository GitHub di LibMan</span><span class="sxs-lookup"><span data-stu-id="f4943-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
