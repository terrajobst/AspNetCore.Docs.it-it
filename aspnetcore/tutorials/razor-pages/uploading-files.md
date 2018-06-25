---
title: Caricare file in una pagina Razor in ASP.NET Core
author: guardrex
description: Informazioni su come caricare i file in una pagina Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 43268e24b67279b57c990a6289922ae38d883221
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275957"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>Caricare file in una pagina Razor in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

In questa sezione viene illustrato il caricamento di file con una pagina Razor.

In questa esercitazione l'[app di esempio Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) per caricare i file usa l'associazione di modelli semplice che funziona correttamente per file di piccole dimensioni. Per informazioni sulla trasmissione di file di grandi dimensioni, vedere [Caricamento di file di grandi dimensioni con il flusso](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

Nella procedura descritta di seguito viene aggiunta una funzionalità di caricamento di file di pianificazione di film all'app di esempio. Ogni pianificazione di film è rappresentata da una classe `Schedule`. La classe include due versioni della pianificazione. La versione `PublicSchedule` viene messa a disposizione dei clienti. L'altra versione, `PrivateSchedule`, viene usata per i dipendenti della società. Le versioni vengono caricate come file separati. Nell'esercitazione viene illustrato come eseguire il caricamento di due file da una pagina con un solo invio al server.

## <a name="security-considerations"></a>Considerazioni sulla sicurezza

Prestare attenzione quando si offre agli utenti la possibilità di caricare file in un server. Gli utenti malintenzionati possono eseguire attacchi [Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) e di altro tipo su un sistema. Alcuni passaggi di sicurezza che riducono le probabilità di riuscita degli attacchi sono:

* Caricare i file in un'area di caricamento dedicata nel sistema, in modo da poter imporre più facilmente misure di sicurezza per il contenuto caricato. Quando si permettono caricamenti di file, assicurarsi che le autorizzazioni di esecuzione siano disabilitate nella posizione di caricamento.
* Usare nomi di file sicuri determinati dall'app e non dall'input dell'utente o non usare il nome del file caricato.
* Consentire solo un set specifico di estensioni di file approvate.
* Verificare che vengano eseguiti controlli sul lato client nel server. I controlli sul lato client sono facili da aggirare.
* Controllare le dimensioni del caricamento e impedire caricamenti di dimensioni maggiori del previsto.
* Eseguire un rilevatore di virus/malware sul contenuto caricato.

> [!WARNING]
> Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:
> * Prendere il controllo totale di un sistema.
> * Sovraccaricare un sistema causandone il malfunzionamento completo.
> * Compromettere i dati di sistemi o utenti.
> * Applicare graffiti a un'interfaccia pubblica (defacing).

## <a name="add-a-fileupload-class"></a>Aggiungere una classe FileUpload

Creare una pagina Razor per gestire una coppia di caricamenti di file. Aggiungere una classe `FileUpload`, che è associata alla pagina per ottenere i dati di pianificazione. Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Denominare la classe **FileUpload** e aggiungere le proprietà seguenti:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

La classe ha una proprietà per il titolo della pianificazione e una proprietà per ognuna delle due versioni della pianificazione. Tutte e tre le proprietà sono obbligatorie e il titolo deve avere una lunghezza compresa fra 3 e 60 caratteri.

## <a name="add-a-helper-method-to-upload-files"></a>Aggiungere un metodo helper per caricare file

Per evitare la duplicazione del codice per l'elaborazione dei file di pianificazione caricati, è necessario per prima cosa aggiungere un metodo helper statico. Creare una cartella *Utilità* nell'app e aggiungere un file *FileHelpers.cs* con il contenuto seguente. Il metodo helper, `ProcessFormFile`, accetta [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) e [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) e restituisce una stringa contenente le dimensioni e il contenuto del file. Il tipo di contenuto e la lunghezza vengono controllati. Se il file non supera un controllo di convalida, viene aggiunto un errore a `ModelState`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a>Salvare il file su disco

L'app di esempio salva i file caricati in campi di database. Per salvare un file su disco, usare un [FileStream](/dotnet/api/system.io.filestream). L'esempio seguente copia un file gestito da `FileUpload.UploadPublicSchedule` in un `FileStream` in un metodo `OnPostAsync`. `FileStream` scrive il file su disco in corrispondenza del `<PATH-AND-FILE-NAME>` specificato:

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

Il processo di lavoro deve disporre delle autorizzazioni di scrittura per il percorso specificato da `filePath`.

> [!NOTE]
> Il `filePath` *deve* includere il nome del file. Se il nome file non è specificato, viene generata un'eccezione [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) in fase di esecuzione.

> [!WARNING]
> Non mantenere mai i file caricati nello stessa struttura directory in cui si trova l'app.
>
> Il codice di esempio non include alcuna protezione lato server contro il caricamento di file dannosi. Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:
>
> * [Caricamento di file senza restrizioni](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Protezione di Azure: Verificare i controlli appropriati quando si accettano file dagli utenti](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>Salvare il file in Archiviazione BLOB di Azure

Per caricare il contenuto del file in Archiviazione Blob di Azure, vedere [Introduzione all'archivio BLOB di Azure con .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Questo argomento dimostra come usare [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) per salvare un [FileStream](/dotnet/api/system.io.filestream) nell'archivio BLOB.

## <a name="add-the-schedule-class"></a>Aggiungere la classe Schedule

Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Denominare la classe **Schedule** e aggiungere le proprietà seguenti:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

La classe usa gli attributi `Display` e `DisplayFormat`, che producono titoli descrittivi e la formattazione quando viene eseguito il rendering dei dati di pianificazione.

## <a name="update-the-moviecontext"></a>Aggiornare MovieContext

Specificare un `DbSet` in `MovieContext` (*Models/MovieContext.cs*) per le pianificazioni:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Aggiungere la tabella Schedule al database

Aprire la Console di Gestione pacchetti: **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

Nella console di Gestione pacchetti eseguire i comandi seguenti. In questo modo viene aggiunta una tabella `Schedule` al database:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Aggiungere una pagina Razor di caricamento file

Nella cartella *Pages* creare una cartella *Schedules*. Nella cartella *Schedules* creare una pagina denominata *Index.cshtml* per il caricamento di una pianificazione con il contenuto seguente:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Ogni gruppo di moduli include un oggetto **\<label>** ovvero un'etichetta che visualizza il nome di ogni proprietà della classe. Gli attributi `Display` del modello `FileUpload` specificano i valori di visualizzazione per le etichette. Ad esempio, il nome visualizzato della proprietà `UploadPublicSchedule` è impostato con `[Display(Name="Public Schedule")]` e pertanto viene visualizzato "Public Schedule" nell'etichetta quando viene eseguito il rendering del modulo.

Ogni gruppo di moduli include una convalida **\<span>**. Se l'input dell'utente non soddisfa gli attributi della proprietà impostati nella classe `FileUpload` o se uno dei controlli della convalida file del metodo `ProcessFormFile` ha esito negativo, il modello non viene convalidato. Quando il modello non viene convalidato, viene eseguito il rendering di un messaggio di convalida utile per l'utente. Ad esempio, la proprietà `Title` viene annotata con `[Required]` e `[StringLength(60, MinimumLength = 3)]`. Se l'utente non specifica un titolo, riceve un messaggio che indica che è necessario specificare un valore. Se l'utente immette un valore inferiore a tre caratteri o superiore a sessanta, riceve un messaggio che indica che il valore ha una lunghezza non corretta. Se viene specificato un file che non presenta alcun contenuto, viene visualizzato un messaggio che indica che il file è vuoto.

## <a name="add-the-page-model"></a>Aggiungere il modello di pagina

Aggiungere il modello di pagina *Index.cshtml.cs* nella cartella *Schedules*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Il modello di pagina (`IndexModel` in *Index.cshtml.cs*) associa la classe `FileUpload`:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Il modello usa anche un elenco delle pianificazioni, `IList<Schedule>`, per visualizzare le pianificazioni archiviate nel database nella pagina:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Quando la pagina viene caricata con `OnGetAsync`, la tabella `Schedules` viene popolata dal database e usata per generare una tabella HTML di pianificazioni caricate:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Quando il modulo viene pubblicato nel server, viene eseguito il controllo di `ModelState`. Se la convalida dà esito negativo, `Schedule` viene ricompilata, e il rendering della pagina genera uno o più messaggi di convalida che indicano il motivo per cui la convalida non è riuscita. Se la convalida ha esisto positivo, le proprietà `FileUpload` vengono usate in *OnPostAsync* per completare il caricamento del file per le due versioni della pianificazione e per creare un nuovo oggetto `Schedule` per archiviare i dati. La pianificazione viene quindi salvata nel database:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Aggiungere un collegamento alla pagina Razor di caricamento file

Aprire *_Layout.cshtml* e aggiungere un collegamento alla barra di navigazione per accedere alla pagina di caricamento dei file:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Aggiungere una pagina per confermare l'eliminazione della pianificazione

Quando l'utente fa clic per eliminare una pianificazione, viene offerta la possibilità di annullare l'operazione. Aggiungere una pagina di conferma dell'eliminazione (*Delete.cshtml*) nella cartella *Schedules*:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Il modello di pagina *Delete.cshtml.cs* carica una singola pianificazione identificata da `id` nei dati della route della richiesta. Aggiungere il file *Delete.cshtml.cs* nella cartella *Schedules*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

Il metodo `OnPostAsync` gestisce l'eliminazione della pianificazione tramite `id`:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Dopo aver completato l'eliminazione della pianificazione, `RedirectToPage` invia nuovamente l'utente alla pagina delle pianificazioni *Index.cshtml*.

## <a name="the-working-schedules-razor-page"></a>La pagina Razor Schedules operativa

Quando la pagina viene caricata, il rendering delle etichette e degli input per il titolo della pianificazione, della pianificazione pubblica e della pianificazione privata viene eseguito con un pulsante di invio:

![La pagina Razor Schedules come viene visualizzata al caricamento iniziale in assenza di errori di convalida e con i campi vuoti](uploading-files/_static/browser1.png)

Se si seleziona il pulsante **Carica** senza aver popolato i campi, vengono violati gli attributi `[Required]` nel modello. `ModelState` non è valido. I messaggi di errore di convalida vengono visualizzati all'utente:

![I messaggi di errore di convalida vengono visualizzati accanto a ogni controllo input](uploading-files/_static/browser2.png)

Digitare due lettere nel campo **Titolo**. Il messaggio di convalida cambia e indica che il titolo deve avere una lunghezza compresa tra 3 e 60 caratteri:

![Messaggio di convalida titolo modificato](uploading-files/_static/browser3.png)

Quando vengono caricate una o più pianificazioni, la sezione **Loaded Schedules** (Pianificazioni caricate) esegue il rendering delle pianificazioni caricate:

![Tabella delle pianificazioni caricate che include il titolo di ogni pianificazione, la data di caricamento con l'ora UTC, le dimensioni del file in versione pubblica e in versione privata](uploading-files/_static/browser4.png)

L'utente può scegliere il collegamento **Eliminazione** per accedere alla schermata di conferma dell'eliminazione, da dove è possibile confermare o annullare l'operazione.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Per risolvere i problemi relativi al caricamento di `IFormFile`, vedere [Caricamento di file in ASP.NET Core: Risoluzione dei problemi](xref:mvc/models/file-uploads#troubleshooting).

Grazie aver completato questa introduzione alle pagine Razor. I suggerimenti degli utenti sono importanti. [Get started with MVC and EF Core](xref:data/ef-mvc/intro) (Introduzione a MVC ed Entity Framework Core) è un complemento ideale per questa esercitazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Caricamento di file in ASP.NET Core](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [Articolo Precedente: Convalida](xref:tutorials/razor-pages/validation)
