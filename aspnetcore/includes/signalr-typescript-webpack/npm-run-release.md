---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210438"
---
```console
npm run release
```

<span data-ttu-id="9655d-101">Questo comando restituisce le risorse lato client che devono essere servite durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="9655d-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="9655d-102">Le risorse si trovano nella cartella *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9655d-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="9655d-103">Webpack ha completato le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="9655d-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="9655d-104">Ha ripulito il contenuto della directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9655d-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="9655d-105">Ha convertito TypeScript in JavaScript, un processo noto con il termine tecnico *transpiling*.</span><span class="sxs-lookup"><span data-stu-id="9655d-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="9655d-106">Ha modificato il codice JavaScript generato per ridurre le dimensioni del file, un processo noto con il termine tecnico *minimizzazione*.</span><span class="sxs-lookup"><span data-stu-id="9655d-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="9655d-107">Ha copiato i file JavaScript, CSS e HTML elaborati dalla directory *src* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9655d-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="9655d-108">Ha inserito gli elementi seguenti nel file *wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="9655d-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
  * <span data-ttu-id="9655d-109">Un tag `<link>` che fa riferimento al file *wwwroot/main.\<hash\>.css*.</span><span class="sxs-lookup"><span data-stu-id="9655d-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="9655d-110">Questo tag viene inserito immediatamente prima del tag `</head>` di chiusura.</span><span class="sxs-lookup"><span data-stu-id="9655d-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
  * <span data-ttu-id="9655d-111">Un tag `<script>` che fa riferimento al file *wwwroot/main.\<hash\>.js* minimizzato.</span><span class="sxs-lookup"><span data-stu-id="9655d-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="9655d-112">Questo tag viene inserito immediatamente prima del tag `</body>` di chiusura.</span><span class="sxs-lookup"><span data-stu-id="9655d-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
