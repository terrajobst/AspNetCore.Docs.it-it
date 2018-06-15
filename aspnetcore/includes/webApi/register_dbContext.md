## <a name="register-the-database-context"></a><span data-ttu-id="2376a-101">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="2376a-101">Register the database context</span></span>

<span data-ttu-id="2376a-102">In questo passaggio il contesto del database viene registrato nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2376a-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2376a-103">I servizi, come il contesto del database, che sono registrati con il contenitore di inserimento delle dipendenze sono disponibili per i controller.</span><span class="sxs-lookup"><span data-stu-id="2376a-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="2376a-104">Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2376a-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="2376a-105">Sostituire il contenuto del file *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2376a-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2376a-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="2376a-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2376a-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="2376a-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>

::: moniker-end  

<span data-ttu-id="2376a-108">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2376a-108">The preceding code:</span></span>

* <span data-ttu-id="2376a-109">Rimuove il codice non usato.</span><span class="sxs-lookup"><span data-stu-id="2376a-109">Removes the unused code.</span></span>
* <span data-ttu-id="2376a-110">Specifica che un database in memoria viene inserito nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="2376a-110">Specifies an in-memory database is injected into the service container.</span></span>
