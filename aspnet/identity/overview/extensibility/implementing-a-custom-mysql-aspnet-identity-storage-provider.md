---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementazione di un Provider di archiviazione MySQL personalizzato ASP.NET Identity | Microsoft Docs
author: raquelsa
description: ASP.NET Identity è un sistema estendibile che consente di creare un proprio provider di archiviazione e di inserirli direttamente nell'applicazione senza utilizzare nuovamente il appli...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 358b1a3b7277f21c63a1d395f2a5fce79bbe0d56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832789"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementazione di un Provider di archiviazione MySQL personalizzato ASP.NET Identity
====================
dal [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity è un sistema estendibile che consente di creare un proprio provider di archiviazione e di inserirli direttamente nell'applicazione senza utilizzare nuovamente l'applicazione. Questo argomento descrive come creare un provider di archiviazione MySQL per ASP.NET Identity. Per una panoramica della creazione di provider di archiviazione personalizzati, vedere [Panoramica di personalizzato provider di archiviazione per ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Per completare questa esercitazione, è necessario disporre di Visual Studio 2013 con Update 2.
> 
> Questa esercitazione illustra come:
> 
> - Mostra come creare un'istanza di database MySQL in Azure.
> - Mostra come usare uno strumento client MySQL (MySQL Workbench) per creare tabelle e gestire il database remoto in Azure.
> - Mostra come sostituire il valore predefinito di implementazione dell'archiviazione di ASP.NET Identity con l'implementazione personalizzata in un progetto di applicazione MVC.
> 
> Questa esercitazione è stato scritto originariamente Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Il progetto di esempio è stato aggiornato per identità 2.0 da Suhas Joshi. L'argomento è stato aggiornato per identità 2.0 da Tom FitzMacken.


## <a name="download-completed-project"></a>Download completato progetto

Al termine di questa esercitazione, sarà necessario un progetto di applicazione MVC con ASP.NET Identity con un database MySQL ospitato in Azure.

È possibile scaricare il provider di archiviazione MySQL completato a [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>I passaggi da eseguire

In questa esercitazione si apprenderà come:

1. Creare un database MySQL in Azure
2. Creare l'identità di ASP.NET le tabelle presenti in MySQL
3. Creare un'applicazione MVC e configurarlo per usare il provider di MySQL
4. Eseguire l'app

Questo argomento non viene illustrata l'architettura di ASP.NET Identity e le decisioni da prendere quando si implementa un provider di archiviazione del cliente. Per ulteriori informazioni, vedere [Panoramica di archiviazione provider personalizzati per ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Esaminare le classi del provider di archiviazione MySQL

Prima di approfondire i passaggi per creare il provider di archiviazione MySQL, esaminiamo le classi che costituiscono il provider di archiviazione. È necessario le classi che gestiscono le operazioni di database e che vengono chiamati dall'applicazione per gestire utenti e ruoli.

### <a name="storage-classes"></a>Classi di archiviazione

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contiene le proprietà per l'utente.
- [Oggetto UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -include operazioni per l'aggiunta, aggiornamento o il recupero degli utenti.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contiene le proprietà per i ruoli.
- [Oggetto RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -contiene le operazioni di aggiunta, eliminazione, aggiornamento e recupero di ruoli.

### <a name="data-access-layer-classes"></a>Classi del livello accesso dati

Per questo esempio, le classi del livello accesso dati contengono le istruzioni SQL per l'utilizzo con le tabelle. Tuttavia, nel codice è possibile utilizzare object relational mapping (ORM), ad esempio Entity Framework o NHibernate. In particolare, l'applicazione verifichi una riduzione delle prestazioni senza un ORM che include il caricamento lazy e la memorizzazione nella cache di oggetti. Per altre informazioni, vedere [ASP.NET Identity 2.0 senza Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contiene la connessione al database MySQL e i metodi per l'esecuzione di operazioni di database. Oggetto UserStore e oggetto RoleStore sono entrambi creata un'istanza con un'istanza di questa classe.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contiene operazioni di database per la tabella che contiene i ruoli.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contiene operazioni di database per la tabella che contiene le attestazioni utente.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contiene operazioni di database per la tabella che archivia le informazioni di accesso utente.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -contiene operazioni di database per la tabella che contiene gli utenti che vengono assegnati a quali ruoli.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -contiene operazioni di database per la tabella che contiene gli utenti.

## <a name="create-a-mysql-database-instance-on-azure"></a>Creare un'istanza di database MySQL in Azure

1. Accedere al [portale di Azure](https://manage.windowsazure.com/).
2. Fare clic su **+ nuovo** nella parte inferiore della pagina e quindi selezionare **STORE**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Nel **Choose e componente aggiuntivo** procedura guidata, selezionare **ClearDB MySQL Database** e fare clic sulla freccia avanti nella parte inferiore destra della finestra di dialogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenere il valore predefinito **gratuito** pianificare e modificare le **Name** a **IdentityMySQLDatabase**. Selezionare l'area più vicina e quindi fare clic sulla freccia avanti.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Fare clic sul segno di spunta per completare la creazione del database.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Dopo aver creato il database, è possibile gestirlo dal **ONS** scheda nel portale di gestione.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. È possibile ottenere le informazioni di connessione del database facendo clic su **le informazioni di connessione** nella parte inferiore della pagina.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copiare la stringa di connessione facendo clic sul pulsante di copia e salvarlo in modo che è possibile usare in un secondo momento nell'applicazione MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Creare l'identità di ASP.NET le tabelle in un database MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installare lo strumento MySQL Workbench per connettersi e gestire database MySQL

1. Installare il **MySQL Workbench** dello strumento di [pagina di download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Avviare l'app e aggiungere fare clic sui **MySQLConnections +** pulsante per aggiungere una nuova connessione. Usare i dati di stringa di connessione copiata dal database MySQL di Azure creata in precedenza in questa esercitazione.
3. Dopo avere stabilito la connessione, aprire una nuova **Query** scheda, incollare i comandi dal [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) nella query ed eseguirlo per creare le tabelle del database.
4. Hai ora tutte le identità ASP.NET necessarie tabelle create in un database MySQL ospitato in Azure, come illustrato di seguito.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Creare un progetto di applicazione MVC da modello e configurarlo per usare il provider di MySQL

Se necessario, installare [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Scaricare il progetto ASP.NET.Identity.MySQL da CodePlex

1. Passare all'URL del repository al [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Scaricare il codice sorgente.
3. Estrarre il file con estensione zip in una cartella locale.
4. Aprire la soluzione AspNet.Identity.MySQL e compilare la soluzione.

### <a name="create-a-new-mvc-application-project-from-template"></a>Creare un nuovo progetto di applicazione MVC da modello

1. Fare clic il **AspNet.Identity.MySQL** soluzione e **Add**, **nuovo progetto**
2. Nel **Aggiungi nuovo progetto** selezione finestra di dialogo **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto **IdentityMySQLDemo**; e quindi fare clic su OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Nel **nuovo progetto ASP.NET** finestra di dialogo, selezionare il modello MVC con le opzioni predefinite (che include **account utente individuali** come metodo di autenticazione) e fare clic su **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. In Esplora soluzioni fare doppio clic su progetto IdentityMySQLDemo e selezionare **Gestisci pacchetti NuGet**. Nella finestra casella di testo di ricerca, digitare **Identity.EntityFramework**. Selezionare il pacchetto nell'elenco dei risultati e fare clic su **Disinstalla**. Verrà richiesto di disinstallare il pacchetto di dipendenza EntityFramework. Fare clic su Sì perché è non saranno più in questo pacchetto su questa applicazione.
5. Fare clic con il pulsante destro del progetto IdentityMySQLDemo, selezionare **Add**, **riferimento, soluzioni, progetti;** selezionare il progetto AspNet.Identity.MySQL e fare clic su **OK**.
6. Nel progetto IdentityMySQLDemo, sostituire tutti i riferimenti a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   con  
     `using AspNet.Identity.MySQL;`
7. In IdentityModels.cs, impostare **ApplicationDbContext** da cui derivare **MySqlDatabase** e includere un costruttore che accettano un parametro singolo con il nome della connessione.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Aprire il file IdentityConfig.cs. Nel **ApplicationUserManager.Create** (metodo), sostituzione, creazione di un'istanza UserManager con il codice seguente:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Aprire il file Web. config e sostituire la stringa DefaultConnection con questa voce sostituendo i valori evidenziati con la stringa di connessione del database MySQL creato nei passaggi precedenti:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Eseguire l'app e connettersi al database MySQL

1. Fare clic il **IdentityMySQLDemo** del progetto e selezionare **imposta come progetto di avvio**
2. Premere **Ctrl + F5** per compilare ed eseguire l'app.
3. Fare clic su **registrare** scheda nella parte superiore della pagina.
4. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Il nuovo utente è ora registrato e connesso.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Tornare allo strumento MySQL Workbench ed esaminare i **IdentityMySQLDatabase** contenuti della tabella. Esaminare ogni tabella gli utenti per le voci di registrazione di nuovi utenti.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come abilitare altri metodi di autenticazione in questa app, fare riferimento a [creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 Sign-on OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Per informazioni su come integrare il database con OAuth e configurare ruoli per limitare l'accesso degli utenti all'App, vedere [distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
