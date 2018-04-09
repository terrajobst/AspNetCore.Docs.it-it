---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'Identità di ASP.NET: Uso di archiviazione di MySQL con un Provider di MySQL EntityFramework (c#) | Documenti Microsoft'
author: maumar
description: In questa esercitazione viene illustrato come sostituire il meccanismo di archiviazione di dati predefinito per ASP.NET Identity EntityFramework (provider SQL client) con un essere ampliati o ridotti MySQL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Uso di archiviazione di MySQL con un Provider di EntityFramework MySQL (c#)
====================
dal [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> In questa esercitazione viene illustrato come sostituire il meccanismo di archiviazione di dati predefinito per [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) con EntityFramework (provider SQL client) con un provider MySQL.


In questa esercitazione verranno trattati i seguenti argomenti:

- Creazione di un database MySQL su Azure
- Creazione di un'applicazione MVC utilizzando il modello MVC di Visual Studio 2013
- Configurazione EntityFramework per funzionare con un provider di database MySQL
- Esecuzione dell'applicazione per verificare i risultati

Al termine di questa esercitazione, è un'applicazione MVC con l'identità ASP.NET archiviare che utilizza un database MySQL in cui è ospitato in Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Creazione di un'istanza di database MySQL su Azure

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Fare clic su **NEW** nella parte inferiore della pagina e quindi selezionare **archivio**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Nel **scegliere e componente aggiuntivo** procedura guidata, selezionare **MySQL Database ClearDB**e quindi fare clic su di **successivo** freccia nella parte inferiore del riquadro:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenere il valore predefinito **libero** pianificare, modificare il **nome** a **IdentityMySQLDatabase**, selezionare l'area che è più vicino è e quindi fare clic su di **Avanti** freccia nella parte inferiore del riquadro:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Fare clic su di **acquisto** segno di spunta per completare la creazione del database.  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Dopo aver creato il database, è possibile gestirlo dal **componenti aggiuntivi** scheda nel portale di gestione. Per recuperare le informazioni di connessione per il database, fare clic su **le informazioni di connessione** nella parte inferiore della pagina:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copiare la stringa di connessione facendo clic sul pulsante Copia per il **CONNECTIONSTRING** campo e salvarlo, si utilizzerà queste informazioni più avanti in questa esercitazione per l'applicazione MVC:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Creazione di un progetto di applicazione MVC

Per completare i passaggi descritti in questa sezione dell'esercitazione, è necessario innanzitutto installare [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Dopo aver installato Visual Studio, utilizzare la procedura seguente per creare un nuovo progetto di applicazione MVC:

1. Aprire Visual Studio 2103.
2. Fare clic su **nuovo progetto** dal **avviare** pagina oppure è possibile fare clic su di **File** menu e quindi **nuovo progetto**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando il **nuovo progetto** è visualizzata la finestra di dialogo, espandere **Visual c#** nell'elenco dei modelli, fare clic su **Web**e selezionare **applicazione Web ASP.NET**. Denominare il progetto **IdentityMySQLDemo** e quindi fare clic su **OK**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **MVC** templatewith le opzioni predefinite; in questo modo configurare **singoli account utente di** come metodo di autenticazione. Fare clic su **OK**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurare EntityFramework per lavorare con un database MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aggiorna l'assembly di Entity Framework per il progetto

L'applicazione MVC che è stato creato dal modello di Visual Studio 2013 contiene un riferimento al [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) del pacchetto, ma è stato aggiornamenti a tale assembly dopo il rilascio contenenti significativi miglioramenti delle prestazioni. Per utilizzare questi ultimi aggiornamenti dell'applicazione, utilizzare la procedura seguente.

1. Aprire il progetto MVC in Visual Studio 2013.
2. Fare clic su **strumenti**, quindi fare clic su **Gestione pacchetti libreria**, quindi fare clic su **Package Manager Console**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Il **Package Manager Console** verranno visualizzati nella parte inferiore di Visual Studio. Tipo &quot; **pacchetto di aggiornamento EntityFramework** &quot; e premere INVIO:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installare il provider di MySQL per EntityFramework

Affinché EntityFramework per connettersi al database MySQL, è necessario installare un provider MySQL. A tale scopo, aprire il **Package Manager Console** e tipo &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, quindi premere INVIO.

> [!NOTE]
> Si tratta di una versione non definitiva dell'assembly e di conseguenza può contenere bug. Utilizzare una versione non definitiva del provider non nell'ambiente di produzione.


[Fare clic nell'immagine seguente per espandere il nodo].  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Apportare modifiche di configurazione di progetto nel file Web. config dell'applicazione

In questa sezione che si configurerà Entity Framework per utilizzare il provider di MySQL appena installata, registrare la factory del provider MySQL e aggiungere la stringa di connessione da Azure.

> [!NOTE]
> Nell'esempio seguente contengono una versione di assembly specifico per MySql.Data.dll. Se viene modificata la versione dell'assembly, è necessario modificare le impostazioni di configurazione appropriato con la versione corretta.


1. Aprire il file Web. config per il progetto in Visual Studio 2013.
2. Individuare le seguenti impostazioni di configurazione, che definiscono il provider di database predefinito e la factory per Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Sostituire le impostazioni di configurazione con il codice seguente, che verrà configurato per utilizzare il provider di MySQL Entity Framework: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Individuare il &lt;connectionStrings&gt; sezione e sostituirlo con il codice seguente, che verrà definita la stringa di connessione per il database di MySQL che è ospitato in Azure (si noti che il valore di providerName anche è stato modificato dal originale):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Aggiunta di contesto MigrationHistory personalizzato

Code First di Entity Framework Usa un **MigrationHistory** tabella per tenere traccia delle modifiche apportate al modello e per garantire la coerenza tra lo schema di database e un schema concettuale. Tuttavia, questa tabella non funziona per MySQL per impostazione predefinita perché la chiave primaria è troppo grande. Per risolvere questo problema, è necessario ridurre la dimensione della chiave per la tabella. A tale scopo, attenersi alla procedura seguente:

1. Le informazioni sullo schema per questa tabella viene acquisite un **HistoryContext**, che può essere modificato come qualsiasi altro **DbContext**. A tale scopo, aggiungere un nuovo file di classe denominato **MySqlHistoryContext.cs** al progetto e sostituirne il contenuto con il codice seguente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Successivamente sarà necessario configurare Entity Framework per l'utilizzo di modificati **HistoryContext**, anziché quella predefinita. Questa operazione può essere eseguita sfruttando le funzionalità di configurazione basato sul codice. A tale scopo, aggiungere nuovi file di classe denominato **MySqlConfiguration.cs** al progetto e sostituirne il contenuto con:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Creazione di un inizializzatore EntityFramework personalizzato per ApplicationDbContext

Il provider di MySQL è disponibile in questa esercitazione non supporta attualmente le migrazioni di Entity Framework, pertanto è necessario usare gli inizializzatori di modello per la connessione al database. In questa esercitazione Usa un'istanza di MySQL in Azure, è necessario creare un inizializzatore personalizzato in Entity Framework.

> [!NOTE]
> Questo passaggio non è obbligatorio se ci si connette a un'istanza di SQL Server in Azure o se si utilizza un database in cui è ospitato in locale.


Per creare un inizializzatore personalizzato in Entity Framework per MySQL, attenersi alla procedura seguente:

1. Aggiungere un nuovo file di classe denominato **MySqlInitializer.cs** è per il progetto, quindi sostituire il contenuto con il codice seguente: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Aprire il **IdentityModels.cs** file per il progetto, che si trova nella **modelli** directory e sostituire il contenuto con quanto segue: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Esecuzione dell'applicazione e verifica del database

Dopo aver completato i passaggi descritti nelle sezioni precedenti, è necessario testare il database. A tale scopo, attenersi alla procedura seguente:

1. Premere **Ctrl + F5** per compilare ed eseguire l'applicazione web.
2. Fare clic su di **registrare** scheda nella parte superiore della pagina:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. A questo punto in cui vengono create le tabelle di ASP.NET Identity nel Database di MySQL e l'utente è registrato e connesso all'applicazione:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installazione dello strumento di MySQL Workbench per verificare i dati

1. Installare il **MySQL Workbench** dello strumento la [pagina dei download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Nell'installazione guidata: **Selezione funzionalità** , selezionare **MySQL Workbench** in **applicazioni** sezione.
3. Avviare l'app e aggiungere una nuova connessione utilizzando i dati di stringa di connessione del database MySQL Azure creato al colmare di questa esercitazione.
4. Dopo avere stabilito la connessione, controllare la **ASP.NET Identity** le tabelle create nel **IdentityMySQLDatabase.**
5. Si noterà che tutte le identità di ASP.NET necessari le tabelle vengono create come illustrato nell'immagine riportata di seguito:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Controllare il **aspnetusers** per l'istanza di tabella per cercare le voci di registrazione di nuovi utenti.  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
