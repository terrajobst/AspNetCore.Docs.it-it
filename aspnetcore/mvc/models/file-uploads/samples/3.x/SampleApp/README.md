# <a name="upload-files-sample-app"></a>App di esempio per il caricamento di file

Questa app di esempio illustra i concetti descritti nell'argomento [caricare file in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads) .

## <a name="security-considerations"></a>Considerazioni relative alla sicurezza

Prestare attenzione quando si fornisce agli utenti la possibilità di caricare file in un server. Gli utenti malintenzionati possono eseguire attacchi [di tipo Denial of Service](/windows-hardware/drivers/ifs/denial-of-service) , tentare di caricare virus o malware oppure provare a compromettere le reti e i server in altri modi.

I passaggi di sicurezza che riducono la probabilità di un attacco riuscito sono:

* Caricare i file in un'area dedicata di caricamento di file nel sistema, preferibilmente in un'unità non di sistema. L'uso di un percorso dedicato rende più semplice l'imposizione di misure di sicurezza nei file caricati. Disabilitare le autorizzazioni di esecuzione per il percorso di caricamento del file. &dagger;
* Non salvare mai i file caricati nella stessa struttura di directory dell'app. &dagger;
* Usare un nome file sicuro determinato dall'app. Non usare un nome file fornito dall'input dell'utente o il nome file non attendibile del file caricato. &dagger;
* Consenti solo un set specifico di estensioni di file approvate. &dagger;
* Controllare la firma del formato di file per impedire a un utente di caricare un file mascherato (ad esempio, il caricamento di un file *exe* con estensione *txt* ). &dagger;
* Verificare che anche i controlli lato client vengano eseguiti sul server. I controlli lato client sono facili da aggirare. &dagger;
* Controllare le dimensioni del caricamento e impedire i caricamenti maggiori del previsto. &dagger;
* Quando i file non devono essere sovrascritti da un file caricato con lo stesso nome, controllare il nome del file sul database o sull'archiviazione fisica prima di caricare il file.
* **Eseguire uno scanner virus/malware sul contenuto caricato prima che il file venga archiviato.**

l'app di esempio &dagger;The illustra un approccio che soddisfa i criteri.

> [!WARNING]
> Il caricamento di codice dannoso in un sistema è spesso il primo passaggio per l'esecuzione di codice in grado di:
>
> * Prendere il controllo totale di un sistema.
> * Sovraccaricare un sistema con il risultato che si verifica un arresto anomalo del sistema.
> * Compromettere i dati di sistemi o utenti.
> * Applicare i graffiti a un'interfaccia utente pubblica.
>
> Per informazioni sulla riduzione della superficie di attacco quando si accettano file dagli utenti, vedere le risorse seguenti:
>
> * [Caricamento di file senza restrizioni](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * Sicurezza [Azure: Verificare che siano presenti i controlli appropriati quando si accettano file dagli utenti @ no__t-0

Per ulteriori informazioni, vedere [caricare file in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="how-to-use-the-sample"></a>Come usare l'esempio

Nel file *appSettings. JSON* :

1. Imposta il percorso per i file archiviati (`StoredFilesPath`).

   * L'app di esempio imposta il valore su `c:\\files`, che presuppone che esista una cartella denominata *files* nella radice dell'unità C: del sistema.
   * È necessario specificare un percorso esistente. Creare una cartella *file* nell'unità C: del sistema o impostare il percorso su un percorso appropriato.
   * Il processo dell'app richiede autorizzazioni di lettura/scrittura per il percorso.
   * **IMPORTANTE!** Disabilitare le autorizzazioni di esecuzione per tutti gli utenti nel percorso.

1. Impostare il limite delle dimensioni del file (`FileSizeLimit`) in byte. Il valore predefinito dell'app di esempio `2097152` (2.097.152 byte) consente il caricamento di file fino a 2 MB.
