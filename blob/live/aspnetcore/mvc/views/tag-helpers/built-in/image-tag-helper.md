---
title: Helper di Tag di immagine | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di immagine
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="103de-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="103de-104">ImageTagHelper</span></span>

<span data-ttu-id="103de-105">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="103de-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="103de-106">L'Helper di Tag di immagine migliora la `img` (`<img>`) tag.</span><span class="sxs-lookup"><span data-stu-id="103de-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="103de-107">Richiede un `src` tag, nonché `boolean` attributo `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="103de-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="103de-108">Se l'origine dell'immagine (`src`) è un file statico nel server web host, una cache univoca busting stringa viene aggiunto come un parametro di query per l'origine dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="103de-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="103de-109">In questo modo si garantisce che se il file nel server web host cambia, richiesta univoco viene generato un URL che include il parametro di richiesta di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="103de-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="103de-110">La cache busting stringa è un valore univoco che rappresenta l'hash del file di immagine statica.</span><span class="sxs-lookup"><span data-stu-id="103de-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="103de-111">Se l'origine dell'immagine (`src`) non è un file statico (ad esempio un URL remoto o il file non esiste nel server), il `<img>` tag `src` attributo viene generato senza cache busting parametro stringa di query.</span><span class="sxs-lookup"><span data-stu-id="103de-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="103de-112">Attributi Helper di Tag di immagine</span><span class="sxs-lookup"><span data-stu-id="103de-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="103de-113">versione aggiungere ASP</span><span class="sxs-lookup"><span data-stu-id="103de-113">asp-append-version</span></span>

<span data-ttu-id="103de-114">Quando specificato insieme a un `src` viene richiamato l'attributo, l'Helper di Tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="103de-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="103de-115">Un esempio di un oggetto valido `img` helper di tag è:</span><span class="sxs-lookup"><span data-stu-id="103de-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="103de-116">Se il file statico esiste nella directory *... Wwwroot/Images/asplogo.PNG* il codice html generato è simile al seguente (il valore hash saranno diversi):</span><span class="sxs-lookup"><span data-stu-id="103de-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="103de-117">Il valore assegnato al parametro `v` è il valore hash dei file su disco.</span><span class="sxs-lookup"><span data-stu-id="103de-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="103de-118">Se il server web è in grado di ottenere l'accesso in lettura al file statico a cui fa riferimento, no `v` i parametri vengono aggiunti i `src` attributo.</span><span class="sxs-lookup"><span data-stu-id="103de-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="103de-119">src</span><span class="sxs-lookup"><span data-stu-id="103de-119">src</span></span>

<span data-ttu-id="103de-120">Per attivare l'Helper di Tag di immagine, è necessario l'attributo src di `<img>` elemento.</span><span class="sxs-lookup"><span data-stu-id="103de-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="103de-121">Utilizza l'Helper di Tag di immagine di `Cache` provider nel server web locale per archiviare calcolata `Sha512` di un file specificato.</span><span class="sxs-lookup"><span data-stu-id="103de-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="103de-122">Se il file viene richiesto di nuovo il `Sha512` non deve essere ricalcolata.</span><span class="sxs-lookup"><span data-stu-id="103de-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="103de-123">La Cache viene invalidata da un controllo di file che è associato al file quando il file `Sha512` viene calcolata.</span><span class="sxs-lookup"><span data-stu-id="103de-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="103de-124">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="103de-124">Additional resources</span></span>

* <xref:performance/caching/memory>
