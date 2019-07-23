* `-F|--no-formatting`

  <span data-ttu-id="dde6a-101">Flag la cui presenza elimina la formattazione della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dde6a-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="dde6a-102">Imposta un'intestazione della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dde6a-102">Sets an HTTP request header.</span></span> <span data-ttu-id="dde6a-103">Sono supportati i due formati di valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="dde6a-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="dde6a-104">Specifica un file in cui deve essere scritta l'intera risposta HTTP, incluse le intestazioni e il corpo.</span><span class="sxs-lookup"><span data-stu-id="dde6a-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="dde6a-105">Ad esempio `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="dde6a-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="dde6a-106">Se il file non esiste, verrà creato.</span><span class="sxs-lookup"><span data-stu-id="dde6a-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="dde6a-107">Specifica un file in cui deve essere scritto il corpo della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dde6a-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="dde6a-108">Ad esempio `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="dde6a-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="dde6a-109">Se il file non esiste, verrà creato.</span><span class="sxs-lookup"><span data-stu-id="dde6a-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="dde6a-110">Specifica un file in cui devono essere scritte le intestazioni della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dde6a-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="dde6a-111">Ad esempio `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="dde6a-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="dde6a-112">Se il file non esiste, verrà creato.</span><span class="sxs-lookup"><span data-stu-id="dde6a-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="dde6a-113">Flag la cui presenza abilita il flusso della risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="dde6a-113">A flag whose presence enables streaming of the HTTP response.</span></span>
