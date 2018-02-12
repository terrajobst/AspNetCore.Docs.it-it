---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: aggiornamento di un Database di distribuzione | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Distribuzione Web ASP.NET utilizzando Visual Studio: aggiornamento di un Database di distribuzione
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto di avvio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione è apportare una modifica al database e le modifiche correlate, verificare le modifiche in Visual Studio, quindi distribuire l'aggiornamento in ambienti di test, gestione temporanea e produzione.

L'esercitazione innanzitutto illustrato come aggiornare un database gestito da migrazioni Code First e in seguito viene illustrato come aggiornare un database tramite il provider dbDacFx.

Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Distribuire un aggiornamento del database utilizzando migrazioni Code First

In questa sezione, si aggiunge una colonna di data di nascita per il `Person` classe di base per il `Student` e `Instructor` entità. È quindi necessario aggiornare la pagina che visualizza i dati di istruttore in modo da visualizzare la nuova colonna. Infine, le modifiche vengono distribuite a test, gestione temporanea e produzione.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Aggiungere una colonna a una tabella nel database dell'applicazione

1. Nel *ContosoUniversity.DAL* progetto, aprire *Person* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Aggiornare quindi il `Seed` metodo in modo che fornisce un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente, che include informazioni sulla data di nascita:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compilare la soluzione e quindi aprire il **Package Manager Console** finestra. Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come il **progetto predefinito**.
3. Nel **Package Manager Console** selezionare **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna. Il `Up` metodo consente di creare la colonna quando si implementa la modifica e `Down` metodo elimina la colonna quando si esegue il rollback della modifica.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compilare la soluzione e quindi immettere il comando seguente nel **Package Manager Console** finestra (assicurarsi che sia ancora selezionato il progetto ContosoUniversity.DAL):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework viene eseguito il `Up` (metodo) e quindi viene eseguito il `Seed` metodo.

### <a name="display-the-new-column-in-the-instructors-page"></a>Visualizzare la nuova colonna nella pagina i docenti

1. Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita. Aggiungere il progetto tra quelle per l'assegnazione di office e di data di assunzione:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Se il rientro di codice ottiene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)
2. Eseguire l'applicazione e fare clic su di **i docenti** collegamento.

    Quando il caricamento della pagina, vedrai che contiene il nuovo campo Data di nascita.

    ![Pagina istruttori con data di nascita](deploying-a-database-update/_static/image2.png)
3. Chiudere il browser.

### <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

1. In **Esplora** selezionare il progetto ContosoUniversity.
2. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**. (Se la barra degli strumenti è disabilitato, selezionare il progetto ContosoUniversity in **Esplora**.)

    Visual Studio distribuisce l'applicazione aggiornata e il browser viene visualizzata la pagina home.
3. Eseguire il **i docenti** pagina per verificare che l'aggiornamento è stato distribuito correttamente.

    Quando l'applicazione tenta di accedere al database per questa pagina, il primo codice aggiorna lo schema del database e viene eseguito il `Seed` metodo. Quando viene visualizzata la pagina, viene visualizzato il previsto **data di nascita** colonna delle date in essa contenuti.
4. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **gestione temporanea** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.
5. Eseguire il **i docenti** pagina di gestione temporanea per verificare che l'aggiornamento è stato distribuito correttamente.
6. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.
7. Eseguire il **i docenti** pagina nell'ambiente di produzione per verificare che l'aggiornamento è stato distribuito correttamente.

    Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione durante la distribuzione tramite *app\_offline.htm*, come visto nell'esercitazione precedente.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Distribuire un aggiornamento del database tramite il provider dbDacFx

In questa sezione, si aggiunge un *commenti* colonna per il *utente* tabella nel database delle appartenenze e creare una pagina che consente di visualizzare e modificare commenti per ogni utente. Distribuire le modifiche a test, gestione temporanea e produzione.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Aggiungere una colonna a una tabella nel database delle appartenenze

1. In Visual Studio, aprire **Esplora oggetti di SQL Server**.
2. Espandere **(localdb) \v11.0**, espandere **database**, espandere **aspnet ContosoUniversity** (non **aspnet-ContosoUniversity-Prod**) e quindi espandere **tabelle**.

    Se non viene visualizzato **(localdb) \v11.0** sotto il **SQL Server** nodo, fare doppio clic su di **SQL Server** nodo e fare clic su **aggiungere SQL Server**. Nel **Connetti al Server** la finestra di dialogo immettere *(localdb) \v11.0* come il **nome del Server**, quindi fare clic su **Connetti**.

    Se non viene visualizzato **aspnet ContosoUniversity**, eseguire il progetto e accedere con il *admin* credenziali (password è *devpwd*), quindi aggiornare il  **Esplora oggetti di SQL Server** finestra.
3. Fare doppio clic su di **utenti** tabella e quindi fare clic su **Visualizza finestra di progettazione**.

    ![Finestra di progettazione sillaba SSOX](deploying-a-database-update/_static/image3.png)
4. Nella finestra di progettazione, aggiungere un *commenti* colonna e renderlo *nvarchar (128)* e ammette valori null, quindi fare clic su **aggiornamento**.

    ![Aggiunta di commenti colonna](deploying-a-database-update/_static/image4.png)
5. Nel **Anteprima aggiornamenti Database** fare clic su **aggiornamento Database**.

    ![Anteprima aggiornamenti Database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Creare una pagina per visualizzare e modificare la nuova colonna

1. In **Esplora**, fare doppio clic su di **Account** cartella nel progetto ContosoUniversity, fare clic su **Aggiungi**e quindi fare clic su **nuovo elemento** .
2. Creare un nuovo **pagina Web Form utilizzando Master** e denominarlo *UserInfo.aspx*. Accettare il valore predefinito *Site. master* file della pagina master.
3. Copiare il seguente markup nel `MainContent` `Content` elemento (l'ultimo di 3 `Content` elementi):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Fare doppio clic su di *UserInfo.aspx* pagina e fare clic su **Visualizza nel Browser**.
5. Accedere con il *admin* le credenziali dell'utente (password *devpwd*) e aggiungere alcuni commenti a un utente per verificare il corretto funzionamento della pagina.

    ![Pagina informazioni utente](deploying-a-database-update/_static/image6.png)
6. Chiudere il browser.

## <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

Per distribuire tramite il provider dbDacFx, è sufficiente selezionare il **Aggiorna database** opzione nel profilo di pubblicazione. Tuttavia, per la distribuzione iniziale quando si utilizza questa opzione è inoltre configurato alcuni script SQL aggiuntive da eseguire: quelli presenti nel profilo, è necessario evitare che venga eseguita di nuovo.

1. Aprire il **pubblica sul Web** guidata facendo clic su progetto ContosoUniversity e facendo clic su **pubblica**.
2. Selezionare il **Test** profilo.
3. Fare clic su di **impostazioni** scheda.
4. In **DefaultConnection**selezionare **Aggiorna database**.
5. Disabilitare gli script aggiuntivi configurati per essere eseguiti per la distribuzione iniziale:

    1. Fare clic su **configurare gli aggiornamenti di database**.
    2. Nel **Configura Aggiornamenti Database** la finestra di dialogo, deselezionare le caselle di controllo accanto a *Grant.sql* e *aspnet-data-dev.sql*.
    3. Fare clic su **Chiudi**.
6. Fare clic su di **anteprima** scheda.
7. In **database** e a destra del **DefaultConnection**, fare clic su di **database di anteprima** collegamento.

    ![Anteprima di database](deploying-a-database-update/_static/image7.png)

    La finestra di anteprima mostra lo script che verrà eseguito nel database di destinazione affinché corrisponda allo schema del database di origine schema del database. Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.
8. Chiudi il **anteprima di Database** la finestra di dialogo e quindi fare clic su **pubblica**.

    Visual Studio distribuisce l'applicazione aggiornata e il browser viene visualizzata la pagina home.
9. Eseguire la pagina UserInfo (aggiungere *Account/UserInfo.aspx* per l'URL della home page) per verificare che l'aggiornamento è stato distribuito correttamente. È necessario accedere immettendo *admin* e *devpwd*.

    Per impostazione predefinita non vengono distribuiti i dati nelle tabelle e si non è stato configurato uno script di distribuzione di dati per l'esecuzione, in modo da non individuare il commento è stato aggiunto in fase di sviluppo. È possibile aggiungere un nuovo commento a questo punto di gestione temporanea per verificare che la modifica è stata distribuita il database e corretto funzionamento della pagina.
10. Seguire la stessa procedura per la distribuzione di gestione temporanea e produzione.

    Non dimenticare di disattivare gli script aggiuntivi. L'unica differenza rispetto al profilo di Test è che si disabiliterà solo uno script di gestione temporanea e produzione profili perché sono stati configurati per l'esecuzione solo *aspnet-op-data.sql*.

    Le credenziali per la gestione temporanea e produzione sono prodpwd e amministrazione.

    Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione durante la distribuzione caricando *app\_offline.htm* prima della pubblicazione e l'eliminazione in seguito, come specificato nella [esercitazione precedente](deploying-a-code-update.md).

## <a name="summary"></a>Riepilogo

Aggiornamento di un'applicazione che include una modifica di database utilizzando migrazioni Code First sia il provider dbDacFx distribuiti a questo punto.

![Pagina istruttori con data di nascita](deploying-a-database-update/_static/image8.png)

![Pagina informazioni utente](deploying-a-database-update/_static/image9.png)

L'esercitazione successiva viene illustrato come eseguire le distribuzioni tramite la riga di comando.

>[!div class="step-by-step"]
[Precedente](deploying-a-code-update.md)
[Successivo](command-line-deployment.md)
