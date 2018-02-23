# <a name="custom-model-binding-demo"></a>Demo di associazione di modelli personalizzati

È possibile testare `ByteArrayModelBinder` eseguendo l'applicazione e la registrazione di una stringa con codifica base64 per l'endpoint ImageController (/api/immagine/). È necessario specificare il file e le proprietà filename nella richiesta Corpo come dati del modulo (usando Postman o uno strumento simile). È possibile usare [questa stringa di esempio](Base64String.txt). Il risultato verrà salvato nella cartella wwwroot/images/upload con il filename specificato.

Per testare l'esempio di associazione personalizzata, provare i seguenti endpoint: /api/authors/1 /api/authors/2 (non trovato) /api/boundauthors/1 /api/boundauthors/2 (non trovato) /api/boundauthors/get/2 /api/boundauthors/get/1 (nessun contenuto) - questa azione non verifica i valori null e restituisce un valore non trovato
