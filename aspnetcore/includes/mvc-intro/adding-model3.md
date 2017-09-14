
## <a name="test-the-app"></a>Eseguire il test dell'applicazione

* Eseguire l'app e toccare il collegamento **Mvc film**.
* Toccare il collegamento **Crea nuovo** e creare un filmato.

  ![Creare una vista con i campi per genere, prezzo, data di rilascio e titolo](../../tutorials/first-mvc-app/adding-model/_static/movies.png)

* Potrebbe non essere possibile immettere separatori decimali o virgole nel campo `Price`. Per supportare la [convalida jQuery](http://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app. Vedere https://github.com/aspnet/Docs/issues/4076 e [Risorse aggiuntive](#additional-resources) per ulteriori informazioni. Per il momento, immettere solo numeri interi come 10.

<a name="displayformatdatelocal"></a>

* In alcune impostazioni locali è necessario specificare il formato di data. Vedere il codice evidenziato di seguito.

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

`DataAnnotations` verrà trattato più avanti nell'esercitazione.

Toccando **Crea** il modulo viene inviato al server, dove le informazioni relative al filmato vengono salvate in un database. L'applicazione reindirizza all'URL */Movies*, in cui vengono visualizzate le informazioni relative al filmato appena creato.

![Vista di Movies che visualizza l'elenco dei filmati appena creati](../../tutorials/first-mvc-app/adding-model/_static/h.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.
