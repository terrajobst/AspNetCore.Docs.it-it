## <a name="register-the-database-context"></a><span data-ttu-id="833dd-101">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="833dd-101">Register the database context</span></span>

<span data-ttu-id="833dd-102">In questo passaggio il contesto del database viene registrato nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="833dd-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="833dd-103">I servizi, come il contesto del database, che sono registrati con il contenitore di inserimento delle dipendenze sono disponibili per i controller.</span><span class="sxs-lookup"><span data-stu-id="833dd-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="833dd-104">Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="833dd-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="833dd-105">Sostituire il contenuto del file *Startup.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="833dd-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="833dd-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="833dd-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="833dd-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="833dd-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>
::: moniker-end

<span data-ttu-id="833dd-108">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="833dd-108">The preceding code:</span></span>

* <span data-ttu-id="833dd-109">Rimuove il codice non usato.</span><span class="sxs-lookup"><span data-stu-id="833dd-109">Removes the unused code.</span></span>
* <span data-ttu-id="833dd-110">Specifica che un database in memoria viene inserito nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="833dd-110">Specifies an in-memory database is injected into the service container.</span></span>