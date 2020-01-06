# <a name="custom-model-binding-demo"></a>Demo di associazione di modelli personalizzati

Testare `ByteArrayModelBinder` eseguendo l'app e registrando una stringa con codifica base64 per l'endpoint `ImageController` (`/api/image/`). Specificare le proprietà file e filename nel corpo della richiesta come form-data (usando [Postman](https://www.getpostman.com/) o uno strumento simile). È possibile usare [questa stringa di esempio](Base64String.txt). Il risultato è salvato nella cartella *wwwroot/images/upload* con il filename specificato.

Per testare l'esempio di associazione personalizzata, provare i seguenti endpoint:

* /api/authors/1
* /api/authors/2 (NOT FOUND)
* /api/boundauthors/1
* /api/boundauthors/2 (NOT FOUND)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (NO CONTENT) &ndash; Azione che non verifica la presenza di valori null e restituisce *404 Not Found*.
