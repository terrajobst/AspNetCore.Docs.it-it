---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Ricerca per categorie]: rendere persistente lo stato di un controllo utente durante un Postback | Documenti Microsoft'
author: rick-anderson
description: "In questo video di Chris Pels Mostra come rendere persistente lo stato di uno o più oggetti in un controllo utente. Innanzitutto, viene creato un controllo utente che rappresenta il abilit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="f886b-104">[Ricerca per categorie]: rendere persistente lo stato di un controllo utente durante un Postback</span><span class="sxs-lookup"><span data-stu-id="f886b-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="f886b-105">da [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="f886b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="f886b-106">In questo video di Chris Pels Mostra come rendere persistente lo stato di uno o più oggetti in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="f886b-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="f886b-107">Innanzitutto, viene creato un controllo utente che rappresenta la possibilità per un utente di specificare i criteri di filtro per una ricerca.</span><span class="sxs-lookup"><span data-stu-id="f886b-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="f886b-108">Inoltre, una classe di filtro complementare viene creata per archiviare le informazioni di filtro.</span><span class="sxs-lookup"><span data-stu-id="f886b-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="f886b-109">Per il controllo filtro con alcuni metodi e proprietà per archiviare le informazioni sul filtro corrente dell'istanza di classe di filtro, vengono aggiunti diversi elementi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="f886b-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="f886b-110">Successivamente, la persistenza del controllo utente viene implementata utilizzando il metodo RegisterRequiresControlState e metodi Salva/Ripristina associati.</span><span class="sxs-lookup"><span data-stu-id="f886b-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="f886b-111">Questi metodi archiviano l'istanza della classe di filtro e i dati durante i postback di pagina.</span><span class="sxs-lookup"><span data-stu-id="f886b-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="f886b-112">Infine, è disponibile una descrizione delle modalità archiviare più oggetti nell'implementazione di stato del controllo.</span><span class="sxs-lookup"><span data-stu-id="f886b-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="f886b-113">&#9654; Guardare video (minuti 23)</span><span class="sxs-lookup"><span data-stu-id="f886b-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
