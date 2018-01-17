<span data-ttu-id="c3c2f-101">Aggiungere le proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="c3c2f-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="c3c2f-102">Il campo `ID` è richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="c3c2f-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="c3c2f-103">Aggiungere una classe di contesto dei dati</span><span class="sxs-lookup"><span data-stu-id="c3c2f-103">Add a database context class</span></span>

<span data-ttu-id="c3c2f-104">Aggiungere la classe derivata `DbContext` seguente denominata *MovieContext.cs* alla cartella *Models*: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="c3c2f-104">Add the following `DbContext` derived class named *MovieContext.cs* to the *Models* folder: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="c3c2f-105">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="c3c2f-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="c3c2f-106">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="c3c2f-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
