
## <a name="test-the-app"></a><span data-ttu-id="4523f-101">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4523f-101">Test the app</span></span>

* <span data-ttu-id="4523f-102">Eseguire l'app e toccare il collegamento **Mvc film**.</span><span class="sxs-lookup"><span data-stu-id="4523f-102">Run the app and tap the **Mvc Movie** link.</span></span>
* <span data-ttu-id="4523f-103">Toccare il collegamento **Crea nuovo** e creare un filmato.</span><span class="sxs-lookup"><span data-stu-id="4523f-103">Tap the **Create New** link and create a movie.</span></span>

  ![Creare una vista con i campi per genere, prezzo, data di rilascio e titolo](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* <span data-ttu-id="4523f-105">Potrebbe non essere possibile immettere separatori decimali o virgole nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="4523f-105">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="4523f-106">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="4523f-106">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="4523f-107">Vedere https://github.com/aspnet/Docs/issues/4076 e [Risorse aggiuntive](#additional-resources) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="4523f-107">See https://github.com/aspnet/Docs/issues/4076 and [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="4523f-108">Per il momento, immettere solo numeri interi come 10.</span><span class="sxs-lookup"><span data-stu-id="4523f-108">For now, just enter whole numbers like 10.</span></span>

<a name="displayformatdatelocal"></a>

* <span data-ttu-id="4523f-109">In alcune impostazioni locali è necessario specificare il formato di data.</span><span class="sxs-lookup"><span data-stu-id="4523f-109">In some locales you need to specify the date format.</span></span> <span data-ttu-id="4523f-110">Vedere il codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4523f-110">See the highlighted code below.</span></span>

<span data-ttu-id="4523f-111">[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span><span class="sxs-lookup"><span data-stu-id="4523f-111">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]</span></span>

<span data-ttu-id="4523f-112">`DataAnnotations` verrà trattato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="4523f-112">We'll talk about `DataAnnotations` later in the tutorial.</span></span>

<span data-ttu-id="4523f-113">Toccando **Crea** il modulo viene inviato al server, dove le informazioni relative al filmato vengono salvate in un database.</span><span class="sxs-lookup"><span data-stu-id="4523f-113">Tapping **Create** causes the form to be posted to the server, where the movie information is saved in a database.</span></span> <span data-ttu-id="4523f-114">L'applicazione reindirizza all'URL */Movies*, in cui vengono visualizzate le informazioni relative al filmato appena creato.</span><span class="sxs-lookup"><span data-stu-id="4523f-114">The app redirects to the */Movies* URL, where the newly created movie information is displayed.</span></span>

![Vista di Movies che visualizza l'elenco dei filmati appena creati](../../tutorials/first-mvc-app/adding-model/_static/h.png)

<span data-ttu-id="4523f-116">Creare altre due voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="4523f-116">Create a couple more movie entries.</span></span> <span data-ttu-id="4523f-117">Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.</span><span class="sxs-lookup"><span data-stu-id="4523f-117">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>
