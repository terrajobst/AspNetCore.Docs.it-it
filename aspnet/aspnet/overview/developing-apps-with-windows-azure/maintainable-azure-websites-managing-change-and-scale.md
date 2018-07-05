---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratorio pratico: siti Web di Azure gestibili: gestione del cambiamento e scalabilità | Microsoft Docs'
author: rick-anderson
description: In questo laboratorio, informazioni su come Microsoft Azure semplifica compilare e distribuire siti Web nell'ambiente di produzione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: e7dcca855e55d10926de6d5e11e5a40e766128fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385353"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratorio pratico: siti Web di Azure gestibili: gestione del cambiamento e scalabilità
====================
da [Camp Web Team](https://twitter.com/webcamps)

[Download Web Camp Kit di formazione](http://aka.ms/webcamps-training-kit)

> Microsoft Azure semplifica compilare e distribuire siti Web nell'ambiente di produzione. Ma non abbiamo ancora finito quando l'applicazione è in tempo reale, appena agli inizi. È necessario gestire modifica i requisiti, gli aggiornamenti del database, scalabilità e altro ancora. Fortunatamente, servizio App di Azure è te, con una vasta gamma di funzionalità che consentono di mantenere i siti in esecuzione senza problemi.
> 
> Azure offre sicure e flessibile di sviluppo, distribuzione e opzioni di ridimensionamento per qualsiasi applicazione web di dimensioni. Sfrutta gli strumenti esistenti per creare e distribuire le applicazioni senza la complessità di gestione dell'infrastruttura.
> 
> Effettuare il provisioning di un'applicazione web di produzione se stessi in pochi minuti con facilità la distribuzione di contenuti creati usando lo strumento di sviluppo preferito. È possibile distribuire un sito esistente direttamente dal controllo del codice sorgente con il supporto per **Git**, **GitHub**, **Bitbucket**, **TFS**e persino  **DropBox**. Distribuire direttamente dal tuo IDE preferito o da script mediante **PowerShell** in Windows o **CLI** strumenti in esecuzione in qualsiasi sistema operativo. Una volta distribuito, mantenere i tuoi siti costantemente aggiornato con il supporto per la distribuzione continua.
> 
> Azure offre soluzioni di ripristino, backup e archiviazione cloud scalabili e durevoli per tutti i dati, indipendentemente dalle dimensioni. Quando si distribuiscono applicazioni per un ambiente di produzione, servizi di archiviazione, ad esempio tabelle, BLOB e database SQL, che consentono di ridimensionare l'applicazione nel cloud.
> 
> Con i database SQL, è importante mantenere aggiornato il database produttività durante la distribuzione di nuove versioni dell'applicazione. Grazie a **migrazioni di Entity Framework Code First**, sviluppo e alla distribuzione del modello di dati è stata semplificata per aggiornare gli ambienti in pochi minuti. Questa esercitazione pratica illustra i diversi argomenti che potrebbe riscontrare quando si distribuisce l'app web in ambienti di produzione in Microsoft Azure.
> 
> Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile all'indirizzo [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
> 
> Per una trattazione più approfondita di questo argomento, vedere la [creazione di App Cloud reali con e-book Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questo laboratorio pratico, si apprenderà come:

- Abilitare migrazioni di Entity Framework con un modello esistente
- Aggiornare il modello a oggetti e il database conseguenza tramite migrazioni di Entity Framework
- Distribuire nel servizio App di Azure tramite Git
- Eseguire il rollback a una distribuzione precedente tramite il portale di gestione di Azure
- Usare l'archiviazione di Azure per ridimensionare un'app web
- Configurare la scalabilità automatica per un'app web usando il portale di gestione di Azure
- Creare e configurare un progetto di test di carico in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questo laboratorio pratico:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Azure SDK per .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Sistema di controllo della versione GIT](http://git-scm.com/download)
- Una sottoscrizione di Microsoft Azure 

    - Iscriversi per una [versione di valutazione gratuita](http://aka.ms/watk-freetrial)
    - Se sei un Visual Studio Professional, Test Professional, Premium o Ultimate con MSDN o MSDN Platforms sottoscrittore, attivare i [benefici MSDN per](http://aka.ms/watk-msdn) adesso per iniziare lo sviluppo e test in Azure
    - [BizSpark](http://aka.ms/watk-bizspark) i membri ricevono automaticamente Azure vantaggio attraverso le Visual Studio sottoscrizioni Ultimate con MSDN
    - I membri del [Microsoft Partner Network](http://aka.ms/watk-mpn) programma Cloud Essentials ricevono crediti mensili di Azure senza costi aggiuntivi

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi in questo laboratorio pratico, è necessario configurare prima di tutto l'ambiente.

1. Aprire Windows Explorer e passare del lab **origine** cartella.
2. Fare clic su **Setup. cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi che tutte le dipendenze per questo ambiente lab che sono stati verificati prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando i frammenti di codice

In tutto il documento di laboratorio, verrà invitati a inserire blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013, per evitare che sia necessario aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia che si trova nel **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dagli altri. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a quando non si have completato l'esercizio. All'interno del codice sorgente per un esercizio, si noterà anche un **End** cartella che contiene una soluzione di Visual Studio con il codice che scaturisce da completare i passaggi nell'esercizio corrispondente. È possibile usare queste soluzioni come prassi consigliata se ti serve assistenza aggiuntiva durante l'esecuzione in questo laboratorio pratico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questo laboratorio pratico include gli esercizi seguenti:

1. [Tramite migrazioni di Entity Framework](#Exercise1)
2. [Distribuisce un'App Web in gestione temporanea](#Exercise2)
3. [Esegue il Rollback di distribuzione nell'ambiente di produzione](#Exercise3)
4. [Scalabilità con archiviazione di Azure](#Exercise4)
5. [Usa la scalabilità automatica per le app Web](#Exercise5) (facoltativo per l'edizione di Visual Studio 2013 Ultimate)

Tempo stimato per completare questa esercitazione: **75 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è pensata per associare un particolare stile di sviluppo e determina i layout delle finestre, il comportamento dell'editor, frammenti di codice IntelliSense e opzioni della finestra di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si usa la **delle impostazioni di sviluppo generale** raccolta. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario tenere conto.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Esercizio 1: Tramite migrazioni di Entity Framework

Quando si sviluppa un'applicazione, il modello di dati può cambiare nel tempo. Queste modifiche rischia di rallentare il modello esistente nel database (se si sta creando una nuova versione) ed è importante tenere aggiornate per evitare errori di database.

Per semplificare il rilevamento delle modifiche nel modello, **migrazioni di Entity Framework Code First** automaticamente rilevare le modifiche di confronto del modello con lo schema del database e genera il codice specifico per aggiornare il database, creazione di nuove *versioni* del database.

Questo esercizio illustra come abilitare **migrazioni** per l'applicazione e come si possono facilmente rilevare e generare le modifiche per aggiornare i database.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Attività 1: abilitare le migrazioni

In questa attività verrà illustrata la procedura di abilitazione **migrazioni di Entity Framework Code First** per il **fanatico Quiz** database, la modifica del modello e la comprensione di come tali modifiche si rifletteranno nel database.

1. Aprire Visual Studio e aprire il **GeekQuiz.sln** file di soluzione dal **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Compilare la soluzione per scaricare e installare il **NuGet** dipendenze dei pacchetti. A tale scopo, fare doppio clic la soluzione e fare clic su **Compila soluzione** o premere **Ctrl + MAIUSC + B**.
3. Dal **degli strumenti** dal menu di Visual Studio, selezionare **Library Package Manager**e quindi fare clic su **Package Manager Console**.
4. Nel **Console di gestione pacchetti**, immettere il comando seguente e quindi premere **invio**. Verrà creata una migrazione iniziale in base al modello esistente.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Abilitare migrazioni](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "consentendo migrazioni")

    *Abilitare migrazioni*

    > [!NOTE]
    > Questo comando aggiunge un **migrazioni** cartella al progetto fanatico Quiz contenente un file chiamato **Configuration.cs**. Il **configurazione** classe consente di configurare il comportamento di migrazioni per il contesto.
5. Le migrazioni abilitate, è necessario aggiornare il **Configuration** classe per popolare il database con dati iniziali che **fanatico Quiz** richiede. Sotto **migrazioni**, sostituire il **Configuration.cs** file con quello che si trova nel **Source\Assets** cartella della presente esercitazione.

    > [!NOTE]
    > Poiché **migrazioni** chiamerà il **Seed** metodo con ogni aggiornamento del database, è necessario verificare che i record non siano duplicati nel database. Il **AddOrUpdate** metodo è utile per evitare dati duplicati.
6. Per aggiungere una migrazione iniziale, immettere il comando seguente e quindi premere **invio**.

    > [!NOTE]
    > Assicurarsi che non vi sia alcun database denominato &quot;GeekQuizProd&quot; nell'istanza di LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Aggiunta di migrazione dello schema di base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "migrazione dello schema di base di aggiunta")

    *Aggiunta di migrazione dello schema di base*

    > [!NOTE]
    > **Add-Migration** eseguirà lo scaffolding della migrazione successiva in base alle modifiche apportate al modello poiché è stata creata l'ultima migrazione. In questo caso, perché è la prima migrazione del progetto, aggiungerà gli script per creare tutte le tabelle definite nel **TriviaContext** classe.
7. Eseguire la migrazione per aggiornare il database eseguendo il comando seguente. Per questo comando si specificherà il **Verbose** flag per visualizzare le istruzioni SQL viene applicate al database di destinazione.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Database iniziale di creazione](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "creazione database iniziale")

    *Creazione di database iniziale*

    > [!NOTE]
    > **Update-Database** applicherà eventuali in sospeso migrazioni al database. In questo caso, creerà il database usando la stringa di connessione definita nel file Web. config.
8. Passare a **View** menu e open **Esplora oggetti di SQL Server**.

    ![Apri in Esplora oggetti SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Apri in Esplora oggetti SQL Server")

    *Apri in Esplora oggetti SQL Server*
9. Nel **Esplora oggetti di SQL Server** finestra, connettersi all'istanza di Local DB facendo clic con il **SQL Server** nodo e selezionando **Aggiungi SQL Server...**  opzione.

    ![Aggiunta di un'istanza di SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "aggiunta di un'istanza di SQL Server")

    *Aggiunta di un'istanza di SQL Server da Esplora oggetti di SQL Server*
10. Impostare il **nome server** al *\v11.0 (localdb)* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connect** per continuare.

    ![La connessione a LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "la connessione a LocalDB")

    *La connessione a LocalDB*
11. Aprire il **GeekQuizProd** del database ed espandere le **tabelle** nodo. Come può notare, il **Update-Database** comando generate tutte le tabelle definite nel **TriviaContext** classe. Individuare il **dbo. TriviaQuestions** di tabella e aprire il nodo colonne. Nell'attività successiva, si verrà aggiunta una nuova colonna alla tabella e aggiornare i database usando **migrazioni**.

    ![Gli elementi semplici domande a cui le colonne](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "gli elementi semplici domande a cui le colonne")

    *Gli elementi semplici domande a cui le colonne*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Attività 2: aggiornamento dello Schema di Database usando migrazioni

In questa attività si userà **migrazioni di Entity Framework Code First** rileva una modifica nel modello e generare il codice necessario per aggiornare il database. Si aggiornerà il **TriviaQuestions** entità mediante l'aggiunta una nuova proprietà. Quindi si eseguirà i comandi per creare una nuova migrazione in modo da includere la nuova colonna nella tabella.

1. In **Esplora soluzioni**, fare doppio clic sui **TriviaQuestion.cs** file che si trova all'interno di **modelli** cartella.
2. Aggiungere una nuova proprietà denominata **Hint**, come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Nel **Console di gestione pacchetti**, immettere il comando seguente e quindi premere **invio**. Verrà creata una nuova migrazione che riflette la modifica nel modello.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > Un file di migrazione è costituito da due metodi, **iscrizione** e **verso il basso**.
    > 
    > - Il **backup** metodo verrà utilizzato per specificare quali modifiche la versione corrente di esigenze dell'applicazione da applicare al database.
    > - Il **verso il basso** viene utilizzato per annullare le modifiche sono state aggiunte per il **backup** (metodo).
    > 
    > Quando la migrazione del Database aggiorna il database, eseguirà tutte le migrazioni in ordine di timestamp e solo quelli che non sono stati utilizzati dopo l'ultimo aggiornamento (il \_MigrationHistory tabella tiene traccia di quali migrazioni sono state applicate). Il **backup** metodo di tutte le migrazioni verrà chiamato e apporta le modifiche è stato specificato per il database. Se decidiamo di tornare a una migrazione precedente, il **verso il basso** metodo verrà chiamato per ripristinare le modifiche in ordine inverso.
4. Nel **Console di gestione pacchetti**, immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. L'output del **Update-Database** comando generato un **Alter Table** istruzione SQL per aggiungere una nuova colonna per il **TriviaQuestions** di tabella, come illustrato nell'immagine seguente.

    ![Aggiungere l'istruzione SQL colonna generata](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Aggiungi istruzione di colonna SQL generato")

    *Aggiungi istruzione di colonna SQL generato*
6. Nelle **Esplora oggetti di SQL Server**, aggiornare il **dbo. TriviaQuestions** tabella e verificare che il nuovo **Hint** colonna viene visualizzata.

    ![Che mostra la nuova colonna Hint](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "che mostra la nuova colonna Hint")

    *Che mostra la nuova colonna Hint*
7. Nel **TriviaQuestion.cs** editor aggiungere una **StringLength** vincolo per il *Hint* proprietà, come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Nel **Console di gestione pacchetti**, immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Nel **Console di gestione pacchetti**, immettere il comando seguente e quindi premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. L'output del **Update-Database** comando generato un **Alter Table** istruzione SQL per aggiornare il *hint* colonna di tipo i **TriviaQuestions** tabella, come illustrato nell'immagine seguente.

    ![Istruzione di modifica colonna SQL generato](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "istruzione di modifica colonna SQL generato")

    *Istruzione di modifica colonna SQL generato*
11. Nelle **Esplora oggetti di SQL Server**, aggiornare il **dbo. TriviaQuestions** tabella e verificare che il **Hint** tipo di colonna **nvarchar(150)**.

    ![Che mostra il nuovo vincolo](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "che mostra il nuovo vincolo")

    *Che mostra il nuovo vincolo*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Esercizio 2: Distribuzione di un'App Web in gestione temporanea

**App Web nel servizio App di Azure** consente di eseguire la pubblicazione di gestione temporanea. Pubblicazione di gestione temporanea crea uno slot di sito di gestione temporanea per ogni sito di produzione predefinito e consente di sostituire questi slot senza tempo di inattività. Ciò è particolarmente utile per convalidare le modifiche prima del rilascio al pubblico, in modo incrementale integrare il contenuto del sito ed eseguire il rollback delle modifiche non funzionino come previsto.

In questo esercizio, verrà distribuito il **fanatico Quiz** applicazione all'ambiente di staging dell'app web con controllo del codice sorgente Git. A tale scopo, verrà creare l'app web e i componenti necessari nel portale di gestione di effettuare il provisioning, configurare un **Git** repository e il push dell'applicazione nel codice sorgente dal computer locale per lo slot di staging. Anche se si aggiornerà il database di produzione con il **migrazioni Code First** creata nell'esercizio precedente. Quindi si eseguirà l'applicazione nell'ambiente di test per verificare il funzionamento. Quando sarà soddisfatti del funzionamento in base alle aspettative, alzare di livello l'applicazione nell'ambiente di produzione.

> [!NOTE]
> Per abilitare la pubblicazione di gestione temporanea, l'app web deve essere **modalità Standard**. Si noti che saranno soggette a costi aggiuntivi se si modifica l'app web in modalità Standard. Per altre informazioni sui prezzi, vedere [prezzi di servizio App](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Attività 1: creazione di un'App Web in servizio App di Azure

In questa attività si creerà un'app web nel **servizio App di Azure** dal portale di gestione. Si configurerà anche un **Database SQL** per rendere persistenti i dati dell'applicazione e configurare un repository Git locale per controllo del codice sorgente.

1. Andare alla [portale di gestione Azure](https://manage.windowsazure.com) e accedere usando l'account Microsoft associato alla sottoscrizione.

    ![Accedere al portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Accedere al portale di gestione di Azure*
2. Fare clic su **New** nella barra dei comandi nella parte inferiore della pagina.

    ![Creazione di una nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "crea una nuova app web")

    *Creazione di una nuova app web*
3. Fare clic su **Compute**, **sito Web** e quindi **creazione personalizzata**.

    ![Crea una nuova app web usando creazione personalizzata](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "crea una nuova app web usando creazione personalizzata")

    *Crea una nuova app web usando creazione personalizzata*
4. Nel **nuovo sito Web - creazione personalizzata** finestra di dialogo immettere un disponibile **URL** (ad esempio, *fanatico quiz*), selezionare un percorso nel **area** elenco a discesa e scegliere **creare un nuovo database SQL** nel **Database** elenco a discesa. Selezionare infine il **pubblica dal controllo del codice sorgente** casella di controllo e fare clic su **successivo**.

    ![Personalizzare la nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalizzare la nuova app web*
5. Specificare le informazioni seguenti per le impostazioni del database:

   - Nel **Name** testo casella, immettere un nome di database (ad esempio *geekquiz\_db*)
   - Nel Server **elenco a discesa** elenco, selezionare **server di database SQL nuova**. In alternativa, è possibile selezionare un server esistente.
   - Nel **nome utente del Database** e **password Database** caselle, immettere il nome utente dell'amministratore e la password per il server di database SQL. Se si seleziona un server è già stato creato, verrà richiesto di specificarla.

     ![Specifica delle impostazioni del database](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Specifica delle impostazioni del database*
6. Fare clic su **Avanti** per continuare.
7. Selezionare **repository Git locale** per il controllo del codice sorgente da usare e fare clic su **successivo**.

    > [!NOTE]
    > Potrebbero essere richieste le credenziali di distribuzione (nome utente e password).

    ![Creazione del Repository Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Creazione del repository Git*
8. Attendere finché non viene creata la nuova app web.

    > [!NOTE]
    > Per impostazione predefinita, Azure offre domini *azurewebsites.net* ma offre anche la possibilità di impostare i domini personalizzati tramite il portale di gestione di Azure. Tuttavia, è possibile gestire i domini personalizzati solo se si utilizza determinate modalità di servizio App di Azure.
    > 
    > Servizio App di Azure è disponibile nelle edizioni gratuito, condiviso, Basic, Standard e Premium. Nella modalità gratuito e condiviso, tutte le app web eseguite in un ambiente multi-tenant e sono previste quote per l'utilizzo della CPU, memoria e rete. Il numero massimo di App gratuite può variare con il piano. Nella modalità Standard, si scelgono le risorse di calcolo quali le app eseguite in macchine virtuali dedicate che corrispondono al standard di Azure. È possibile trovare la configurazione della modalità di app web nel **scalabilità** menu dell'app web.
    > 
    > ![Modalità di servizio App di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "le modalità di servizio App di Azure")
    > 
    > Se si usa **Shared** oppure **Standard** modalità, sarà in grado di gestire i domini personalizzati per l'app web, passare all'app **configura** menu e scegliendo **Manage Domains** sotto *nomi di dominio*.
    > 
    > ![Gestire i domini](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "gestire domini")
    > 
    > ![Gestire domini personalizzati](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "gestire domini personalizzati")
9. Dopo aver creato l'app web, fare clic sul collegamento sotto i **URL** colonna per verificare che la nuova app web sia in esecuzione.

    ![Esplorazione per la nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Esplorazione per la nuova app web*

    ![app Web in esecuzione](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *app Web in esecuzione*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Attività 2: creazione del Database SQL di produzione

In questa attività si userà il **migrazioni di Entity Framework Code First** per creare il database come destinazione il **Database SQL di Azure** istanza creata nell'attività precedente.

1. Nel portale di gestione, passare all'app web creata nell'attività precedente e passare al relativo **Dashboard**.
2. Nel **Dashboard** pagina, fare clic su **Visualizza stringhe di connessione** collegamento sotto i **riepilogo rapido** sezione.

    ![Visualizza stringhe di connessione](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Visualizza stringhe di connessione")

    *Visualizza stringhe di connessione*
3. Copia il **stringa di connessione** valore e chiudere la finestra di dialogo.

    ![Stringa di connessione nel portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "stringa di connessione nel portale di gestione di Azure")

    *Stringa di connessione nel portale di gestione di Azure*
4. Fare clic su **database SQL** per visualizzare l'elenco di database SQL di Azure

    ![Menu di Database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "dal menu di Database SQL")

    *Menu di Database SQL*
5. Individuare il database creato nell'attività precedente e fare clic sul Server.

    ![Server di Database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "server di Database SQL")

    *Server di Database SQL*
6. Nel **avvio rapido** pagina del server, fare clic su **configura**.

    ![Menu Configura](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "menu Configura")

    *Configurare i menu*
7. Nel **Allowed IP addresses** sezione, fare clic su **aggiungere agli indirizzi IP consentiti** collegamento per abilitare l'indirizzo IP per la connessione al server di Database SQL.

    ![Indirizzi IP consentiti](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "gli indirizzi IP consentiti")

    *Indirizzi IP consentiti*
8. Fare clic su **salvare** nella parte inferiore della pagina per completare il passaggio.
9. Tornare a Visual Studio.
10. Nel **Console di gestione pacchetti**, eseguire il comando seguente sostituendo *[YOUR-CONNECTION-STRING]* segnaposto con la stringa di connessione copiata da Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aggiornare il database come destinazione Database SQL di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "aggiornare il database come destinazione Database SQL di Azure")

    *Aggiornare il database come destinazione Database SQL di Azure*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Attività 3: distribuzione Quiz appassionato alla gestione temporanea usando Git

In questa attività si abiliterà pubblicazione di gestione temporanea nell'app web. Quindi, si utilizzerà Git per pubblicare l'applicazione fanatico Quiz direttamente dal computer locale all'ambiente di staging dell'app web.

1. Tornare al portale e fare clic sul nome dell'app web con il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione di app web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Aprire le pagine di gestione di app web*
2. Passare il **scalabilità** pagina. Sotto il **generali** sezione, selezionare **Standard** per la configurazione e fare clic su **Salva** nella barra dei comandi.

    > [!NOTE]
    > Per eseguire tutte le app web nell'area corrente di sottoscrizione nel **Standard** modalità, lasciare il **Seleziona tutto** selezionata nella casella di controllo il **siti scegliere** configurazione. In caso contrario, deselezionare il **Seleziona tutto** casella di controllo.

    ![L'aggiornamento dell'app web alla modalità Standard](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "l'aggiornamento dell'app web alla modalità Standard")

    *L'aggiornamento dell'App Web alla modalità Standard*
3. Fare clic su **Sì** per confermare le modifiche.

    ![Per confermare la modifica alla modalità Standard](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "continuare con la modifica della modalità di app web")

    *Per confermare la modifica alla modalità Standard*
4. Andare alla **Dashboard** e fare clic su **Abilita pubblicazione di staging** sotto il **riepilogo rapido** sezione.

    ![Abilitazione della pubblicazione di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Abilita pubblicazione di staging")

    *Abilitazione della pubblicazione di gestione temporanea*
5. Fare clic su **Sì** per abilitare la pubblicazione di gestione temporanea.

    ![Per confermare la pubblicazione di staging](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "facendo clic su Sì per abilitare la pubblicazione di gestione temporanea")

    *Per confermare di pubblicazione di gestione temporanea*
6. Nell'elenco delle App web, espandere il contrassegno a sinistra del nome dell'app web per visualizzare lo slot di sito di staging. Ha il nome dell'app web, seguito da ***(gestione temporanea)***. Fare clic sul sito di staging per passare alla pagina di gestione.

    ![Passare all'app web di staging](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "passando all'app web di staging")

    *Passare all'app di gestione temporanea*
7. Si noti che tale pagina di gestione he è simile a pagina di gestione di qualsiasi dell'app web. Passare al **distribuzioni** pagina e copiare la **URL Git** valore. Si userà, più avanti in questo esercizio.

    ![Quando il valore di URL Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Quando il valore di URL Git*
8. Aprire una nuova **Git Bash** console ed eseguire i comandi seguenti. Aggiornamento di *[YOUR-APPLICATION-PATH]* segnaposto con il percorso il **GeekQuiz** soluzione, che si trova nel **Source\Ex1 DeployingWebSiteToStaging\Begin** cartella di questa esercitazione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inizializzazione di GIT e primo commit](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inizializzazione di GIT e primo commit*
9. Eseguire il comando seguente per eseguire il push all'app web remota **Git** repository. Sostituire il segnaposto con l'URL ottenuto dal portale di gestione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Eseguire il push in Microsoft Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Il push in Azure*

    > [!NOTE]
    > Quando si distribuisce contenuto nell'host FTP o un repository GIT di un'app web, è necessario eseguire l'autenticazione usando il **credenziali di distribuzione** che è stato creato da dell'app web **avvio rapido** o **Dashboard**  le pagine di gestione. Se non si conoscono le credenziali di distribuzione è possibile reimpostare con facilità usando il portale di gestione. Aprire l'app web **Dashboard** e fare clic sui **reimpostare le credenziali di distribuzione** collegamento. Fornire una nuova password e fare clic su **OK**. Le credenziali di distribuzione sono valide per l'uso con tutte le app web associate alla sottoscrizione.
10. Per verificare l'app web è stato eseguito il push in Azure, tornare al portale di gestione e fare clic su **siti Web**.
11. Individuare l'app web ed espandere la voce per visualizzare lo slot di sito di staging. Fare clic sul relativo **nome** per passare alla pagina di gestione.
12. Fare clic su **distribuzioni** per visualizzare i **cronologia di distribuzione**. Verificare che sia presente un' **distribuzione attiva** con il  *&quot;Commit iniziale&quot;*.

    ![Distribuzione attiva](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Distribuzione attiva*
13. Infine, fare clic su **esplorare** nella barra dei comandi per passare all'app web.

    ![Esplorare l'app web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Esplorare l'app web*
14. Se l'applicazione viene distribuita correttamente, si verrà visualizzata la pagina di accesso fanatico Quiz.

    > [!NOTE]
    > L'URL dell'indirizzo dell'applicazione distribuita contiene il nome dell'app web aggiungendo *-gestione temporanea*.

    ![Applicazione in esecuzione nell'ambiente di staging](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Applicazione in esecuzione nell'ambiente di staging*
15. Se si vuole esplorare l'applicazione, fare clic su **registrare** per registrare un nuovo utente. Completare i dettagli dell'account, immettere un nome utente, indirizzo di posta elettronica e una password. Successivamente, l'applicazione mostra la prima domanda del quiz. Rispondi ad alcune domande per assicurarsi che tutto funzioni come previsto.

    ![Applicazione pronta per essere usata](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Applicazione pronta per essere usata*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Attività 4: innalzamento di livello all'App Web di produzione

Ora che è accertato che l'app web funzioni correttamente nell'ambiente di staging, si è pronti a promuoverlo alla produzione. In questa attività si eseguirà lo scambio di slot di sito di staging con lo slot di sito di produzione.

1. Tornare al portale di gestione e selezionare lo slot di sito di staging. Fare clic su **scambiare** nella barra dei comandi.

    ![Passa in produzione](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Passa in produzione*
2. Fare clic su **Sì** nella finestra di dialogo di conferma per procedere con l'operazione di scambio. Azure eseguirà immediatamente lo scambio il contenuto del sito di produzione con il contenuto del sito di staging.

    > [!NOTE]
    > Alcune impostazioni dalla versione di gestione temporanea verranno copiate automaticamente la versione di produzione (ad esempio, stringa di connessione sostituzioni, mapping dei gestori e così via) ma non modificate altre impostazioni (ad esempio, gli endpoint DNS, le associazioni SSL e così via).

    ![Conferma operazione di scambio](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Conferma operazione di scambio*
3. Una volta completato lo scambio, selezionare lo slot di produzione e fare clic su **esplorare** nella barra dei comandi per aprire il sito di produzione. Si noti che l'URL nella barra degli indirizzi.

    > [!NOTE]
    > Si potrebbe essere necessario aggiornare il browser per la cancellazione della cache. In Internet Explorer, è possibile farlo premendo **CTRL + R**.

    ![App Web in esecuzione nell'ambiente di produzione](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Nel **GitBash** console, aggiornare l'URL remoto per il repository Git locale per lo slot di produzione di destinazione. A tale scopo, eseguire il comando seguente, sostituendo i segnaposto con il nome utente di distribuzione e il nome dell'app web.

    > [!NOTE]
    > Negli esercizi seguenti, si effettuerà il push delle modifiche al sito di produzione, invece di gestione temporanea solo per la semplicità del lab. In uno scenario reale, è consigliabile verificare le modifiche nell'ambiente di staging prima di promuovere nell'ambiente di produzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Esercizio 3: Esecuzione di rollback della distribuzione nell'ambiente di produzione

Esistono scenari in cui non è uno slot di staging per l'esecuzione frequente scambio tra gestione temporanea e produzione, ad esempio, se si sta lavorando **gratuito** oppure **condiviso** modalità. In tali scenari, è consigliabile testare l'applicazione in un ambiente di testing – in locale o in un sito remoto: prima della distribuzione nell'ambiente di produzione. Tuttavia, è possibile che potrebbe verificarsi un problema non viene rilevato durante la fase di test nel sito di produzione. In questo caso, è importante disporre di un meccanismo per passare facilmente a una versione precedente e maggiore stabilità dell'applicazione nel minor tempo.

Nelle **servizio App di Azure**, la distribuzione continua dal controllo del codice sorgente rende questo possibile grazie alla **ridistribuire** azione disponibile nel portale di gestione. Azure tiene traccia delle distribuzioni associate ai commit inseriti nel repository e offre un'opzione per ridistribuire l'applicazione utilizzando qualsiasi delle distribuzioni precedenti, in qualsiasi momento.

In questo esercizio si eseguirà una modifica al codice nel **fanatico Quiz** dell'applicazione che inserisce intenzionalmente una *bug*. Si distribuirà l'applicazione nell'ambiente di produzione per vedere l'errore e quindi si sfrutterà la funzionalità di ridistribuzione per tornare allo stato precedente.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Attività 1: aggiornamento dell'applicazione di Quiz fanatico

In questa attività è eseguire il refactoring una piccola parte di codice le **TriviaController** classe per estrarre una parte della logica che consente di recuperare l'opzione quiz selezionato dal database in un nuovo metodo.

1. Passare all'istanza di Visual Studio con il **GeekQuiz** soluzione nell'esercizio precedente.
2. Nella **Esplora soluzioni**, aprire il **TriviaController.cs** all'interno del file il **controller** cartella.
3. Individuare il **StoreAsync** (metodo) e selezionare il codice evidenziato nella figura seguente.

    ![Selezionare il codice](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Selezionare il codice*
4. Fare clic sul codice selezionato, espandere la **refactoring** menu e selezionare **Estrai metodo...** .

    ![Estrarre il codice come un nuovo metodo](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Selezione metodo di estrazione*
5. Nel **Estrai metodo** della finestra di dialogo Nome nuovo metodo *MatchesOption* e fare clic su **OK**.

    ![Specifica il nome del metodo](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Che specifica il nome per il metodo estratto*
6. Il codice selezionato viene quindi estratto nel **MatchesOption** (metodo). Il codice risulta è illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Premere **CTRL + S** per salvare le modifiche.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Attività 2: ridistribuire l'applicazione di Quiz fanatico

Si effettuerà il push a questo punto le modifiche apportate nell'attività precedente al repository, che verrà attivata una nuova distribuzione nell'ambiente di produzione. Quindi, verrà risoluzione dei problemi un problema utilizzando il **strumenti di sviluppo F12** ottenuta con Internet Explorer e quindi eseguire il rollback alla distribuzione precedente dal portale di gestione di Azure.

1. Aprire una nuova **Git Bash** console per distribuire l'applicazione aggiornata in servizio App di Azure.
2. Eseguire i comandi seguenti per il push delle modifiche in Azure. Aggiornamento di *[YOUR-APPLICATION-PATH]* segnaposto con il percorso per il **GeekQuiz** soluzione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Il push del codice sottoposto a refactoring in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Il push del codice sottoposto a refactoring in Azure*
3. Aprire Internet Explorer e passare all'App web (ad esempio `http://<your-web-site>.azurewebsites.net`). Accedere usando le credenziali create in precedenza.
4. Premere **F12** per avviare gli strumenti di sviluppo, selezionare la **Network** scheda e fare clic sui **riprodurre** pulsante per avviare la registrazione.

    ![Avviare la registrazione di rete](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "avviare la registrazione di rete")

    *Avviare la registrazione di rete*
5. Selezionare qualsiasi opzione del quiz. Si noterà che non accade nulla.
6. Nel **F12** finestra, la voce corrispondente alla richiesta HTTP POST viene illustrato un HTTP **500** risultato.

    ![HTTP 500 Errore](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Errore*
7. Selezionare il **Console** scheda. Con i dettagli della causa, viene registrato un errore.

    ![Errore registrato](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Errore registrato*
8. Individuare la parte di dettagli dell'errore. Ovviamente, questo errore è causato dal codice di refactoring è stato eseguito il commit nei passaggi precedenti.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Non chiudere il browser.
10. In una nuova istanza del browser, passare al [portale di gestione Azure](https://manage.windowsazure.com) e accedere usando l'account Microsoft associato alla sottoscrizione.
11. Selezionare **siti Web** e fare clic sull'app web creata nell'esercizio 2.
12. Passare il **distribuzioni** pagina. Si noti che tutti i commit eseguiti sono elencati nella cronologia di distribuzione.

    ![Elenco delle distribuzioni esistenti](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Elenco delle distribuzioni esistenti*
13. Selezionare il commit precedente e fare clic su **ridistribuire** sulla barra dei comandi.

    ![Ridistribuire il commit precedente](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ridistribuire il commit precedente*
14. Quando viene richiesto di confermare, fare clic su **Sì**.

    ![Per confermare la ridistribuzione](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Al termine della distribuzione, tornare a questa istanza del browser con l'app web e premere **CTRL + F5**.
16. Fare clic su una delle opzioni. L'animazione flip richiederà a questo punto sul posto e il risultato (*corretta/*) verranno visualizzati.
17. (Facoltativo) Passare al **Git Bash** console ed eseguire i comandi seguenti per ripristinare il commit precedente.

    > [!NOTE]
    > Questi comandi creano un nuovo commit che annulla tutte le modifiche nel repository Git che sono state apportate nel commit non valido. Azure verrà quindi ridistribuire l'applicazione usando il nuovo commit.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Esercizio 4: Scalabilità tramite archiviazione di Azure

**I BLOB** costituiscono il modo più semplice per archiviare grandi quantità di testo non strutturato o dati binari, ad esempio video, audio e immagini. Sposta il contenuto dell'applicazione in un archivio statico, consente di ridimensionare l'applicazione, è necessario gestire le immagini o documenti direttamente al browser.

In questo esercizio, si passerà il contenuto statico dell'applicazione in un contenitore Blob. Quindi si configurerà l'applicazione per aggiungere un **regola di riscrittura URL ASP.NET** nel **Web. config** per reindirizzare i contenuti del contenitore Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Attività 1: creazione di un Account di archiviazione di Azure

In questa attività si apprenderà come creare un nuovo account di archiviazione nel portale di gestione.

1. Passare al [portale di gestione Azure](https://manage.windowsazure.com) e accedere usando l'account Microsoft associato alla sottoscrizione.
2. Selezionare **New | Servizi dati | Archiviazione | Creazione rapida** per iniziare a creare un nuovo account di archiviazione. Immettere un nome univoco per l'account e selezionare una **regione** dall'elenco. Fare clic su **crea Account di archiviazione** per continuare.

    ![Crea un nuovo Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "creando un nuovo Account di archiviazione")

    *Crea un nuovo account di archiviazione*
3. Nel **memorizzazione** sezione, attendere fino a quando assume lo stato del nuovo account di archiviazione *Online* per poter continuare con il passaggio seguente.

    ![Account di archiviazione creato](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Account di archiviazione creato")

    *Account di archiviazione creato*
4. Fare clic sul nome dell'account di archiviazione e quindi scegliere il **Dashboard** collegamento nella parte superiore della pagina. Il **Dashboard** pagina offre informazioni sullo stato dell'account e gli endpoint del servizio che possono essere utilizzati all'interno delle applicazioni.

    ![La visualizzazione Dashboard dell'Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "visualizzazione Dashboard dell'Account di archiviazione")

    *La visualizzazione Dashboard dell'Account di archiviazione*
5. Scegliere il **Gestisci chiavi di accesso** pulsante nella barra di spostamento.

    ![Pulsante Gestisci chiavi di accesso](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "pulsante Gestisci chiavi di accesso")

    *Pulsante di chiavi di accesso per la gestione*
6. Nel **Gestisci chiavi di accesso** finestra di dialogo, copiare la **nome Account di archiviazione** e **chiave di accesso primaria** perché sarà necessario utilizzarli nell'esercizio seguente. Quindi, chiudere la finestra di dialogo.

    ![Finestra di dialogo Gestisci chiave di accesso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "nella finestra di dialogo Gestisci chiave di accesso")

    *Gestire la finestra di dialogo chiave di accesso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Attività 2: caricare un Asset in archivio Blob di Azure

In questa attività, si utilizzerà la finestra di Esplora Server da Visual Studio per connettersi all'account di archiviazione. Si verrà quindi creare un contenitore blob e caricare un file con il logo fanatico Quiz nel contenitore.

1. Passare all'istanza di Visual Studio con il **GeekQuiz** soluzione nell'esercizio precedente.
2. Dalla barra dei menu, selezionare **View** e quindi fare clic su **Esplora Server**.
3. Nelle **Esplora Server**, fare doppio clic il **Azure** nodo e selezionare **connettersi ad Azure...** . Accedere usando l'account Microsoft associato alla sottoscrizione.

    ![Connettersi a Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Connettersi ad Azure*
4. Espandere la **Azure** nodo, fare doppio clic su **archiviazione** e selezionare **associa archiviazione esterna...** .
5. Nel **Aggiungi nuovo Account di archiviazione** finestra di dialogo immettere il **nome Account** e **chiave dell'Account** ottenuta nell'attività precedente e scegliere **OK**.

    ![Aggiungi finestra di dialogo Nuovo Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Aggiungi finestra di dialogo Nuovo Account di archiviazione*
6. L'account di archiviazione deve essere visualizzato sotto il **archiviazione** nodo. Espandere l'account di archiviazione, fare doppio clic su **BLOB** e selezionare **crea contenitore Blob...** .

    ![Crea contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "crea contenitore Blob")

    *Crea contenitore Blob*
7. Nel **crea contenitore Blob** finestra di dialogo casella, immettere un nome per il contenitore blob e fare clic su **OK**.

    ![Finestra di dialogo Crea contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "nella finestra di dialogo Crea contenitore Blob")

    *Dialogo Crea contenitore Blob*
8. Il nuovo contenitore blob deve essere aggiunto per il **BLOB** nodo. Modificare le autorizzazioni di accesso del contenitore per rendere pubblico il contenitore. A tale scopo, fare doppio clic il **immagini** contenitore e selezionare **proprietà**.

    ![le proprietà del contenitore di immagini](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "contenitore proprietà di immagini")

    *Proprietà di immagini contenitore*
9. Nel **delle proprietà** impostare nella finestra di **accesso in lettura pubblico** per **contenitore**.

    ![Modifica delle proprietà di accesso in lettura pubblico](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "la modifica di proprietà di accesso in lettura pubblico")

    *Modifica della proprietà di accesso in lettura pubblico*
10. Quando viene richiesto se si è certi che si desidera modificare la proprietà di accesso pubblico, fare clic su **Sì**.

    ![Avviso di Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "avviso di Microsoft Visual Studio")

    *Avviso di Microsoft Visual Studio*
11. Nelle **Esplora Server**, fare doppio clic nella **immagini** contenitore blob e selezionare **Visualizza contenitore Blob**.

    ![Visualizza contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Visualizza contenitore Blob")

    *Visualizza contenitore Blob*
12. Il contenitore di immagini per l'apertura di una nuova finestra e deve essere visualizzata una legenda senza voci. Scegliere il **caricare** icona per caricare un file nel contenitore blob.

    ![Il contenitore immagini senza voci](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "contenitore immagini senza voci")

    *Contenitore immagini senza voci*
13. Nel **carica Blob** finestra di dialogo passare alle **asset** cartella del lab. Selezionare il **logo big.png** del file e fare clic su **Open**.
14. Attendere che il file viene caricato. Al termine dell'aggiornamento, il file dovrebbe essere elencato nel contenitore di immagini. La voce del file e scegliere **Copia URL**.

    ![Copiare l'URL del blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "copiare URL del file blob")

    *Copiare l'URL del blob*
15. Aprire Internet Explorer e incollare l'URL. L'immagine seguente deve essere visualizzata nel browser.

    ![immagine logo big.png dall'archivio Blob di Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "immagine logo big.png dall'archiviazione")

    *immagine del logo big.png da archiviazione Blob di Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Attività 3: l'aggiornamento della soluzione per l'utilizzo di contenuto statico da un archivio Blob di Azure

In questa attività si configurerà il **GeekQuiz** soluzioni per utilizzare l'immagine caricato in archiviazione Blob di Azure (anziché l'immagine che si trova nell'app web) mediante l'aggiunta di una regola di riscrittura URL di ASP.NET nel **Web. config**file.

1. In Visual Studio, aprire il **Web. config** file all'interno di **GeekQuiz** del progetto e individuare il **&lt;System. webServer&gt;** elemento.
2. Aggiungere il codice seguente per aggiungere un URL rewrite regola, l'aggiornamento del segnaposto con il nome di account di archiviazione.

    (Code - Snippet *WebSitesInProduction - Ex4 - UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > La riscrittura degli URL è il processo di intercettazione di una richiesta Web in ingresso e reindirizza la richiesta a un'altra risorsa. Regole di riscrittura URL indica al motore di riscrittura quando deve essere reindirizzata una richiesta e in cui essi verrà reindirizzate. Una regola di riscrittura è costituita da due stringhe: il criterio da cercare nell'URL richiesto (in genere, usando le espressioni regolari), e la stringa con cui sostituire il modello, se trovata. Per altre informazioni, vedere [riscrittura URL in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Premere **CTRL + S** per salvare le modifiche.
4. Aprire una nuova **Git Bash** console per distribuire l'applicazione aggiornata in servizio App di Azure.
5. Eseguire i comandi seguenti per il push delle modifiche in Azure. Aggiornamento di *[YOUR-APPLICATION-PATH]* segnaposto con il percorso per il **GeekQuiz** soluzione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Distribuzione aggiornamento ad Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Distribuzione aggiornamento ad Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Attività 4: verifica

In questa attività si userà **Internet Explorer** per selezionare la **fanatico Quiz** dell'applicazione e verificare che l'URL rewrite regola per il funzionamento di immagini e si verrà reindirizzati per l'immagine ospitata nel **Blob di Azure Archiviazione**.

1. Aprire Internet Explorer e passare all'App web (ad esempio `http://<your-web-site>.azurewebsites.net`). Accedere usando le credenziali create in precedenza.

    ![Che mostra l'app web Quiz fanatico con l'immagine](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "che mostra l'app web Quiz fanatico con l'immagine")

    *Che mostra l'app web Quiz fanatico con l'immagine*
2. Premere **F12** per avviare gli strumenti di sviluppo, selezionare la **rete** scheda e avviare la registrazione.

    ![Avviare la registrazione di rete](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "avviare la registrazione di rete")

    *Avviare la registrazione di rete*
3. Premere **CTRL + F5** per aggiornare la pagina web.
4. Dopo la pagina ha terminato il caricamento, viene visualizzata una richiesta HTTP per il **/img/logo-big.png** URL con un HTTP **301** risultato (reindirizzamento) e un'altra richiesta di `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL con un HTTP **200** risultato.

    ![Verifica per determinare se il reindirizzamento dell'URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "che mostra il reindirizzamento in strumenti di sviluppo")

    *Verifica per determinare se il reindirizzamento dell'URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Esercizio 5: Usa la scalabilità automatica per le app Web

> [!NOTE]
> Questo esercizio è facoltativo, poiché richiede il supporto per il carico Web &amp; cui è disponibile solo per il test delle prestazioni **Visual Studio 2013 Ultimate Edition**. Per altre informazioni su funzionalità specifiche di Visual Studio 2013, confrontare le versioni [qui](https://www.microsoft.com/visualstudio/eng/products/compare).


**App Web di Azure App Service** fornisce la funzionalità scalabilità automatica per le app web in esecuzione in **modalità Standard**. Scalabilità automatica consente a Azure di ridimensionare automaticamente il numero di istanze dell'app web a seconda del carico. Quando è abilitata la scalabilità automatica, Azure controlla la CPU dell'app web una volta ogni cinque minuti e aggiunge istanze in base alle esigenze in quel punto nel tempo. Se l'utilizzo della CPU è basso, Azure rimuoverà le istanze di una volta ogni due ore per assicurarsi che le prestazioni dell'app web non sono danneggiata.

In questo esercizio si passerà attraverso i passaggi necessari per configurare il **scalabilità automatica** funzionalità per il **fanatico Quiz** app web. Questa funzionalità verrà verificato eseguendo un test di carico di Visual Studio per generare un carico sufficiente CPU dell'applicazione per attivare un aggiornamento dell'istanza.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Attività 1: configurazione di scalabilità automatica in base alla metrica della CPU

In questa attività si userà il portale di gestione di Azure per abilitare la funzionalità di scalabilità automatica per l'app web creata nell'esercizio 2.

1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/), selezionare **siti Web** e fare clic sull'app web creata nell'esercizio 2.
2. Passare il **scalabilità** pagina. Sotto il **capacità** sezione, selezionare **CPU** per il **ridimensiona in base alla metrica** configurazione.

    > [!NOTE]
    > Quando si ridimensiona da CPU, Azure consente di regolare dinamicamente il numero di istanze che l'app Usa se viene modificato l'utilizzo della CPU.

    ![Se si sceglie di scala in base alla CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "selezionando la metrica della CPU per la scalabilità automatica")

    *Se si sceglie di scala in base alla CPU*
3. Modifica il **CPU destinazione** configurazione **20**-**40** percento.

    > [!NOTE]
    > Questo intervallo rappresenta l'utilizzo medio della CPU per l'app web. Azure aggiungerà o rimuoverà istanze per mantenere l'app web in questo intervallo. Viene specificato il numero minimo e massimo di istanze utilizzate per la scalabilità nel **numero di istanze** configurazione. Azure si rivolgerà mai di sopra o oltre il limite stabilito.
    > 
    > Il valore predefinito **CPU destinazione** i valori vengono modificati solo ai fini di questa esercitazione. Configurando l'intervallo di CPU con valori di piccole dimensioni, si stanno aumentando le probabilità per attivare la scalabilità automatica, quando l'applicazione presenta un carico moderato.

    ![Modifica della destinazione della CPU deve essere compresa tra 20 e il 40 percento](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "modifica della CPU di destinazione per essere compreso tra 20 e il 40%")

    *Modifica la CPU di destinazione per essere compreso tra 20 e il 40%*
4. Fare clic su **salvare** nella barra dei comandi per salvare le modifiche.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Attività 2: test di carico con Visual Studio

A questo punto **scalabilità automatica** è stato configurato, si creerà un **progetto di Test di carico e prestazioni Web** in Visual Studio per generare un carico della CPU dell'app web.

1. Aprire **Visual Studio Ultimate 2013** e selezionare **File | New | Progetto...**  per avviare una nuova soluzione.

    ![Crea un nuovo progetto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** finestra di dialogo **progetto di Test di carico e prestazioni Web** sotto il **Visual c# | Test** scheda. Assicurarsi che **.NET Framework 4.5** è selezionata, denominare il progetto *WebAndLoadTestProject*, scegliere un **posizione** e fare clic su **OK**.

    ![Crea un nuovo progetto Web e Test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "creando un nuovo progetto di Test di carico e Web")

    *Crea un nuovo progetto di Test di carico e Web*
3. Nel **WebTest1.webtest** pulsante destro del mouse il **WebTest1** nodo e fare clic su **Aggiungi richiesta**.

    ![Aggiunta di una richiesta a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "aggiunta di una richiesta a WebTest1")

    *Aggiunta di una richiesta a WebTest1*
4. Nel **delle proprietà** finestra del nuovo nodo richiesta, aggiornare il **Url** proprietà in modo che punti all'URL dell'app web (ad esempio *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Impossibile modificare la proprietà Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "modificando la proprietà Url")

    *Impossibile modificare la proprietà Url*
5. Nel **WebTest1.webtest** finestra, fare doppio clic su **WebTest1** e fare clic su **Aggiungi ciclo...** .

    ![Aggiunta di un ciclo a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "aggiunta di un ciclo a WebTest1")

    *Aggiunta di un ciclo a WebTest1*
6. Nel **Aggiungi regola condizionale ed elementi al ciclo** finestra di dialogo, seleziona la **ciclo For** della regola e modificare le proprietà seguenti.

   1. **Valore di terminazione:** 1000
   2. **Nome di parametro di contesto:** iteratore
   3. **Valore di incremento:** 1

      ![Selezionando la regola ciclo For e aggiornare le proprietà](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "selezionando la regola ciclo For e aggiornare le proprietà")

      *Selezionando la regola ciclo For e aggiornare le proprietà*
7. Sotto il **elementi nel ciclo** , selezionare la richiesta creata in precedenza per l'elemento e il cognome del ciclo. Fare clic su **OK** per continuare.

    ![Selezionando il primo e l'ultimo elemento per il ciclo](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "selezionando il primo e l'ultimo elemento per il ciclo")

    *Selezionando il primo e l'ultimo elemento per il ciclo*
8. In **Esplora soluzioni**, fare doppio clic il **WebAndLoadTestProject** del progetto, espandere il **Add** menu e selezionare **Test di carico...** .

    ![Aggiunta di un Test di carico al progetto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "aggiunta di un Test di carico al progetto WebAndLoadTestProject")

    *Aggiunta di un Test di carico al progetto WebAndLoadTestProject*
9. Nel **Creazione guidata Test di carico** finestra di dialogo, fare clic su **successivo**.

    ![Creazione guidata Test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Creazione guidata Test di carico")

    *Creazione guidata Test di carico*
10. Nel **Scenario** pagina, selezionare **non usare tempi interazione utente** e fare clic su **Next**.

    ![Se si seleziona di non utilizzare tempi interazione utente](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting a non usare i tempi interazione utente")

    *Se si sceglie di non per usare i tempi interazione utente*
11. Nel **modello di carico** pagina, assicurarsi che le **carico costante** opzione è selezionata. Modifica il **numero di utenti** se si imposta su **250** utenti e fare clic su **Avanti**.

    ![Modifica il numero di utenti a 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "modificando il numero di utenti a 250")

    *Modifica il numero di utenti a 250*
12. Nel **modello di combinazione di Test** pagina, selezionare **basato sull'ordine sequenziale dei test** e fare clic su **Next**.

    ![Selezionare il modello di combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "selezionando il modello di combinazione di test")

    *Selezionare il modello di combinazione di test*
13. Nel **modello di combinazione di Test** pagina, fare clic su **Aggiungi...**  per aggiungere test alla combinazione.

    ![Aggiunta di un test alla combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "aggiunta di un test alla combinazione di test")

    *Aggiunta di un test alla combinazione di test*
14. Nel **Aggiungi test** della finestra di dialogo fare doppio clic su **WebTest1** per aggiungere il test per il **test selezionati** elenco. Fare clic su **OK** per continuare.

    ![Aggiunta del test WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "aggiunta test WebTest1")

    *Aggiunta del test WebTest1*
15. Nel **combinazione di Test** pagina, fare clic su **successivo**.

    ![Pagina Completamento della combinazione di Test](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "pagina Completamento della combinazione di Test")

    *Pagina Completamento della combinazione di Test*
16. Nel **combinazione di reti** pagina, fare clic su **successivo**.

    ![Facendo clic su Avanti nella pagina di combinazione di reti](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "facendo clic su Avanti nella pagina di combinazione di reti")

    *Facendo clic su Avanti nella pagina di combinazione di reti*
17. Nel **combinazioni di Browser** pagina, selezionare **Internet Explorer 10.0** come tipo di browser e fare clic su **Next**.

    ![Selezione del tipo di browser](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "selezionando il tipo di browser")

    *Selezione del tipo di browser*
18. Nel **insiemi di contatori** pagina, fare clic su **successivo**.

    ![Facendo clic su Avanti nella pagina insiemi di contatori](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "facendo clic su Avanti nella pagina insiemi di contatori")

    *Facendo clic su Avanti nella pagina insiemi di contatori*
19. Nel **impostazioni di esecuzione** pagina, impostare il **durata test di carico** al **5 minuti** e fare clic su **fine**.

    ![Impostare la durata del test di carico a 5 minuti](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "impostando la durata del test di carico a 5 minuti")

    *Impostare la durata del test di carico a 5 minuti*
20. Nelle **Esplora soluzioni**, fare doppio clic il **Local.settings** file per esaminare le impostazioni di test. Per impostazione predefinita, Visual Studio Usa il computer locale per eseguire i test.

    > [!NOTE]
    > In alternativa, è possibile configurare il progetto di test per eseguire i test di carico nel cloud usando **Visual Studio Online (VSO)**. Visual Studio Online offre un carico basati sul cloud servizio che simula un carico più realistico di test, evitare i vincoli dell'ambiente locale, ad esempio la capacità della CPU, memoria disponibile e la larghezza di banda di rete. Per altre informazioni sull'utilizzo di Visual Studio online per eseguire i test di carico, vedere [questo articolo](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Impostazioni di test](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Attività 3: verifica della scalabilità automatica

Verrà ora eseguire il test di carico che è stato creato nell'attività precedente e vedere come l'app web di intenso carico.

1. Nelle **Esplora soluzioni**, fare doppio clic su **LoadTest1.loadtest** per aprire il test di carico.

    ![Apertura LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "apertura LoadTest1.loadtest")

    *Apertura LoadTest1.loadtest*
2. Nel **LoadTest1.loadtest** finestra, fare clic sul primo pulsante della casella degli strumenti per eseguire il test di carico.

    ![Esecuzione del test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "eseguendo il test di carico")

    *Esecuzione del test di carico*
3. Attendere il completamento del test di carico.

    > [!NOTE]
    > Il test di carico simula più utenti che inviano richieste all'app web contemporaneamente. Quando viene eseguito il test, è possibile monitorare i contatori disponibili per rilevare eventuali errori, avvisi o altre informazioni correlate all'esecuzione dei test di carico.

    ![Esecuzione di test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "in attesa fino al completamento del test di carico")

    *Test di carico in esecuzione*
4. Dopo il completamento del test, tornare al portale di gestione e passare al **scalabilità** pagina dell'app web. Sotto il **capacità** sezione, si dovrebbe vedere nel grafico che è stata distribuita automaticamente una nuova istanza.

    ![Nuova istanza distribuito automaticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nuova istanza distribuito automaticamente*

    > [!NOTE]
    > Potrebbero occorrere alcuni minuti per le modifiche vengano visualizzate nel grafico (premere **CTRL + F5** periodicamente per aggiornare la pagina). Se non è possibile visualizzare tutte le modifiche, è possibile procedere come segue:
    > 
    > - Aumentare la durata del test di carico (ad esempio, al **10 minuti**)
    > - I valori minimo e massimi di ridurre la **CPU destinazione** intervallo nella configurazione di scalabilità automatica dell'app web
    > - Eseguire il test di carico nel cloud con **Visual Studio Online**. Altre informazioni [qui](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questo laboratorio pratico, è stato descritto come configurare e distribuire l'applicazione in App web di produzione in Azure. È stato avviato rilevando e aggiornare i database con **migrazioni di Entity Framework Code First**, quindi continua distribuendo nuove versioni del sito usando **Git** e l'esecuzione di ripristini al versione stabile più recente del sito. Inoltre, si è appreso come ridimensionare l'app usando l'archiviazione per spostare il contenuto statico in un contenitore Blob.
