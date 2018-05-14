# <a name="data-protection-key-folder"></a>Cartella chiave di protezione dati

Questo file è un segnaposto per creare una cartella condivisa per le chiavi di protezione dati.

In una distribuzione di produzione, posizionare le chiavi all'esterno di radice di sviluppo e mai controllo file in questa directory nel controllo del codice sorgente. Proteggere le chiavi di protezione dati nei file con DPAPI o un X509Certificate.

Vedere [protezione dei dati in ASP.NET Core: le API di Consumer, configurazione, API di estensibilità e implementazione](https://docs.microsoft.com/aspnet/core/security/data-protection/) per ulteriori informazioni.
