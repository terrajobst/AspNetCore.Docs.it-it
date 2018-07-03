---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento di Database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389189"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del Database
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto iniziale](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).


## <a name="overview"></a>Panoramica

In questa esercitazione, si apportano una modifica al database e le modifiche al codice correlato, testare le modifiche in Visual Studio, quindi distribuire l'aggiornamento agli ambienti di test, staging e produzione.

L'esercitazione illustra innanzitutto la procedura per aggiornare un database gestito da migrazioni Code First e versioni successive viene illustrato come aggiornare un database tramite il provider dbDacFx.

Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Distribuire un aggiornamento del database usando migrazioni Code First

In questa sezione si aggiunge una colonna di date di nascita per il `Person` classe di base per il `Student` e `Instructor` entità. Quindi necessario aggiornare la pagina che visualizza i dati di instructor in modo da visualizzare la nuova colonna. Infine, distribuire le modifiche al test, staging e produzione.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Aggiungere una colonna a una tabella nel database dell'applicazione

1. Nel *ContosoUniversity.DAL* progetto aprire *Person.cs* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    A questo punto, aggiornare il `Seed` metodo in modo che l'it conferisce un valore per la nuova colonna. Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include informazioni sulla data di nascita:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra. Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come le **progetto predefinito**.
3. Nel **Console di gestione pacchetti** finestra, seleziona **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna. Il `Up` metodo consente di creare la colonna quando si implementa la modifica e il `Down` metodo elimina la colonna quando si esegue il rollback della modifica.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compilare la soluzione e quindi immettere il comando seguente nel **Console di gestione pacchetti** finestra (assicurarsi che il progetto ContosoUniversity.DAL sia ancora selezionato):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework esegue il `Up` metodo e quindi esegue il `Seed` (metodo).

### <a name="display-the-new-column-in-the-instructors-page"></a>Visualizzare la nuova colonna nella pagina instructors (insegnanti)

1. Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita. Aggiungerlo tra quelle per l'assegnazione di office e data di assunzione:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Se rientro del codice viene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)
2. Eseguire l'applicazione, quindi scegliere il **instructors (insegnanti)** collegamento.

    Quando la pagina viene caricata, si noterà che che contiene il nuovo campo di data di nascita.

    ![Pagina instructors (insegnanti) con data di nascita](deploying-a-database-update/_static/image2.png)
3. Chiudere il browser.

### <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

1. Nelle **Esplora soluzioni** selezionare il progetto ContosoUniversity.
2. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**. (Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity **Esplora soluzioni**.)

    Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page.
3. Eseguire la **instructors (insegnanti)** pagina per verificare che l'aggiornamento è stato distribuito correttamente.

    Quando l'applicazione prova ad accedere al database per questa pagina, Code First Aggiorna lo schema del database ed esegue il `Seed` (metodo). Quando viene visualizzata la pagina, noterete che quella prevista **data di nascita** colonna delle date in esso.
4. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **Staging** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.
5. Eseguire la **instructors (insegnanti)** pagina in gestione temporanea per verificare che l'aggiornamento è stato distribuito correttamente.
6. Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.
7. Eseguire la **instructors (insegnanti)** pagina nell'ambiente di produzione per verificare che l'aggiornamento è stato distribuito correttamente.

    Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione in modalità offline durante la distribuzione usando *app\_offline.htm*, come illustrato nell'esercitazione precedente.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Distribuire un aggiornamento del database tramite il provider dbDacFx

In questa sezione aggiunge un *commenti* colonna il *utente* tabella nel database delle appartenenze e creare una pagina che consente di visualizzare e modificare i commenti per ogni utente. Quindi distribuire le modifiche al test, staging e produzione.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Aggiungere una colonna a una tabella nel database delle appartenenze

1. In Visual Studio, aprire **Esplora oggetti di SQL Server**.
2. Espandere **(localdb) \v11.0**, espandere **database**, espandere **aspnet-ContosoUniversity** (non **aspnet-ContosoUniversity-produzione**) e infine **tabelle**.

    Se non viene visualizzata **\v11.0 (localdb)** sotto il **SQL Server** nodo, fare doppio clic il **SQL Server** nodo e fare clic su **Aggiungi SQL Server**. Nel **Connetti al Server** finestra di dialogo immettere *\v11.0 (localdb)* come il **nome del Server**e quindi fare clic su **Connect**.

    Se non viene visualizzato **aspnet-ContosoUniversity**, eseguire il progetto e accedere con il *admin* credenziali (password viene *devpwd*) e quindi aggiornare il  **Esplora oggetti di SQL Server** finestra.
3. Fare doppio clic il **gli utenti** tabella e quindi fare clic su **Progettazione viste**.

    ![Progettazione viste SSOX](deploying-a-database-update/_static/image3.png)
4. Nella finestra di progettazione, aggiungere un *commenti* colonna e renderlo *nvarchar (128)* e ammette valori null, quindi fare clic su **Update**.

    ![Aggiunta di commenti colonna](deploying-a-database-update/_static/image4.png)
5. Nel **Anteprima aggiornamenti Database** fare clic su **Update Database**.

    ![Anteprima aggiornamenti Database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Creare una pagina per visualizzare e modificare la nuova colonna

1. In **Esplora soluzioni**, fare doppio clic sul **Account** cartella nel progetto ContosoUniversity, fare clic su **Add**, quindi fare clic su **nuovo elemento** .
2. Creare una nuova **della pagina Master utilizzando di Web Form** e denominarlo *UserInfo.aspx*. Accettare il valore predefinito *Site. master* file come pagina master.
3. Copiare il seguente markup nel `MainContent` `Content` elemento (l'ultimo di 3 `Content` elementi):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Fare doppio clic il *UserInfo.aspx* e fare clic su **Visualizza nel Browser**.
5. Accedi con il *admin* le credenziali dell'utente (password viene *devpwd*) e aggiungere alcuni commenti a un utente per verificare il corretto funzionamento della pagina.

    ![Pagina informazioni utente](deploying-a-database-update/_static/image6.png)
6. Chiudere il browser.

## <a name="deploy-the-database-update"></a>Distribuire l'aggiornamento del database

Per distribuire usando il provider dbDacFx, è sufficiente selezionare la **aggiornare il database** opzione nel profilo di pubblicazione. Tuttavia, per la distribuzione iniziale quando è stata utilizzata questa opzione inoltre configurato alcuni altri script SQL per eseguire: sono ancora nel profilo e sarà necessario impedirne l'esecuzione anche in questo caso.

1. Aprire il **pubblica sul Web** procedura guidata facendo clic sul progetto ContosoUniversity e facendo clic su **Publish**.
2. Selezionare il **Test** profilo.
3. Scegliere il **impostazioni** scheda.
4. Sotto **DefaultConnection**, selezionare **Aggiorna database**.
5. Disabilitare gli script aggiuntivi che è configurato per l'esecuzione per la distribuzione iniziale:

    1. Fare clic su **Configura Aggiornamenti database**.
    2. Nel **configurare gli aggiornamenti del Database** della finestra di dialogo deselezionare le caselle di controllo accanto a *Grant.sql* e *aspnet-data-dev.sql*.
    3. Fare clic su **Chiudi**.
6. Scegliere il **Preview** scheda.
7. Sotto **database** e a destra di **DefaultConnection**, fare clic sui **anteprima database** collegamento.

    ![Database (anteprima)](deploying-a-database-update/_static/image7.png)

    La finestra di anteprima mostra lo script che verrà eseguito nel database di destinazione per rendere tale schema del database corrispondono allo schema del database di origine. Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.
8. Chiudi il **Database (anteprima)** finestra di dialogo e quindi fare clic su **Publish**.

    Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page.
9. Eseguire la pagina della UserInfo (aggiungere *Account/UserInfo.aspx* a URL della home page) per verificare che l'aggiornamento è stato distribuito correttamente. È possibile accedere immettendo *admin* e *devpwd*.

    Per impostazione predefinita non vengono distribuiti i dati nelle tabelle e non sono state configurate uno script di distribuzione dei dati per l'esecuzione, in modo che non sono disponibili il commento aggiunto in fase di sviluppo. È possibile aggiungere un nuovo commento a questo punto nella gestione temporanea per verificare che la modifica è stata distribuita nel database e la pagina funziona correttamente.
10. Seguire la stessa procedura per distribuire in gestione temporanea e produzione.

    Non dimenticare di disabilitare gli script aggiuntivi. L'unica differenza rispetto al profilo di Test è che si disabiliterà un solo script nel processo di Staging e produzione profili perché sono stati configurati per l'esecuzione solo *aspnet-prod-data.sql*.

    Le credenziali per lo staging e produzione sono prodpwd e amministratore.

    Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database è in genere anche sarebbe portare offline l'applicazione durante la distribuzione caricando *app\_offline.htm* prima della pubblicazione e l'eliminazione in seguito, come illustrato nelle [nell'esercitazione precedente](deploying-a-code-update.md).

## <a name="summary"></a>Riepilogo

A questo punto è stata distribuita un aggiornamento dell'applicazione che una modifica al database tramite le migrazioni Code First e il provider dbDacFx incluso.

![Pagina instructors (insegnanti) con data di nascita](deploying-a-database-update/_static/image8.png)

![Pagina informazioni utente](deploying-a-database-update/_static/image9.png)

L'esercitazione successiva illustra come eseguire distribuzioni usando la riga di comando.

> [!div class="step-by-step"]
> [Precedente](deploying-a-code-update.md)
> [Successivo](command-line-deployment.md)
