---
title: Usare LibMan con ASP.NET Core in Visual Studio
author: scottaddie
description: Informazioni su come usare LibMan in un progetto ASP.NET Core con Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: e92e6bc28ec58b26785dd6c79e71512368202a26
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658312"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>Usare LibMan con ASP.NET Core in Visual Studio

Di [Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio include il supporto incorporato per [LibMan](xref:client-side/libman/index) in progetti di ASP.NET Core, tra cui:

* Supporto per la configurazione e l'esecuzione di operazioni di ripristino di LibMan durante la compilazione.
* Voci di menu per l'attivazione delle operazioni di ripristino e pulizia del LibMan.
* Finestra di dialogo Cerca per la ricerca di librerie e l'aggiunta di file a un progetto.
* Modifica del supporto per *libman. json*&mdash;il file manifesto libman.

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(procedura per il download)](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Prerequisites

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**

## <a name="add-library-files"></a>Aggiungi file di libreria

I file di libreria possono essere aggiunti a un progetto ASP.NET Core in due modi diversi:

1. [Usare la finestra di dialogo Aggiungi libreria sul lato client](#use-the-add-client-side-library-dialog)
1. [Configurare manualmente le voci del file manifesto LibMan](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>Usare la finestra di dialogo Aggiungi libreria sul lato client

Per installare una libreria sul lato client, attenersi alla procedura seguente:

* In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella del progetto in cui devono essere aggiunti i file. Scegliere **aggiungi** > **libreria sul lato client**. Viene visualizzata la finestra di dialogo **Aggiungi libreria sul lato client** :

  ![Finestra di dialogo Aggiungi libreria sul lato client](_static/add-library-dialog.png)

* Selezionare il provider di librerie dall'elenco a discesa **provider** . CDNJS è il provider predefinito.
* Digitare il nome della libreria da recuperare nella casella di testo **libreria** . IntelliSense fornisce un elenco di librerie che iniziano con il testo fornito.
* Selezionare la libreria dall'elenco IntelliSense. Si noti che il nome della libreria è suffisso con il simbolo di `@` e la versione stabile più recente nota al provider selezionato.
* Decidere quali file includere:
  * Selezionare il pulsante di opzione **Includi tutti i file di libreria** per includere tutti i file della libreria.
  * Selezionare il pulsante di opzione **Scegli file specifici** per includere un subset dei file della libreria. Quando il pulsante di opzione è selezionato, la struttura ad albero del selettore file è abilitata. Selezionare le caselle a sinistra dei nomi file da scaricare.
* Specificare la cartella di progetto per l'archiviazione dei file nella casella di testo **percorso di destinazione** . Come raccomandazione, archiviare ogni libreria in una cartella separata.

  La cartella del **percorso di destinazione** suggerita è basata sul percorso da cui è stata avviata la finestra di dialogo:

  * Se avviato dalla radice del progetto:
    * Se *wwwroot* esiste, viene usato *wwwroot/lib* .
    * *lib* viene usato se *wwwroot* non esiste.
  * Se avviato da una cartella del progetto, viene usato il nome della cartella corrispondente.

  Il suggerimento della cartella è con suffisso con il nome della libreria. Nella tabella seguente vengono illustrati i suggerimenti per le cartelle quando si installa jQuery in un progetto Razor Pages.
  
  |Percorso di avvio                           |Cartella consigliata      |
  |------------------------------------------|----------------------|
  |radice del progetto (se *wwwroot* esiste)        |*Wwwroot/lib/jQuery/* |
  |radice del progetto (se *wwwroot* non esiste) |*lib/jQuery/*         |
  |Cartella *pages* nel progetto                 |*Pagine/jQuery/*       |

* Fare clic sul pulsante **Install (installa** ) per scaricare i file in base alla configurazione in *libman. JSON*.
* Per informazioni sull'installazione, vedere il feed di **Gestione librerie** della finestra di **output** . Ad esempio:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>Configurare manualmente le voci del file manifesto LibMan

Tutte le operazioni LibMan in Visual Studio sono basate sul contenuto del manifesto LibMan della radice del progetto (*LibMan. JSON*). È possibile modificare manualmente *libman. JSON* per configurare i file di libreria per il progetto. Quando viene salvato *libman. JSON* , Visual Studio Ripristina tutti i file di libreria.

Per aprire *libman. JSON* per la modifica, sono disponibili le opzioni seguenti:

* Fare doppio clic sul file *libman. JSON* in **Esplora soluzioni**.
* Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Gestisci librerie lato client**. **&#8224;**
* Selezionare **Gestisci librerie lato client** dal menu **progetto** di Visual Studio. **&#8224;**

**&#8224;** Se il file *libman. JSON* non esiste già nella radice del progetto, verrà creato con il contenuto del modello di elemento predefinito.

Visual Studio offre supporto avanzato per la modifica di JSON, ad esempio la colorazione, la formattazione, IntelliSense e la convalida dello schema. Lo schema JSON del manifesto LibMan è disponibile in [https://json.schemastore.org/libman](https://json.schemastore.org/libman).

Con il file manifesto seguente, LibMan recupera i file in base alla configurazione definita nella proprietà `libraries`. Di seguito è riportata una spiegazione dei valori letterali di oggetto definiti nel `libraries`:

* Un subset di [jQuery](https://jquery.com/) versione 3.3.1 viene recuperato dal provider CDNJS. Il subset è definito nella proprietà `files`&mdash;*jQuery. min. js*, *jQuery. js*e *jQuery. min. map*. I file vengono inseriti nella cartella *wwwroot/lib/jQuery* del progetto.
* L'intera versione di [bootstrap](https://getbootstrap.com/) 4.1.3 viene recuperata e inserita in una cartella *wwwroot/lib/bootstrap* . La proprietà `provider` del valore letterale dell'oggetto esegue l'override del valore della proprietà `defaultProvider`. LibMan recupera i file bootstrap dal provider unpkg.
* Un subset di [Lodash](https://lodash.com/) è stato approvato da un corpo di governance all'interno dell'organizzazione. I file *lodash. js* e *lodash. min. js* vengono recuperati dal file system locale in *C:\\Temp\\lodash\\* . I file vengono copiati nella cartella *wwwroot/lib/lodash* del progetto.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan supporta solo una versione di ogni libreria di ogni provider. Il file *libman. JSON* non riesce a convalidare lo schema se contiene due librerie con lo stesso nome di libreria per un determinato provider.

## <a name="restore-library-files"></a>Ripristinare i file di libreria

Per ripristinare i file di libreria da Visual Studio, è necessario che nella radice del progetto sia presente un file *libman. JSON* valido. I file ripristinati vengono inseriti nel progetto nel percorso specificato per ogni libreria.

I file di libreria possono essere ripristinati in un progetto ASP.NET Core in due modi:

1. [Ripristinare i file durante la compilazione](#restore-files-during-build)
1. [Ripristino manuale dei file](#restore-files-manually)

### <a name="restore-files-during-build"></a>Ripristinare i file durante la compilazione

LibMan consente di ripristinare i file di libreria definiti come parte del processo di compilazione. Per impostazione predefinita, il comportamento di *ripristino su compilazione* è disabilitato.

Per abilitare e testare il comportamento di ripristino in base alla compilazione:

* Fare clic con il pulsante destro del mouse su *libman. JSON* in **Esplora soluzioni** e selezionare **Abilita ripristino di librerie sul lato client in compilazione** dal menu di scelta rapida.
* Quando viene richiesto di installare un pacchetto NuGet, fare clic sul pulsante **Sì** . Al progetto viene aggiunto il pacchetto NuGet [Microsoft. Web. librarymanager. Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) :

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* Compilare il progetto per confermare che si verifica il ripristino del file LibMan. Il pacchetto di `Microsoft.Web.LibraryManager.Build` inserisce una destinazione MSBuild che esegue LibMan durante l'operazione di compilazione del progetto.
* Esaminare il feed di **compilazione** della finestra di **output** per un log attività LibMan:

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

Quando il comportamento di ripristino in fase di compilazione è abilitato, il menu di scelta rapida *libman. JSON* Visualizza un'opzione **Disable Restore client-side Libraries on Build** . Se si seleziona questa opzione, il riferimento al pacchetto `Microsoft.Web.LibraryManager.Build` verrà rimosso dal file di progetto. Di conseguenza, le librerie sul lato client non vengono più ripristinate in ogni compilazione.

Indipendentemente dall'impostazione di ripristino in base alla compilazione, è possibile eseguire manualmente il ripristino in qualsiasi momento dal menu di scelta rapida *libman. JSON* . Per ulteriori informazioni, vedere [ripristino manuale dei file](#restore-files-manually).

### <a name="restore-files-manually"></a>Ripristino manuale dei file

Per ripristinare manualmente i file di libreria:

* Per tutti i progetti nella soluzione:
  * Fare clic con il pulsante destro del mouse sul nome della soluzione in **Esplora soluzioni**.
  * Selezionare l'opzione **Ripristina librerie lato client** .
* Per un progetto specifico:
  * Fare clic con il pulsante destro del mouse sul file *libman. JSON* in **Esplora soluzioni**.
  * Selezionare l'opzione **Ripristina librerie lato client** .

Durante l'esecuzione dell'operazione di ripristino:

* L'icona centro stato attività (TSC) sulla barra di stato di Visual Studio verrà animata e l'operazione di ripristino verrà letta *avviata*. Facendo clic sull'icona si apre una descrizione comando che elenca le attività in background note.
* I messaggi verranno inviati alla barra di stato e al feed di **Gestione librerie** della finestra di **output** . Ad esempio:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>Elimina file di libreria

Per eseguire l'operazione di *pulizia* , che elimina i file di libreria ripristinati in precedenza in Visual Studio:

* Fare clic con il pulsante destro del mouse sul file *libman. JSON* in **Esplora soluzioni**.
* Selezionare l'opzione **Pulisci librerie lato client** .

Per evitare la rimozione accidentale di file non di libreria, l'operazione di pulizia non Elimina intere directory. Rimuove solo i file inclusi nel ripristino precedente.

Durante l'esecuzione dell'operazione di pulizia:

* L'icona TSC sulla barra di stato di Visual Studio verrà animata e verrà letta l' *operazione delle librerie client avviata*. Facendo clic sull'icona si apre una descrizione comando che elenca le attività in background note.
* I messaggi vengono inviati alla barra di stato e al feed di **Gestione librerie** della finestra di **output** . Ad esempio:

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

L'operazione di pulizia elimina solo i file dal progetto. I file di libreria vengono mantenuti nella cache per un recupero più rapido delle operazioni di ripristino future. Per gestire i file di libreria archiviati nella cache del computer locale, usare l'interfaccia della riga di comando di [LibMan](xref:client-side/libman/libman-cli).

## <a name="uninstall-library-files"></a>Disinstalla file di libreria

Per disinstallare i file di libreria:

* Aprire *libman. JSON*.
* Posizionare il punto di inserimento all'interno del valore letterale dell'oggetto `libraries` corrispondente.
* Fare clic sull'icona a bulbo chiaro visualizzata nel margine sinistro e selezionare **disinstalla \<library_name > @\<library_version >** :

  ![Opzione del menu di scelta rapida Disinstalla libreria](_static/uninstall-menu-option.png)

In alternativa, è possibile modificare e salvare manualmente il manifesto LibMan (*LibMan. JSON*). L' [operazione di ripristino](#restore-library-files) viene eseguita quando il file viene salvato. I file di libreria che non sono più definiti in *libman. JSON* vengono rimossi dal progetto.

## <a name="update-library-version"></a>Aggiornare la versione della libreria

Per verificare la presenza di una versione aggiornata della libreria:

* Aprire *libman. JSON*.
* Posizionare il punto di inserimento all'interno del valore letterale dell'oggetto `libraries` corrispondente.
* Fare clic sull'icona a bulbo chiaro visualizzata nel margine sinistro. Passare il mouse su **Verifica disponibilità aggiornamenti**.

LibMan verifica la presenza di una versione della libreria più recente della versione installata. Possono verificarsi i risultati seguenti:

* Se è già installata la versione più recente, viene visualizzato un messaggio di **Nessun aggiornamento trovato** .
* Se non è già installata, viene visualizzata la versione stabile più recente.

  ![Opzione del menu di scelta rapida Controlla aggiornamenti](_static/update-menu-option.png)

* Se è disponibile una versione provvisoria più recente di quella installata, viene visualizzata la versione non definitiva.

Per eseguire il downgrade a una versione precedente della libreria, modificare manualmente il file *libman. JSON* . Quando il file viene salvato, l' [operazione di ripristino](#restore-library-files)LibMan:

* Rimuove i file ridondanti dalla versione precedente.
* Aggiunge file nuovi e aggiornati dalla nuova versione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:client-side/libman/libman-cli>
* [Repository GitHub di LibMan](https://github.com/aspnet/LibraryManager)
