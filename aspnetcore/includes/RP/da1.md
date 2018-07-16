# <a name="update-the-generated-pages"></a><span data-ttu-id="866d2-101">Aggiornare le pagine generate</span><span class="sxs-lookup"><span data-stu-id="866d2-101">Update the generated pages</span></span>

<span data-ttu-id="866d2-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="866d2-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="866d2-103">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="866d2-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="866d2-104">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="866d2-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome con i dati sui film](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="866d2-106">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="866d2-106">Update the generated code</span></span>

<span data-ttu-id="866d2-107">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="866d2-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
