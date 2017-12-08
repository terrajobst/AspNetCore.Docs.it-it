---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementazione di un Provider di archiviazione di ASP.NET Identity MySQL personalizzato | Documenti Microsoft
author: raquelsa
description: "Identità di ASP.NET è un sistema estendibile che consente di creare un provider di archiviazione e collegarlo all'applicazione senza utilizzare nuovamente il appli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 3bfbccd91705755fc24bb8305fff171baa26f370
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementazione di un Provider di archiviazione di ASP.NET Identity MySQL personalizzato
====================
da [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> Identità di ASP.NET è un sistema estendibile che consente di creare un provider di archiviazione e collegarlo all'applicazione senza utilizzare nuovamente l'applicazione. In questo argomento viene descritto come creare un provider di archiviazione di MySQL per l'identità ASP.NET. Per una panoramica della creazione di provider di archiviazione personalizzati, vedere [panoramica dei personalizzato provider di archiviazione per l'identità ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Per completare questa esercitazione, è necessario disporre di Visual Studio 2013 con Update 2.
> 
> In questa esercitazione verrà:
> 
> - Viene illustrato come creare un'istanza di database MySQL su Azure.
> - Viene illustrato come utilizzare uno strumento client di MySQL (MySQL Workbench) per creare tabelle e gestire il database remoto in Azure.
> - Viene illustrato come sostituire il valore predefinito di implementazione di archiviazione di ASP.NET Identity con l'implementazione personalizzata di un progetto di applicazione MVC.
> 
> In questa esercitazione è stato scritto originariamente dal Raquel Soares De Almeida e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Per l'identità 2.0 Suhas Joshi ha aggiornato il progetto di esempio. L'argomento è stato aggiornato per identità 2.0 da Tom FitzMacken.


## <a name="download-completed-project"></a>Download completato progetto

Al termine di questa esercitazione, si avrà un progetto di applicazione MVC con identità di ASP.NET utilizza un database MySQL ospitato in Azure.

È possibile scaricare il provider di archiviazione completato MySQL in [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>I passaggi che verranno eseguiti

In questa esercitazione sarà possibile:

1. Creare un database MySQL su Azure
2. Creare tabelle di ASP.NET Identity di MySQL
3. Creare un'applicazione MVC e configurarlo per utilizzare il provider di MySQL
4. Eseguire l'app

Questo argomento vengono illustrati l'architettura di ASP.NET Identity e le decisioni da prendere quando si implementa un provider di archiviazione del cliente. Per ulteriori informazioni, vedere [panoramica dell'archiviazione provider personalizzati per l'identità ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Esaminare le classi provider di archiviazione di MySQL

Prima di passare alla procedura per creare il provider di archiviazione di MySQL, esaminiamo le classi che costituiscono il provider di archiviazione. È necessario che le classi che gestiscono le operazioni di database e le classi che vengono chiamate dall'applicazione per gestire utenti e ruoli.

### <a name="storage-classes"></a>Classi di archiviazione

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -contiene le proprietà per l'utente.
- [Oggetto UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -include operazioni per l'aggiunta, aggiornamento o il recupero degli utenti.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -contiene le proprietà per i ruoli.
- [Oggetto RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -include operazioni per l'aggiunta, eliminazione, aggiornamento e il recupero dei ruoli.

### <a name="data-access-layer-classes"></a>Classi del livello di accesso ai dati

Per questo esempio, le classi di livello accesso dati contengono istruzioni SQL per l'utilizzo con le tabelle. Tuttavia, nel codice è consigliabile usare mapping relazionale a oggetti (ORM), ad esempio Entity Framework o NHibernate. In particolare, l'applicazione potrebbe influire negativamente sulle prestazioni senza ORM che include il caricamento lazy e la memorizzazione nella cache di oggetto. Per ulteriori informazioni, vedere [ASP.NET 2.0 identità senza Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -contiene i metodi per l'esecuzione di operazioni di database e la connessione al database MySQL. Oggetto UserStore e oggetto RoleStore sono entrambi creata un'istanza con un'istanza di questa classe.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -contiene operazioni di database per la tabella che contiene i ruoli.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -contiene operazioni di database per la tabella che contiene le attestazioni utente.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -contiene operazioni di database per la tabella che contiene informazioni sull'accesso utente.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -contiene operazioni di database per la tabella che contiene gli utenti che vengono assegnati a determinati ruoli.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -contiene operazioni di database per la tabella che contiene gli utenti.

## <a name="create-a-mysql-database-instance-on-azure"></a>Creare un'istanza di database MySQL su Azure

1. Accedere al [portale di Azure](https://manage.windowsazure.com/).
2. Fare clic su **+ nuovo** nella parte inferiore della pagina e quindi selezionare **archivio**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. Nel **scegliere e componente aggiuntivo** procedura guidata, selezionare **MySQL Database ClearDB** e fare clic sulla freccia avanti nella parte inferiore destra della finestra di dialogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenere il valore predefinito **libero** pianificare e modificare il **nome** a **IdentityMySQLDatabase**. Selezionare il paese più vicino e quindi fare clic sulla freccia avanti.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Fare clic sul segno di spunta per completare la creazione del database.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Dopo aver creato il database, è possibile gestirlo dal **componenti aggiuntivi** scheda nel portale di gestione.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. È possibile ottenere le informazioni di connessione di database facendo clic su **le informazioni di connessione** nella parte inferiore della pagina.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copiare la stringa di connessione facendo clic sul pulsante Copia e salvarla in modo che è possibile utilizzare in un secondo momento nell'applicazione MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Creare l'identità ASP.NET tabelle in un database MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installare MySQL Workbench strumento per connettersi e gestire database MySQL

1. Installare il **MySQL Workbench** dello strumento la [pagina di download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Avviare l'app e aggiungere fare clic su di **MySQLConnections +** pulsante per aggiungere una nuova connessione. Utilizzare i dati di stringa di connessione che è copiati dal database di MySQL di Azure creata precedentemente in questa esercitazione.
3. Dopo avere stabilito la connessione, aprire una nuova **Query** scheda; incollare i comandi da [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) nella query ed eseguirla per creare le tabelle di database.
4. È ora tutte le identità di ASP.NET necessarie tabelle create in un database MySQL ospitato in Azure, come illustrato di seguito.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Creare un progetto di applicazione MVC dal modello e configurarlo per utilizzare il provider di MySQL

Se necessario, installare uno [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Scaricare il progetto ASP.NET.Identity.MySQL da CodePlex

1. Passare all'URL del repository in [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Scaricare il codice sorgente.
3. Estrarre il file con estensione zip in una cartella locale.
4. Aprire la soluzione AspNet.Identity.MySQL e compilare la soluzione.

### <a name="create-a-new-mvc-application-project-from-template"></a>Creare un nuovo progetto di applicazione MVC da modello

1. Fare clic destro la **AspNet.Identity.MySQL** soluzione e **Aggiungi**, **nuovo progetto**
2. Nel **Aggiungi nuovo progetto** selezionare finestra di dialogo **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**. Denominare il progetto **IdentityMySQLDemo**; e quindi fare clic su OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. Nel **nuovo progetto ASP.NET** finestra di dialogo, selezionare il modello MVC con le opzioni predefinite (che include **singoli account utente di** come metodo di autenticazione) e fare clic su **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. In Esplora soluzioni fare doppio clic su progetto IdentityMySQLDemo e selezionare **Gestisci pacchetti NuGet**. Nella finestra di dialogo di testo di ricerca, digitare **Identity.EntityFramework**. Selezionare il pacchetto nell'elenco dei risultati e fare clic su **Disinstalla**. Verrà richiesto di disinstallare il pacchetto della dipendenza EntityFramework. Fare clic su Sì come indicato non è più il pacchetto in questa applicazione.
5. Fare clic con il pulsante destro del progetto IdentityMySQLDemo, selezionare **Aggiungi**, **riferimento, soluzioni, progetti;** selezionare il progetto AspNet.Identity.MySQL e fare clic su **OK**.
6. Nel progetto IdentityMySQLDemo, sostituire tutti i riferimenti a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
 con  
     `using AspNet.Identity.MySQL;`
7. In IdentityModels.cs, impostare **ApplicationDbContext** da cui derivare **MySqlDatabase** e includere un costruttore che accetta un parametro singolo con il nome della connessione.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Aprire il file IdentityConfig.cs. Nel **ApplicationUserManager.Create** (metodo), replace creazione UserManager con il codice seguente:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Aprire il file Web. config e sostituire la stringa DefaultConnection con questa voce sostituendo i valori evidenziati con la stringa di connessione del database MySQL che è stato creato in precedenza:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Eseguire l'app e connettersi al database MySQL

1. Fare clic destro la **IdentityMySQLDemo** del progetto e selezionare **imposta come progetto di avvio**
2. Premere **Ctrl + F5** per compilare ed eseguire l'app.
3. Fare clic su **registrare** scheda nella parte superiore della pagina.
4. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Il nuovo utente è ora registrato e connesso.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Tornare allo strumento MySQL Workbench ed esaminare il **IdentityMySQLDatabase** contenuto della tabella. Esaminare la tabella di utenti per le voci di registrazione di nuovi utenti.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Passaggi successivi

Per ulteriori informazioni su come abilitare altri metodi di autenticazione in questa app, fare riferimento a [creare un'App di ASP.NET MVC 5 con Facebook, Google OAuth2 e OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Per informazioni su come integrare i database con OAuth e per configurare i ruoli per limitare l'accesso agli utenti per l'app, vedere [distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
