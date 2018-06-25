---
title: Helper tag di immagine in ASP.NET Core
author: pkellner
description: Informazioni sull'uso dell'helper tag di immagine
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 7ed160354b25aa0183ac49db93307b1f1b4d0666
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276643"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="d19ea-103">Helper tag di immagine in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d19ea-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="d19ea-104">Di [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d19ea-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="d19ea-105">L'helper tag di immagine migliora il tag `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="d19ea-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="d19ea-106">Richiede un tag `src` e l'attributo `boolean` `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="d19ea-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="d19ea-107">Se l'origine dell'immagine (`src`) è un file statico nel server Web host, una stringa di cache busting univoca viene accodata come parametro di query all'origine dell'immagine.</span><span class="sxs-lookup"><span data-stu-id="d19ea-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="d19ea-108">Questo garantisce che se il file nel server Web host cambia viene generato un URL di richiesta univoco, che include il parametro di richiesta di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="d19ea-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="d19ea-109">La stringa di cache busting è un valore univoco che rappresenta l'hash del file immagine statico.</span><span class="sxs-lookup"><span data-stu-id="d19ea-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="d19ea-110">Se l'origine dell'immagine (`src`) non è un file statico (ad esempio se è un URL remoto o se il file non esiste nel server), l'attributo `src` del tag `<img>` viene generato senza il parametro di query con stringa di cache busting.</span><span class="sxs-lookup"><span data-stu-id="d19ea-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="d19ea-111">Attributi dell'helper tag di immagine</span><span class="sxs-lookup"><span data-stu-id="d19ea-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="d19ea-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="d19ea-112">asp-append-version</span></span>

<span data-ttu-id="d19ea-113">Quando viene specificato insieme a un attributo `src`, viene chiamato l'helper tag di immagine.</span><span class="sxs-lookup"><span data-stu-id="d19ea-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="d19ea-114">Un esempio di helper tag `img` valido è il seguente:</span><span class="sxs-lookup"><span data-stu-id="d19ea-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="d19ea-115">Se il file statico esiste nella directory *..wwwroot/images/asplogo.png* il codice HTML generato è simile al seguente (il codice hash è diverso):</span><span class="sxs-lookup"><span data-stu-id="d19ea-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="d19ea-116">Il valore assegnato al parametro `v` è il valore hash dei file su disco.</span><span class="sxs-lookup"><span data-stu-id="d19ea-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="d19ea-117">Se il server Web non è in grado di ottenere l'accesso in lettura al file statico di riferimento, non viene aggiunto nessun parametro `v` all'attributo `src`.</span><span class="sxs-lookup"><span data-stu-id="d19ea-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="d19ea-118">src</span><span class="sxs-lookup"><span data-stu-id="d19ea-118">src</span></span>

<span data-ttu-id="d19ea-119">Per attivare l'helper tag di immagine l'attributo src è obbligatorio nell'elemento `<img>`.</span><span class="sxs-lookup"><span data-stu-id="d19ea-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="d19ea-120">L'helper tag di immagine usa il provider `Cache` nel server Web locale per archiviare l'elemento `Sha512` calcolato di un file dato.</span><span class="sxs-lookup"><span data-stu-id="d19ea-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="d19ea-121">Se il file viene richiesto di nuovo, non è necessario ricalcolare `Sha512`.</span><span class="sxs-lookup"><span data-stu-id="d19ea-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="d19ea-122">La cache viene invalidata da un watcher di file che viene associato al file quando si calcola l'elemento `Sha512` del file stesso.</span><span class="sxs-lookup"><span data-stu-id="d19ea-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d19ea-123">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d19ea-123">Additional resources</span></span>

* <xref:performance/caching/memory>
