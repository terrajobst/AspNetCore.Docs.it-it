---
title: Helper di Tag di immagine | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di immagine
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="d57ba-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="d57ba-103">ImageTagHelper</span></span>

<span data-ttu-id="d57ba-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d57ba-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="d57ba-105">L'Helper di Tag di immagine migliora la `img` (`<img>`) tag.</span><span class="sxs-lookup"><span data-stu-id="d57ba-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="d57ba-106">Richiede un `src` tag, nonché `boolean` attributo `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="d57ba-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="d57ba-107">Se l'origine dell'immagine (`src`) è un file statico nel server web host, una cache univoca busting stringa viene aggiunto come un parametro di query per l'origine dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="d57ba-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="d57ba-108">In questo modo si garantisce che se il file nel server web host cambia, richiesta univoco viene generato un URL che include il parametro di richiesta di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="d57ba-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="d57ba-109">La cache busting stringa è un valore univoco che rappresenta l'hash del file di immagine statica.</span><span class="sxs-lookup"><span data-stu-id="d57ba-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="d57ba-110">Se l'origine dell'immagine (`src`) non è un file statico (ad esempio un URL remoto o il file non esiste nel server), il `<img>` tag `src` attributo viene generato senza cache busting parametro stringa di query.</span><span class="sxs-lookup"><span data-stu-id="d57ba-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="d57ba-111">Attributi Helper di Tag di immagine</span><span class="sxs-lookup"><span data-stu-id="d57ba-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="d57ba-112">versione aggiungere ASP</span><span class="sxs-lookup"><span data-stu-id="d57ba-112">asp-append-version</span></span>

<span data-ttu-id="d57ba-113">Quando specificato insieme a un `src` viene richiamato l'attributo, l'Helper di Tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="d57ba-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="d57ba-114">Un esempio di un oggetto valido `img` helper di tag è:</span><span class="sxs-lookup"><span data-stu-id="d57ba-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="d57ba-115">Se il file statico esiste nella directory *... Wwwroot/Images/asplogo.PNG* il codice html generato è simile al seguente (il valore hash saranno diversi):</span><span class="sxs-lookup"><span data-stu-id="d57ba-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="d57ba-116">Il valore assegnato al parametro `v` è il valore hash dei file su disco.</span><span class="sxs-lookup"><span data-stu-id="d57ba-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="d57ba-117">Se il server web è in grado di ottenere l'accesso in lettura al file statico a cui fa riferimento, no `v` i parametri vengono aggiunti i `src` attributo.</span><span class="sxs-lookup"><span data-stu-id="d57ba-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="d57ba-118">src</span><span class="sxs-lookup"><span data-stu-id="d57ba-118">src</span></span>

<span data-ttu-id="d57ba-119">Per attivare l'Helper di Tag di immagine, è necessario l'attributo src di `<img>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d57ba-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="d57ba-120">Utilizza l'Helper di Tag di immagine di `Cache` provider nel server web locale per archiviare calcolata `Sha512` di un file specificato.</span><span class="sxs-lookup"><span data-stu-id="d57ba-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="d57ba-121">Se il file viene richiesto di nuovo il `Sha512` non devono essere ricalcolati.</span><span class="sxs-lookup"><span data-stu-id="d57ba-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="d57ba-122">La Cache viene invalidata da un controllo di file che è associato al file quando il file `Sha512` viene calcolata.</span><span class="sxs-lookup"><span data-stu-id="d57ba-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d57ba-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d57ba-123">Additional resources</span></span>

* <xref:performance/caching/memory>
