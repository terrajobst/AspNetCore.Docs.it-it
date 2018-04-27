---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilare le API REST con ASP.NET Web API | Documenti Microsoft
author: rick-anderson
description: Negli ultimi anni, è diventato evidente che HTTP non è presente solo per mettere a disposizione le pagine HTML. È anche una potente piattaforma per la compilazione di API Web, utilizzando un numero limitato di o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ded549109ca6e7ad806f1c3f53387766527e5a94
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Compilare le API REST con ASP.NET Web API
====================
Da [categorie Web Team](https://twitter.com/webcamps)

> Negli ultimi anni, è diventato evidente che HTTP non è presente solo per mettere a disposizione le pagine HTML. È inoltre una potente piattaforma per la compilazione di API Web, utilizzando un numero limitato di verbi (GET, POST e così via) e alcuni concetti semplici, ad esempio *URI* e *intestazioni*. API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP. Poiché è basato sul runtime di ASP.NET MVC, API Web gestisce automaticamente i dettagli di basso livello di trasporto HTTP. Allo stesso tempo, API Web espone naturalmente il modello di programmazione HTTP. In effetti, è uno degli obiettivi di API Web *non* astrarre la realtà di HTTP. Di conseguenza, l'API Web è flessibile e di facile estendere. In questa esercitazione pratica, si utilizzerà API Web per compilare una semplice API REST per un'applicazione Gestione contatti. Si creerà anche un client per utilizzare l'API. Lo stile dell'architettura REST ha dimostrato di essere un metodo efficace per sfruttare HTTP, anche se non è certamente l'approccio valido solo per HTTP. Gestione contatti esporrà il RESTful per elencare, aggiunta ed eliminazione di contatti, tra gli altri. Questa esercitazione richiede una conoscenza di base HTTP, REST e si suppone una conoscenza di base di HTML, JavaScript e jQuery.
> 
> > [!NOTE]
> > Il sito Web ASP.NET dispone di un'area dedicata per il framework di ASP.NET Web API nel [ https://asp.net/web-api ](https://asp.net/web-api). Questo sito continuerà a fornire le informazioni più recenti, esempi e le notizie relative all'API Web, quindi archiviarlo spesso se si desidera approfondire la conoscenza arte della creazione di API Web personalizzate disponibili per qualsiasi framework di sviluppo o dispositivo.
> > 
> > ASP.NET Web API, simile a ASP.NET MVC 4, ha una grande flessibilità in termini di separare il livello di servizio dai controller di che consente di utilizzare alcuni dei framework Dependency Injection disponibili piuttosto semplice. È un buon esempio in MSDN che illustra come utilizzare Ninject per l'inserimento di dipendenze in un progetto di API Web ASP.NET che è possibile scaricarlo da [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Implementare un'API Web RESTful
- Chiamare l'API da un client HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice B](#AppendixB) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**L'installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: con i frammenti di codice](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include le procedure descritte di seguito:

1. [Esercizio 1: Creare una sola lettura Web API](#Exercise1)
2. [Esercizio 2: Creare un'API Web di lettura/scrittura](#Exercise2)
3. [Esercizio 3: Utilizzare l'API Web da un Client HTML](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio. Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Esercizio 1: Creare una sola lettura Web API

In questo esercizio si implementerà i metodi GET di sola lettura per la gestione di contatto.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Attività 1 - Creazione del progetto di API

In questa attività, utilizzare i nuovi modelli di progetto web ASP.NET per creare un'applicazione web API Web.

1. Eseguire **Visual Studio Express 2012 per Web**, a tale scopo passare alla **avviare** e tipo **Visual Studio Express per Web** premere **invio**.
2. Dal **File** dal menu **nuovo progetto**. Selezionare il **Visual c# | Web** tipo nella visualizzazione ad albero di progetto tipo di progetto, quindi selezionare il **applicazione Web ASP.NET MVC 4** tipo di progetto. Impostare il progetto **nome** a *ContactManager* e **Nome soluzione** a *iniziare*, quindi fare clic su **OK**.

    ![Creazione di un nuovo progetto ASP.NET MVC 4.0 Web Application](build-restful-apis-with-aspnet-web-api/_static/image1.png "la creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione")

    *Creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione*
3. Nella finestra di tipo di progetto ASP.NET MVC 4, selezionare il **API Web** tipo di progetto. Fare clic su **OK**.

    ![Che specifica il tipo di progetto di Web API](build-restful-apis-with-aspnet-web-api/_static/image2.png "che specifica il tipo di progetto API Web")

    *Che specifica il tipo di progetto API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Attività 2 - Creazione i controller API Contact Manager

In questa attività si creerà le classi controller in cui risiederà metodi dell'API.

1. Eliminare il file denominato **ValuesController.cs** all'interno di **controller** cartella dal progetto.
2. Fare doppio clic su di **controller** cartella nel progetto e selezionare **Aggiungi | Controller** dal menu di scelta rapida.

    ![Aggiunge un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "aggiunge un nuovo controller al progetto")

    *Aggiunge un nuovo controller al progetto*
3. Nel **Aggiungi Controller** finestra di dialogo visualizzata, selezionare **Controller API vuoto** dal menu modello. Nome della classe controller **ContactController**. Quindi, fare clic su **Aggiungi.**

    ![Utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller Web API](build-restful-apis-with-aspnet-web-api/_static/image4.png "utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web")

    *Utilizzando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web*
4. Aggiungere il codice seguente per il **ContactController**.

    (- Frammento di codice *Web API Lab - Ex01 - ottenere il metodo API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Premere **F5** il debug dell'applicazione. La home page predefinita per un progetto di API Web compariranno.

    ![La home page predefinita di un'applicazione ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la home page predefinita di un'applicazione ASP.NET Web API")

    *La home page predefinita di un'applicazione ASP.NET Web API*
6. Nella finestra di Internet Explorer, premere il **F12** tasto per aprire la **gli strumenti di sviluppo** finestra. Fare clic sul **rete** scheda e quindi scegliere il **Avvia acquisizione** pulsante per avviare l'acquisizione del traffico di rete nella finestra.

    ![Acquisizione di rete aprire la scheda di rete e di avvio](build-restful-apis-with-aspnet-web-api/_static/image6.png "all'apertura della scheda di rete e di avvio di acquisizione di rete")

    *Aprire la scheda di rete e avviare l'acquisizione di rete*
7. Aggiungere l'URL nella barra degli indirizzi del browser con **/api/contatto** e premere INVIO. I dettagli di trasmissione verranno visualizzata la finestra di acquisizione di rete. Si noti che è di tipo MIME della risposta **application/json**. Ciò dimostra come formato di output predefinito è JSON.

    ![Visualizzazione dell'output della richiesta API Web nella visualizzazione rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualizza l'output della richiesta API Web nella visualizzazione rete")

    *Visualizzazione dell'output della richiesta API Web nella visualizzazione rete*

    > [!NOTE]
    > Comportamento predefinito di Internet Explorer 10 a questo punto è necessario chiedere se si desidera salvare o aprire il flusso risultante dalla chiamata all'API Web. L'output sarà un file di testo contenente il risultato JSON della chiamata di URL di Web API. Non annullare la finestra di dialogo per poter visualizzare il contenuto della risposta tramite una finestra degli strumenti sviluppatori.
8. Fare clic su di **passare alla visualizzazione dettagliata** pulsante per visualizzare ulteriori dettagli sulla risposta di questa chiamata API.

    ![Passare alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "passare alla visualizzazione dettagli")

    *Passare alla visualizzazione dettagliata*
9. Fare clic su di **corpo della risposta** scheda per visualizzare il testo di risposta JSON effettivo.

    ![Visualizzazione JSON output di testo in network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "visualizzazione JSON output di testo in network monitor")

    *Visualizzazione del testo di output JSON in network monitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Attività 3: creazione di modelli di contatto e aumentano il Controller di contatto

In questa attività si creerà le classi controller in cui risiederà metodi dell'API.

1. Fare doppio clic su di **modelli** cartella e selezionare **Aggiungi | Classe...**  dal menu di scelta rapida.

    ![Aggiunta di un nuovo modello per l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image10.png "aggiunta di un nuovo modello per l'applicazione web")

    *Aggiunta di un nuovo modello per l'applicazione web*
2. Nel **Aggiungi nuovo elemento** finestra di dialogo Nome nuovo file **Contact.cs** e fare clic su **Aggiungi.**

    ![Creazione del nuovo file di classe contatto](build-restful-apis-with-aspnet-web-api/_static/image11.png "creazione del nuovo file di classe di contatto")

    *Creazione del nuovo file di classe di contatto*
3. Aggiungere il codice evidenziato di seguito per il **contatto** classe.

    (- Frammento di codice *Web API Lab - Ex01 - contatto classe*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. Nel **ContactController** classe, selezionare la parola **stringa** nella definizione di metodo del **ottenere** (metodo) e digitare la parola *contatto*. Dopo aver digitata la parola, un indicatore verrà visualizzato all'inizio della parola **contatto**. Uno tenendo premuto il **Ctrl** chiave e premere il tasto punto (.) o fare clic sull'icona con il mouse per aprire la finestra di dialogo assistenza nell'editor di codice, compilare automaticamente il **utilizzando** direttiva per i modelli spazio dei nomi.

    ![Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi*
5. Modificare il codice per il **ottenere** metodo in modo che restituisca una matrice di istanze di modello di contatto.

    (- Frammento di codice *Web API Lab - Ex01 - restituzione di un elenco di contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Premere **F5** il debug dell'applicazione web nel browser. Per visualizzare le modifiche apportate all'output di risposta dell'API, eseguire la procedura seguente.

   1. Una volta aperto il browser, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.
   2. Fare clic su di **rete** scheda.
   3. Premere il **Avvia acquisizione** pulsante.
   4. Aggiungere il suffisso URL **/api/contatto** per l'URL nella barra degli indirizzi e premere il **invio** chiave.
   5. Premere il **passare alla visualizzazione dettagliata** pulsante.
   6. Selezionare il **corpo della risposta** scheda. Dovrebbe essere una stringa JSON che rappresenta il formato serializzato di una matrice di istanze di contatto.

      ![JSON serializzato output di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializzato output di una chiamata al metodo API Web complessa")

      *Output JSON serializzato di una chiamata al metodo API Web complessa*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Attività 4 - funzionalità di estrazione in un livello di servizio

Questa attività verrà illustrato come estrarre funzionalità in un livello di servizio per semplificare per gli sviluppatori separare le funzionalità del servizio dal livello dei controller, consentendo la possibilità di riutilizzo dei servizi che effettivamente svolgono il lavoro.

1. Creare una nuova cartella nella radice della soluzione e denominarlo **servizi**. A tale scopo, fare doppio clic su **ContactManager** progetto, selezionare **Aggiungi** | **nuova cartella**, denominarla *servizi*.

    ![Cartella servizi di creazione](build-restful-apis-with-aspnet-web-api/_static/image14.png "cartella creazione di servizi")

    *La creazione della cartella di servizi*
2. Fare doppio clic su di **servizi** cartella e selezionare **Aggiungi | Classe...**  dal menu di scelta rapida.

    ![Aggiunta di una nuova classe per la cartella servizi](build-restful-apis-with-aspnet-web-api/_static/image15.png "aggiunta una nuova classe nella cartella Services")

    *Aggiunta di una nuova classe nella cartella Services*
3. Quando il **Aggiungi nuovo elemento** viene visualizzata la finestra, denominare la nuova classe **ContactRepository** e fare clic su **Aggiungi**.

    ![Creazione di un file di classe per contenere il codice per il livello di servizio Repository contatto](build-restful-apis-with-aspnet-web-api/_static/image16.png "creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti")

    *Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti*
4. Aggiungere un tramite la direttiva per il **ContactRepository.cs** file da includere lo spazio dei nomi modelli.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. Aggiungere il codice evidenziato di seguito per il **ContactRepository.cs** file per implementare il metodo GetAllContacts.

    (- Frammento di codice *Web API Lab - Ex01 - Repository di contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Aprire il **ContactController.cs** file se non è già aperto.
7. Aggiungere la seguente istruzione using alla sezione della dichiarazione dello spazio dei nomi del file.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. Aggiungere il codice evidenziato di seguito per il **ContactController.cs** classe per aggiungere un campo privato per rappresentare l'istanza dell'archivio, in modo che il resto della classe i membri possono eseguire usare dell'implementazione del servizio.

    (- Frammento di codice *API Lab - Ex01 - contatto Controller Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Modifica il **ottenere** metodo in modo che rende utilizzare il servizio di repository di contatti.

    (- Frammento di codice *restituzione di un elenco di contatti tramite l'archivio Web API Lab - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Inserire un punto di interruzione di **ContactController**del **ottenere** la definizione di metodo.

   ![Aggiunta di punti di interruzione al controller di contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "aggiunta di punti di interruzione al controller di contatto")

   *Aggiunta di punti di interruzione al controller di contatto*
11. Premere **F5** per eseguire l'applicazione.
12. Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.
13. Fare clic su di **rete** scheda.
14. Fare clic su di **Avvia acquisizione** pulsante.
15. Aggiungere l'URL nella barra degli indirizzi con il suffisso **/api/contatto** e premere **invio** per caricare il controller API.
16. Visual Studio 2012 è presente un'interruzione di una volta **ottenere** metodo inizia l'esecuzione.

   ![All'interno del metodo Get di rilievo](build-restful-apis-with-aspnet-web-api/_static/image18.png "interruzione all'interno del metodo Get")

   *Interruzione all'interno del metodo Get*
17. Premere **F5** per continuare.
18. Tornare a Internet Explorer se non è già attivo. Si noti la finestra di acquisizione di rete.

    ![Rete di visualizzazione che mostra i risultati della chiamata API Web di Internet Explorer](build-restful-apis-with-aspnet-web-api/_static/image19.png "vista in Internet Explorer che mostra i risultati della chiamata API Web di rete")

    *Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata API Web*
19. Fare clic su di **passare alla visualizzazione dettagliata** pulsante.
20. Fare clic su di **corpo della risposta** scheda. Si noti l'output JSON della chiamata API, e come rappresenta due contatti recuperati tramite il livello di servizio.

    ![Visualizza l'output JSON dall'API Web nella finestra di strumenti per sviluppatori](build-restful-apis-with-aspnet-web-api/_static/image20.png "visualizzando l'output JSON dalle API Web nella finestra di strumenti per sviluppatori")

    *Visualizza l'output JSON dall'API Web nella finestra di strumenti per sviluppatori*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Esercizio 2: Creare un'API Web di lettura/scrittura

In questo esercizio verrà implementata POST e PUT metodi per la gestione di contatto abilitare la funzionalità con le funzionalità di modifica dei dati.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Attività 1: aprire il progetto API Web

In questa attività si prepara migliorare il progetto API Web creato nell'esercizio 1, in modo che possa accettare l'input dell'utente.

1. Eseguire **Visual Studio Express 2012 per Web**, a tale scopo passare alla **avviare** e tipo **Visual Studio Express per Web** premere **invio**.
2. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex02-ReadWriteWebAPI/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire il **Services/ContactRepository.cs** file.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Attività 2: aggiunta di funzionalità di persistenza dei dati per l'implementazione del Repository di contatti

In questa attività, si verrà aumentano la classe ContactRepository del progetto API Web creato nell'esercizio 1 in modo che possa rendere persistente e accettare l'input dell'utente e le nuove istanze di contatto.

1. La costante seguente per aggiungere il **ContactRepository** classe per rappresentare il nome del nome del server web cache elemento chiave più avanti in questo esercizio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Aggiungere un costruttore per il **ContactRepository** contenente il codice seguente.

    (- Frammento di codice *Web API Lab - Ex02 - Repository di contatti costruttore*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modificare il codice per il **GetAllContacts** metodo come illustrato di seguito.

    (- Frammento di codice *Web API Lab - Ex02 - ottenere tutti i contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Questo esempio è a scopo dimostrativo e utilizza la cache del server web come un supporto di archiviazione, in modo che i valori verranno messe a disposizione più client contemporaneamente, anziché utilizzare un meccanismo di archiviazione di sessione o una durata di archiviazione richiesta. Una possibile utilizzare Entity Framework, l'archiviazione XML o qualsiasi altra varietà al posto di cache del server web.
4. Implementare un nuovo metodo denominato **SaveContact** per il **ContactRepository** classe per le operazioni di salvataggio di un contatto. Il **SaveContact** metodo deve accettare un singolo **contatto** parametro e restituire un valore booleano valore indicante l'esito positivo o negativo.

    (- Frammento di codice *Web API Lab - Ex02 - implementazione del metodo SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Esercizio 3: Utilizzare l'API Web da un Client HTML

In questo esercizio si creerà un client HTML per chiamare l'API Web. Questo client faciliterà scambio di dati con l'API Web con JavaScript e i risultati verrà visualizzati in un web browser utilizzando il markup HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Attività 1: modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti

In questa attività si modificherà la visualizzazione dell'indice predefinito dell'applicazione web per supportare il requisito di visualizzazione dell'elenco di contatti esistenti in un browser HTML.

1. Aprire **Visual Studio Express 2012 per Web** se non è già aperto.
2. Aprire il **iniziare** soluzione disponibile all'indirizzo **origine/Ex03-ConsumingWebAPI/Begin/** cartella. In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto. Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto. È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire il **cshtml** file si trova in **Views/Home** cartella.
4. Sostituire il codice HTML all'interno dell'elemento div con id **corpo** in modo che risulti simile al codice seguente.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. Aggiungere il codice Javascript seguente nella parte inferiore del file per eseguire la richiesta HTTP nell'API Web.


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. Aprire il **ContactController.cs** file se non è già aperto.
7. Inserire un punto di interruzione di **ottenere** metodo il **ContactController** classe.

    ![Inserendo un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "inserendo un punto di interruzione nel metodo Get del controller API")

    *Inserendo un punto di interruzione nel metodo Get del controller API*
8. Premere **F5** per eseguire il progetto. Il browser verrà caricato il documento HTML.

    > [!NOTE]
    > Verificare che si sta esplorando l'URL radice dell'applicazione.
9. Dopo il caricamento della pagina e viene eseguito il codice JavaScript, il punto di interruzione verrà raggiunto e sospende l'esecuzione del codice nel controller.

    ![Debug di chiamate API Web usando Visual Studio Express per Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debug chiamate API Web usando Visual Studio Express per Web")

    *Debug di una chiamata API Web usando Visual Studio Express 2012 per Web*
10. Rimuovere il punto di interruzione e premere **F5** o sulla barra degli strumenti Debug **continua** pulsante per continuare il caricamento della visualizzazione nel browser. Una volta completata la chiamata all'API Web dovrebbe essere i contatti restituiti dall'API Web chiamare visualizzati come elementi di elenco nel browser.

    ![I risultati della chiamata API visualizzata nel browser come elementi di elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "i risultati della chiamata API visualizzata nel browser come elementi di elenco")

    *Risultati della chiamata API visualizzata nel browser come elementi di elenco*
11. Terminare il debug.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Attività 2: modifica della visualizzazione dell'indice per fornire un'interfaccia utente grafica per la creazione di contatti

In questa attività, si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC. Un modulo verrà aggiunto alla pagina HTML che l'input dell'utente di acquisire e inviarlo all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo controller API Web per raccogliere data dalla GUI.

1. Aprire il **ContactController.cs** file.
2. Aggiungere un nuovo metodo alla classe controller denominata **Post** come illustrato nel codice seguente.

    (- Frammento di codice *API Lab - Ex03 - Post metodo Web*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. Aprire il **cshtml** file in Visual Studio, se non è già aperto.
4. Aggiungere il codice HTML seguente subito dopo l'elenco non ordinato che nell'attività precedente è stato aggiunto il file.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. All'interno dell'elemento di script nella parte inferiore del documento, aggiungere il codice evidenziato di seguito per gestire gli eventi clic sul pulsante, che verranno registrati i dati per l'API Web mediante una chiamata HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. In **ContactController.cs**, inserire un punto di interruzione di **Post** metodo.
7. Premere **F5** per eseguire l'applicazione nel browser.
8. Una volta la pagina viene caricata nel browser, digitare un nuovo nome di contatto e l'Id e fare clic su di **salvare** pulsante.

    ![Caricare il documento client HTML nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "caricare il documento client HTML nel browser")

    *Il documento HTML client caricata nel browser*
9. Quando la finestra del debugger si interrompe **Post** metodo, esaminare le proprietà di prendere il **contattare** parametro. I valori devono corrispondere i dati che immessi nel modulo.

    ![L'oggetto contatto viene inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "oggetto contatto di inviati dal client per l'API Web")

    *L'oggetto contatto viene inviato all'API Web dal client*
10. Passaggio tramite il metodo nel debugger fino a quando il **risposta** variabile è stata creata. Al momento di ispezione nel **variabili locali** finestra nel debugger, si noterà che tutte le proprietà sono state impostate.

   ![La risposta dopo la creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "la risposta dopo la creazione nel debugger")

   *La risposta dopo la creazione nel debugger*
11. Se si preme **F5** oppure fare clic su **continua** nel debugger verrà completata la richiesta. Quando si passa al browser, il nuovo contatto è stato aggiunto all'elenco di contatti archiviate dal **ContactRepository** implementazione.

   ![Il browser riflette alla creazione della nuova istanza contatta](build-restful-apis-with-aspnet-web-api/_static/image27.png "browser riflette alla creazione della nuova istanza di contatto")

   *Il browser riflette alla creazione della nuova istanza di contatto*

> [!NOTE]
> Inoltre, è possibile distribuire l'applicazione Azure seguente [appendice c: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Questa esercitazione ha introdotto il nuovo framework di ASP.NET Web API e l'implementazione di RESTful API Web usando il framework. Da qui è possibile creare un nuovo repository che facilita la persistenza dei dati utilizzando un numero qualsiasi di meccanismi e associare tale servizio anziché quello semplice fornito in questa esercitazione, ad esempio. API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione delle comunicazioni da client non HTML scritti in qualsiasi linguaggio che supporti HTTP e JSON o XML. La possibilità di ospitare un'API Web all'esterno di un'applicazione web tipica è anche possibile, nonché è la possibilità di creare formati di serializzazione.

Il sito Web ASP.NET dispone di un'area dedicata per il framework di ASP.NET Web API nel [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Questo sito continuerà a fornire le informazioni più recenti, esempi e le notizie relative all'API Web, quindi archiviarlo spesso se si desidera approfondire la conoscenza arte della creazione di API Web personalizzate disponibili per qualsiasi framework di sviluppo o dispositivo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice a: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

    ![Iniziare a digitare il nome del frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image29.png "inizia a digitare il nome del frammento di codice")

    *Iniziare a digitare il nome del frammento di codice*

    ![Premere Tab per selezionare il frammento di codice evidenziata](build-restful-apis-with-aspnet-web-api/_static/image30.png "premere Tab per selezionare il frammento evidenziato")

    *Premere Tab per selezionare il frammento di codice evidenziata*

    ![Premere Tab nuovamente e il frammento di codice verranno espansi](build-restful-apis-with-aspnet-web-api/_static/image31.png "espanderà premere Tab nuovamente e il frammento di codice")

    *Premere Tab nuovamente e il frammento di codice verranno espansi*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)

1. Fare clic in cui si desidera inserire il frammento di codice.
2. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
3. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

    ![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](build-restful-apis-with-aspnet-web-api/_static/image32.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

    *Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

    ![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

    *Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice b: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installa Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express per il riquadro Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice c: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

Questa appendice mostra come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato acquistato seguendo il lab, sfruttando la funzionalità di pubblicazione distribuzione Web fornita da Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1 - Creazione di un nuovo sito Web dal portale di Azure

1. Passare al [il portale di gestione di Azure](https://manage.windowsazure.com/) e accedere utilizzando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure è possibile ospitare gratuito di 10 siti Web ASP.NET e quindi aumentare in base al traffico. È possibile iscriversi [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "accedere al portale Windows Azure")

    *Accedere al portale*
2. Fare clic su **nuovo** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **calcolo** | **sito Web**. Selezionare quindi **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completa in Azure dall'esterno al portale. Non include i passaggi per configurare un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "creando un nuovo sito Web utilizzando Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Una volta creato il sito Web, fare clic sul collegamento sotto il **URL** colonna. Verificare il funzionamento del nuovo sito Web.

    ![Esplorazione per il nuovo sito web](build-restful-apis-with-aspnet-web-api/_static/image42.png "esplorazione per il nuovo sito web")

    *Esplorazione per il nuovo sito web*

    ![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione del sito web](build-restful-apis-with-aspnet-web-api/_static/image44.png "apertura delle pagine di gestione del sito web")

    *Aprire le pagine di gestione del sito Web*
7. Nel **Dashboard** nella pagina di **riepilogo rapido** fare clic su di **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene gli URL, le credenziali dell'utente e le stringhe di database necessari per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in Azure.

    ![Profilo di pubblicazione del sito web di download](build-restful-apis-with-aspnet-web-api/_static/image45.png "profilo di pubblicazione del sito web di download")

    *Profilo di pubblicazione del sito Web di download*
8. Scaricare il file del profilo di pubblicazione in un percorso noto. In questo esercizio si ulteriormente verrà visualizzato come utilizzare questo file per pubblicare un'applicazione web in Azure da Visual Studio.

    ![Salvare il file del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "il salvataggio del profilo di pubblicazione")

    *Salvare il file del profilo di pubblicazione*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione viene utilizzato SQL Server database, è necessario creare un server di Database SQL. Se si desidera distribuire un'applicazione semplice che non utilizza SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure in **database Sql** | **server** | **Dashboard del Server**. Se non si dispone di un server creato, è possibile creare tramite il **Aggiungi** pulsante sulla barra dei comandi. Annotare il **nome server e URL, il nome di accesso di amministratore e la password**, come verranno utilizzati in operazioni successive. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard del Server Database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard del Server Database SQL")

    *Dashboard del Server Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server di **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP da **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e fare clic su di ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) pulsante.

    ![Aggiunta indirizzo IP del Client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto agli indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confermare le modifiche*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora**, fare clic sul progetto sito web e selezionare **pubblica**.

    ![La pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "la pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "l'importazione del profilo di pubblicazione")

    *L'importazione di profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Una volta completata la convalida, fare clic su **Avanti**.

    > [!NOTE]
    > Dopo aver visualizzato un segno di spunta verde visualizzata accanto al pulsante convalida connessione, la convalida è stata completata.

    ![Convalida della connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** fare clic sul pulsante accanto casella di testo della connessione di database (ad esempio **DefaultConnection**).

    ![Configurazione della distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL utilizzando il *tcp:* prefisso.
   - In **nome utente** digitare il nome di accesso di amministratore di server.
   - In **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database fare clic su **Sì**.

    ![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si utilizzerà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di casella di testo di connessione predefinito. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **anteprima** pagina, fare clic su **pubblica**.

    ![Pubblicare l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image58.png "pubblicare l'applicazione web")

    *Pubblicare l'applicazione web*
9. Una volta terminato il processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "applicazione pubblicata in Windows Azure")

    *Applicazione pubblicata in Azure*
