---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffolding e migrazioni | Microsoft Docs
author: rick-anderson
description: Se ha familiarità con i metodi del controller ASP.NET MVC 4 oppure ha completato il &quot;helper, moduli e convalida&quot; laboratorio pratico, è necessario essere consapevoli...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814122"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Le migrazioni e lo Scaffolding di ASP.NET MVC 4 Entity Framework

da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](https://aka.ms/webcamps-training-kit)

Se ha familiarità con i metodi del controller ASP.NET MVC 4 oppure ha completato il &quot;helper, moduli e convalida&quot; laboratorio pratico, è necessario essere consapevoli che molti della logica per creare, aggiornare, elencare e rimuovere qualsiasi entità di dati viene ripetuta tra l'applicazione. Senza dimenticare che, se il modello contiene diverse classi per modificare, è probabile che un certo tempo a scrivere i metodi di azione POST e GET per ogni operazione di entità, nonché a ogni visualizzazione.

In questo laboratorio si apprenderà come usare lo scaffolding di ASP.NET MVC 4 per generare automaticamente la linea di base di CRUD dell'applicazione (Create, Read, Update e Delete). A partire da una classe di modello semplice e, senza scrivere una singola riga di codice, si creerà un controller che conterrà tutte le operazioni CRUD, nonché di tutte le necessarie visualizzazioni. Dopo aver compilato ed eseguito la soluzione più semplice, si disporrà del database dell'applicazione generato, insieme alla logica di MVC e le viste per la manipolazione dei dati.

Inoltre, si apprenderà come è facile usare migrazioni di Entity Framework per eseguire aggiornamenti di modelli in tutta l'intera applicazione. Migrazioni di Entity Framework consente di modificare il database dopo il modello è stato modificato con semplici passaggi. Con tutti questi aspetti, sarà possibile creare e gestire le applicazioni web in modo più efficiente, sfruttando i vantaggi delle funzionalità più recenti di ASP.NET MVC 4.

> [!NOTE]
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit). Il progetto specifico per questo lab è disponibile all'indirizzo [Scaffolding di ASP.NET MVC 4 Entity Framework e migrazioni](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Usare lo scaffolding di ASP.NET per le operazioni CRUD nei controller.
- Modificare il modello di database tramite migrazioni di Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Sono necessari gli elementi seguenti per completare questa esercitazione:

- [Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

**Installazione di frammenti di codice**

Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio. Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.

Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

L'esercizio seguente che costituiscono questo laboratorio pratico:

1. [Usare lo Scaffolding di ASP.NET MVC 4 con migrazioni di Entity Framework](#Exercise1)

> [!NOTE]
> Questo esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercizio. Se ti serve assistenza aggiuntiva, utilizzo l'esercizio, è possibile usare questa soluzione come guida.


Tempo stimato per completare questa esercitazione: **30 minuti**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Esercizio 1: Usare lo Scaffolding di ASP.NET MVC 4 con migrazioni di Entity Framework

Scaffolding di MVC ASP.NET fornisce un modo rapido per generare le operazioni CRUD in modo standardizzato, creare la logica necessaria che consente all'applicazione di interagire con il livello di database.

In questo esercizio, si apprenderà come usare lo scaffolding di ASP.NET MVC 4 con il codice prima di tutto per creare i metodi CRUD. Quindi, si apprenderà come aggiornare il modello di applicazione delle modifiche nel database tramite migrazioni di Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Attività 1-Creazione in corso un nuovo ASP.NET MVC 4 progetto usando lo Scaffolding

1. Se non è già aperta, avviare **Visual Studio 2012**.
2. Selezionare **File | Nuovo progetto**. Nella nuova finestra di dialogo progetto, sotto il **Visual c# | Web** sezione, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto di **MVC4andEFMigrations** e impostare il percorso al **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** cartella della presente esercitazione. Impostare il **Nome soluzione** a **Begin** e verificare che **Crea directory per soluzione** sia selezionata. Fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "finestra di dialogo Nuovo progetto ASP.NET MVC 4")

    *Finestra di dialogo Nuovo progetto ASP.NET MVC 4*
3. Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo per selezionare la **applicazione Internet** modello e verificare che **Razor** è selezionato **motoredivisualizzazione**. Fare clic su **OK** per creare il progetto.

    ![Nuova applicazione Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nuova applicazione Internet ASP.NET MVC 4")

    *Nuova applicazione Internet ASP.NET MVC 4*
4. In Esplora soluzioni fare doppio clic **modelli** e selezionare **Add | Classe** per creare un utente di classi semplice (POCO). Denominarlo **Person** e fare clic su **OK**.
5. Aprire la classe Person e inserire le proprietà seguenti.

    (Code - Snippet *ASP.NET MVC 4 e migrazioni di Entity Framework - proprietà persona Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Fare clic su **compilare | Compila soluzione** per salvare le modifiche e compilare il progetto.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilazione dell'applicazione")

    *Compilazione dell'applicazione*
7. In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Add | Controller**.
8. Denominare il controller *PersonController* e completare la **opzioni di Scaffolding** con i valori seguenti.

   1. Nel **modello** elenco a discesa, seleziona la **controller MVC con azioni di lettura/scrittura e visualizzazioni, usando Entity Framework** opzione.
   2. Nel **classe modello** elenco a discesa, seleziona la **persona** classe.
   3. Nel **alla classe contesto dati** elenco, selezionare  **&lt;nuovo contesto dati... &gt;**. Scegliere qualsiasi nome e fare clic su **OK**.
   4. Nel **viste** elenco a discesa elenco, assicurarsi che **Razor** sia selezionata.

      ![Aggiunta di controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "aggiungendo il controller di persona con scaffolding")

      *Aggiunta di controller di persona con scaffolding*
9. Fare clic su **Add** per creare il nuovo controller per persona con scaffolding. Le azioni del controller, nonché le visualizzazioni sono stati generati.

    ![Dopo aver creato il controller di persona con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "dopo aver creato il controller di persona con scaffolding")

    *Dopo aver creato il controller di persona con scaffolding*
10. Aprire **PersonController** classe. Si noti che i metodi di azione CRUD completi siano stati generati automaticamente.

   ![All'interno del controller di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controller all'interno di persona")

   *All'interno del controller di persona*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Attività 2-esecuzione dell'applicazione

A questo punto, il database non è ancora creato. In questa attività, eseguire l'applicazione per la prima volta e i test verranno le operazioni CRUD. Il database verrà creato in tempo reale con Code First.

1. Premere **F5** per eseguire l'applicazione.
2. Nel browser, aggiungere **/Person** all'URL per aprire la pagina di persona.

    ![Prima dell'esecuzione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "prima dell'esecuzione dell'applicazione")

    *Dell'applicazione: eseguire prima*
3. Verrà ora esplorare le pagine di persona e testare le operazioni CRUD.

    1. Fare clic su **Crea nuovo** per aggiungere una nuova persona. Immettere un nome e un cognome e fare clic su **Create**.

        ![Aggiunta di una nuova persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "aggiunta una nuova persona")

        *Aggiunta di una nuova persona*
    2. Nell'elenco di una persona, è possibile eliminare, modificare o aggiungere elementi.

        ![elenco di persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "elenco persone")

        *Elenco di persona*
    3. Fare clic su **dettagli** per aprire i dettagli di una persona.

        ![I dettagli della persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "i dettagli della persona")

        *Dettagli della persona*
4. Chiudere il browser e tornare a Visual Studio. Si noti che sia stato creato l'intera CRUD per l'entità person in tutta l'applicazione - dal modello per le visualizzazioni - senza dover scrivere una singola riga di codice!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Attività 3: aggiornamento di database tramite migrazioni di Entity Framework

In questa attività si aggiornerà il database tramite migrazioni di Entity Framework. Si scoprirà quanto è facile modificare il modello e riflettere le modifiche nei database utilizzando la funzionalità migrazioni di Entity Framework.

1. Aprire la Console di gestione pacchetti. Selezionare **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti**.
2. Nella Console di gestione pacchetti immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Abilitare migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "consentendo migrazioni")

    *Abilitare migrazioni*

    Il comando Enable-migrazione crea il **migrazioni** cartella che contiene uno script per inizializzare il database.

    ![Cartella migrazioni](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "cartella migrazioni")

    *Cartella migrazioni*
3. Aprire il **Configuration.cs** file nella cartella migrazioni. Individuare il costruttore della classe e modificare il **AutomaticMigrationsEnabled** valore *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Aprire la classe Person e aggiungere un attributo per il nome della persona intermedio. Con questo nuovo attributo, si modifica il modello.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Selezionare **compilare | Compila soluzione** nel menu per compilare l'applicazione.

    ![Compilazione dell'applicazione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilazione dell'applicazione")

    *Compilazione dell'applicazione*
6. Nella Console di gestione pacchetti immettere il comando seguente:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Questo comando Cerca modifiche agli oggetti dati e quindi, aggiunge i comandi necessari per modificare di conseguenza il database.

    ![Aggiunta di un nome intermedio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "aggiungendo un secondo nome")

    *Aggiunta di un secondo nome*
7. (Facoltativo) È possibile eseguire il comando seguente per generare uno script SQL con l'aggiornamento di backup differenziale. Questo modo sarà possibile aggiornare manualmente il database (In questo caso non è necessario), o applicare le modifiche in altri database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generazione di uno script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generazione di uno script SQL")

    *Generazione di uno script SQL*

    ![Aggiornamento di Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aggiornamento Script SQL")

    *Aggiornamento di Script SQL*
8. Nella Console di gestione pacchetti immettere il comando seguente per aggiornare il database:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![L'aggiornamento del Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "l'aggiornamento del Database")

    *L'aggiornamento del Database*

    Verrà aggiunta la **MiddleName** colonna il **persone** tabella corrisponde alla definizione corrente del **persona** classe.
9. Dopo aver aggiornato il database, fare doppio clic nella cartella Controller e selezionare **Add | Controller** per aggiungere nuovamente il controller persona (completa con gli stessi valori). Verranno aggiornate le visualizzazioni aggiunta del nuovo attributo e i metodi esistenti.

    ![Aggiunta di un aggiornamento del controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "aggiunta di un aggiornamento del controller")

    *L'aggiornamento del controller*
10. Fare clic su **Aggiungi**. Quindi, selezionare i valori **sovrascrivere PersonController.cs** e il **Sovrascrivi visualizzazioni associate** e fare clic su **OK**.

   ![Aggiunta di una sovrascrittura controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *L'aggiornamento del controller*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - esecuzione dell'applicazione

1. Premere **F5** per eseguire l'applicazione.
2. Aprire **/Person**. Si noti che i dati è stati mantenuti, mentre il secondo nome colonna è stata aggiunta.

    ![Secondo nome aggiunti](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name aggiunto")

    *Secondo nome aggiunto*
3. Se si sceglie **modifica**, sarà possibile aggiungere un secondo nome per l'utente corrente.

    ![Secondo nome edizione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "secondo nome edizione")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico, si sono appreso semplici passaggi per creare operazioni CRUD con ASP.NET MVC 4 Scaffolding usando qualsiasi classe di modello. Quindi, si è appreso come eseguire un aggiornamento to end nell'applicazione - dal database per le viste - mediante migrazioni di Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Appendice a: installazione di Visual Studio Express 2012 per Web

È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.

1. Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.
2. Fare clic su **Installa ora**. Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.
3. Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.

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
7. Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.
8. Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Visual Studio Express per il riquadro Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Appendice b: uso dei frammenti di codice

Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione. Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.

![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "frammenti di codice con Visual Studio per inserire codice nel progetto")

*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*

***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***

1. Posizionare il cursore in cui si vuole inserire il codice.
2. Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).
3. Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.
4. Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).
5. Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.

![Iniziare a digitare il nome di frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "inizia a digitare il nome del frammento di codice")

*Iniziare a digitare il nome del frammento di codice*

![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "premere Tab per selezionare il frammento di codice evidenziata")

*Premere Tab per selezionare il frammento di codice evidenziata*

![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "si espanderà il frammento di codice e premere nuovamente Tab")

*Il frammento di codice e premere nuovamente Tab espanderà*

***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1. Pulsante destro del mouse in cui si desidera inserire il frammento di codice.

1. Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.
2. Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.

![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")

*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*

![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")

*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*
