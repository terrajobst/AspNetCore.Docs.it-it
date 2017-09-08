# <a name="custom-model-binding-demo"></a>Demo di associazione di un modello personalizzato

È possibile testare il `ByteArrayModelBinder` eseguendo l'applicazione e la registrazione di una stringa con codifica base64 per l'endpoint ImageController (/ api/immagine /). È necessario specificare il file e il nome file proparties nella richiesta corpo come dati del modulo (usando Postman o uno strumento simile). È possibile utilizzare [questa stringa di esempio](Base64String.txt). Il risultato verrà salvato nella cartella wwwroot/immagini/caricamento con il nome del file specificato.

Per testare l'esempio di associazione personalizzata, provare i seguenti endpoint: /api/authors/1 /api/authors/2 (non trovato) /api/boundauthors/1 /api/boundauthors/2 (non trovato) /api/boundauthors/get/2 /api/boundauthors/get/1 (nessun contenuto) - non verifica questa azione null e restituire non trovato
