---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Un laboratorio pratico: gestibile siti Web di Azure: gestione del cambiamento e scala | Documenti Microsoft'
author: rick-anderson
description: "Microsoft Azure rende più semplice compilare e distribuire siti Web nell'ambiente di produzione. Ma non si è connessi quando l'applicazione è in tempo reale, principianti! Si..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 1d6d9265d93fbd32e2d9c22e2ac3db9b5ffd9776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Un laboratorio pratico: gestibile siti Web di Azure: gestione del cambiamento e scala
====================
da [categorie Web Team](https://twitter.com/webcamps)

[Download Web categorie Kit di formazione](http://aka.ms/webcamps-training-kit)

> Microsoft Azure rende più semplice compilare e distribuire siti Web nell'ambiente di produzione. Ma non si è connessi quando l'applicazione è in tempo reale, principianti! È necessario gestire modifica i requisiti, gli aggiornamenti del database, scala e altro ancora. Fortunatamente, Azure App Service è si è interessati, con una notevole quantità di funzionalità che consentono di mantenere i siti in esecuzione senza problemi.
> 
> Azure offre sicuro e flessibile sviluppo, distribuzione e scalabilità e le opzioni per qualsiasi applicazione web di dimensioni. Usare gli strumenti esistenti per creare e distribuire applicazioni senza dover gestire l'infrastruttura.
> 
> Eseguire il provisioning di un'applicazione web di produzione manualmente in minuti facilmente la distribuzione di contenuti creati utilizzando lo strumento di sviluppo preferito. È possibile distribuire un sito esistente direttamente dal controllo del codice sorgente con il supporto per **Git**, **GitHub**, **Bitbucket**, **TFS**e anche ** DropBox**. Distribuire direttamente l'IDE preferito o dagli script utilizzando **PowerShell** in Windows o **CLI** strumenti in esecuzione in qualsiasi sistema operativo. Una volta distribuito, aggiornare i siti costantemente con il supporto per la distribuzione continua.
> 
> Azure offre soluzioni di ripristino, backup e archiviazione cloud scalabile e durevole per i dati, grande o piccola. Quando si distribuiscono applicazioni per un ambiente di produzione, i servizi di archiviazione, ad esempio tabelle, BLOB e database SQL, che consentono di ridimensionare l'applicazione nel cloud.
> 
> Con i database SQL, è importante mantenere aggiornati il database produttivo durante la distribuzione di nuove versioni dell'applicazione. Grazie a **migrazioni di Entity Framework Code First**, lo sviluppo e distribuzione del modello di dati è stata semplificata per aggiornare gli ambienti in minuti. Questa esercitazione verrà illustrato i diversi argomenti che possono essere riscontrati quando si distribuisce l'app web in ambienti di produzione in Microsoft Azure.
> 
> Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).
> 
> Per ulteriori approfondite di questo argomento, vedere il [creazione di App per Cloud del mondo reale con Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Panoramica

<a id="Objectives"></a>
### <a name="objectives"></a>Obiettivi

In questa esercitazione pratica, si apprenderà come:

- Abilitare le migrazioni di Entity Framework con un modello esistente
- Aggiornare il modello a oggetti e il database di conseguenza utilizzando le migrazioni di Entity Framework
- Distribuire in Azure App Service tramite Git
- Eseguire il rollback a una distribuzione precedente tramite il portale di gestione di Azure
- Utilizzare l'archiviazione di Azure per ridimensionare un'app web
- Configurare la scalabilità automatica per un'app web tramite il portale di gestione di Azure
- Creare e configurare un progetto di test di carico in Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisiti

Di seguito è necessario per completare questa esercitazione pratica:

- [Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/) o versione successiva
- [Azure SDK per .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Il controllo delle versioni GIT](http://git-scm.com/download)
- Una sottoscrizione di Microsoft Azure 

    - Iscriversi a un [versione di valutazione gratuita](http://aka.ms/watk-freetrial)
    - Se si è un Visual Studio Professional, Test Professional, Premium o Ultimate con MSDN o MSDN Platforms sottoscrittore, attivare il [benefici MSDN](http://aka.ms/watk-msdn) ora per avviare lo sviluppo e test in Azure
    - [BizSpark](http://aka.ms/watk-bizspark) ricevono automaticamente Azure vantaggio tramite i relativi Visual di Studio Ultimate, con abbonamenti MSDN
    - I membri del [Microsoft Partner Network](http://aka.ms/watk-mpn) programma Cloud Essentials ricevono crediti Azure mensili senza costi

<a id="Setup"></a>
### <a name="setup"></a>Configurazione

Per eseguire gli esercizi di questa esercitazione pratica, è necessario configurare prima di tutto l'ambiente.

1. Aprire Esplora risorse e passare nel lab **origine** cartella.
2. Fare clic su **Setup.cmd** e selezionare **Esegui come amministratore** per avviare il processo di installazione che la configurazione dell'ambiente e installare i frammenti di codice di Visual Studio per questo ambiente lab.
3. Se viene visualizzata la finestra di dialogo controllo dell'Account utente, confermare l'azione per continuare.

> [!NOTE]
> Assicurarsi di avere selezionato tutte le dipendenze per questa esercitazione prima di eseguire il programma di installazione.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Utilizzo dei frammenti di codice

In tutto il documento lab, verrà invitati a inserire i blocchi di codice. Per praticità, la maggior parte di questo codice viene fornito come Visual Studio frammenti di codice che è possibile accedere da all'interno di Visual Studio 2013 per evitare di dover aggiungerla manualmente.

> [!NOTE]
> Ogni esercizio è accompagnata da una soluzione inizia all'interno di **iniziare** cartella dell'esercizio che consente di seguire ogni esercizio indipendentemente dalle altre. Tenere presente che i frammenti di codice aggiunti durante un esercizio non sono presenti queste soluzioni di avvio e potrebbero non funzionare fino a completare l'esercizio. All'interno del codice sorgente per un esercizio, si avrà inoltre un **fine** cartella che contiene una soluzione di Visual Studio con il codice generato da completare i passaggi nell'esercizio corrispondente. Se è necessario ulteriore assistenza durante l'utilizzo tramite questa esercitazione pratica, è possibile usare queste soluzioni come guida.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Esercizi

Questa esercitazione include gli esercizi seguenti:

1. [Utilizzando le migrazioni di Entity Framework](#Exercise1)
2. [La distribuzione di un'App Web in gestione temporanea](#Exercise2)
3. [Esecuzione del Rollback di distribuzione nell'ambiente di produzione](#Exercise3)
4. [Scaling mediante l'archiviazione di Azure](#Exercise4)
5. [Utilizza la scalabilità automatica per le app Web](#Exercise5) (facoltativo per l'edizione di Visual Studio 2013 Ultimate)

Tempo stimato per completare questa esercitazione: **75 minuti**

> [!NOTE]
> Al primo avvio di Visual Studio, è necessario selezionare una delle raccolte di impostazioni predefinite. Ogni raccolta predefinita è progettato in modo che corrisponda a un particolare stile di sviluppo e determina il layout di finestra, il comportamento dell'editor, frammenti di codice IntelliSense e finestre di dialogo. Le procedure descritte in questa esercitazione vengono descritte le azioni necessarie per eseguire una determinata attività in Visual Studio quando si utilizza il **impostazioni generali per lo sviluppo** insieme. Se si sceglie una raccolta di impostazioni diverse per l'ambiente di sviluppo, possono essere presenti differenze nei passaggi che è necessario prendere in considerazione.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Esercizio 1: Utilizzare le migrazioni di Entity Framework

Quando si sviluppa un'applicazione, il modello di dati potrebbe cambiare nel tempo. Queste modifiche possano influire sul modello esistente nel database (se si sta creando una nuova versione) ed è importante mantenere aggiornati per evitare errori del database.

Per semplificare il rilevamento delle modifiche nel modello, **migrazioni di Entity Framework Code First** automaticamente rilevate le modifiche, il confronto del modello con lo schema del database e genera il codice specifico per aggiornare il database, creazione di nuovi *versioni* del database.

Questo esercizio illustra come abilitare **migrazioni** per l'applicazione e come si può facilmente rilevare e generare le modifiche per aggiornare i database.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Attività 1: abilitare le migrazioni

In questa attività si passerà tramite la procedura di abilitazione **migrazioni di Entity Framework Code First** per il **appassionato Quiz** database, la modifica del modello e comprendere come tali modifiche si rifletteranno nel database.

1. Aprire Visual Studio e aprire il **GeekQuiz.sln** file della soluzione da **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Compilare la soluzione per scaricare e installare il **NuGet** dipendenze dei pacchetti. A tale scopo, fare doppio clic la soluzione e fare clic su **Compila soluzione** o premere **Ctrl + MAIUSC + B**.
3. Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi fare clic su **Console di gestione pacchetti**.
4. Nel **Package Manager Console**, immettere il comando seguente e premere **invio**. Verrà creata una migrazione iniziale in base al modello esistente.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Abilitare le migrazioni](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "abilitazione migrazioni")

    *L'abilitazione delle migrazioni*

    > [!NOTE]
    > Questo comando aggiunge un **migrazioni** cartella progetto appassionato Quiz che contiene un file denominato **Configuration.cs**. Il **configurazione** classe consente di configurare il comportamento delle migrazioni per il contesto.
5. Con le migrazioni abilitate, è necessario aggiornare il **configurazione** classe per popolare il database con i dati iniziali che **appassionato Quiz** richiede. In **migrazioni**, sostituire il **Configuration.cs** file con quello nella **Source\Assets** cartella di questa esercitazione.

    > [!NOTE]
    > Poiché **migrazioni** chiamerà il **valore di inizializzazione** (metodo) con ogni aggiornamento del database, è necessario assicurarsi che i record non vengono duplicati nel database. Il **AddOrUpdate** metodo consente di evitare che i dati duplicati.
6. Per aggiungere una migrazione iniziale, immettere il comando seguente e quindi premere **invio**.

    > [!NOTE]
    > Assicurarsi che non è presente alcun database denominato &quot;GeekQuizProd&quot; nell'istanza di LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Aggiunta di migrazione dello schema di base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "aggiunta allo schema di base migrazione")

    *Aggiunta di migrazione dello schema di base*

    > [!NOTE]
    > **Migrazione aggiungere** verrà lo scaffolding della migrazione successiva in base alle modifiche apportate al modello poiché è stata creata l'ultima migrazione. In questo caso, perché è la migrazione prima del progetto, aggiungerà gli script per creare tutte le tabelle definite nel **TriviaContext** classe.
7. Eseguire la migrazione per aggiornare il database eseguendo il comando seguente. Per questo comando si specificherà il **Verbose** flag per visualizzare le istruzioni SQL da applicare al database di destinazione.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Creazione database iniziale](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "database iniziale di creazione")

    *Creazione di un database iniziale*

    > [!NOTE]
    > **Update-Database** applicare le migrazioni in sospeso al database. In questo caso, creerà il database utilizzando la stringa di connessione definita nel file Web. config.
8. Passare a **vista** menu e aprire **Esplora oggetti di SQL Server**.

    ![Apri in Esplora oggetti SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Apri in Esplora oggetti SQL Server")

    *Apri in Esplora oggetti SQL Server*
9. Nel **Esplora oggetti di SQL Server** finestra, connettersi all'istanza di LocalDB facendo clic con il **SQL Server** nodo e selezionando **aggiungere SQL Server... ** opzione.

    ![Aggiunta di un'istanza del Server SQL](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "aggiunta di un'istanza SQL Server")

    *Aggiunta di un'istanza di SQL Server in Esplora oggetti di SQL Server*
10. Impostare il **nome server** a *(localdb) \v11.0* e lasciare **l'autenticazione di Windows** come modalità di autenticazione. Fare clic su **Connetti** per continuare.

    ![Connessione a LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "connessione a LocalDB")

    *Connessione a LocalDB*
11. Aprire il **GeekQuizProd** database ed espandere il **tabelle** nodo. Come si può notare, la **Update-Database** comando generato tutte le tabelle definite nel **TriviaContext** classe. Individuare il **dbo. TriviaQuestions** tabella e aprire il nodo colonne. Nell'attività successiva, verrà aggiunta una nuova colonna alla tabella e aggiornare il database utilizzando **migrazioni**.

    ![Gli elementi semplici domande colonne](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "gli elementi semplici domande colonne")

    *Gli elementi semplici domande colonne*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Attività 2: aggiornamento dello Schema di Database utilizzando migrazioni

In questa attività, si utilizzerà **migrazioni di Entity Framework Code First** per rilevare una modifica nel modello e generare il codice necessario per aggiornare il database. Si aggiornerà il **TriviaQuestions** entità aggiungendovi una nuova proprietà. Eseguire quindi i comandi per creare una nuova migrazione per includere la nuova colonna nella tabella.

1. In **Esplora**, fare doppio clic sul **TriviaQuestion.cs** file si trova all'interno di **modelli** cartella.
2. Aggiungere una nuova proprietà denominata **Hint**, come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. Nel **Package Manager Console**, immettere il comando seguente e premere **invio**. Verrà creata una nuova migrazione riflettere la modifica nel modello in esame.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Migrazione aggiungere](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "aggiungere-migrazione")

    *Aggiungere la migrazione*

    > [!NOTE]
    > Un file di migrazione è costituito da due metodi, **backup** e **verso il basso**.
    > 
    > - Il **backup** metodo verrà utilizzato per specificare quali modifiche la versione corrente dell'esigenza di applicazione da applicare al database.
    > - Il **verso il basso** viene utilizzato per annullare le modifiche sono state aggiunte per la **backup** metodo.
    > 
    > Quando la migrazione di Database aggiorna il database, verrà eseguito tutte le migrazioni in ordine di timestamp e solo quelli che non sono stati utilizzati dopo l'ultimo aggiornamento (il \_MigrationHistory tabella tiene traccia di quali migrazioni sono state applicate). Il **backup** tutte le migrazioni metodo verrà chiamato e apporta le modifiche al database specificato. Se si decide di tornare indietro per una migrazione precedente, il **verso il basso** metodo verrà chiamato per ripristinare le modifiche in ordine inverso.
4. Nel **Package Manager Console**, immettere il comando seguente e premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. L'output del **Update-Database** comando generato un **Alter Table** istruzione SQL per aggiungere una nuova colonna per il **TriviaQuestions** tabella, come illustrato nell'immagine seguente.

    ![Aggiungere l'istruzione SQL colonna generata](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "aggiungere l'istruzione SQL colonna generata")

    *Aggiungere l'istruzione SQL colonna generata*
6. In **Esplora oggetti di SQL Server**, aggiornare il **dbo. TriviaQuestions** tabella e verificare che il nuovo **Hint** colonna viene visualizzata.

    ![Con la nuova colonna Hint](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "che mostra la nuova colonna Hint")

    *Con la nuova colonna Hint*
7. Nel **TriviaQuestion.cs** editor aggiungere una **StringLength** vincolo per il *Hint* proprietà, come illustrato nel frammento di codice seguente.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. Nel **Package Manager Console**, immettere il comando seguente e premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. Nel **Package Manager Console**, immettere il comando seguente e premere **invio**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. L'output del **Update-Database** comando generato un **Alter Table** istruzione SQL per aggiornare il *hint* colonna di tipo di **TriviaQuestions** tabella, come illustrato nell'immagine seguente.

    ![Istruzione ALTER column SQL generato](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "istruzione Alter column SQL generato")

    *Istruzione ALTER column SQL generato*
11. In **Esplora oggetti di SQL Server**, aggiornare il **dbo. TriviaQuestions** tabella e verificare che il **Hint** è di tipo di colonna **nvarchar(150)**.

    ![Con il nuovo vincolo](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "che mostra il nuovo vincolo")

    *Con il nuovo vincolo*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Esercizio 2: Distribuzione di un'App Web di gestione temporanea

**Le App in Azure App Service Web** consente di eseguire la pubblicazione di gestione temporanea. Pubblicazione di gestione temporanea crea uno slot del sito di gestione temporanea per ciascun sito di produzione predefinito e consente di scambiare questi slot senza tempi di inattività. È molto utile per convalidare le modifiche prima di rilasciare al pubblico, in modo incrementale integrare il contenuto del sito e il rollback se le modifiche non funzionino come previsto.

In questo esercizio, si distribuirà il **appassionato Quiz** applicazione nell'ambiente di gestione temporanea dell'applicazione web tramite controllo del codice sorgente Git. A tale scopo, verrà creare l'app web e i componenti necessari nel portale di gestione di eseguire il provisioning, configurare un **Git** repository e push dell'applicazione nel codice sorgente dal computer locale per lo slot di gestione temporanea. Anche se si aggiornerà il database di produzione con la **migrazioni Code First** creato nell'esercizio precedente. Verrà quindi eseguita l'applicazione nell'ambiente di test per verificare l'operazione. Dopo avere effettuato tale funzionamento in base alle proprie aspettative, alzare di livello l'applicazione nell'ambiente di produzione.

> [!NOTE]
> Per abilitare la pubblicazione di gestione temporanea, l'app web deve essere **modalità Standard**. Si noti che sarà necessario sostenere costi aggiuntivi se si modifica l'app web in modalità Standard. Per ulteriori informazioni sui prezzi, vedere [prezzi del servizio App](https://azure.microsoft.com/en-us/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Attività 1: creare un'App Web nel servizio App di Azure

In questa attività si creerà un'app web nel **Azure App Service** dal portale di gestione. Si configurerà inoltre un **Database SQL** per rendere persistenti i dati dell'applicazione e configurare un repository Git locale per controllo del codice sorgente.

1. Passare al [portale di gestione di Azure](https://manage.windowsazure.com) e accedere utilizzando l'account Microsoft associato alla sottoscrizione.

    ![Accedi al portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Accedi al portale di gestione di Azure*
2. Fare clic su **New** nella barra dei comandi nella parte inferiore della pagina.

    ![Creazione di una nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "la creazione di una nuova app web")

    *Creazione di una nuova app web*
3. Fare clic su **calcolo**, **sito Web** e quindi **creazione personalizzata**.

    ![Creazione di una nuova app web utilizzando creazione personalizzata](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "la creazione di una nuova app web utilizzando creazione personalizzata")

    *Creazione di una nuova app web utilizzando creazione personalizzata*
4. Nel **nuovo sito Web - creazione personalizzata** finestra di dialogo, fornire un **URL** (ad esempio *appassionato quiz*), selezionare un percorso nel **area** elenco a discesa e scegliere **creare un nuovo database SQL** nel **Database** elenco a discesa. Infine, selezionare il **pubblica dal controllo del codice sorgente** casella di controllo e fare clic su **Avanti**.

    ![Personalizzare la nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalizzare la nuova app web*
5. Specificare le informazioni seguenti per le impostazioni del database:

    - Nel **nome** testo, immettere un nome di database (ad esempio *geekquiz\_db*)
    - Nel Server **elenco a discesa** elenco, selezionare **server di database SQL nuova**. In alternativa, è possibile selezionare un server esistente.
    - Nel **nome utente del Database** e **password Database** caselle, immettere il nome utente amministratore e la password per il server di database SQL. Se si seleziona un server è già stato creato, verrà richiesto per la password.

    ![Specifica le impostazioni del database](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    *Specifica le impostazioni del database*
6. Fare clic su **Avanti** per continuare.
7. Selezionare **repository Git locale** per il controllo del codice sorgente da utilizzare e fare clic su **Avanti**.

    > [!NOTE]
    > Potrebbero essere richieste le credenziali di distribuzione (nome utente e password).

    ![Creazione dell'archivio Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Creazione dell'archivio Git*
8. Attendere finché non viene creata la nuova app web.

    > [!NOTE]
    > Per impostazione predefinita, Azure fornisce domini *azurewebsites.net* , ma fornisce anche la possibilità di impostare i domini personalizzati, tramite il portale di gestione di Azure. Tuttavia, è possibile gestire i domini personalizzati solo se si utilizza determinate modalità di servizio App di Azure.
    > 
    > Servizio App di Azure è disponibile nelle edizioni Free, condiviso, Basic, Standard e Premium. In modalità disponibile e condiviso, tutte le applicazioni web eseguite in un ambiente multi-tenant e dispone di quote per l'utilizzo della CPU, memoria e rete. Il numero massimo di App gratuite può variare con il piano. In modalità Standard, si sceglie di quali App eseguire in macchine virtuali dedicate che corrispondono a standard Azure le risorse di calcolo. È possibile trovare la configurazione della modalità di applicazione web nel **scala** menu dell'app web.
    > 
    > ![Modalità di servizio App di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "modalità di servizio App di Azure")
    > 
    > Se si utilizza **Shared** o **Standard** modalità, sarà in grado di gestire i domini personalizzati per l'app web, passare all'app **configura** menu e scegliendo **Gestisci domini** in *i nomi di dominio*.
    > 
    > ![Gestire i domini](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "gestire domini")
    > 
    > ![Gestire i domini personalizzati](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "gestire domini personalizzati")
9. Una volta creato l'app web, fare clic sul collegamento sotto il **URL** colonna per verificare che la nuova app web è in esecuzione.

    ![Esplorazione per la nuova app web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Esplorazione per la nuova app web*

    ![app Web in esecuzione](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *app Web in esecuzione*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Attività 2: creazione del Database SQL di produzione

In questa attività, si utilizzerà il **migrazioni di Entity Framework Code First** per creare il database di destinazione di **Database SQL di Azure** istanza creata nell'attività precedente.

1. Nel portale di gestione, passare all'app web è stato creato nell'attività precedente e passare alla relativa **Dashboard**.
2. Nel **Dashboard** pagina, fare clic su **consente di visualizzare le stringhe di connessione** collegamento sotto il **riepilogo rapido** sezione.

    ![Visualizzare le stringhe di connessione](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "consente di visualizzare le stringhe di connessione")

    *Visualizza le stringhe di connessione*
3. Copia il **stringa di connessione** valore e chiudere la finestra di dialogo.

    ![Stringa di connessione nel portale di gestione di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "stringa di connessione nel portale di gestione di Azure")

    *Stringa di connessione nel portale di gestione di Azure*
4. Fare clic su **database SQL** per visualizzare l'elenco di database SQL di Azure

    ![Menu di Database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "menu Database SQL")

    *Menu di Database SQL*
5. Individuare il database che è stato creato nell'attività precedente e fare clic sul Server.

    ![Server di Database SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "server di Database SQL")

    *Server di Database SQL*
6. Nel **avvio rapido** pagina del server, fare clic su **configura**.

    ![Configura menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configura menu")

    *Configurare i menu*
7. Nel **gli indirizzi IP consentiti** sezione, fare clic su **aggiungere agli indirizzi IP consentiti** collegamento per consentire l'IP per la connessione al server di Database SQL.

    ![Indirizzi IP consentiti](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "gli indirizzi IP consentiti")

    *Indirizzi IP consentiti*
8. Fare clic su **salvare** nella parte inferiore della pagina per completare il passaggio.
9. Tornare a Visual Studio.
10. Nel **Package Manager Console**, eseguire il comando seguente sostituendo *[stringa di connessione nella]* segnaposto con la stringa di connessione è stato copiato da Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aggiorna i database per Database SQL di Azure](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Aggiorna database come destinazione Database SQL di Azure")

    *Aggiornare il database di destinazione Database SQL di Azure*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Attività 3: distribuzione Quiz appassionato di gestione temporanea tramite Git

In questa attività verrà abilitata la pubblicazione di gestione temporanea nell'app web. Quindi, si utilizzerà Git per pubblicare l'applicazione appassionato Quiz direttamente dal computer locale all'ambiente di gestione temporanea dell'app web.

1. Tornare al portale e fare clic sul nome dell'app web sotto il **nome** colonna per visualizzare le pagine di gestione.

    ![Aprire le pagine di gestione di app web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Aprire le pagine di gestione di app web*
2. Passare il **scala** pagina. Sotto il **generale** selezionare **Standard** per la configurazione e fare clic su **salvare** nella barra dei comandi.

    > [!NOTE]
    > Per eseguire tutte le app web nell'area corrente di sottoscrizione in **Standard** modalità, lasciare il **Seleziona tutto** selezionata nella casella di controllo di **scegliere siti** configurazione. In caso contrario, deselezionare il **Seleziona tutto** casella di controllo.

    ![L'aggiornamento di app web alla modalità Standard](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "l'aggiornamento di app web alla modalità Standard")

    *L'aggiornamento di App Web alla modalità Standard*
3. Fare clic su **Sì** per confermare le modifiche.

    ![Confermare le modifiche alla modalità Standard](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "continuare con la modifica della modalità di applicazione web")

    *Confermare le modifiche alla modalità Standard*
4. Passare al **Dashboard** pagina e fare clic su **abilita la pubblicazione di staging** sotto il **riepilogo rapido** sezione.

    ![Abilitazione della pubblicazione di staging](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "abilitazione di gestione temporanea pubblicazione")

    *Abilitazione della pubblicazione di gestione temporanea*
5. Fare clic su **Sì** per abilitare la pubblicazione di gestione temporanea.

    ![Per confermare la pubblicazione di staging](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "facendo clic su Sì per abilitare la pubblicazione di gestione temporanea")

    *Conferma pubblicazione di gestione temporanea*
6. Nell'elenco di App web, espandere il contrassegno a sinistra del nome dell'app web per visualizzare lo slot del sito di gestione temporanea. Contiene il nome dell'app web, seguito da ***(staging)***. Selezionare il sito di staging per passare alla pagina di gestione.

    ![Passare all'App web di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "passare all'App web di gestione temporanea")

    *Passare all'app di gestione temporanea*
7. Si noti che la pagina di gestione ha simile a gestione pagina di qualsiasi altra app web. Passare il **distribuzioni** pagina e copia il **URL Git** valore. Si utilizzerà più avanti in questo esercizio.

    ![Copia il valore dell'URL di Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copia il valore dell'URL di Git*
8. Aprire una nuova **Git Bash** console ed eseguire i comandi seguenti. Aggiornamento di *[percorso applicazione YOUR]* segnaposto con il percorso il **GeekQuiz** soluzione, si trova nel **Source\Ex1 DeployingWebSiteToStaging\Begin** cartella di questa esercitazione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inizializzazione di GIT e primo commit](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inizializzazione di GIT e primo commit*
9. Eseguire il comando seguente per effettuare il push dell'app web per il computer remoto **Git** repository. Sostituire il segnaposto con l'URL ottenuto dal portale di gestione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Inserimento di Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Inserimento di Azure*

    > [!NOTE]
    > Quando si distribuisce il contenuto nell'host FTP o nell'archivio GIT di un'app web, è necessario eseguire l'autenticazione utilizzando il **le credenziali di distribuzione** che è stato creato da dell'app web **avvio rapido** o **Dashboard ** pagine di gestione. Se non si conoscono le credenziali di distribuzione è possibile reimpostare facilmente tramite il portale di gestione. Aprire l'app web **Dashboard** pagina e fare clic su di **reimpostare le credenziali di distribuzione** collegamento. Fornire una nuova password e fare clic su **OK**. Le credenziali di distribuzione sono valide per l'utilizzo con tutte le app web associate alla sottoscrizione.
10. Per verificare l'app web è stato inserito correttamente in Azure, tornare al portale di gestione e fare clic su **siti Web**.
11. Individuare l'app web ed espandere la voce per visualizzare lo slot del sito di gestione temporanea. Fare clic su relativo **nome** per passare alla pagina di gestione.
12. Fare clic su **distribuzioni** per visualizzare il **cronologia della distribuzione**. Verificare che sia presente un **distribuzione attiva** con il * &quot;Commit iniziale&quot;*.

    ![Distribuzione attiva](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Distribuzione attiva*
13. Infine, fare clic su **Sfoglia** nella barra dei comandi per passare all'app web.

    ![Esplorare app web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Esplorare app web*
14. Se l'applicazione viene distribuita correttamente, si verrà visualizzata la pagina di accesso appassionato Quiz.

    > [!NOTE]
    > L'URL dell'applicazione distribuita contiene il nome dell'app web, seguito da *-gestione temporanea*.

    ![Applicazione in esecuzione nell'ambiente di gestione temporanea](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Applicazione in esecuzione nell'ambiente di gestione temporanea*
15. Se si desidera esplorare l'applicazione, fare clic su **registrare** per registrare un nuovo utente. Completare i dettagli dell'account, immettere un nome utente, l'indirizzo di posta elettronica e password. Successivamente, l'applicazione mostra la prima domanda del quiz. Rispondere ad alcune domande per assicurarsi che funzioni come previsto.

    ![Pronto per l'utilizzo dell'applicazione](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Pronto per l'utilizzo dell'applicazione*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Attività 4: innalzamento di livello all'App Web di produzione

Ora che è accertato che l'applicazione web funzioni correttamente nell'ambiente di gestione temporanea, si è pronti per alzare di livello nell'ambiente di produzione. In questa attività si scambiano slot del sito di gestione temporanea con lo slot di sito di produzione.

1. Tornare al portale di gestione e selezionare lo slot del sito di gestione temporanea. Fare clic su **scambiare** nella barra dei comandi.

    ![Passare alla produzione](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Passare alla produzione*
2. Fare clic su **Sì** nella finestra di dialogo di conferma per procedere con l'operazione di scambio. Azure verrà immediatamente Scambia il contenuto del sito di produzione con il contenuto del sito di staging.

    > [!NOTE]
    > Alcune impostazioni dalla versione di gestione temporanea verranno copiate automaticamente alla versione di produzione (ad esempio, stringa di connessione sostituzioni, i mapping del gestore e così via) ma non modificate altre impostazioni (ad esempio, gli endpoint DNS, le associazioni SSL e così via).

    ![Confermare l'operazione di scambio](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confermare l'operazione di scambio*
3. Una volta completato lo scambio, selezionare lo slot di produzione e fare clic su **Sfoglia** nella barra dei comandi per aprire il sito di produzione. Si noti l'URL nella barra degli indirizzi.

    > [!NOTE]
    > Potrebbe essere necessario aggiornare il browser per la cancellazione della cache. In Internet Explorer, è possibile farlo premendo **CTRL + R**.

    ![App Web in esecuzione nell'ambiente di produzione](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. Nel **GitBash** console, aggiornare l'URL remoto per il repository Git locale per lo slot di produzione di destinazione. A tale scopo, eseguire il comando seguente, sostituendo i segnaposto con il nome utente di distribuzione e il nome dell'app web.

    > [!NOTE]
    > Negli esercizi seguenti, si effettuerà il push delle modifiche nel sito di produzione, invece di gestione temporanea per la semplicità del lab. In uno scenario reale, è consigliabile verificare le modifiche nell'ambiente di gestione temporanea prima di alzare di livello nell'ambiente di produzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Esercizio 3: Esecuzione del Rollback di distribuzione nell'ambiente di produzione

Esistono scenari in cui non è uno slot di gestione temporanea per eseguire lo scambio attivo tra gestione temporanea e produzione, ad esempio, se si lavora con **libero** o **Shared** modalità. In tali scenari, è necessario testare l'applicazione in un ambiente di testing: localmente o in un sito remoto: prima di distribuire nell'ambiente di produzione. Tuttavia, è possibile che nel sito di produzione potrebbe verificarsi un problema non viene rilevato durante la fase di test. In questo caso, è importante disporre di un meccanismo per passare a una versione precedente e stabilità dell'applicazione al più presto.

In **Azure App Service**, una distribuzione continua dal controllo del codice sorgente rende questo possibile grazie al **ridistribuire** azione disponibile nel portale di gestione. Azure tiene traccia delle distribuzioni associate viene eseguito il commit eseguito il push archivio e fornisce un'opzione per ridistribuire l'applicazione utilizzando una delle distribuzioni precedenti in qualsiasi momento.

In questo esercizio si eseguirà una modifica al codice di **appassionato Quiz** applicazione che inserisce intenzionalmente un *bug*. Verrà distribuito l'applicazione nell'ambiente di produzione per verificare l'errore e quindi si sfrutterà la funzionalità di ridistribuzione per tornare allo stato precedente.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Attività 1: aggiornamento dell'applicazione Quiz appassionato

In questa attività eseguire il refactoring una piccola parte di codice del **TriviaController** classe per estrarre una parte della logica di che recupera l'opzione quiz selezionato dal database in un nuovo metodo.

1. Passare all'istanza di Visual Studio con il **GeekQuiz** soluzione esercizio precedente.
2. In **Esplora**, aprire il **TriviaController.cs** file all'interno di **controller** cartella.
3. Individuare il **StoreAsync** (metodo) e selezionare il codice evidenziato nella figura seguente.

    ![Se si seleziona il codice](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Se si seleziona il codice*
4. Fare doppio clic sul codice selezionato, espandere il **refactoring** dal menu **Estrai metodo... **.

    ![Estrarre il codice come un nuovo metodo](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Selezione metodo di estrazione*
5. Nel **Estrai metodo** nella finestra di dialogo Nome nuovo metodo *MatchesOption* e fare clic su **OK**.

    ![Specifica il nome del metodo](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Specifica il nome per il metodo estratto*
6. Il codice selezionato viene quindi estratto il **MatchesOption** metodo. Il codice risultante è illustrato nel frammento riportato di seguito.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Premere **CTRL + S** per salvare le modifiche.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Attività 2: ridistribuire l'applicazione di Quiz appassionato

A questo punto si effettuerà il push le modifiche apportate nell'attività precedente nel repository, che verrà attivata una nuova distribuzione nell'ambiente di produzione. Quindi verrà risoluzione dei problemi un problema usando il **gli strumenti di sviluppo F12** di Internet Explorer e quindi eseguire un'operazione di rollback per la distribuzione precedente dal portale di gestione di Azure.

1. Aprire una nuova **Git Bash** console per distribuire l'applicazione di servizio App di Azure aggiornata.
2. Eseguire i comandi seguenti per riflettere le modifiche in Azure. Aggiornamento di *[percorso applicazione YOUR]* segnaposto con il percorso di **GeekQuiz** soluzione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Inserimento di codice sottoposto a refactoring in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Inserimento di codice sottoposto a refactoring in Azure*
3. Aprire Internet Explorer e passare all'app web (ad esempio `http://<your-web-site>.azurewebsites.net`). Accedere con le credenziali create in precedenza.
4. Premere **F12** per avviare gli strumenti di sviluppo, selezionare il **rete** scheda e scegliere il **riprodurre** pulsante per avviare la registrazione.

    ![Avvio registrazione rete](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "avvio registrazione di rete")

    *Avvio registrazione di rete*
5. Selezionare un'opzione del quiz. Si noterà che viene eseguita alcuna operazione.
6. Nel **F12** finestra, la voce corrispondente alla richiesta HTTP POST Mostra HTTP **500** risultato.

    ![HTTP 500 Errore](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 Errore*
7. Selezionare il **Console** scheda. Con i dettagli della causa, viene registrato un errore.

    ![Errore registrato](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Errore registrato*
8. Individuare la parte di dettagli dell'errore. Chiaramente, questo errore è causato dal codice refactoring è stato eseguito il commit in precedenza.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Non chiudere il browser.
10. In una nuova istanza del browser, individuare il [portale di gestione di Azure](https://manage.windowsazure.com) e accedere utilizzando l'account Microsoft associato alla sottoscrizione.
11. Selezionare **siti Web** e fare clic sull'app web è stato creato nell'esercizio 2.
12. Passare il **distribuzioni** pagina. Si noti che tutti i commit eseguiti sono elencati nella cronologia di distribuzione.

    ![Elenco delle distribuzioni esistenti](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Elenco delle distribuzioni esistenti*
13. Selezionare il commit precedente e fare clic su **ridistribuire** sulla barra dei comandi.

    ![Ridistribuire il commit precedente](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ridistribuire il commit precedente*
14. Quando viene richiesto di confermare, fare clic su **Sì**.

    ![Per confermare la ridistribuzione](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Al termine della distribuzione, passare all'istanza del browser con l'applicazione web e premere **CTRL + F5**.
16. Fare clic su una delle opzioni. Verrà visualizzata l'animazione capovolto sul posto e il risultato (*corretta/*) verranno visualizzati.
17. (Facoltativo) Passare il **Git Bash** console ed eseguire i comandi seguenti per ripristinare il commit precedente.

    > [!NOTE]
    > Questi comandi creano un nuovo commit che annulla tutte le modifiche nel repository Git che sono state apportate nel commit non valido. Azure verrà quindi ridistribuire l'applicazione utilizzando il commit di nuovo.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Esercizio 4: Scalabilità mediante l'archiviazione di Azure

**BLOB** rappresentano il modo più semplice per archiviare grandi quantità di testo non strutturato o dati binari come video, audio e immagini. Spostare il contenuto statico dell'applicazione di archiviazione, consente di ridimensionare l'applicazione, è necessario gestire le immagini o documenti direttamente al browser.

In questo esercizio si sposterà il contenuto statico di un'applicazione a un contenitore Blob. Quindi è necessario configurare l'applicazione per aggiungere un **regola di riscrittura dell'URL di ASP.NET** nel **Web. config** per reindirizzare il contenuto del contenitore Blob.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Attività 1: creazione di un Account di archiviazione di Azure

In questa attività si apprenderà come creare un nuovo account di archiviazione tramite il portale di gestione.

1. Passare il [portale di gestione di Azure](https://manage.windowsazure.com) e accedere utilizzando l'account Microsoft associato alla sottoscrizione.
2. Selezionare **New | Servizi dati | Archiviazione | Creazione rapida** per iniziare a creare un nuovo account di archiviazione. Immettere un nome univoco per l'account e selezionare un **area** dall'elenco. Fare clic su **crea Account di archiviazione** per continuare.

    ![Crea un nuovo Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "crea un nuovo Account di archiviazione")

    *Crea un nuovo account di archiviazione*
3. Nel **archiviazione** sezione, attendere che lo stato del nuovo account di archiviazione diventa *Online* per continuare con la seguente procedura.

    ![Account di archiviazione creato](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Account di archiviazione creato")

    *Account di archiviazione creato*
4. Fare clic sul nome dell'account di archiviazione e quindi fare clic su di **Dashboard** collegamento nella parte superiore della pagina. Il **Dashboard** pagina vengono fornite informazioni sullo stato dell'account e gli endpoint del servizio che possono essere utilizzati all'interno delle applicazioni.

    ![Visualizzazione Dashboard dell'Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "visualizzazione Dashboard dell'Account di archiviazione")

    *Visualizzazione Dashboard dell'Account di archiviazione*
5. Fare clic su di **Gestisci chiavi di accesso** pulsante nella barra di spostamento.

    ![Pulsante di chiavi di accesso di gestione](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "pulsante Gestisci chiavi di accesso")

    *Gestire il pulsante di chiavi di accesso*
6. Nel **Gestisci chiavi di accesso** della finestra di dialogo Copia il **nome Account di archiviazione** e **chiave di accesso primaria** come saranno necessari nell'esercizio seguente. Quindi, chiudere la finestra di dialogo.

    ![Finestra di dialogo Gestisci chiave di accesso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "la finestra di dialogo Gestisci chiave di accesso")

    *Gestire la finestra di dialogo di chiave di accesso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Attività 2: caricamento di un Asset di archiviazione Blob di Azure

In questa attività, si utilizzerà la finestra di Esplora Server da Visual Studio per connettersi all'account di archiviazione. Verrà quindi creare un contenitore blob e caricare un file con il logo appassionato Quiz al contenitore.

1. Passare all'istanza di Visual Studio con il **GeekQuiz** soluzione esercizio precedente.
2. Dalla barra dei menu, selezionare **vista** e quindi fare clic su **Esplora Server**.
3. In **Esplora Server**, fare doppio clic su di **Azure** nodo e selezionare **Connetti a Azure... **. Accedere utilizzando l'account Microsoft associato alla sottoscrizione.

    ![Connettersi a Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Connettersi a Azure*
4. Espandere il **Azure** nodo, fare doppio clic su **archiviazione** e selezionare **collega archiviazione esterna... **.
5. Nel **aggiungere di nuovo Account di archiviazione** finestra di dialogo immettere il **nome Account** e **chiave dell'Account** ottenuti nell'attività precedente e fare clic su **OK**.

    ![Aggiungere la finestra di dialogo Nuovo Account di archiviazione](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Aggiungere la finestra di dialogo Nuovo Account di archiviazione*
6. L'account di archiviazione dovrebbe essere visualizzato sotto il **archiviazione** nodo. Espandere l'account di archiviazione, fare doppio clic su **BLOB** e selezionare **crea contenitore Blob... **.

    ![Crea contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "crea contenitore Blob")

    *Crea contenitore Blob*
7. Nel **crea contenitore Blob** finestra di dialogo immettere un nome per il contenitore blob e fare clic su **OK**.

    ![Finestra di dialogo Crea contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "la finestra di dialogo Crea contenitore Blob")

    *Creare la finestra di dialogo contenitore Blob*
8. Il nuovo contenitore blob deve essere aggiunti al **BLOB** nodo. Modificare le autorizzazioni di accesso per rendere pubblico il contenitore nel contenitore. A tale scopo, fare doppio clic su di **immagini** contenitore e selezionare **proprietà**.

    ![le proprietà del contenitore di immagini](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "contenitore proprietà di immagini")

    *Proprietà di immagini contenitore*
9. Nel **proprietà** finestra, impostare il **accesso in lettura pubblico** a **contenitore**.

    ![Modifica proprietà di accesso in lettura pubblico](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "la modifica di proprietà di accesso in lettura pubblico")

    *Modifica della proprietà di accesso in lettura pubblico*
10. Quando viene richiesto se si è certi che si desidera modificare la proprietà di accesso pubblico, fare clic su **Sì**.

    ![Avviso di Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "avviso di Microsoft Visual Studio")

    *Avviso di Microsoft Visual Studio*
11. In **Esplora Server**, fare clic del mouse il **immagini** contenitore blob e selezionare **Visualizza contenitore Blob**.

    ![Visualizza contenitore Blob](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Visualizza contenitore Blob")

    *Visualizza contenitore Blob*
12. Il contenitore di immagini per l'apertura di una nuova finestra e deve essere visualizzata una legenda senza voci. Fare clic su di **caricare** sull'icona per caricare un file nel contenitore blob.

    ![Contenitore di immagini senza voci](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "contenitore immagini senza voci")

    *Contenitore di immagini senza voci*
13. Nel **carica Blob** finestra di dialogo passare al **asset** cartella dell'esercitazione. Selezionare il **logo big.png** file e fare clic su **aprire**.
14. Attendere finché non viene caricato il file. Al termine dell'aggiornamento, il file deve essere elencato nel contenitore di immagini. La voce del file e scegliere **Copia URL**.

    ![Copiare l'URL del blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "copiare l'URL del file blob")

    *Copiare l'URL del blob*
15. Aprire Internet Explorer e incollare l'URL. Nell'immagine seguente deve essere visualizzato nel browser.

    ![immagine del logo big.png da archiviazione Blob di Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "immagine logo big.png dall'archiviazione")

    *immagine del logo big.png da archiviazione Blob di Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Attività 3: aggiornare la soluzione per utilizzare il contenuto statico dall'archiviazione Blob di Azure

In questa attività si configurerà il **GeekQuiz** soluzione per utilizzare l'immagine caricata archiviazione Blob di Azure (anziché l'immagine si trova nell'app web) tramite l'aggiunta di una regola di riscrittura degli URL di ASP.NET nel **Web. config**file.

1. In Visual Studio, aprire il **Web. config** file all'interno di **GeekQuiz** del progetto e individuare il ** &lt;System. webServer&gt; ** elemento.
2. Aggiungere il codice seguente per aggiungere un URL rewrite regola, l'aggiornamento il segnaposto con il nome di account di archiviazione.

    (- Frammento di codice *UrlRewriteRule WebSitesInProduction - Ex4 -*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > La riscrittura URL è il processo di intercettazione di una richiesta Web in ingresso e reindirizzando la richiesta a una risorsa diversa. Regole di riscrittura URL indica al motore di riscrittura quando una richiesta deve essere reindirizzato e in cui si viene reindirizzati. Una regola di riscrittura è costituita da due stringhe: il modello da cercare nell'URL richiesto, in genere, utilizzando espressioni regolari, e la stringa con cui sostituire il modello, se trovata. Per ulteriori informazioni, vedere [la riscrittura URL in ASP.NET](https://msdn.microsoft.com/en-us/library/ms972974.aspx).
3. Premere **CTRL + S** per salvare le modifiche.
4. Aprire una nuova **Git Bash** console per distribuire l'applicazione di servizio App di Azure aggiornata.
5. Eseguire i comandi seguenti per riflettere le modifiche in Azure. Aggiornamento di *[percorso applicazione YOUR]* segnaposto con il percorso di **GeekQuiz** soluzione. Verrà richiesto di specificare la password di distribuzione.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Distribuzione di aggiornamento in Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Distribuzione di aggiornamento in Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Attività 4: verifica

In questa attività si utilizzerà **Internet Explorer** per esplorare il **appassionato Quiz** applicazione e verificare che l'URL rewrite regola per il funzionamento di immagini e vengono reindirizzate all'immagine di cui è ospitato in **Blob di Azure Archiviazione**.

1. Aprire Internet Explorer e passare all'app web (ad esempio `http://<your-web-site>.azurewebsites.net`). Accedere con le credenziali create in precedenza.

    ![Con l'app web appassionato Quiz con l'immagine](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "che mostra l'app web appassionato Quiz con l'immagine")

    *Con l'app web appassionato Quiz con l'immagine*
2. Premere **F12** per avviare gli strumenti di sviluppo, selezionare il **rete** scheda e avviare la registrazione.

    ![Avvio registrazione rete](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "avvio registrazione di rete")

    *Avvio registrazione di rete*
3. Premere **CTRL + F5** per aggiornare la pagina web.
4. Al termine del caricamento della pagina, dovrebbe essere una richiesta HTTP per il **/img/logo-big.png** URL con HTTP **301** risultato (reindirizzamento) e un'altra richiesta di `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL con un HTTP **200** risultato.

    ![Verifica per determinare se il reindirizzamento dell'URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "che mostra il reindirizzamento in strumenti di sviluppo")

    *Verifica per determinare se il reindirizzamento dell'URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Esercizio 5: Utilizza la scalabilità automatica per le app Web

> [!NOTE]
> Questo esercizio è facoltativo, poiché richiede il supporto per il carico Web &amp; che è disponibile solo per il test delle prestazioni **Visual Studio 2013 Ultimate Edition**. Per ulteriori informazioni su funzionalità specifiche di Visual Studio 2013, confrontare le versioni [qui](https://www.microsoft.com/visualstudio/eng/products/compare).


**Le app Web di Azure App Service** fornisce la funzionalità di scalabilità automatica per le app web in esecuzione in **modalità Standard**. Scalabilità automatica consente di Azure ridimensionare automaticamente il conteggio delle istanze dell'app web a seconda del carico. Quando sono abilitata la scalabilità automatica, Azure controlla la CPU dell'app web una volta ogni cinque minuti e aggiunge istanze in base alle esigenze in quel punto nel tempo. Se l'utilizzo della CPU è bassa, Azure verrà rimosse le istanze di una volta ogni due ore per garantire che le prestazioni dell'app web non è danneggiata.

In questo esercizio si passerà i passaggi necessari per configurare il **scalabilità automatica** funzionalità per il **appassionato Quiz** app web. Questa funzionalità si verificherà eseguendo un test di carico di Visual Studio per generare un carico sufficiente CPU in modo da attivare un aggiornamento dell'istanza.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Attività 1: configurazione di scalabilità automatica basata sulla metrica della CPU

In questa attività si utilizzerà il portale di gestione di Azure per abilitare la funzionalità di scalabilità automatica per l'app web che è stato creato nell'esercizio 2.

1. Nel [portale di gestione di Azure](https://manage.windowsazure.com/)selezionare **siti Web** e fare clic sull'app web è stato creato nell'esercizio 2.
2. Passare il **scala** pagina. Sotto il **capacità** selezionare **CPU** per il **scala in base alla metrica** configurazione.

    > [!NOTE]
    > In caso di ridimensionamento per CPU, Azure si adatta dinamicamente il numero di istanze che l'app utilizzata se viene modificata l'utilizzo della CPU.

    ![Se si sceglie di scala in base alla CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "selezionando la metrica della CPU per il ridimensionamento automatico")

    *Se si sceglie di scala in base alla CPU*
3. Modifica il **CPU destinazione** configurazione **20**-**40** percento.

    > [!NOTE]
    > Questo intervallo rappresenta l'utilizzo medio della CPU per app web. Azure aggiungerà o rimuoverà istanze per mantenere l'app web in questo intervallo. Il numero minimo e massimo di istanze utilizzate per la scalabilità è incluso il **conteggio delle istanze** configurazione. Azure non andrà mai di sopra o oltre il limite stabilito.
    > 
    > Il valore predefinito **CPU destinazione** valori vengono modificati solo per fini di questa esercitazione. Configurando l'intervallo di CPU con valori di piccole dimensioni, si stanno aumentando le probabilità per l'attivazione di scalabilità automatica, quando un carico moderato si trova l'applicazione.

    ![Modifica della destinazione della CPU deve essere compresa tra 20 e il 40 percento](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "modifica della CPU di destinazione per essere compreso tra 20 e il 40 percento")

    *Modifica la CPU di destinazione per essere compreso tra 20 e il 40 percento*
4. Fare clic su **salvare** nella barra dei comandi per salvare le modifiche.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Attività 2: test di carico con Visual Studio

Ora che **scalabilità automatica** è stato configurato, si creerà un **progetto di Test di carico e prestazioni Web** in Visual Studio per generare un carico della CPU dell'app web.

1. Aprire **Visual Studio Ultimate 2013** e selezionare **File | New | Progetto... ** per avviare una nuova soluzione.

    ![Crea un nuovo progetto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "creando un nuovo progetto")

    *Crea un nuovo progetto*
2. Nel **nuovo progetto** nella finestra di dialogo **il progetto di Test di carico e prestazioni Web** sotto il **Visual c# | Test** scheda. Assicurarsi che **.NET Framework 4.5** è selezionata, denominare il progetto *WebAndLoadTestProject*, scegliere un **percorso** e fare clic su **OK**.

    ![Crea un nuovo progetto di Test di carico e Web](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "creando un nuovo progetto di Test di carico e Web")

    *Crea un nuovo progetto di Test di carico e Web*
3. Nel **WebTest1. webtest** destro la **WebTest1** nodo e fare clic su **Aggiungi richiesta**.

    ![Aggiunta di una richiesta a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "aggiunta di una richiesta a WebTest1")

    *Aggiunta di una richiesta a WebTest1*
4. Nel **proprietà** finestra del nuovo nodo di richiesta, aggiornare il **Url** proprietà in modo che punti all'URL dell'app web (ad esempio * [http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/) *).

    ![La modifica della proprietà Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "la modifica della proprietà Url")

    *La modifica della proprietà Url*
5. Nel **WebTest1. webtest** finestra, fare doppio clic su **WebTest1** e fare clic su **aggiungere ciclo... **.

    ![Aggiunta di un ciclo per WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "aggiunta di un ciclo per WebTest1")

    *Aggiunta di un ciclo per WebTest1*
6. Nel **Aggiungi regola condizionale ed elementi al ciclo** la finestra di dialogo, seleziona il **ciclo For** regola e modificare le proprietà seguenti.

    1. **Valore di terminazione:** 1000
    2. **Nome di parametro di contesto:** iteratore
    3. **Il valore di incremento:** 1

    ![Selezionando la regola ciclo For e l'aggiornamento delle proprietà](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "selezionando la regola ciclo For e l'aggiornamento delle proprietà")

    *Selezionando la regola ciclo For e l'aggiornamento delle proprietà*
7. Sotto il **elementi nel ciclo** , selezionare la richiesta per il primo e l'ultimo elemento per il ciclo di essere creato in precedenza. Fare clic su **OK** per continuare.

    ![Selezionare il primo e l'ultimo elemento per il ciclo](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "selezionando il primo e l'ultimo elemento per il ciclo")

    *Selezionare il primo e l'ultimo elemento per il ciclo*
8. In **Esplora**, fare doppio clic su di **WebAndLoadTestProject** del progetto, espandere il **Aggiungi** dal menu **Test di carico... **.

    ![Aggiunta di un Test di carico al progetto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "aggiunta di un Test di carico al progetto WebAndLoadTestProject")

    *Aggiunta di un Test di carico al progetto WebAndLoadTestProject*
9. Nel **Creazione guidata Test di carico** la finestra di dialogo, fare clic su **Avanti**.

    ![Creazione guidata Test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Creazione guidata Test di carico")

    *Creazione guidata Test di carico*
10. Nel **Scenario** selezionare **non utilizzano i tempi interazione utente** e fare clic su **Avanti**.

    ![Selezione di non utilizzare i tempi interazione utente](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "selezione non è consigliabile usare i tempi interazione utente")

    *Se si sceglie di non per utilizzare i tempi interazione utente*
11. Nel **modello di carico** pagina, assicurarsi che il **carico costante** opzione è selezionata. Modifica il **numero utenti** impostando su **250** utenti e fare clic su **Avanti**.

    ![Modifica il numero di utenti a 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "modifica il numero di utenti a 250")

    *Modifica il numero di utenti a 250*
12. Nel **modello di combinazione di Test** selezionare **in base a ordine sequenziale dei test** e fare clic su **Avanti**.

    ![Selezionare il modello di combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "selezionando il modello di combinazione di test")

    *Selezionare il modello di combinazione di test*
13. Nel **il modello di combinazione di Test** pagina, fare clic su **Aggiungi... ** per aggiungere test alla combinazione.

    ![Aggiunta di un test alla combinazione di test](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "aggiunta di un test alla combinazione di test")

    *Aggiunta di un test alla combinazione di test*
14. Nel **Aggiungi test** la finestra di dialogo, fare doppio clic su **WebTest1** per aggiungere test al **test selezionati** elenco. Fare clic su **OK** per continuare.

    ![Aggiunta di test WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "aggiungendo il test WebTest1")

    *Aggiunta di test WebTest1*
15. Nel **combinazione di Test** pagina, fare clic su **Avanti**.

    ![Pagina Completamento della combinazione di Test](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "pagina Completamento della combinazione di Test")

    *Pagina Completamento della combinazione di Test*
16. Nel **combinazione di reti** pagina, fare clic su **Avanti**.

    ![Fare clic su Avanti nella pagina combinazione di reti](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "facendo clic su Avanti nella pagina combinazione di reti")

    *Fare clic su Avanti nella pagina combinazione di reti*
17. Nel **combinazione di Browser** selezionare **Internet Explorer 10.0** come tipo di browser e fare clic su **Avanti**.

    ![Selezione del tipo di browser](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "selezionando il tipo di browser")

    *Selezione del tipo di browser*
18. Nel **insiemi di contatori** pagina, fare clic su **Avanti**.

    ![Fare clic su Avanti nella pagina insiemi di contatori](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "facendo clic su Avanti nella pagina di insiemi di contatori")

    *Fare clic su Avanti nella pagina insiemi di contatori*
19. Nel **impostazioni di esecuzione** pagina, impostare il **durata test di carico** a **5 minuti** e fare clic su **fine**.

    ![Impostare la durata del test di carico su 5 minuti](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "impostando la durata del test di carico su 5 minuti")

    *Impostare la durata del test di carico su 5 minuti*
20. In **Esplora**, fare doppio clic su di **Local.settings** file per esplorare le impostazioni di test. Per impostazione predefinita, Visual Studio utilizza il computer locale per eseguire i test.

    > [!NOTE]
    > In alternativa, è possibile configurare il progetto di test per eseguire i test di carico nel cloud usando **Visual Studio Online (VSO)**. Visual Studio Online offre un carico basato su cloud la verifica del servizio che consente di simulare un carico più realistica, evitando i vincoli di ambiente locale ad esempio la capacità della CPU, memoria e larghezza di banda di rete. Per ulteriori informazioni sull'utilizzo di Visual Studio online per eseguire i test di carico, vedere [questo articolo](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Impostazioni di test](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Attività 3: verifica di scalabilità automatica

Verrà ora eseguire il test di carico che è stato creato nell'attività precedente e vedere come l'app web si comporta in condizioni di carico.

1. In **Esplora**, fare doppio clic su **LoadTest1. LoadTest** per aprire il test di carico.

    ![Apertura LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "apertura LoadTest1. LoadTest")

    *Apertura LoadTest1. LoadTest*
2. Nel **LoadTest1. LoadTest** finestra, fare clic sul primo pulsante della casella degli strumenti per eseguire il test di carico.

    ![Esecuzione del test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "eseguendo il test di carico")

    *Esecuzione del test di carico*
3. Attendere il completamento del test di carico.

    > [!NOTE]
    > Il test di carico simula più utenti che inviano richieste per l'app web contemporaneamente. Quando l'esecuzione del test, è possibile monitorare i contatori disponibili per rilevare eventuali errori, avvisi o altre informazioni correlate all'esecuzione dei test di carico.

    ![Esecuzione del test di carico](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "in attesa fino al completamento del test di carico")

    *Esecuzione del test di carico*
4. Dopo il completamento del test, tornare al portale di gestione e passare al **scala** pagina dell'app web. Sotto il **capacità** sezione, verrà visualizzato nel grafico che una nuova istanza è stata distribuita automaticamente.

    ![Nuova istanza distribuito automaticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nuova istanza distribuito automaticamente*

    > [!NOTE]
    > Potrebbe richiedere alcuni minuti per le modifiche vengono visualizzati nel grafico (premere **CTRL + F5** periodicamente per aggiornare la pagina). Se non è possibile visualizzare tutte le modifiche, è possibile provare le operazioni seguenti:
    > 
    > - Aumentare la durata del test di carico (ad esempio, per **10 minuti**)
    > - I valori minimi e massimo di ridurre il **CPU destinazione** intervallo nella configurazione di scalabilità automatica dell'app web
    > - Eseguire il test di carico nel cloud con **Visual Studio Online**. Altre informazioni [qui](https://www.visualstudio.com/en-us/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Riepilogo

In questa esercitazione pratica, è stato descritto come configurare e distribuire l'applicazione in App web di produzione in Azure. Per iniziare a rilevare e aggiornando i database utilizzando **migrazioni di Entity Framework Code First**, quindi continuare la distribuzione di nuove versioni del tuo sito con **Git** e l'esecuzione di operazioni di rollback per il ultima versione stabile del sito. Inoltre, è stato descritto come ridimensionare l'app usando l'archiviazione per spostare il contenuto statico in un contenitore Blob.
