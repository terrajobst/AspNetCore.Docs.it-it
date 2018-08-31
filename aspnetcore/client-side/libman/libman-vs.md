---
title: Usare LibMan con ASP.NET Core in Visual Studio
author: scottaddie
description: Informazioni su come usare LibMan in un progetto ASP.NET Core con Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: a653b1a5c07feca8672ba38e0cda3ddc30482c5a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312179"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>Usare LibMan con ASP.NET Core in Visual Studio

Di [Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio offre supporto predefinito per [LibMan](xref:client-side/libman/index) nei progetti ASP.NET Core, tra cui:

* Supporto per la configurazione ed esecuzione di operazioni di ripristino LibMan in fase di compilazione.
* Voci di menu per l'attivazione LibMan ripristino e le operazioni di pulizia.
* Finestra di dialogo di ricerca per trovare le librerie e aggiunge i file a un progetto.
* Supporto per *libman.json*&mdash;LibMan file manifesto.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>Prerequisiti

* Visual Studio 2017 versione 15,8 o versione successiva con il **sviluppo ASP.NET e web** carico di lavoro

## <a name="add-library-files"></a>Aggiungere i file di libreria

I file di libreria possono essere aggiunti a un progetto ASP.NET Core in due modi diversi:

1. [Utilizzare la finestra di dialogo Aggiungi libreria lato Client](#use-the-add-client-side-library-dialog)
1. [Configurare manualmente le voci di file manifesto LibMan](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>Utilizzare la finestra di dialogo Aggiungi libreria lato Client

Seguire questi passaggi per installare una libreria lato client:

* Nelle **Esplora soluzioni**, fare clic sulla cartella di progetto in cui i file devono essere aggiunti. Scegli **aggiungere** > **libreria lato Client**. Il **Aggiungi libreria lato Client** viene visualizzata la finestra:

  ![Aggiungi finestra di dialogo libreria lato Client](_static/add-library-dialog.png)

* Selezionare il provider della libreria dal **Provider** elenco a discesa. CDNJS è il provider predefinito.
* Digitare il nome della libreria da recuperare nel **libreria** casella di testo. IntelliSense offre un elenco di librerie che iniziano con il testo specificato.
* Selezionare la libreria dall'elenco di IntelliSense. Si noti che il nome della libreria viene aggiunto il suffisso di `@` simboli e la versione stabile più recente noto per il provider selezionato.
* Decidere quali file da includere:
  * Selezionare il **includere tutti i file di libreria** pulsante di opzione per includere tutti i file della libreria.
  * Selezionare il **scegliere file specifici** pulsante di opzione per includere un sottoinsieme di file della libreria. Quando il pulsante di opzione è selezionato, l'albero di selettore file è abilitato. Selezionare le caselle alla sinistra di nomi di file da scaricare.
* Specificare la cartella di progetto per archiviare i file nei **percorso di destinazione** casella di testo. Sotto forma di suggerimento, archiviare tutte le librerie in una cartella separata.

  Suggeriti **percorso di destinazione** cartella è basata sulla posizione da cui avviare la finestra di dialogo:

  * Se avviato dalla radice del progetto:
    * *Wwwroot/lib* viene usato se *wwwroot* esiste.
    * *lib* viene usato se *wwwroot* non esiste.
  * Se avviata da una cartella di progetto, viene utilizzato il nome della cartella corrispondente.

  Il suggerimento di cartella viene aggiunto il suffisso con il nome della libreria. La tabella seguente illustra i suggerimenti di cartella quando si installa jQuery in un progetto Razor Pages.
  
  |Posizione di avvio                           |Cartella suggerita      |
  |------------------------------------------|----------------------|
  |radice del progetto (se *wwwroot* esiste)        |*Wwwroot/lib/jquery /* |
  |radice del progetto (se *wwwroot* non esiste) |*jquery/lib /*         |
  |*Pagine* cartella nel progetto                 |*Pagine/jquery /*       |

* Scegliere il **installare** pulsante per scaricare i file, per la configurazione nel *libman.json*.
* Esaminare i **Gestione librerie** feed del **Output** finestra per i dettagli di installazione. Ad esempio:

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

### <a name="manually-configure-libman-manifest-file-entries"></a>Configurare manualmente le voci di file manifesto LibMan

Tutte le operazioni di LibMan in Visual Studio sono basate sul contenuto del manifesto LibMan della radice del progetto (*libman.json*). È possibile modificare manualmente *libman.json* per configurare i file di libreria per il progetto. Visual Studio Ripristina tutti i file di libreria, una volta *libman.json* viene salvato.

Per aprire *libman.json* per la modifica, esistono le seguenti opzioni:

* Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.
* Fare clic sul progetto in **Esplora soluzioni** e selezionare **Gestisci librerie lato Client**. **&#8224;**
* Selezionare **gestire le librerie Client-Side** Visual Studio **progetto** menu. **&#8224;**

**&#8224;** Se il *libman.json* file non esiste già nella radice del progetto, verrà creato con il contenuto del modello di elemento predefinito.

Visual Studio offre JSON completo, supporto, ad esempio la colorazione di modifica, formattazione, IntelliSense e convalida dello schema. Schema JSON del manifesto LibMan si trova in corrispondenza [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).

Con il file di manifesto seguente, LibMan recupera i file per la configurazione definita nel `libraries` proprietà. Una spiegazione dei valori letterali di oggetto definiti all'interno di `libraries` segue:

* Un subset delle [jQuery](https://jquery.com/) versione 3.3.1 viene recuperato dal provider CDNJS. Il subset è definito nel `files` proprietà&mdash;*jquery.min.js*, *jquery.js*, e *jquery.min.map*. I file vengono inseriti del progetto *wwwroot/lib/jquery* cartella.
* Nella sua totalità [Bootstrap](https://getbootstrap.com/) versione 4.1.3 viene recuperato e inserita in un *wwwroot/lib/bootstrap* cartella. Del valore letterale di oggetto `provider` override delle proprietà di `defaultProvider` valore della proprietà. LibMan recupera i file Bootstrap del provider unpkg.
* Un subset delle [Lodash](https://lodash.com/) è stata approvata da un corpo che implementano la governance all'interno dell'organizzazione. Il *lodash.js* e *lodash.min.js* i file vengono recuperati dal file system locale a *c:\\temp\\lodash\\*. I file vengono copiati nel progetto *wwwroot/lib/lodash* cartella.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan supporta solo una versione di tutte le librerie da ogni provider. Il *libman.json* file ha esito negativo di convalida dello schema, se contiene due raccolte con lo stesso nome di libreria per un determinato provider.

## <a name="restore-library-files"></a>Ripristinare i file di libreria

Per ripristinare i file di libreria dall'interno di Visual Studio, è necessario un valore valido *libman.json* file nella radice del progetto. File ripristinati vengono inseriti nel progetto in corrispondenza della posizione specificata per ogni raccolta.

I file di libreria possono essere ripristinati in un progetto ASP.NET Core in due modi:

1. [Ripristinare i file durante la compilazione](#restore-files-during-build)
1. [Ripristinare i file manualmente](#restore-files-manually)

### <a name="restore-files-during-build"></a>Ripristinare i file durante la compilazione

LibMan puoi ripristinare i file di libreria definiti come parte del processo di compilazione. Per impostazione predefinita, il *ripristino in fase di compilazione* comportamento è disabilitato.

Per abilitare e testare il comportamento di ripristino in fase di compilazione:

* Fare doppio clic su *libman.json* nelle **Esplora soluzioni** e selezionare **abilitare ripristino configurazione di librerie lato Client in fase di compilazione** dal menu di scelta rapida.
* Scegliere il **Sì** pulsante quando viene richiesto di installare un pacchetto NuGet. Il [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) pacchetto NuGet viene aggiunto al progetto:

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* Compilare il progetto per verificare che il ripristino dei file LibMan si verifica. Il `Microsoft.Web.LibraryManager.Build` pacchetto inserisce una destinazione di MSBuild che esegue LibMan durante l'operazione di compilazione del progetto.
* Esaminare la **compilare** feed del **Output** finestra per il registro attività LibMan:

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

Quando è abilitato il comportamento di ripristino in fase di compilazione, il *libman.json* menu di scelta rapida visualizza un **disabilitare ripristino configurazione di librerie lato Client in fase di compilazione** opzione. Se si seleziona questa opzione rimuove il `Microsoft.Web.LibraryManager.Build` riferimento dal file di progetto del pacchetto. Di conseguenza, le librerie lato client non vengono più ripristinate in ogni fase di compilazione.

Indipendentemente dall'impostazione di ripristino in fase di compilazione, è possibile ripristinare manualmente in qualsiasi momento dal *libman.json* menu di scelta rapida. Per altre informazioni, vedere [ripristinare manualmente i file](#restore-files-manually).

### <a name="restore-files-manually"></a>Ripristinare i file manualmente

Per ripristinare manualmente i file di libreria:

* Per tutti i progetti nella soluzione:
  * Fare clic sul nome della soluzione in **Esplora soluzioni**.
  * Selezionare il **ripristinare librerie lato Client** opzione.
* Per un progetto specifico:
  * Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.
  * Selezionare il **ripristinare librerie lato Client** opzione.

Durante l'esecuzione dell'operazione di ripristino:

* Icona del centro stato attività (TSC) sulla barra di stato di Visual Studio verrà animato e leggerà *avviato l'operazione di ripristino*. Facendo clic sull'icona viene visualizzata la descrizione comando Elenca le attività in background noti.
* Messaggi verranno inviati nella barra di stato e il **Gestione librerie** feed del **Output** finestra. Ad esempio:

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

## <a name="delete-library-files"></a>Eliminare i file di libreria

Per eseguire la *pulita* , operazione che comporta l'eliminazione di file di libreria precedentemente ripristinati in Visual Studio:

* Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.
* Selezionare il **pulita librerie lato Client** opzione.

Per evitare la rimozione accidentale di file non di libreria, l'operazione di pulizia non comporta l'eliminazione intere directory. Rimuove solo i file che sono stati inclusi nel ripristino precedente.

Mentre l'operazione di pulizia è in esecuzione:

* L'icona TSC sulla barra di stato di Visual Studio verrà animato e leggerà *operazione le librerie Client avviata*. Facendo clic sull'icona viene visualizzata la descrizione comando Elenca le attività in background noti.
* I messaggi vengono inviati nella barra di stato e il **Gestione librerie** feed del **Output** finestra. Ad esempio:

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

L'operazione di pulizia Elimina solo i file dal progetto. File della libreria di rimangono nella cache per il recupero più veloce sulle operazioni di ripristino future. Per gestire i file di libreria memorizzati nella cache del computer locale, usare il [LibMan CLI](xref:client-side/libman/libman-cli).

## <a name="uninstall-library-files"></a>Disinstallare i file di libreria

Per disinstallare i file di libreria:

* Aprire *libman.json*.
* Posizionare il cursore all'interno di corrispondente `libraries` valore letterale di oggetto.
* Scegliere l'icona lampadina visualizzata nel margine sinistro e selezionare **disinstallazione \<library_name > @\<library_version >**:

  ![Disinstallare l'opzione del menu di scelta rapida della libreria](_static/uninstall-menu-option.png)

In alternativa, è possibile manualmente modificare e salvare il manifesto LibMan (*libman.json*). Il [l'operazione di ripristino](#restore-library-files) viene eseguito quando viene salvato il file. File di libreria non è più definiti nella *libman.json* vengono rimossi dal progetto.

## <a name="update-library-version"></a>Versione di aggiornamento libreria

Per controllare una versione di aggiornamento della libreria:

* Aprire *libman.json*.
* Posizionare il cursore all'interno di corrispondente `libraries` valore letterale di oggetto.
* Scegliere l'icona lampadina visualizzata nel margine sinistro. Passare il mouse su **verificare la presenza di aggiornamenti**.

LibMan controlla una versione della libreria più recente della versione installata. Possono verificarsi i seguenti risultati:

* Oggetto **non sono stati trovati aggiornamenti** messaggio viene visualizzato se è già installata la versione più recente.
* La versione stabile più recente viene visualizzata se non già installata.

  ![Verificare la presenza di opzione del menu di scelta rapida degli aggiornamenti](_static/update-menu-option.png)

* Se è disponibile una versione non definitiva più recente della versione installata, viene visualizzata la versione non definitiva.

Per effettuare il downgrade a una versione precedente di libreria, modificare manualmente il *libman.json* file. Quando il file viene salvato, la LibMan [l'operazione di ripristino](#restore-library-files):

* Rimuove i file con ridondanza della versione precedente.
* Aggiunge i file nuovi e aggiornati della nuova versione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:client-side/libman/libman-cli>
* [Repository GitHub di LibMan](https://github.com/aspnet/LibraryManager)
