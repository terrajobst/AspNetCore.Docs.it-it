<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="3611c-101">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="3611c-101">Add a database connection string</span></span>

<span data-ttu-id="3611c-102">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3611c-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="3611c-103">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="3611c-103">Register the database context</span></span>

<span data-ttu-id="3611c-104">Registrare il contesto del database con il contenitore [Dependency Injection](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3611c-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>