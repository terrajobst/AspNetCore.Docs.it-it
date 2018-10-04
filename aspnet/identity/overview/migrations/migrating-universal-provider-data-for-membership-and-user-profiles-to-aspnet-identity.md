---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: La migrazione di dati del Provider universali per appartenenza e i profili utente mobili ad ASP.NET Identity (c#) | Microsoft Docs
author: rustd
description: Questa esercitazione vengono descritti i passaggi necessari eseguire la migrazione di utenti e i dati del ruolo e i dati di profilo utente creati con i provider universali di un'app esistente...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a91bb6ac51819d7dbb8eb3c63bd36a9d830eecce
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578226"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>La migrazione dei dati del Provider universali per appartenenza e i profili utente mobili ad ASP.NET Identity (c#)
====================
dal [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Questa esercitazione vengono descritti i passaggi necessari eseguire la migrazione di utenti e i dati del ruolo e dati del profilo utente creati con i provider universali di un'applicazione esistente per il modello di identità di ASP.NET. L'approccio sopra indicate per eseguire la migrazione di dati del profilo utente possono essere utilizzate in un'applicazione con appartenenza SQL anche.


Con la versione di Visual Studio 2013, il team ASP.NET ha introdotto un nuovo sistema di identità di ASP.NET e altre informazioni sulla versione [qui](../../index.md). Dopo l'analisi dell'articolo per eseguire la migrazione di applicazioni web dai [appartenenza SQL nel nuovo sistema di identità](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), questo articolo illustra i passaggi per eseguire la migrazione di applicazioni esistenti che seguono il modello di provider per la gestione di utenti e ruoli il nuovo modello di identità. L'obiettivo di questa esercitazione sarà principalmente sulla migrazione di dati del profilo utente per associarlo in modo trasparente nel nuovo sistema. Migrazione delle informazioni utente e il ruolo è simile per l'appartenenza SQL. L'approccio seguito per eseguire la migrazione dei dati di profilo può essere utilizzato in un'applicazione con anche l'appartenenza SQL.

Ad esempio, si inizierà con un'app web creata utilizzando Visual Studio 2012 che viene utilizzato il modello di provider. Si verrà quindi aggiungere codice per la gestione dei profili, registrazione di un utente, aggiungere dati del profilo per gli utenti, eseguire la migrazione dello schema del database e modificare l'applicazione a usare il sistema di identità per la gestione di utenti e ruoli. Come un test della migrazione, gli utenti creati tramite Universal Providers dovrebbero essere possibile accedere e devono essere in grado di registrare i nuovi utenti.

> [!NOTE]
> È possibile trovare l'esempio completo alla [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Riepilogo della migrazione dei dati del profilo

Prima di iniziare con le migrazioni, esaminiamo l'esperienza di archiviazione dei dati del profilo nel modello di provider. Profilo dati per applicazione gli utenti possono essere archiviati in diversi modi, i più comuni tra di essi in corso usando provider di profili incorporato fornito con i provider universali. Includono i passaggi

1. Aggiungere una classe che ha le proprietà utilizzate per archiviare i dati del profilo.
2. Aggiungere una classe che estende 'ProfileBase' e implementa i metodi per ottenere i dati del profilo precedente per l'utente.
3. Consentire l'uso di provider di profili predefiniti in di *Web. config* file e definire la classe dichiarata nel passaggio #2 da utilizzare per l'accesso a informazioni sul profilo.

Le informazioni sul profilo vengono archiviati come dati binari nella tabella "Profiles" nel database e xml serializzato.

Dopo la migrazione dell'applicazione per usare il nuovo sistema di identità di ASP.NET, le informazioni sul profilo viene deserializzati e archiviati come proprietà della classe utente. Ogni proprietà possono quindi essere mappate in colonne nella tabella utente. Il vantaggio è che le proprietà possono essere svolte direttamente usando la classe dell'utente oltre a disporre di non serializzare o deserializzare informazioni dati ogni ora durante l'accesso.

## <a name="getting-started"></a>Introduzione

1. Creare una nuova applicazione Web Form ASP.NET 4.5 in Visual Studio 2012. L'esempio corrente Usa il modello Web Form, ma è possibile usare anche applicazione MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Creare una nuova cartella 'Modelli' per archiviare le informazioni sul profilo  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Ad esempio, invia i tuoi commenti archiviare la data di nascita, città, altezza e spessore dell'utente nel profilo. L'altezza e spessore vengono archiviati come una classe personalizzata denominata 'PersonalStats'. Per archiviare e recuperare il profilo, è necessaria una classe che estende 'ProfileBase'. È possibile creare una nuova classe 'AppProfile' per ottenere e archiviare le informazioni del profilo.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Abilitare il profilo nel *Web. config* file. Immettere il nome della classe da utilizzare per archiviare/recuperare le informazioni utente create nel passaggio #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Aggiungere una pagina web form nella cartella "Account" per ottenere i dati del profilo da parte dell'utente e archiviarli. Fare clic con il pulsante destro sul progetto e selezionare "Aggiungi nuovo elemento". Aggiungere una nuova pagina Web Form con pagina master 'AddProfileData.aspx'. Copiare quanto segue nella sezione 'MainContent':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Aggiungere il codice seguente nel code-behind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Aggiungere lo spazio dei nomi in cui AppProfile classe è definita per rimuovere gli errori di compilazione.
6. Eseguire l'app e creare un nuovo utente con il nome utente '**olduser'.** Passare alla pagina 'AddProfileData' e aggiungere informazioni sul profilo dell'utente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

È possibile verificare che i dati vengono archiviati come codice xml serializzato nella tabella "Profiles" utilizzando la finestra Esplora Server. In Visual Studio, scegliere dal menu 'View', 'Esplora Server'. Deve essere presente una connessione dati per il database definito nel *Web. config* file. Fare clic sulla connessione dati per visualizzare diverse sottocategorie. Espandere 'Tabelle' per mostrare le diverse tabelle nel database, quindi fare clic con il pulsante destro su "Profiles" e scegliere "Mostra dati tabella' per visualizzare i dati di profilo archiviati nella tabella di profili.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>La migrazione di schemi di database

Per correggere il database esistente con il sistema di identità, è necessario aggiornare lo schema nel database di identità per supportare i campi che sono aggiunte al database originale. Questa operazione può essere eseguita con gli script SQL per creare nuove tabelle e copiare le informazioni esistenti. Nella finestra 'Server Explorer' espandere 'DefaultConnection' per visualizzare le tabelle. Fare clic con il pulsante destro tabelle e selezionare 'Nuova Query'

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Incollare lo script SQL dalla [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ed eseguirlo. Se la stringa 'DefaultConnection' vengono aggiornate, noteremo che vengono aggiunte le nuove tabelle. È possibile controllare i dati all'interno di tabelle per verificare che le informazioni sono stata eseguita la migrazione.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>La migrazione dell'applicazione per usare ASP.NET Identity

1. Installare i pacchetti Nuget necessari per ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Altre informazioni sulla gestione dei pacchetti Nuget sono reperibili [qui](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Per lavorare con i dati esistenti nella tabella, è necessario creare le classi modello che eseguire il mapping a tabelle e agganciare il sistema di identità. Come parte del contratto dell'identità, le classi del modello devono implementare le interfacce definite nella dll Identity.Core o estendono l'implementazione esistente di queste interfacce disponibili in EntityFramework. Si useranno le classi esistenti per ruolo, gli accessi utente e le attestazioni utente. È necessario usare un utente personalizzato per questo esempio. Fare clic con il pulsante destro sul progetto e creare una nuova cartella 'IdentityModels'. Aggiungere una nuova classe di 'User', come illustrato di seguito:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Si noti che il 'ProfileInfo' a questo punto è una proprietà nella classe utente. Di conseguenza è possibile usare la classe dell'utente per lavorare direttamente con i dati del profilo.

Copiare i file nei **IdentityModels** e **IdentityAccount** cartelle dall'origine dei download ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Queste sono le altre classi di modello e le nuove pagine necessari per l'utente e la gestione dei ruoli usando le API di identità di ASP.NET. L'approccio usato è simile all'appartenenza SQL e una descrizione dettagliata è reperibile [qui](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copia dei dati di profilo per le nuove tabelle

Come accennato in precedenza, è necessario deserializzare i dati xml nelle tabelle di profili e archiviarlo in colonne della tabella AspNetUsers. Nella tabella di utenti nel passaggio precedente sono state create le nuove colonne in modo che tutto ciò che resta consiste nel popolare le colonne con i dati necessari. A tale scopo, si userà un'applicazione console che viene eseguita una sola volta per popolare le colonne della tabella utente appena create.

1. Creare una nuova applicazione console della soluzione in fase di chiusura.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installare la versione più recente del pacchetto di Entity Framework.
3. Aggiungere l'applicazione web creata in precedenza come riferimento per l'applicazione console. Per eseguire questo pulsante destro del mouse fare clic sul progetto, quindi 'Aggiungi riferimenti', quindi soluzioni, quindi fare clic sul progetto e fare clic su OK.
4. Copiare il seguente codice nella classe Program.cs. Questa logica legge i dati del profilo per ogni utente, lo serializza come 'ProfileInfo' oggetto e la archivia nel database.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Alcuni dei modelli utilizzati sono definiti nella cartella 'IdentityModels' del progetto dell'applicazione web, pertanto è necessario includere gli spazi dei nomi corrispondente.
5. Il codice sopra riportato funziona sul file di database nell'App\_cartella dati del progetto dell'applicazione web creata nei passaggi precedenti. Per fare riferimento a tale, aggiornare la stringa di connessione nel file app. config dell'applicazione console con la stringa di connessione in Web. config dell'applicazione web. Specificare anche il percorso fisico completo nella proprietà 'AttachDbFilename'.
6. Aprire un prompt dei comandi e passare alla cartella bin dell'applicazione console precedente. Eseguire il file eseguibile ed esaminare l'output del log, come illustrato nell'immagine seguente.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Aprire la tabella 'AspNetUsers' in Esplora Server e verificare i dati in nuove colonne che contengono le proprietà. È necessario aggiornarli con i valori di proprietà corrispondenti.

## <a name="verify-functionality"></a>Verificare le funzionalità

Utilizzare le pagine di appartenenze appena aggiunto che vengono implementate usando ASP.NET Identity per eseguire l'accesso di un utente dal database obsoleto. L'utente deve essere in grado di accedere usando le stesse credenziali. Provare altre funzionalità quali l'aggiunta di OAuth, creando un nuovo utente, la modifica di una password, l'aggiunta di ruoli, aggiungere utenti a ruoli e così via.

I dati del profilo dell'utente precedente e i nuovi utenti devono essere recuperati e archiviati nella tabella degli utenti. La vecchia tabella non deve non sarà più possibile fare riferimento.

## <a name="conclusion"></a>Conclusione

L'articolo ha illustrato il processo di migrazione delle applicazioni web che utilizza il modello di provider per l'appartenenza ad ASP.NET Identity. L'articolo è descritta anche la migrazione dei dati di profilo per gli utenti di effettuare l'hook nel sistema di identità. Lasciare commenti sotto per domande e risolvere i problemi riscontrati quando si esegue la migrazione all'app.

*Rick Anderson e Robert McMurray grazie per aver letto l'articolo.*
