---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: La migrazione dei Provider Universal dati per l'appartenenza e i profili di ASP.NET Identity (c#) | Documenti Microsoft
author: rustd
description: In questa esercitazione vengono descritti i passaggi necessari eseguire la migrazione di utenti e dei dati dei ruoli e i dati di profilo utente creati utilizzando Universal Providers di un'app esistente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f65f93b20543d06ea70a9009b6921e297477c99e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>La migrazione dei Provider Universal dati per l'appartenenza e i profili di ASP.NET Identity (c#)
====================
dal [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> In questa esercitazione vengono descritti i passaggi necessari eseguire la migrazione di utenti e dei dati dei ruoli e i dati di profilo utente creati utilizzando Universal Providers di un'applicazione esistente per il modello di identità di ASP.NET. Per eseguire la migrazione di dati del profilo utente da utilizzare in un'applicazione con nonché appartenenze SQL, l'approccio sopra indicate.


Con la versione di Visual Studio 2013, il team di ASP.NET ha introdotto un nuovo sistema di identità di ASP.NET e altre informazioni su tale versione [qui](../../index.md). Il completamento di articolo per eseguire la migrazione di applicazioni web dalla [appartenenza SQL nel nuovo sistema di identità](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), in questo articolo vengono illustrati i passaggi per eseguire la migrazione delle applicazioni esistenti che seguono il modello di provider per la gestione di utenti e ruoli per il nuovo modello di identità. L'obiettivo di questa esercitazione verranno principalmente la migrazione dei dati di profilo utente per facilmente agganciarla nel nuovo sistema. Migrazione delle informazioni utente e il ruolo è simile per l'appartenenza SQL. In un'applicazione con nonché appartenenze SQL, è possibile utilizzare l'approccio seguito per eseguire la migrazione di dati di profilo.

Ad esempio, si inizierà con un'app web creata utilizzando Visual Studio 2012 che usa il modello di provider. Si verrà quindi aggiungere il codice per la gestione dei profili, registrazione di un utente, aggiungere dati di profilo per gli utenti, eseguire la migrazione dello schema del database e quindi modificare l'applicazione di utilizzare il sistema di identità per gestione di utenti e ruoli. Come un test della migrazione, gli utenti creati utilizzando i provider universali devono essere in grado di accedere e nuovi utenti devono essere in grado di registrare.

> [!NOTE]
> È possibile trovare l'esempio completo alla [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Riepilogo di migrazione dei dati di profilo

Prima di iniziare la migrazione, esaminiamo l'esperienza di archiviazione di dati di profilo nel modello di provider. Dati di profilo per l'applicazione gli utenti possono essere archiviati in più modi, i più comuni tra di essi viene tramite i provider di profili incorporato forniti con i provider Universal. Includono i passaggi

1. Aggiungere una classe che ha le proprietà utilizzate per archiviare i dati di profilo.
2. Aggiungere una classe che estende "ProfileBase" e implementa i metodi per ottenere i dati del profilo precedente per l'utente.
3. Consentire l'utilizzo di provider di profili predefiniti nel *Web. config* file e definire la classe dichiarata nel passaggio 2 # da utilizzare per l'accesso a informazioni sul profilo.

Le informazioni sul profilo viene memorizzati come codice xml serializzato e dati binari nella tabella 'Profili' nel database.

Dopo la migrazione dell'applicazione per utilizzare il nuovo sistema di identità di ASP.NET, le informazioni sul profilo viene deserializzati e archiviati come proprietà della classe utente. Ogni proprietà possono essere mappate colonne della tabella utente. Il vantaggio è che le proprietà possono essere svolte direttamente tramite la classe utente oltre a disporre di non serializzare o deserializzare le informazioni dati tempo quando si accede a tale colonna.

## <a name="getting-started"></a>Introduzione

1. Creare una nuova applicazione Web Form ASP.NET 4.5 in Visual Studio 2012. L'esempio corrente Usa il modello Web Form, ma è possibile utilizzare anche applicazione MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Creare una nuova cartella 'Modelli' per archiviare le informazioni sul profilo  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Ad esempio, segnalare il problema di archiviare la data di nascita, città, altezza e il peso dell'utente nel profilo. L'altezza e peso vengono archiviati come una classe personalizzata denominata 'PersonalStats'. Per archiviare e recuperare il profilo, è necessaria una classe che estende 'ProfileBase'. È possibile crearne una nuova classe 'AppProfile' per recuperare e archiviare le informazioni sul profilo.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Abilitare il profilo nel *Web. config* file. Immettere il nome della classe da utilizzare per archiviare/recuperare le informazioni utente create nel passaggio 3 #.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Aggiungere una pagina web form nella cartella 'Account' per ottenere i dati del profilo dell'utente e archiviarlo. Fare clic con il pulsante destro sul progetto e selezionare "Aggiungi nuovo elemento". Aggiungere una nuova pagina Web Form con pagina master 'AddProfileData.aspx'. Copiare quanto segue nella sezione 'MainContent':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Aggiungere il codice seguente nel code-behind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Aggiungere lo spazio dei nomi in cui AppProfile classe è definita per rimuovere gli errori di compilazione.
6. Eseguire l'app e creare un nuovo utente con il nome utente '**olduser'.** Passare alla pagina 'AddProfileData' e aggiungere informazioni sul profilo per l'utente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

È possibile verificare che i dati vengono archiviati come xml serializzato nella tabella 'Profili' utilizzando la finestra di Esplora Server. In Visual Studio, scegliere dal menu 'Visualizza', 'Esplora Server'. Deve esistere una connessione dati per il database definito nel *Web. config* file. Fare clic sulla connessione dati mostra diverse sottocategorie. Espandere 'Tabelle' per mostrare le diverse tabelle del database, quindi fare clic con il pulsante destro su "Profili" e scegliere "Mostra i dati di tabella' per visualizzare i dati di profilo archiviati nella tabella dei profili.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>La migrazione di schemi di database

Per correggere il database esistente con il sistema di identità, è necessario aggiornare lo schema del database di identità per supportare i campi che sono aggiunte al database originale. Questa operazione può essere eseguita tramite gli script SQL per creare nuove tabelle e copiare le informazioni esistenti. In '' Esplora Server, espandere 'DefaultConnection' per visualizzare le tabelle. Fare clic con il pulsante destro tabelle e selezionare 'Nuova Query'

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Incollare lo script SQL dalla [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ed eseguirlo. Se la stringa DefaultConnection viene aggiornata, si noterà che vengono aggiunte le nuove tabelle. È possibile controllare i dati all'interno di tabelle per verificare che le informazioni sono stata eseguita la migrazione.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>La migrazione dell'applicazione per utilizzare ASP.NET Identity

1. Installare i pacchetti Nuget necessari per l'identità ASP.NET:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Sono disponibili ulteriori informazioni sulla gestione dei pacchetti Nuget [qui](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Per utilizzare i dati esistenti nella tabella, è necessario creare le classi modello che eseguire nuovamente il mapping di tabelle e associare il sistema di identità. Come parte del contratto di identità, le classi di modello devono implementare le interfacce definite nella dll Identity.Core o estendono l'implementazione di queste interfacce disponibili in EntityFramework esistente. Si utilizzerà le classi esistenti per ruolo, gli account di accesso utente e le attestazioni utente. È necessario usare un utente personalizzato per l'esempio. Fare clic con il pulsante destro sul progetto e creare una nuova cartella 'IdentityModels'. Aggiungere una nuova classe di 'User', come illustrato di seguito:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Si noti che 'ProfileInfo' è una proprietà sulla classe utente. Di conseguenza è possibile usare la classe dell'utente di utilizzare direttamente dati di profilo.

Copiare i file nel **IdentityModels** e **IdentityAccount** cartelle dall'origine del download ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Queste sono le altre classi di modello e le nuove pagine necessarie per l'utente e la gestione dei ruoli usando le API di identità di ASP.NET. L'approccio utilizzato è simile all'appartenenza SQL ed è disponibile una descrizione dettagliata [qui](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

## <a name="copying-profile-data-to-the-new-tables"></a>Copia dei dati di profilo per le nuove tabelle

Come accennato in precedenza, è necessario deserializzare i dati xml nelle tabelle di profili e archiviarlo in colonne della tabella AspNetUsers. Le nuove colonne create nella tabella gli utenti nel passaggio precedente in modo che tutto ciò che resta popolare le colonne con i dati necessari. A tale scopo, si utilizzerà un'applicazione console che viene eseguita una volta per popolare le colonne della tabella utente appena create.

1. Nella soluzione esistente, creare una nuova applicazione console.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installare la versione più recente del pacchetto di Entity Framework.
3. Aggiungere l'applicazione web creato in precedenza come riferimento per l'applicazione console. Per eseguire questo fare clic sul progetto, quindi 'Aggiungi riferimenti', quindi soluzione, quindi fare clic sul progetto e fare clic su OK.
4. Copia il seguente codice nella classe Program.cs. Questa logica legge i dati di profilo per ogni utente, serializza come 'ProfileInfo' oggetto e lo archivia nel database.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Alcuni dei modelli utilizzati vengono definiti nella cartella 'IdentityModels' del progetto di applicazione web, è necessario includere gli spazi dei nomi corrispondente.
5. Il codice sopra riportato funziona sul file di database nell'App\_cartella dati del progetto di applicazione web creata nei passaggi precedenti. Per fare riferimento a tale, aggiornare la stringa di connessione nel file app. config dell'applicazione console con stringa di connessione in Web. config dell'applicazione web. Forniscono anche il percorso fisico completo nella proprietà 'AttachDbFilename'.
6. Aprire un prompt dei comandi e passare alla cartella bin dell'applicazione console precedente. Eseguire il file eseguibile ed esaminare l'output del log, come illustrato nella figura seguente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Aprire la tabella 'AspNetUsers' in Esplora Server e verificare i dati nelle nuove colonne che contengono le proprietà. Cui devono essere aggiornati con i valori di proprietà corrispondenti.

## <a name="verify-functionality"></a>Verificare le funzionalità di

Utilizzare le pagine di appartenenze appena aggiunto che vengono implementate mediante ASP.NET Identity eseguire l'accesso a un utente dal database precedente. L'utente deve essere in grado di accedere con le stesse credenziali. Provare altre funzionalità quali l'aggiunta di OAuth, creando un nuovo utente, la modifica di una password, aggiunta di ruoli, aggiungere utenti a ruoli e così via.

I dati di profilo per l'utente precedente e i nuovi utenti devono essere recuperati e archiviati nella tabella gli utenti. La vecchia tabella non deve fare riferimento.

## <a name="conclusion"></a>Conclusione

L'articolo viene descritto il processo di migrazione di applicazioni web che utilizzano il modello di provider di appartenenze di ASP.NET Identity. L'articolo viene descritto inoltre la migrazione dei dati di profilo per gli utenti di eseguire l'hook il sistema di identità. Lasciare commenti di seguito per domande e problemi riscontrati quando si esegue la migrazione dell'app.

*Ringraziamenti Rick Anderson e Robert McMurray per la verifica dell'articolo.*
