## <a name="overview"></a>Panoramica

Questa esercitazione consente di creare l'API seguente:

|API | Descrizione | Corpo della richiesta | Corpo della risposta |
|--- | ---- | ---- | ---- |
|GET /api/todo | Ottiene tutti gli elementi attività | nessuno | Matrice di elementi attività|
|GET /api/todo/{id} | Ottiene un elemento in base all'ID | nessuno | Elemento attività|
|POST /api/todo | Aggiunge un nuovo elemento | Elemento attività | Elemento attività |
|PUT /api/todo/{id} | Aggiorna un elemento esistente &nbsp; | Elemento attività | nessuno |
|DELETE /api/todo/{id} &nbsp; &nbsp; | Elimina un elemento &nbsp; &nbsp; | nessuno | nessuno|

Il diagramma seguente mostra la struttura base dell'app.

![Il client è rappresentato da una casella a sinistra e invia una richiesta e riceve una risposta dall'applicazione, una casella a destra. Nella riquadro dell'applicazione tre caselle rappresentano il controller, il modello e il livello di accesso ai dati. La richiesta viene ricevuta dal controller dell'applicazione e vengono eseguite operazioni di lettura/scrittura tra il controller e il livello di accesso ai dati. Il modello viene serializzato e restituito al client nella risposta.](../../tutorials/first-web-api/_static/architecture.png)

* Tutti gli elementi che usano l'API Web, come le app per dispositivi mobili, i browser e così via, possono essere definiti client. In questa esercitazione non viene creato un client, ma viene usato [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) come client per testare l'app.

* Un *modello* è un oggetto che rappresenta i dati nell'app. In questo caso l'unico modello è un elemento attività. I modelli sono rappresentati come classi C#, noti anche come oggetti **P**lain **O**ld **C**# **O**bject (POCO).

* Un *controller* è un oggetto che gestisce le richieste HTTP e crea la risposta HTTP. Questa app ha un singolo controller.

* Per mantenere semplice l'esercitazione, l'app non usa un database persistente. L'app di esempio archivia gli elementi attività in un database in memoria.
