---
title: Caricamento di file in ASP.NET Core
author: ardalis
description: Come usare l'associazione del modello e lo streaming per caricare i file in ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/models/file-uploads
ms.openlocfilehash: 5e6e2cd5fac25e2abe27915c2f4caa64b13e90bd
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56409983"
---
# <a name="file-uploads-in-aspnet-core"></a>Caricamento di file in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Le azioni di ASP.NET MVC supportano il caricamento di uno o più file tramite un'associazione di modelli semplice per i file di dimensioni inferiori o tramite streaming per i file di dimensioni maggiori.

[Visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Caricamento di file di piccole dimensioni tramite associazione di modelli

Per caricare file di piccole dimensioni, è possibile usare un form HTML multiparte o costruire una richiesta POST con JavaScript. Di seguito è illustrato un form di esempio con Razor, che supporta più file caricati:

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

Per supportare il caricamento di file, i form HTML devono specificare un `enctype` corrispondente a `multipart/form-data`. L'elemento input `files` illustrato in precedenza supporta il caricamento di più file. Omettere l'attributo `multiple` dell'elemento di input per consentire il caricamento di un unico file. Il rendering del markup sopra riportato corrisponde a:

![Form di caricamento di file](file-uploads/_static/upload-form.png)

I singoli file caricati nel server sono accessibili tramite [associazione di modelli](xref:mvc/models/model-binding) attraverso l'interfaccia [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile). `IFormFile` ha la struttura seguente:

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> Non basarsi sulla proprietà `FileName` né considerarla attendibile senza convalida. La proprietà `FileName` deve essere usata solo per scopi di visualizzazione.

Durante il caricamento di file tramite l'associazione di modelli e l'interfaccia `IFormFile`, il metodo di azione può accettare una singola `IFormFile` oppure una `IEnumerable<IFormFile>` (o una `List<IFormFile>`) che rappresenta diversi file. L'esempio seguente esegue il ciclo attraverso uno o più file caricati, li salva nel file system locale e restituisce il numero totale e le dimensioni dei file caricati.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Prima di essere elaborati, i file caricati tramite la tecnica `IFormFile` vengono memorizzati nel buffer in memoria o nel disco del server Web. All'interno del metodo di azione, il contenuto di `IFormFile` è accessibile come flusso. Oltre al file system locale, i file possono essere trasmessi ad [Archiviazione BLOB di Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) o a [Entity Framework](/ef/core/index).

Per archiviare i dati di file binari in un database tramite Entity Framework, definire una proprietà di tipo `byte[]` nell'entità:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Specificare una proprietà viewmodel di tipo `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> L'interfaccia `IFormFile` può essere usata direttamente come parametro di metodo di azione o come proprietà viewmodel, come illustrato in precedenza.

Copiare `IFormFile` in un flusso e salvare quest'ultimo nella matrice di byte:

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser 
        {
            UserName = model.Email,
            Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted

    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> Quando si archiviano dati binari all'interno di database relazionali, è necessario prestare attenzione, perché l'operazione può influire negativamente sulle prestazioni.

## <a name="uploading-large-files-with-streaming"></a>Caricamento di file di grandi dimensioni tramite streaming

Se le dimensioni o la frequenza del caricamento dei file causa problemi di risorse per l'app, è consigliabile caricare i file tramite streaming, anziché memorizzarli interamente nel buffer, come avviene con l'approccio di associazione di modelli illustrato in precedenza. L'uso di `IFormFile` e dell'associazione di modelli rappresenta una soluzione più semplice, mentre l'implementazione corretta dello streaming richiede un certo numero di passaggi.

> [!NOTE]
> Ogni file memorizzato nel buffer superiore a 64 KB viene spostato dalla RAM in un file temporaneo in un disco nel server. Le risorse (disco, RAM) usate dall'operazione di caricamento di file dipendono dal numero e dalle dimensioni dei file caricati simultaneamente. Con lo streaming, il problema non è rappresentato dalle prestazioni ma dalla scalabilità. Se si tenta di caricare nel buffer un numero eccessivo di file, quando la memoria o lo spazio su disco si esaurisce il sito si arresterà in modo anomalo.

L'esempio seguente illustra l'uso di JavaScript/Angular per eseguire lo streaming in un'azione del controller. Il token anti falsificazione del file viene generato tramite un attributo di filtro personalizzato e passato in intestazioni HTTP anziché nel corpo della richiesta. Poiché il metodo di azione elabora i dati caricati direttamente, l'associazione di modelli viene disabilitata da un altro filtro. All'interno dell'azione, il contenuto del form viene letto tramite un `MultipartReader`, che legge ogni singola `MultipartSection`, elaborando il file o archiviandone il contenuto, come appropriato. Dopo che tutte le sezioni sono state lette, l'azione esegue la propria associazione di modelli.

L'azione iniziale carica il form e salva un token anti falsificazione in un cookie tramite l'attributo `GenerateAntiforgeryTokenCookieForAjax`:

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

L'attributo usa il supporto [anti falsificazione](xref:security/anti-request-forgery) incorporato di ASP.NET Core per impostare un cookie con un token della richiesta:

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular passa automaticamente un token anti falsificazione in un'intestazione di richiesta denominata `X-XSRF-TOKEN`. Nel file di configurazione *Startup.cs* l'app ASP.NET Core MVC è configurata per fare riferimento a questa intestazione:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

L'attributo `DisableFormValueModelBinding`, illustrato di seguito, viene usato per disabilitare l'associazione di modelli per il metodo di azione `Upload`.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Poiché l'associazione di modelli è disabilitata, il metodo di azione `Upload` non accetta parametri ma usa direttamente la proprietà `Request` di `ControllerBase`. Per leggere ogni sezione, viene usato un `MultipartReader`. Il file viene salvato con un nome di file GUID e i dati di chiave/valore vengono archiviati in un `KeyValueAccumulator`. Dopo che tutte le sezioni sono state lette, il contenuto del `KeyValueAccumulator` viene usato per associare i dati del form a un tipo di modello.

Il metodo `Upload` è illustrato di seguito:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono trattati alcuni problemi comuni riscontrati durante il caricamento di file, con le soluzioni possibili corrispondenti.

### <a name="unexpected-not-found-error-with-iis"></a>Errore imprevisto Non trovato con IIS

L'errore seguente indica che il caricamento di file supera il valore di `maxAllowedContentLength` configurato per il server:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

L'impostazione predefinita è `30000000`, ovvero circa 28,6 MB. Il valore può essere personalizzato modificando *web.config*:

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

Questa impostazione si applica solo a IIS. Il comportamento non si verifica per impostazione predefinita con l'hosting in Kestrel. Per altre informazioni, vedere [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Limiti delle richieste <requestLimits>).

### <a name="null-reference-exception-with-iformfile"></a>Eccezione per riferimento Null con IFormFile

Se il controller accetta file caricati tramite `IFormFile` ma si rileva che il valore è sempre Null, verificare che il form HTML specifichi un valore `enctype` corrispondente a `multipart/form-data`. Se questo attributo non è impostato per l'elemento `<form>`, il caricamento di file non si verifica e qualsiasi argomento `IFormFile` associato è Null.
