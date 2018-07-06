---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "ASP.NET Identity: Uso dell'archiviazione MySQL con un Provider MySQL EntityFramework (c#) | Microsoft Docs"
author: maumar
description: Questa esercitazione illustra come sostituire il meccanismo di archiviazione di dati predefinito per ASP.NET Identity con Entity Framework (provider di SQL client) con un essere ampliati o ridotti di MySQL...
ms.author: aspnetcontent
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 13cb1d9a862429bfb4e20a8d9700fc17eb721c6b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812675"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Uso dell'archiviazione MySQL con un Provider EntityFramework MySQL (c#)
====================
dal [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Questa esercitazione illustra come sostituire il meccanismo di archiviazione di dati predefinito per [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) con Entity Framework (provider di SQL client) con un provider di MySQL.


In questa esercitazione verranno trattati gli argomenti seguenti:

- Creazione di un database MySQL in Azure
- Creazione di un'applicazione MVC usando il modello MVC di Visual Studio 2013
- Configurazione di Entity Framework per lavorare con un provider di database MySQL
- Esecuzione dell'applicazione per verificare i risultati

Al termine di questa esercitazione, si avrà un'applicazione MVC con ASP.NET Identity archiviare che utilizza un database MySQL in cui è ospitato in Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Creazione di un'istanza di database MySQL in Azure

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Fare clic su **NEW** nella parte inferiore della pagina e quindi selezionare **STORE**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Nel **Choose e componente aggiuntivo** procedura guidata, selezionare **ClearDB MySQL Database**e quindi fare clic sui **Avanti** freccia nella parte inferiore del frame:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenere il valore predefinito **gratuito** pianificare, modificare il **nome** al **IdentityMySQLDatabase**, selezionare l'area più vicina all'utente e quindi fare clic su di **Avanti** freccia nella parte inferiore del frame:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Scegliere il **acquisto** segno di spunta per completare la creazione del database.  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Dopo aver creato il database, è possibile gestirlo dal **ONS** scheda nel portale di gestione. Per recuperare le informazioni di connessione per il database, fare clic su **le informazioni di connessione** nella parte inferiore della pagina:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copiare la stringa di connessione facendo clic sul pulsante Copia per il **CONNECTIONSTRING** campo e salvare il file, si userà queste informazioni più avanti in questa esercitazione per l'applicazione MVC:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Creazione di un progetto di applicazione MVC

Per completare la procedura descritta in questa sezione dell'esercitazione, è necessario innanzitutto installare [Visual Studio Express 2013 per il Web](https://go.microsoft.com/fwlink/?LinkId=299058) oppure [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Dopo aver installato Visual Studio, usare la procedura seguente per creare un nuovo progetto di applicazione MVC:

1. Aprire Visual Studio 2103.
2. Fare clic su **nuovo progetto** dal **avviare** pagina oppure è possibile fare clic sui **File** menu e quindi **nuovo progetto**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quando la **nuovo progetto** verrà visualizzata la finestra di dialogo, espandere **Visual c#** nell'elenco dei modelli, quindi fare clic su **Web**e selezionare **applicazione Web ASP.NET**. Denominare il progetto **IdentityMySQLDemo** e quindi fare clic su **OK**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **MVC** templatewith le opzioni predefinite; questo verrà configurare **account utente individuali** come metodo di autenticazione. Fare clic su **OK**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurare Entity Framework per lavorare con un database MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aggiornare l'assembly di Entity Framework per il progetto

L'applicazione MVC che è stato creato dal modello di Visual Studio 2013 contiene un riferimento al [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) del pacchetto, ma vi sono stati degli aggiornamenti a tale assembly fin dal suo rilascio contenenti significativo miglioramenti delle prestazioni. Per usare questi aggiornamenti più recenti nell'applicazione, usare la procedura seguente.

1. Aprire il progetto MVC in Visual Studio 2013.
2. Fare clic su **degli strumenti**, quindi fare clic su **Library Package Manager**, quindi fare clic su **Package Manager Console**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Il **Console di gestione pacchetti** verranno visualizzati nella parte inferiore di Visual Studio. Tipo di &quot; **Update-Package EntityFramework** &quot; e premere INVIO:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installare il provider di MySQL per Entity Framework

Affinché Entity Framework per connettersi al database MySQL, è necessario installare un provider di MySQL. A tale scopo, aprire il **Console di gestione pacchetti** e digitare &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, quindi premere INVIO.

> [!NOTE]
> Si tratta di una versione non definitiva dell'assembly e di conseguenza può contenere bug. Non si utilizzino una versione non definitiva del provider nell'ambiente di produzione.


[Fare clic sull'immagine seguente per espanderlo.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Apportare modifiche alla configurazione di progetto nel file Web. config per l'applicazione

In questa sezione che si configureranno di Entity Framework per utilizzare il provider di MySQL appena installata, registrare la factory del provider MySQL e aggiungere la stringa di connessione da Azure.

> [!NOTE]
> Negli esempi seguenti contengono una versione di assembly specifici per MySql.Data.dll. Se la versione dell'assembly viene modificato, è necessario modificare le impostazioni di configurazione appropriato con la versione corretta.


1. Aprire il file Web. config per il progetto in Visual Studio 2013.
2. Individuare le impostazioni di configurazione seguente, che definiscono il provider di database predefinito e la factory per Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Sostituire le impostazioni di configurazione con il codice seguente, che verrà configurato di Entity Framework per utilizzare il provider di MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Individuare il &lt;connectionStrings&gt; sezione e sostituirlo con il codice seguente, che consentiranno di definire la stringa di connessione del database MySQL ospitato in Azure (si noti che il valore di providerName anche è stato modificato dal originale):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Aggiunta rapida MigrationHistory personalizzato

Code First di Entity Framework Usa un **MigrationHistory** tabella per tenere traccia delle modifiche al modello e per garantire la coerenza tra lo schema di database e dello schema concettuale. Tuttavia, questa tabella non funziona per MySQL per impostazione predefinita perché la chiave primaria è troppo grande. Per risolvere questo problema, è necessario compattare le dimensioni della chiave per la tabella. A tale scopo, procedere come segue:

1. Le informazioni sullo schema per questa tabella viene acquisite un **HistoryContext**, che può essere modificato come qualsiasi altra **DbContext**. A tale scopo, aggiungere un nuovo file di classe denominato **MySqlHistoryContext.cs** al progetto e sostituirne il contenuto con il codice seguente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Successivamente sarà necessario configurare Entity Framework per l'uso di modificata **HistoryContext**, invece di quello predefinito. Questa operazione può essere eseguita sfruttando le funzionalità di configurazione basato sul codice. A tale scopo, aggiungere file di classe denominato **MySqlConfiguration.cs** al progetto e sostituirne il contenuto con:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Creazione di un inizializzatore personalizzato in EntityFramework per ApplicationDbContext

Il provider di MySQL è disponibile in questa esercitazione non supporta attualmente le migrazioni di Entity Framework, pertanto sarà necessario usare gli inizializzatori del modello per la connessione al database. Poiché questa esercitazione Usa un'istanza di MySQL in Azure, è necessario creare un inizializzatore personalizzato in Entity Framework.

> [!NOTE]
> Questo passaggio non è necessario se ci si connette a un'istanza di SQL Server in Azure o se si usa un database in cui è ospitato in locale.


Per creare un inizializzatore personalizzato in Entity Framework per MySQL, procedere come segue:

1. Aggiungere un nuovo file di classe denominato **MySqlInitializer.cs** è per il progetto e sostituire il contenuto con il codice seguente: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Aprire il **IdentityModels.cs** file per il progetto, che si trova nella **modelli** directory e sostituire il contenuto con quanto segue: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Esecuzione dell'applicazione e verifica del database

Dopo aver completato i passaggi nelle sezioni precedenti, è consigliabile testare il database. A tale scopo, procedere come segue:

1. Premere **Ctrl + F5** per compilare ed eseguire l'applicazione web.
2. Scegliere il **registrare** scheda nella parte superiore della pagina:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Immettere un nuovo nome utente e una password e quindi fare clic su **registrare**:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. A questo punto vengono create le tabelle di ASP.NET Identity nel Database di MySQL, e l'utente è registrato e connesso all'applicazione:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installazione dello strumento MySQL Workbench per verificare i dati

1. Installare il **MySQL Workbench** dello strumento di [pagina di download di MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Nell'installazione guidata: **selezione delle caratteristiche** scheda, seleziona **MySQL Workbench** sotto **applicazioni** sezione.
3. Avviare l'app e aggiungere una nuova connessione utilizzando i dati di stringa di connessione del database MySQL di Azure creato al colmare questa esercitazione.
4. Dopo avere stabilito la connessione, esaminare i **ASP.NET Identity** le tabelle create nel **IdentityMySQLDatabase.**
5. Si noterà che tutte le identità di ASP.NET richiesto le tabelle vengono create come illustrato nell'immagine seguente:  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Esaminare i **aspnetusers** ad esempio di tabella per cercare le voci di registrazione di nuovi utenti.  
  
   [Fare clic sull'immagine seguente per espanderlo. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
