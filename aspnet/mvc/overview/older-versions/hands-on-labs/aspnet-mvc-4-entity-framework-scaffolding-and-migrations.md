---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Lo Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni | Documenti Microsoft
author: rick-anderson
description: Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Le migrazioni e lo Scaffolding di ASP.NET MVC 4 Entity Framework

Da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](https://aka.ms/webcamps-training-kit)

Se si ha familiarità con i metodi di ASP.NET MVC 4 controller o aver completato il &quot;helper, form e convalida&quot; pratica, è necessario essere consapevoli che molte della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione. Non dimenticare che, se il modello contiene diverse classi di modificare, sarà probabilmente di un tempo considerevole scrivono i metodi di azione GET e POST per ogni operazione di entità, nonché a ognuna delle visualizzazioni.

In questo laboratorio si apprenderà come usare lo scaffolding di ASP.NET MVC 4 per generare automaticamente la linea di base del CRUD applicazione (Create, Read, Update e Delete). A partire da una classe di modello semplice e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, nonché di tutte le necessarie visualizzazioni. Dopo aver compilato e l'esecuzione della soluzione semplice, sarà necessario il database dell'applicazione generato, insieme alla logica MVC e viste per la manipolazione dei dati.

Inoltre, si apprenderà come è facile da utilizzare le migrazioni di Entity Framework per eseguire aggiornamenti di modelli in tutta l'intera applicazione. Migrazioni di Entity Framework consente di modificare il database dopo il modello è stato modificato con semplici passaggi. Tutte queste operazioni in considerazione, sarà in grado di creare e gestire applicazioni web in modo più efficiente, sfruttando le funzionalità più recenti di ASP.NET MVC 4.

> [!NOTE]
> Tutto il codice di esempio e i frammenti di codice inclusi in Web categorie Training Kit, disponibile in [versioni Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Il progetto specifico per questa esercitazione è disponibile all'indirizzo [Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa pratica, si apprenderà come:

- Usare lo scaffolding di ASP.NET per le operazioni CRUD nel controller.
- Modificare il modello di database utilizzando le migrazioni di Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

È necessario che gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**L'installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.

Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Nell'esercizio seguente che costituiscono questa pratica:

1. [Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework](#Exercise1)

> [!NOTE]
> Questo esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercizio. Se è necessario aggiuntive sull'utilizzo nell'esercizio, è possibile utilizzare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **30 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Esercizio 1: Utilizzare lo Scaffolding di ASP.NET MVC 4 con le migrazioni di Entity Framework

Lo scaffolding di ASP.NET MVC fornisce un modo rapido per generare le operazioni CRUD in un modo standardizzato, creare la logica necessaria che consente all'applicazione di interagire con il livello di database.

In questo esercizio, si apprenderà come usare lo scaffolding di ASP.NET MVC 4 con il codice per creare i metodi CRUD. Si apprenderà quindi come aggiornare il modello di applicazione delle modifiche nel database utilizzando le migrazioni di Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Attività 1-Creazione di un nuovo ASP.NET MVC 4 progetto mediante Scaffolding

1. Se non è già aperta, avviare **Visual Studio 2012**.
2. Selezionare **File | Nuovo progetto**. Nel nuovo progetto nella finestra di dialogo, il **Visual c# | Web** selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto per **MVC4andEFMigrations** e impostare il percorso su **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** cartella di questa esercitazione. Impostare il **Nome soluzione** a **iniziare** e verificare **Crea directory per soluzione** è selezionata. Fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")

    *Finestra di dialogo Nuovo progetto ASP.NET MVC 4*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo selezionare il **applicazione Internet** modello e assicurarsi che **Razor** è selezionato **motoredivisualizzazione**. Fare clic su **OK** per creare il progetto.

    ![Nuova applicazione Internet di ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nuova applicazione Internet di ASP.NET MVC 4")

    *Nuova applicazione Internet di ASP.NET MVC 4*
4. In Esplora soluzioni fare doppio clic su **modelli** e selezionare **Aggiungi | Classe** per creare un utente di un classe semplice (POCO). Il nome **persona** e fare clic su **OK**.
5. Aprire la classe di persona e inserire le proprietà seguenti.

    (- Frammento di codice *ASP.NET MVC 4 e le migrazioni di Entity Framework - proprietà persona Ex1*)


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. Fare clic su **compilare | Compila soluzione** per salvare le modifiche e compilare il progetto.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilazione dell'applicazione")

    *Compilazione dell'applicazione*
7. In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Aggiungi | Controller**.
8. Denominare il controller *PersonController* e completare il **opzioni Scaffolding** con i valori seguenti.

   1. Nel **modello** elenco a discesa, seleziona il **controller MVC con azioni di lettura/scrittura e viste, mediante Entity Framework** opzione.
   2. Nel **classe modello** elenco a discesa, seleziona il **persona** classe.
   3. Nel **classe contesto dati** elenco, selezionare  **&lt;nuovo contesto dati... &gt;**. Scegliere qualsiasi nome e fare clic su **OK**.
   4. Nel **viste** elenco a discesa elenco, verificare che l'opzione **Razor** è selezionata.

      ![Aggiunta del controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "aggiunta del controller di persona con scaffolding")

      *Aggiunta del controller di persona con scaffolding*
9. Fare clic su **Aggiungi** per creare il nuovo controller di persona con scaffolding. Le azioni del controller, nonché le visualizzazioni sono stati generati.

    ![Dopo aver creato il controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "dopo aver creato il controller di persona con scaffolding")

    *Dopo aver creato il controller di persona con scaffolding*
10. Aprire **PersonController** classe. Si noti che i metodi di azione CRUD completi siano stati generati automaticamente.

   ![All'interno del controller di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controller all'interno di persona")

   *All'interno del controller di persona*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Attività 2-esecuzione dell'applicazione

A questo punto, il database non è ancora creato. In questa attività, eseguire l'applicazione per la prima volta e i test verranno le operazioni CRUD. Il database verrà creato in modo immediato con Code First.

1. Premere **F5** per eseguire l'applicazione.
2. Nel browser, aggiungere **/Person** all'URL per aprire la pagina di persona.

    ![Prima esecuzione applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prima esecuzione applicazione")

    *Dell'applicazione: eseguire prima*
3. Verrà ora esplorare le pagine di persona e testare le operazioni CRUD.

    1. Fare clic su **Crea nuovo** per aggiungere una nuova persona. Immettere un nome e un cognome e fare clic su **crea**.

        ![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "aggiunta una nuova persona")

        *Aggiunta di una nuova persona*
    2. Nell'elenco della persona, è possibile eliminare, modificare o aggiungere elementi.

        ![elenco di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco persona")

        *Elenco di persona*
    3. Fare clic su **dettagli** per aprire i dettagli della persona.

        ![Dettagli della persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "i dettagli dell'utente")

        *Dettagli dell'utente*
4. Chiudere il browser e tornare a Visual Studio. Si noti che sia stato creato l'intero CRUD per l'entità person in tutta l'applicazione - dal modello per le viste di - senza dover scrivere una singola riga di codice.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Attività 3-aggiornamento il database utilizzando le migrazioni di Entity Framework

In questa attività si aggiornerà il database utilizzando le migrazioni di Entity Framework. Si noterà quanto sia facile per modificare il modello di applicazione delle modifiche nei database utilizzando la funzionalità di Entity Framework migrazioni.

1. Aprire la Console di gestione pacchetti. Selezionare **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.
2. Nella Console di gestione pacchetti, immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Abilitare le migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "abilitazione migrazioni")

    *L'abilitazione delle migrazioni*

    Il comando Enable-Migration crea il **migrazioni** cartella che contiene uno script per inizializzare il database.

    ![Cartella Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "cartella Migrations")

    *Cartella Migrations*
3. Aprire il **Configuration.cs** file nella cartella migrazioni. Individuare il costruttore della classe e modificare il **AutomaticMigrationsEnabled** valore *true*.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. Aprire la classe di persona e aggiungere un attributo per il nome della persona intermedio. Con questo nuovo attributo, si modifica il modello.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. Selezionare **compilare | Compila soluzione** del menu per compilare l'applicazione.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilazione dell'applicazione")

    *Compilazione dell'applicazione*
6. Nella Console di gestione pacchetti, immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Questo comando avrà un aspetto per le modifiche negli oggetti dati e quindi, aggiunge i comandi necessari per modificare di conseguenza il database.

    ![Aggiunta di un secondo nome](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "aggiunta di un secondo nome")

    *Aggiunta di un secondo nome*
7. (Facoltativo) È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento differenziale. Ciò consente di aggiornare manualmente il database (In questo caso non è necessario), o applicare le modifiche in altri database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "la generazione di uno script SQL")

    *Generazione di uno script SQL*

    ![Aggiornamento di Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aggiornamento Script SQL")

    *Aggiornamento di Script SQL*
8. Nella Console di gestione pacchetti, immettere il comando seguente per aggiornare il database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![L'aggiornamento del Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "l'aggiornamento del Database")

    *L'aggiornamento del Database*

    Verrà aggiunta la **MiddleName** colonna il **persone** tabella corrisponde alla definizione corrente del **persona** classe.
9. Una volta che il database viene aggiornato, la cartella del Controller e scegliere **Aggiungi | Controller** per aggiungere nuovamente il controller persona (completamento con gli stessi valori). Verranno aggiornate le visualizzazioni aggiungere il nuovo attributo e i metodi esistenti.

    ![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "aggiunta di un aggiornamento del controller")

    *L'aggiornamento del controller*
10. Fare clic su **Aggiungi**. Selezionare quindi i valori **sovrascrivere PersonController.cs** e **sovrascrittura visualizzazioni associate** e fare clic su **OK**.

   ![Aggiunta di una sovrascrittura controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *L'aggiornamento del controller*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - esecuzione dell'applicazione

1. Premere **F5** per eseguire l'applicazione.
2. Aprire **/Person**. Si noti che i dati è stati mantenuti, mentre il secondo nome colonna è stata aggiunta.

    ![Secondo nome aggiunto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "secondo nome aggiunto")

    *Secondo nome aggiunto*
3. Se si fa clic **modifica**, sarà possibile aggiungere un secondo nome dell'utente corrente.

    ![Secondo nome edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "secondo nome edition")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica, si sono appreso semplici passaggi per creare operazioni CRUD con ASP.NET MVC 4 Scaffolding usando qualsiasi classe di modello. Quindi, si è appreso come eseguire un aggiornamento end-to-end dell'applicazione - dal database per le viste, utilizzando le migrazioni di Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **installa**. Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.
3. Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.

    ![Installa Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installa Visual Studio Express")

    *Installa Visual Studio Express*
4. Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.

    ![Accettare le condizioni di licenza](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Accettare le condizioni di licenza*
5. Attendere finché non viene completato il processo di download e l'installazione.

    ![Stato dell'installazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Stato dell'installazione*
6. Al termine dell'installazione, fare clic su **fine**.

    ![Installazione completata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Installazione completata*
7. Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.

    ![VS Express per il riquadro Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: utilizzo dei frammenti di codice

Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano. Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.

![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")

*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore dove si desidera inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).
3. Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "premere Tab per selezionare il frammento evidenziato")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "espanderà premere Tab nuovamente e il frammento di codice")

*Premere Tab nuovamente e il frammento di codice verranno espansi*

***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1. Fare clic in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*
