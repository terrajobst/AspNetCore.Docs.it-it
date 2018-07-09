---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: In questo video Chris Pels illustra come rendere persistente lo stato di uno o più oggetti in un controllo utente. In primo luogo, viene creato un controllo utente che rappresenta il abilit...
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805178"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Procedura]: rendere persistente lo stato di un controllo utente durante un Postback
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="79f8a-104">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="79f8a-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="79f8a-105">In questo video Chris Pels illustra come rendere persistente lo stato di uno o più oggetti in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="79f8a-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="79f8a-106">In primo luogo, viene creato un controllo utente che rappresenta la possibilità per un utente di specificare criteri di filtro per una ricerca.</span><span class="sxs-lookup"><span data-stu-id="79f8a-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="79f8a-107">Inoltre, una classe di filtro complementare viene creato per archiviare le informazioni del filtro.</span><span class="sxs-lookup"><span data-stu-id="79f8a-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="79f8a-108">Diversi elementi dell'interfaccia utente vengono aggiunti al controllo di filtro con alcuni metodi e proprietà per archiviare le informazioni di filtro correnti nell'istanza di classe di filtro.</span><span class="sxs-lookup"><span data-stu-id="79f8a-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="79f8a-109">Successivamente, la persistenza di controllo utente viene implementata usando il metodo di RegisterRequiresControlState e metodi associati Salva/Ripristina.</span><span class="sxs-lookup"><span data-stu-id="79f8a-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="79f8a-110">Questi metodi archiviano l'istanza della classe di filtro e i dati durante i postback di pagina.</span><span class="sxs-lookup"><span data-stu-id="79f8a-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="79f8a-111">Infine, è disponibile una discussione su come archiviare più oggetti nell'implementazione di stato del controllo.</span><span class="sxs-lookup"><span data-stu-id="79f8a-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="79f8a-112">&#9654;Guarda il video (23 minuti)</span><span class="sxs-lookup"><span data-stu-id="79f8a-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
