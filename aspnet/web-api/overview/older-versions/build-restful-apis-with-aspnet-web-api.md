---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilare le API RESTful con l'API Web ASP.NET | Microsoft Docs
author: rick-anderson
description: Negli ultimi anni, è diventato chiaro che HTTP non è sufficiente per mettere a disposizione le pagine HTML. È anche una potente piattaforma per la compilazione di API Web, utilizzando un numero limitato di o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363520"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Compilare le API RESTful con l'API Web ASP.NET
====================
da [Camp Web Team](https://twitter.com/webcamps)

> Negli ultimi anni, è diventato chiaro che HTTP non è sufficiente per mettere a disposizione le pagine HTML. È anche una potente piattaforma per la compilazione di API Web, usando ad esempio un numero limitato di verbi (GET, POST e così via) e alcuni concetti semplici *URI* e *intestazioni*. API Web ASP.NET è un set di componenti che semplificano la programmazione HTTP. Poiché è basato sul runtime di ASP.NET MVC, API Web gestisce automaticamente i dettagli di basso livello trasporto di HTTP. Allo stesso tempo, API Web naturalmente espone il modello di programmazione HTTP. In uno degli obiettivi di API Web è infatti *non* sottraggono la realtà di HTTP. Di conseguenza, API Web è sia flessibile e facile da estendere. In questo laboratorio pratico, si utilizzerà API Web per compilare una semplice API REST per un'applicazione Gestione contatti. Si verrà inoltre compilato un client per usare l'API. Lo stile architetturale REST ha dimostrato di essere un metodo efficace per utilizzare HTTP - anche se non è sicuramente l'approccio valido solo per HTTP. Gestione contatti esporrà il RESTful per gli elenchi, aggiunta e rimozione dei contatti, tra gli altri. Questa esercitazione richiede una conoscenza di base HTTP, REST e si presuppone una conoscenza di base di codice HTML, JavaScript e jQuery.
> 
> > [!NOTE]
> > Il sito Web ASP.NET è presente un'area dedicata per il framework API Web ASP.NET al [ https://asp.net/web-api ](https://asp.net/web-api). Questo sito continuerà a fornire le informazioni più aggiornate, esempi e le notizie relative all'API Web, quindi verificare spesso se si vuole approfondire la conoscenza l'arte della creazione di API Web personalizzate disponibili per qualsiasi framework di dispositivo o lo sviluppo.
> > 
> > API Web ASP.NET, simile a ASP.NET MVC 4, ha una grande flessibilità in termini che separa il livello di servizio verso i controller di poterli usare alcuni dei framework di inserimento delle dipendenze disponibili abbastanza facile. È disponibile un buon esempio in MSDN che illustra come usare Ninject inserimento delle dipendenze in un progetto API Web ASP.NET che è possibile scaricarlo dal [qui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Implementare un'API Web RESTful
- Chiamare l'API da un client HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice B](#AppendixB) per istruzioni su come installarlo).

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice a: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include l'esercizio seguente:

1. [Esercizio 1: Creare una sola API Web](#Exercise1)
2. [Esercizio 2: Creare un'API Web di lettura/scrittura](#Exercise2)
3. [Esercizio 3: Usare l'API Web da un Client HTML](#Exercise3)

> [!NOTE]
> Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi. Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **60 minuti**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Esercizio 1: Creare una sola API Web

In questo esercizio, si implementerà i metodi GET di sola lettura per la gestione di contatti.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Attività 1: creazione del progetto di API

In questa attività si userà i nuovi modelli di progetto web ASP.NET per creare un'applicazione web API Web.

1. Eseguire **Visual Studio Express 2012 per Web**, a tale scopo, passare alla **avviare** e digitare **Visual Studio Express per Web** quindi premere **invio**.
2. Dal **File** dal menu **nuovo progetto**. Selezionare il **Visual c# | Web** tipo di visualizzazione struttura ad albero di tipo progetto di progetto, quindi selezionare il **applicazione Web ASP.NET MVC 4** tipo di progetto. Impostare il progetto **Name** a *ContactManager* e il **Nome soluzione** a *Begin*, quindi fare clic su **OK**.

    ![Creazione di un nuovo progetto ASP.NET MVC 4.0 Web Application](build-restful-apis-with-aspnet-web-api/_static/image1.png "creando un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione")

    *Creazione di un nuovo progetto ASP.NET MVC 4.0 Web dell'applicazione*
3. Nella finestra di tipo del progetto ASP.NET MVC 4, selezionare la **API Web** tipo di progetto. Fare clic su **OK**.

    ![Che specifica il tipo di progetto API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "che specifica il tipo di progetto API Web")

    *Che specifica il tipo di progetto API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Attività 2: creazione di controller dell'API Contact Manager

In questa attività si creerà le classi controller in cui risiederà metodi dell'API.

1. Eliminare il file denominato **ValuesController.cs** entro **controller** cartella dal progetto.
2. Fare doppio clic il **controller** cartella nel progetto e selezionare **Add | Controller** dal menu di scelta rapida.

    ![Aggiunge un nuovo controller al progetto](build-restful-apis-with-aspnet-web-api/_static/image3.png "aggiunge un nuovo controller al progetto")

    *Aggiunge un nuovo controller al progetto*
3. Nel **Aggiungi Controller** finestra di dialogo visualizzata, selezionare **Controller API vuoto** dal menu modello. Denominare la classe controller **ContactController**. Quindi, fare clic su **Add.**

    ![Usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web")

    *Usando la finestra di dialogo Aggiungi Controller per creare un nuovo controller API Web*
4. Aggiungere il codice seguente per il **ContactController**.

    (Code - Snippet *il Lab di API Web - Ex01 - ottenere il metodo API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Premere **F5** per eseguire il debug dell'applicazione. Apparirà la home page predefinita per un progetto API Web.

    ![La home page predefinita di un'applicazione API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "la home page predefinita di un'applicazione API Web ASP.NET")

    *La home page predefinita di un'applicazione API Web ASP.NET*
6. Nella finestra di Internet Explorer, premere il **F12** tasto per aprire il **strumenti di sviluppo** finestra. Fare clic sul **Network** scheda e quindi fare clic sul **Avvia acquisizione** pulsante per avviare l'acquisizione del traffico di rete nella finestra.

    ![Acquisizione di rete aprendo la scheda di rete e avviando](build-restful-apis-with-aspnet-web-api/_static/image6.png "aprendo la scheda di rete e dell'avvio di acquisizione di rete")

    *Aprendo la scheda di rete e dell'avvio di acquisizione di rete*
7. Aggiungere l'URL nella barra degli indirizzi del browser con **/api/contact** e premere INVIO. I dettagli di trasmissione verranno visualizzato nella finestra di acquisizione di rete. Si noti che è di tipo MIME della risposta **application/json**. Ciò dimostra come il formato di output predefinito è JSON.

    ![Visualizzazione dell'output della richiesta di API Web nella visualizzazione rete](build-restful-apis-with-aspnet-web-api/_static/image7.png "visualizzando l'output della richiesta di API Web nella visualizzazione rete")

    *Visualizzazione dell'output della richiesta di API Web nella visualizzazione rete*

    > [!NOTE]
    > Comportamento predefinito di Internet Explorer 10 sarà a questo punto per chiedere se l'utente desidera salvare o aprire il flusso risultante dalla chiamata all'API Web. L'output sarà un file di testo contenente il risultato JSON della chiamata di URL API Web. Non annullare la finestra di dialogo per poter visualizzare del contenuto della risposta tramite una finestra degli strumenti di sviluppatori.
8. Scegliere il **passare alla visualizzazione dettagliata** pulsante per visualizzare altri dettagli sulla risposta di questa chiamata API.

    ![Passare alla visualizzazione dettagliata](build-restful-apis-with-aspnet-web-api/_static/image8.png "passa alla visualizzazione dei dettagli")

    *Passare alla visualizzazione dettagliata*
9. Scegliere il **corpo della risposta** scheda per visualizzare il testo di risposta JSON effettivo.

    ![Visualizzare il codice JSON di output testo il monitoraggio di rete](build-restful-apis-with-aspnet-web-api/_static/image9.png "visualizzando il file JSON di output testo il monitoraggio di rete")

    *Visualizzazione del testo di output JSON in network monitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Attività 3: creazione di modelli di contatto e aumentare il Controller di contatto

In questa attività si creerà le classi controller in cui risiederà metodi dell'API.

1. Fare doppio clic il **modelli** cartella e selezionare **Add | Classe...**  dal menu di scelta rapida.

    ![Aggiunta di un nuovo modello per l'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image10.png "aggiungendo un nuovo modello per l'applicazione web")

    *Aggiunta di un nuovo modello per l'applicazione web*
2. Nel **Aggiungi nuovo elemento** finestra di dialogo Nome nuovo file **Contact.cs** e fare clic su **Add.**

    ![Creazione del nuovo file di classe di contatto](build-restful-apis-with-aspnet-web-api/_static/image11.png "creazione del nuovo file di classe di contatto")

    *Creazione del nuovo file di classe di contatto*
3. Aggiungere il codice evidenziato seguente per il **contatto** classe.

    (Code - Snippet *Web API Lab - Ex01 - contatto classe*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. Nel **ContactController** classe, selezionare la parola **stringa** nella definizione del metodo del **Ottieni** (metodo) e digitare la parola *contatto*. Al termine della parola digitata un indicatore verrà visualizzata all'inizio della parola **contatto**. Sia premuto la **Ctrl** e premere il tasto punto (.) oppure fare clic sull'icona con il mouse per aprire la finestra di dialogo di assistenza nell'editor del codice, per compilare automaticamente il **usando** direttiva per i modelli spazio dei nomi.

    ![Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Con supporto di Intellisense per le dichiarazioni dello spazio dei nomi*
5. Modificare il codice per il **ottenere** metodo in modo che restituisca una matrice di istanze di un modello contatto.

    (Code - Snippet *Lab di API Web - Ex01 - restituzione di un elenco di contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Premere **F5** al debug dell'applicazione web nel browser. Per visualizzare le modifiche apportate all'output di risposta dell'API, eseguire la procedura seguente.

   1. Quando si apre il browser, premere **F12** se gli strumenti di sviluppo non sono ancora aperti.
   2. Scegliere il **rete** scheda.
   3. Premere il **Avvia acquisizione** pulsante.
   4. Aggiungere il suffisso dell'URL **/api/contact** per l'URL nella barra degli indirizzi e premere il **invio** chiave.
   5. Premere il **passare alla visualizzazione dettagliata** pulsante.
   6. Selezionare il **corpo della risposta** scheda. Dovrebbe essere una stringa JSON che rappresenta il form serializzato di una matrice di istanze del contatto.

      ![JSON serializzato output di una chiamata al metodo API Web complessa](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializzato output di una chiamata al metodo API Web complessa")

      *Output JSON serializzato di una chiamata al metodo API Web complessa*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Attività 4: funzionalità di estrazione a un livello di servizio

Questa attività verrà illustrato come estrarre funzionalità in un livello di servizio per semplificare per gli sviluppatori separare le funzionalità del servizio dal livello dei controller, consentendo la possibilità di riutilizzo dei servizi che effettivamente svolgono il lavoro.

1. Creare una nuova cartella nella radice della soluzione e denominarlo **Services**. A tale scopo, fare doppio clic su **ContactManager** progetto, selezionare **Add** | **nuova cartella**, denominarla *servizi*.

    ![Cartella servizi di creazione](build-restful-apis-with-aspnet-web-api/_static/image14.png "cartella la creazione di servizi")

    *Creazione della cartella di servizi*
2. Fare doppio clic il **Services** cartella e selezionare **Add | Classe...**  dal menu di scelta rapida.

    ![Aggiunta di una nuova classe per la cartella Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "aggiunta una nuova classe per la cartella Services")

    *Aggiunta di una nuova classe per la cartella Services*
3. Quando la **Aggiungi nuovo elemento** viene visualizzata la finestra, assegnare un nome alla nuova classe **ContactRepository** e fare clic su **Add**.

    ![Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatto](build-restful-apis-with-aspnet-web-api/_static/image16.png "creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti")

    *Creazione di un file di classe per contenere il codice per il livello di servizio del Repository di contatti*
4. Aggiungere un'usando la direttiva per il **ContactRepository.cs** file da includere lo spazio dei nomi modelli.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Aggiungere il codice evidenziato seguente per il **ContactRepository.cs** file per implementare il metodo GetAllContacts.

    (Code - Snippet *Web API Lab - Ex01 - Repository di contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Aprire il **ContactController.cs** file se non è già aperto.
7. Aggiungere la seguente istruzione using alla sezione della dichiarazione dello spazio dei nomi del file.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Aggiungere il codice evidenziato seguente per il **ContactController.cs** classe per aggiungere un campo privato per rappresentare l'istanza del repository, in modo che il resto della classe membri possono apportare usare dell'implementazione del servizio.

    (Code - Snippet *Web API Lab - Ex01 - Contactcontroller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Modifica il **ottenere** metodo in modo che rende utilizzare il servizio di repository di contatti.

    (Code - Snippet *Lab di API Web - Ex01 - restituzione di un elenco dei contatti tramite il repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Inserire un punto di interruzione il **ContactController**del **ottenere** definizione di metodo.

   ![Aggiunta di punti di interruzione al controller di contatto](build-restful-apis-with-aspnet-web-api/_static/image17.png "aggiungere punti di interruzione al controller di contatto")

   *Aggiunta di punti di interruzione al controller di contatto*
11. Premere **F5** per eseguire l'applicazione.
12. Quando si apre il browser, premere **F12** per aprire gli strumenti di sviluppo.
13. Scegliere il **rete** scheda.
14. Scegliere il **Avvia acquisizione** pulsante.
15. Aggiungere l'URL nella barra degli indirizzi con il suffisso **/api/contact** , quindi premere **invio** per caricare il controller API.
16. Visual Studio 2012 è presente un'interruzione di una volta **ottenere** metodo inizia l'esecuzione.

   ![All'interno del metodo Get di rilievo](build-restful-apis-with-aspnet-web-api/_static/image18.png "rilievo all'interno del metodo Get")

   *Interruzione all'interno del metodo Get*
17. Premere **F5** per continuare.
18. Tornare a Internet Explorer se non è già in stato attivo. Si noti la finestra di acquisizione di rete.

    ![Visualizzazione in Internet Explorer che mostra i risultati della chiamata di API Web di rete](build-restful-apis-with-aspnet-web-api/_static/image19.png "visualizzazione in Internet Explorer che mostra i risultati della chiamata di API Web di rete")

    *Visualizzazione di rete in Internet Explorer che mostra i risultati della chiamata di API Web*
19. Scegliere il **passare alla visualizzazione dettagliata** pulsante.
20. Scegliere il **corpo della risposta** scheda. Si noti l'output JSON della chiamata API e come rappresenta due contatti recuperate dal livello di servizio.

    ![Visualizzazione dell'output JSON dall'API Web nella finestra di strumenti per sviluppatori](build-restful-apis-with-aspnet-web-api/_static/image20.png "visualizzando l'output JSON dall'API Web nella finestra di strumenti di sviluppo")

    *Visualizzazione dell'output JSON dall'API Web nella finestra di strumenti di sviluppo*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Esercizio 2: Creare un'API Web di lettura/scrittura

In questo esercizio, si implementerà POST e PUT metodi per la gestione contatti per abilitarlo con le funzionalità di modifica dei dati.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Attività 1: aprire il progetto API Web

In questa attività viene preparata migliorare il progetto API Web creato nell'esercizio 1, in modo che sia possibile accettare l'input dell'utente.

1. Eseguire **Visual Studio Express 2012 per Web**, a tale scopo, passare alla **avviare** e digitare **Visual Studio Express per Web** quindi premere **invio**.
2. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex02-ReadWriteWebAPI/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire il **Services/ContactRepository.cs** file.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Attività 2: aggiunta di funzionalità di salvataggio permanente dei dati per l'implementazione di Repository di contatti

In questa attività, si verrà Estendi classe ContactRepository del progetto API Web creata nell'esercizio 1, in modo che possa salvare in modo permanente e accettare l'input dell'utente e le nuove istanze di contatto.

1. La costante seguente per aggiungere il **ContactRepository** classe per rappresentare il nome del nome del server web cache elemento chiave più avanti in questo esercizio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Aggiungere un costruttore per la **ContactRepository** contenente il codice seguente.

    (Code - Snippet *Web API Lab - Ex02 - Repository di contatti costruttore*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modificare il codice per il **GetAllContacts** metodo come illustrato di seguito.

    (Code - Snippet *il Lab di API Web - Ex02 - Ottieni tutti i contatti*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Questo esempio è a scopo dimostrativo e utilizzerà la cache del server web come mezzo di archiviazione, in modo che i valori verranno messe a disposizione più client contemporaneamente, anziché usare un meccanismo di archiviazione di sessione o una durata di archiviazione richiesta. Uno è stato possibile usare Entity Framework, l'archiviazione XML o qualsiasi altra varietà al posto di cache del server web.
4. Implementare un nuovo metodo denominato **SaveContact** per il **ContactRepository** classe per le operazioni di salvataggio di un contatto. Il **SaveContact** metodo deve accettare un unico **contatto** parametro e restituiscono un valore booleano valore indicante l'esito positivo o negativo.

    (Code - Snippet *Web API Lab - Ex02 - l'implementazione del metodo SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Esercizio 3: Usare l'API Web da un Client HTML

In questo esercizio si creerà un client HTML per chiamare l'API Web. Questo client faciliterà lo scambio di dati con l'API Web con JavaScript e verrà visualizzati i risultati in un web browser utilizzando il markup HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Attività 1: modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la visualizzazione dei contatti

In questa attività si modificherà la visualizzazione dell'indice predefinito dell'applicazione web per supportare il requisito di visualizzazione elenco dei contatti esistenti in un browser HTML.

1. Aprire **Visual Studio Express 2012 per Web** se non è già aperto.
2. Aprire il **Begin** soluzione disponibile all'indirizzo **inizio/origine/Ex03-ConsumingWebAPI/** cartella. In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.

   1. Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare. A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.
   2. Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.
   3. Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.

      > [!NOTE]
      > Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto. Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto. Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.
3. Aprire il **index. cshtml** file si trova in **Views/Home** cartella.
4. Sostituire il codice HTML all'interno dell'elemento div con id **corpo** in modo che risulti simile al codice seguente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Aggiungere il codice Javascript seguente nella parte inferiore del file per eseguire la richiesta HTTP all'API Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Aprire il **ContactController.cs** file se non è già aperto.
7. Inserire un punto di interruzione il **ottenere** metodo per il **ContactController** classe.

    ![Inserendo un punto di interruzione nel metodo Get del controller API](build-restful-apis-with-aspnet-web-api/_static/image21.png "inserendo un punto di interruzione nel metodo Get del controller API")

    *Inserendo un punto di interruzione nel metodo Get del controller API*
8. Premere **F5** per eseguire il progetto. Il browser caricherà il documento HTML.

    > [!NOTE]
    > Assicurarsi che si sta esplorando l'URL radice dell'applicazione.
9. Dopo il caricamento della pagina e il codice JavaScript viene eseguito, verrà raggiunto il punto di interruzione e sospende l'esecuzione del codice nel controller.

    ![Debug nelle chiamate all'API Web usando Visual Studio Express per il Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debug approda le chiamate all'API Web usando Visual Studio Express per Web")

    *Debug di una chiamata API Web usando Visual Studio Express 2012 per Web*
10. Rimuovere il punto di interruzione e premere **F5** della barra degli strumenti Debug oppure **continua** pulsante per continuare a caricare la visualizzazione nel browser. Una volta completata la chiamata all'API Web dovrebbe essere i contatti restituiti dall'API Web chiamate visualizzate come elementi di elenco nel browser.

    ![Risultati della chiamata API visualizzata nel browser come gli elementi dell'elenco](build-restful-apis-with-aspnet-web-api/_static/image23.png "i risultati della chiamata API visualizzata nel browser come elementi di elenco")

    *Risultati della chiamata API visualizzata nel browser come elementi di elenco*
11. Terminare il debug.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Attività 2 - modificare la visualizzazione dell'indice per fornire un'interfaccia utente grafica per la creazione di contatti

In questa attività, si continuerà a modificare la visualizzazione dell'indice dell'applicazione MVC. Un modulo verrà aggiunto alla pagina HTML che acquisire l'input dell'utente e inviarlo all'API Web per creare un nuovo contatto e verrà creato un nuovo metodo del controller API Web per la raccolta di date dalla GUI.

1. Aprire il **ContactController.cs** file.
2. Aggiungere un nuovo metodo alla classe controller denominata **Post** come illustrato nel codice seguente.

    (Code - Snippet *Lab API - Ex03 - Post metodo Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Aprire il **index. cshtml** file in Visual Studio se non è già aperto.
4. Aggiungere il codice HTML seguente al file subito dopo l'elenco non ordinato che è stato aggiunto nell'attività precedente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. All'interno dell'elemento di script nella parte inferiore del documento, aggiungere il codice evidenziato seguente per gestire gli eventi clic del pulsante, che pubblicherà i dati nell'API Web mediante una chiamata HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. Nelle **ContactController.cs**, inserire un punto di interruzione il **Post** (metodo).
7. Premere **F5** per eseguire l'applicazione nel browser.
8. Dopo la pagina viene caricata nel browser, digitare un nuovo nome di contatto e Id e fare clic sui **salvare** pulsante.

    ![Il documento HTML client caricata nel browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "il documento HTML client caricata nel browser")

    *Il documento HTML client caricata nel browser*
9. Quando la finestra del debugger si interrompe **Post** metodo, esaminare le proprietà delle **contattare** parametro. I valori devono corrispondere i dati che immessi nel form.

    ![L'oggetto contatto viene inviato all'API Web dal client](build-restful-apis-with-aspnet-web-api/_static/image25.png "oggetto Contact The inviato dal client per l'API Web")

    *L'oggetto contatto viene inviato all'API Web dal client*
10. Passaggio attraverso il metodo nel debugger finché il **risposta** variabile è stata creata. Al momento di ispezione nel **variabili locali** finestra nel debugger, si noterà che tutte le proprietà sono state impostate.

   ![La risposta dopo la creazione nel debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "la risposta dopo la creazione nel debugger")

   *La risposta dopo la creazione nel debugger*
11. Se si preme **F5** oppure fare clic su **continua** nel debugger verrà completata la richiesta. Dopo il passaggio al browser, il nuovo contatto è stato aggiunto all'elenco di contatti archiviati per il **ContactRepository** implementazione.

   ![Il browser riflette termine della creazione della nuova istanza di contatto](build-restful-apis-with-aspnet-web-api/_static/image27.png "browser riflette termine della creazione della nuova istanza di contatto")

   *Il browser riflette termine della creazione della nuova istanza di contatto*

> [!NOTE]
> Inoltre, è possibile distribuire questa applicazione a Azure segue [appendice c: pubblicazione un'applicazione ASP.NET MVC 4 con distribuzione Web](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

Questa esercitazione ha illustrato le per il nuovo framework API Web ASP.NET e all'implementazione di API Web RESTful che usano il framework. A questo punto, è possibile creare un nuovo repository che facilita la persistenza dei dati usando un numero qualsiasi di meccanismi e associare tale servizio anziché quello semplice fornito come esempio in questo laboratorio. API Web supporta una serie di funzionalità aggiuntive, ad esempio l'abilitazione della comunicazione da client non HTML scritte in qualsiasi linguaggio che supporti HTTP e JSON o XML. La possibilità di ospitare un'API Web di fuori di un'applicazione web tipica è anche possibile, nonché è la possibilità di creare i propri formati di serializzazione.

Il sito Web ASP.NET è presente un'area dedicata per il framework API Web ASP.NET al [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Questo sito continuerà a fornire le informazioni più aggiornate, esempi e le notizie relative all'API Web, quindi verificare spesso se si vuole approfondire la conoscenza l'arte della creazione di API Web personalizzate disponibili per qualsiasi framework di dispositivo o lo sviluppo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Appendice a: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](build-restful-apis-with-aspnet-web-api/_static/image28.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

    ![Iniziare a digitare il nome di frammento](build-restful-apis-with-aspnet-web-api/_static/image29.png "inizia a digitare il nome del frammento di codice")

    *Iniziare a digitare il nome del frammento di codice*

    ![Premere Tab per selezionare il frammento di codice evidenziata](build-restful-apis-with-aspnet-web-api/_static/image30.png "premere Tab per selezionare il frammento di codice evidenziata")

    *Premere Tab per selezionare il frammento di codice evidenziata*

    ![Il frammento di codice e premere nuovamente Tab espanderà](build-restful-apis-with-aspnet-web-api/_static/image31.png "si espanderà il frammento di codice e premere nuovamente Tab")

    *Il frammento di codice e premere nuovamente Tab espanderà*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)

1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.
2. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
3. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

    ![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](build-restful-apis-with-aspnet-web-api/_static/image32.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

    *Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

    ![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](build-restful-apis-with-aspnet-web-api/_static/image33.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

    *Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Appendice b: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

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
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Appendice c: pubblicazione di un'applicazione ASP.NET MVC 4 con distribuzione Web

In questa appendice spiega come creare un nuovo sito web dal portale di Azure e pubblicare l'applicazione che è stato ottenuto seguendo il lab, sfruttando la funzionalità di pubblicazione di distribuzione Web fornita da Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Attività 1: creazione di un nuovo sito Web dal portale di Azure

1. Andare alla [portale di gestione Azure](https://manage.windowsazure.com/) e accedere usando le credenziali di Microsoft associate alla sottoscrizione.

    > [!NOTE]
    > Con Azure Puoi ospitare gratuitamente 10 siti Web ASP.NET e la scalabilità orizzontale man mano che aumenta il traffico. È possibile effettuare l'iscrizione [qui](http://aka.ms/aspnet-hol-azure).

    ![Accedere al portale di Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "accedere al portale di Azure")

    *Accedere al portale*
2. Fare clic su **New** sulla barra dei comandi.

    ![Creazione di un nuovo sito Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "creando un nuovo sito Web")

    *Creazione di un nuovo sito Web*
3. Fare clic su **Compute** | **sito Web**. Quindi selezionare **creazione rapida** opzione. Specificare un URL disponibile per il nuovo sito web e fare clic su **Crea sito Web**.

    > [!NOTE]
    > Azure è l'host di un'applicazione web in esecuzione nel cloud che è possibile controllare e gestire. L'opzione Creazione rapida consente di distribuire un'applicazione web completata in Azure all'esterno del portale. Non include i passaggi per la configurazione di un database.

    ![Creazione di un nuovo sito Web utilizzando Creazione rapida](build-restful-apis-with-aspnet-web-api/_static/image41.png "creando un nuovo sito Web mediante Creazione rapida")

    *Creazione di un nuovo sito Web utilizzando Creazione rapida*
4. Attendere finché il nuovo **sito Web** viene creato.
5. Dopo aver creato il sito Web fare clic sul collegamento sotto i **URL** colonna. Verificare che il nuovo sito Web sia in funzione.

    ![Passare al nuovo sito web](build-restful-apis-with-aspnet-web-api/_static/image42.png "passare al nuovo sito web")

    *Passare al nuovo sito web*

    ![Sito Web in esecuzione](build-restful-apis-with-aspnet-web-api/_static/image43.png "sito Web in esecuzione")

    *Sito Web in esecuzione*
6. Tornare al portale e fare clic sul nome del sito web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione sito web](build-restful-apis-with-aspnet-web-api/_static/image44.png "aprire le pagine di gestione sito web")

    *Aprire le pagine di gestione sito Web*
7. Nel **Dashboard** nella pagina il **riepilogo rapido** fare clic sui **Scarica profilo di pubblicazione** collegamento.

    > [!NOTE]
    > Il *profilo di pubblicazione* contiene tutte le informazioni necessarie per pubblicare un'applicazione web in un Azure per ogni metodo di pubblicazione abilitato. Il profilo di pubblicazione contiene l'URL, le credenziali dell'utente e stringhe di database necessarie per connettersi a e l'autenticazione in ognuno degli endpoint per cui è abilitato un metodo di pubblicazione. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express per Web** e **Microsoft Visual Studio 2012** supportano la lettura dei profili di pubblicazione per automatizzare la configurazione di questi programmi per pubblicazione di applicazioni web in Azure.

    ![Download del sito web di profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image45.png "scaricando il sito web di profilo di pubblicazione")

    *Profilo di pubblicazione scaricato il sito Web*
8. Scaricare il file di profilo di pubblicazione in una posizione nota. In questo esercizio ulteriormente si vedrà come usare questo file per pubblicare un'applicazione web in Azure da Visual Studio.

    ![Salvare il file di profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image46.png "salvataggio del profilo di pubblicazione")

    *Salvare il file di profilo di pubblicazione*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Attività 2: configurare il Server di Database

Se l'applicazione Usa SQL Server database è necessario creare un Database di SQL server. Se si vuole distribuire una semplice applicazione che non Usa SQL Server è possibile ignorare questa attività.

1. È necessario un server di Database SQL per archiviare il database dell'applicazione. È possibile visualizzare i server di Database SQL dalla sottoscrizione nel portale di gestione di Azure all'indirizzo **database Sql** | **server** | **Dashboard del Server**. Se non si dispone di un server creato, è possibile crearne una usando il **Add** pulsante sulla barra dei comandi. Annotare il **nome server e URL, nome dell'account di accesso amministratore e la password**, come verranno usati nelle attività successiva. Non creare il database, come verrà creato in una fase successiva.

    ![Dashboard di Server di Database SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard di Server di Database SQL")

    *Dashboard di Server di Database SQL*
2. Nell'attività successiva si testerà la connessione al database da Visual Studio, per questo motivo è necessario includere l'indirizzo IP locale nell'elenco del server del **indirizzi IP consentiti**. A tale scopo, fare clic su **configura**, selezionare l'indirizzo IP dal **indirizzo IP Client corrente** e incollarlo nel **indirizzo IP iniziale** e **l'indirizzoIPfinale** caselle di testo e scegliere il ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) pulsante.

    ![Aggiunta indirizzo IP del Client](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Aggiunta indirizzo IP del Client*
3. Una volta il **indirizzo IP del Client** viene aggiunto a indirizzi IP consentiti elenco, fare clic su **salvare** per confermare le modifiche.

    ![Confermare le modifiche](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confermare le modifiche*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Attività 3: pubblicare un'applicazione ASP.NET MVC 4 con distribuzione Web

1. Tornare alla soluzione ASP.NET MVC 4. Nel **Esplora soluzioni**, fare clic sul progetto sito web e selezionare **Publish**.

    ![Pubblicazione dell'applicazione](build-restful-apis-with-aspnet-web-api/_static/image51.png "pubblicazione dell'applicazione")

    *Pubblicazione del sito web*
2. Importare il profilo di pubblicazione che è stato salvato nella prima attività.

    ![L'importazione del profilo di pubblicazione](build-restful-apis-with-aspnet-web-api/_static/image52.png "l'importazione del profilo di pubblicazione")

    *Importazione del profilo di pubblicazione*
3. Fare clic su **convalidare la connessione**. Dopo aver completata la convalida fare clic su **successivo**.

    > [!NOTE]
    > La convalida è stata completata una volta che visualizzato un segno di spunta verde accanto al pulsante convalida connessione.

    ![La convalida connessione](build-restful-apis-with-aspnet-web-api/_static/image53.png "convalida della connessione")

    *Convalida della connessione*
4. Nel **impostazioni** nella pagina il **database** sezione, fare clic sul pulsante accanto alla casella di testo della connessione di database (vale a dire **DefaultConnection**).

    ![Configurazione della distribuzione Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configurazione della distribuzione Web")

    *Configurazione della distribuzione Web*
5. Configurare la connessione al database come segue:

   - Nel **nome Server** digitare l'URL server di Database SQL tramite il *tcp:* prefisso.
   - Nelle **nome utente** digitare il nome dell'account di accesso amministratore server.
   - Nelle **Password** digitare la password dell'account di accesso amministratore server.
   - Digitare un nuovo nome di database, ad esempio: *MVC4SampleDB*.

     ![Configurazione di stringa di connessione di destinazione](build-restful-apis-with-aspnet-web-api/_static/image55.png "configurazione stringa di connessione di destinazione")

     *Configurazione di stringa di connessione di destinazione*
6. Fare quindi clic su **OK**. Quando viene richiesto di creare il database, fare clic su **Sì**.

    ![Creazione del database](build-restful-apis-with-aspnet-web-api/_static/image56.png "creazione della stringa di database")

    *Creazione del database*
7. La stringa di connessione che si userà per connettersi al Database SQL di Windows Azure viene visualizzata all'interno di connessione predefinita nella casella di testo. Scegliere quindi **Avanti**.

    ![Stringa di connessione che punta al Database SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "stringa di connessione che punta al Database SQL")

    *Stringa di connessione che punta al Database SQL*
8. Nel **Preview** pagina, fare clic su **Publish**.

    ![Pubblicazione dell'applicazione web](build-restful-apis-with-aspnet-web-api/_static/image58.png "pubblicazione dell'applicazione web")

    *Pubblicazione dell'applicazione web*
9. Al termine del processo di pubblicazione, il browser predefinito verrà aperto il sito web pubblicato.

    ![Applicazione pubblicata in Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "applicazione pubblicata in Windows Azure")

    *Pubblicata dell'applicazione in Azure*
