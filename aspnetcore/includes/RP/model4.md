<span data-ttu-id="28b69-101">Nella tabella seguente sono specificati i parametri dei generatori di codice ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="28b69-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="28b69-102">Parametro</span><span class="sxs-lookup"><span data-stu-id="28b69-102">Parameter</span></span>               | <span data-ttu-id="28b69-103">Descrizione</span><span class="sxs-lookup"><span data-stu-id="28b69-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="28b69-104">-m</span><span class="sxs-lookup"><span data-stu-id="28b69-104">-m</span></span>  | <span data-ttu-id="28b69-105">Nome del modello.</span><span class="sxs-lookup"><span data-stu-id="28b69-105">The name of the model.</span></span> |
| <span data-ttu-id="28b69-106">-dc</span><span class="sxs-lookup"><span data-stu-id="28b69-106">-dc</span></span>  | <span data-ttu-id="28b69-107">Contesto dati.</span><span class="sxs-lookup"><span data-stu-id="28b69-107">The data context.</span></span> |
| <span data-ttu-id="28b69-108">-udl</span><span class="sxs-lookup"><span data-stu-id="28b69-108">-udl</span></span> | <span data-ttu-id="28b69-109">Uso del layout predefinito.</span><span class="sxs-lookup"><span data-stu-id="28b69-109">Use the default layout.</span></span> |
| <span data-ttu-id="28b69-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="28b69-110">-outDir</span></span> | <span data-ttu-id="28b69-111">Percorso relativo della cartella di output per creare le viste.</span><span class="sxs-lookup"><span data-stu-id="28b69-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="28b69-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="28b69-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="28b69-113">Aggiunge `_ValidationScriptsPartial` per modificare e creare pagine</span><span class="sxs-lookup"><span data-stu-id="28b69-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="28b69-114">Utilizzare il commutatore `h` per ottenere assistenza sul comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="28b69-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="28b69-115">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="28b69-115">Test the app</span></span>

* <span data-ttu-id="28b69-116">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="28b69-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="28b69-117">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="28b69-117">Test the **Create** link.</span></span>

 ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="28b69-119">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="28b69-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="28b69-120">Se viene visualizzato l'errore seguente, verificare di aver eseguito le migrazioni e di aver aggiornato il database:</span><span class="sxs-lookup"><span data-stu-id="28b69-120">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```