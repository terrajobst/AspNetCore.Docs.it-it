---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione viene illustrata la procedura per eseguire la migrazione di un'applicazione web esistente con i dati a utenti e ruoli creati mediante l'appartenenza di SQL per la nuova identità ASP.NET s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2790f32bc74cecf450f5a258fc1ff5b280a63923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity
====================
dal [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> In questa esercitazione viene illustrata la procedura per eseguire la migrazione di un'applicazione web esistente con i dati a utenti e ruoli creati mediante l'appartenenza di SQL per il nuovo sistema di identità di ASP.NET. Questo approccio comporta la modifica di schema di database esistente a quella necessaria alle identità di ASP.NET e hook nelle classi precedente/nuovo a esso. Dopo che si adotta questo approccio, dopo la migrazione del database, verranno gestiti facilmente gli aggiornamenti futuri di identità.


Per questa esercitazione, verrà usato un modello di applicazione web (Web Form) creati utilizzando Visual Studio 2010 per creare dati utente e il ruolo. Si utilizzerà quindi gli script SQL per eseguire la migrazione del database esistente per le tabelle necessite per il sistema di identità. Si verrà quindi installare i pacchetti NuGet necessari e aggiungere nuove pagine di gestione di account che usano il sistema di identità per la gestione delle appartenenze. Come un test della migrazione, gli utenti creati mediante l'appartenenza SQL devono essere in grado di accedere e nuovi utenti devono essere in grado di registrare. È possibile trovare l'esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Vedere anche [migrazione dall'appartenenza ASP.NET ad ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Per iniziare

### <a name="creating-an-application-with-sql-membership"></a>Creazione di un'applicazione con l'appartenenza di SQL

1. È necessario iniziare con un'applicazione esistente che utilizza l'appartenenza SQL e dati utente e il ruolo. Allo scopo di questo articolo, creare un'applicazione web in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Utilizzare lo strumento di configurazione di ASP.NET, creare 2 utenti: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Creare un ruolo denominato amministratore e aggiungere 'oldAdminUser' come utente con tale ruolo.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Creare una sezione di amministrazione del sito con un Default.aspx. Impostare il tag di autorizzazione nel file Web. config per abilitare l'accesso solo agli utenti nei ruoli di amministratore. Altre informazioni, vedere [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Visualizzare il database in Esplora Server per comprendere le tabelle create dal sistema di appartenenze SQL. I dati di accesso utente vengono archiviati in aspnet\_utenti e aspnet\_appartenenza tabelle, mentre i dati di ruolo vengono archiviati in aspnet\_tabella ruoli. Informazioni su quali utenti sono diversi ruoli viene archiviato in aspnet\_UsersInRoles tabella. Per la gestione di appartenenza base è sufficiente trasferire le informazioni nelle tabelle precedenti per il sistema di identità di ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>La migrazione a Visual Studio 2013

1. Installare Visual Studio Express 2013 per Web o di Visual Studio 2013 con il [aggiornamenti più recenti](https://www.microsoft.com/download/details.aspx?id=44921).
2. Nella versione installata di Visual Studio, aprire il progetto precedente. Se SQL Server Express non è installato nel computer, viene visualizzato un prompt dei comandi quando si apre il progetto, poiché la stringa di connessione utilizza SQL Express. È possibile scegliere di installare SQL Express o come aggirare cambiare la stringa di connessione a LocalDb. Per questo articolo, verranno modificate in LocalDb.
3. Aprire Web. config e modificare la stringa di connessione. SQLExpess a v 11.0 (LocalDb). Rimuovere ' User Instance = true' dalla stringa di connessione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Aprire Esplora Server e verificare che lo schema di tabella e i dati possono essere osservati.
5. Il sistema di identità ASP.NET funziona con la versione 4.5 o versione successiva di framework. Ricompila l'applicazione a 4.5 o versione successiva.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compilare il progetto per verificare che non siano presenti errori.

### <a name="installing-the-nuget-packages"></a>L'installazione dei pacchetti Nuget

1. In Esplora soluzioni, fare clic sul progetto &gt; **Gestisci pacchetti NuGet**. Nella casella di ricerca, immettere "Asp.net Identity". Selezionare il pacchetto nell'elenco dei risultati e fare clic su Installa. Accettare il contratto di licenza facendo clic sul pulsante "Accetto". Si noti che questo pacchetto installerà i pacchetti di dipendenza: EntityFramework e Microsoft ASP.NET Identity Core. Analogamente, installare i pacchetti seguenti (se non si desidera abilitare Accedi OAuth, ignorare i pacchetti OWIN ultime 4):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Eseguire la migrazione di database nel nuovo sistema di identità

Il passaggio successivo è eseguire la migrazione di database esistente a uno schema richiesto dal sistema ASP.NET Identity. Per ottenere questo Esegui SQL script che ha un set di comandi per creare nuove tabelle e di eseguire la migrazione delle informazioni utente esistenti nelle nuove tabelle. Il file di script è reperibile [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Questo file di script è specifico per questo esempio. Se lo schema per le tabelle creato mediante l'appartenenza SQL personalizzato o modificato la necessità di script modificati di conseguenza.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Come generare lo script SQL per la migrazione dello schema

Per le classi di ASP.NET Identity a funziona automaticamente con i dati degli utenti esistenti, è necessario eseguire la migrazione dello schema del database a quello richiesto dall'identità di ASP.NET. È possibile farlo mediante l'aggiunta di nuove tabelle e copia le informazioni esistenti in tali tabelle. Per impostazione predefinita ASP.NET Identity utilizza EntityFramework per eseguire il mapping delle classi del modello di identità nel database per archiviare/recuperare le informazioni. Queste classi di modello implementano le interfacce di identità core la definizione di oggetti utente e ruoli. Le tabelle e colonne nel database sono basate su queste classi di modello. Le classi modello EntityFramework identità 2.1.0 e le relative proprietà sono definite come segue

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | stringa | Id | RoleId | Provider | Id |
| Nome utente | stringa | nome | UserId | UserId | ClaimType |
| PasswordHash | stringa |  |  | LoginProvider | ClaimValue |
| SecurityStamp | stringa |  |  |  | Utente\_Id |
| Email | stringa |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Numero di telefono | stringa |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

È necessario disporre di tabelle per ciascuno di questi modelli le cui colonne corrispondono alle proprietà. Il mapping tra le classi e le tabelle viene definito nel `OnModelCreating` metodo il `IdentityDBContext`. Questo è noto come il metodo API fluent della configurazione e altre informazioni sono reperibili [qui](https://msdn.microsoft.com/data/jj591617.aspx). La configurazione per le classi è come indicato di seguito

| **Classe** | **Tabella** | **chiave primaria** | **chiave esterna** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Utente\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey+UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Utente\_Id -&gt;AspnetUsers |

Con queste informazioni è possibile creare istruzioni SQL per creare nuove tabelle. È possibile scrivere ogni istruzione singolarmente o generare lo script utilizzando i comandi di EntityFramework PowerShell è quindi possibile modificare come richiesto. A tale scopo, in Visual Studio aprire il **Package Manager Console** dal **vista** o **strumenti** menu

- Eseguire i comandi "Enable-Migrations" per abilitare le migrazioni EntityFramework.
- Eseguire il comando "Add-migration iniziale", che crea il codice di configurazione iniziale per creare il database in c# o VB.
- Il passaggio finale consiste nel eseguire "Update-Database-Script" comando che genera lo script SQL in base alle classi del modello.

Questo script di generazione del database può essere utilizzato come punto di partenza in cui è possibile apportare ulteriori modifiche per aggiungere nuove colonne e copiare i dati. Il vantaggio consiste nel fatto che verranno generati il `_MigrationHistory` tabella viene utilizzato dalla EntityFramework per modificare lo schema del database quando il modello di classi di modifica per le future versioni del rilascio di identità. 

Le informazioni sull'utente di appartenenza SQL ha altri proprietà oltre a quelli nella classe di modello di identità utente particolare messaggio di posta elettronica, tentativi di password, data di ultimo accesso, l'ultima data di blocco e così via. Si tratta di informazioni utili e si desidera essere trasferito al sistema di identità. Questa operazione può essere eseguita tramite l'aggiunta di proprietà aggiuntive al modello di utente e mapping torna a colonne della tabella nel database. È possibile farlo mediante l'aggiunta di una classe che rappresenta una sottoclasse di `IdentityUser` modello. È possibile aggiungere le proprietà per questa classe personalizzata e modificare lo script SQL per aggiungere le colonne corrispondenti durante la creazione della tabella. Il codice per questa classe è descritto dettagliatamente nell'articolo. Lo script SQL per la creazione di `AspnetUsers` tabella dopo l'aggiunta di nuove proprietà sarebbe

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Successivamente, è necessario copiare le informazioni esistenti dal database delle appartenenze SQL alle tabelle appena aggiunte per l'identità. Copiare i dati direttamente da una tabella in un altro, questo può avvenire tramite SQL. Per aggiungere i dati nelle righe della tabella, viene usato il `INSERT INTO [Table]` costruire. Copiare da un'altra tabella, è possibile utilizzare il `INSERT INTO` istruzione con il `SELECT` istruzione. Per ottenere tutte le informazioni utente, è necessario eseguire una query il *aspnet\_utenti* e *aspnet\_appartenenza* tabelle e copiare i dati di *AspNetUsers*tabella. Utilizziamo la `INSERT INTO` e `SELECT` insieme a `JOIN` e `LEFT OUTER JOIN` istruzioni. Per ulteriori informazioni sull'esecuzione di query e la copia dei dati tra tabelle, fare riferimento a [questo](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) collegamento. Inoltre le tabelle AspnetUserLogins e AspnetUserClaims sono vuote per iniziare poiché non sono disponibili informazioni di appartenenza di SQL che esegue il mapping a questo per impostazione predefinita. L'unica informazione copiato è per utenti e ruoli. Per il progetto creato nei passaggi precedenti, la query SQL per copiare le informazioni nella tabella gli utenti sarebbero

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Nell'istruzione SQL precedente, le informazioni relative a ciascun utente dal *aspnet\_utenti* e *aspnet\_appartenenza* tabelle viene copiato in colonne di  *AspnetUsers* tabella. La modifica sola in questo caso è quando si copia la password. Poiché l'algoritmo di crittografia per le password di appartenenza SQL utilizzato 'PasswordSalt' e 'PasswordFormat', copiamo che troppo e la password con hash in modo che può essere utilizzato per decrittografare la password dall'identità. Questo è ulteriormente descritta nell'articolo quando l'associazione di un hasher password personalizzata. 

Questo file di script è specifico per questo esempio. Per le applicazioni che dispongono di tabelle aggiuntive, gli sviluppatori possono seguire un approccio simile per aggiungere ulteriori proprietà sulla classe di modello utente e di eseguirne il mapping alle colonne della tabella AspnetUsers. Per eseguire lo script,

1. Aprire Esplora Server. Espandere la connessione "ApplicationServices" per visualizzare le tabelle. Fare clic con il pulsante destro sul nodo tabelle e selezionare l'opzione 'Nuova Query'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Nella finestra query, copiare e incollare l'intero script SQL dal file Migrations.sql. Eseguire il file di script facendo clic sul pulsante freccia 'Esegui'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aggiornare la finestra di Esplora Server. Cinque nuove tabelle vengono create nel database.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Di seguito è come le informazioni nelle tabelle delle appartenenze SQL viene eseguito il mapping al nuovo sistema di identità.

    ASPNET\_ruoli -&gt; AspNetRoles

    ASP\_netUsers e asp\_netMembership -&gt; AspNetUsers

    ASPNET\_UserInRoles -&gt; AspNetUserRoles

    Come illustrato nella sezione precedente, le tabelle AspNetUserClaims e AspNetUserLogins sono vuote. Il campo 'Discriminatore' nella tabella AspNetUser deve corrispondere al nome di classe di modello che viene definito come passaggio successivo. Anche la colonna PasswordHash è nel formato ' password crittografata | salt della password | formato password'. In questo modo è possibile utilizzare una logica di crittografia appartenenza SQL in modo che è possibile riutilizzare la password precedente. Che viene spiegata avanti in questo articolo.

### <a name="creating-models-and-membership-pages"></a>Creazione di modelli e pagine di appartenenze

Come accennato in precedenza, la funzionalità di identità Usa Entity Framework per comunicare con il database per l'archiviazione delle informazioni sull'account per impostazione predefinita. Per utilizzare i dati esistenti nella tabella, è necessario creare le classi modello che eseguire nuovamente il mapping di tabelle e associare il sistema di identità. Come parte del contratto di identità, le classi di modello devono implementare le interfacce definite nella dll Identity.Core o estendono l'implementazione di queste interfacce disponibili in EntityFramework esistente.

In questo esempio, le tabelle AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole presentano le colonne che sono simili all'implementazione esistente di sistema di identità. Di conseguenza è possibile riutilizzare le classi esistenti per eseguire il mapping a queste tabelle. La tabella AspNetUser contiene alcune colonne aggiuntive che vengono utilizzati per archiviare informazioni aggiuntive dalle tabelle di appartenenze SQL. Questo può essere mappato tramite la creazione di una classe modello che estendono l'implementazione di 'IdentityUser' esistente e aggiungere le proprietà aggiuntive.

1. Cartella modelli crea un progetto e aggiungere una classe utente. Il nome della classe deve corrispondere i dati aggiunti nella colonna 'Discriminatore' della tabella 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe utente deve estendere la classe di IdentityUser, vedere il *EntityFramework* dll. Dichiarare le proprietà nella classe eseguire nuovamente il mapping delle colonne AspNetUser. Le proprietà ID, nome utente, PasswordHash e SecurityStamp sono definite nel IdentityUser e pertanto sono stati omessi. Di seguito è il codice per la classe utente che dispone di tutte le proprietà

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Una classe DbContext di Entity Framework è necessaria per rendere persistenti i dati nei modelli Torna alle tabelle e recuperare dati da tabelle per popolare i modelli. *EntityFramework* dll definisce la classe di IdentityDbContext che interagisce con le tabelle di identità per recuperare e archiviare le informazioni. IdentityDbContext&lt;tuser&gt; accetta una classe 'TUser', che può essere qualsiasi classe che estende la classe IdentityUser.

    Creare una nuova classe ApplicationDBContext che estende IdentityDbContext sotto la cartella 'Modelli', passando la classe 'User' creata nel passaggio 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gestione degli utenti nel nuovo sistema di identità viene eseguita utilizzando la classe UserManager&lt;tuser&gt; definito nella classe di *EntityFramework* dll. È necessario creare una classe personalizzata che estende UserManager, passando la classe 'User' creata nel passaggio 1.

    Nella cartella Models creare una nuova classe UserManager che estende UserManager&lt;utente&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Le password degli utenti dell'applicazione vengono crittografate e archiviate nel database. L'algoritmo di crittografia utilizzato nell'appartenenza SQL è diverso da quello nel nuovo sistema di identità. Per riutilizzare le vecchie password è necessario decrittografare in modo selettivo le password quando gli utenti precedenti accedere utilizzando l'algoritmo di appartenenze SQL quando si utilizza l'algoritmo di crittografia nell'identità per i nuovi utenti.

    La classe UserManager ha una proprietà 'PasswordHasher', che archivia un'istanza di una classe che implementa l'interfaccia 'IPasswordHasher'. Viene utilizzato per crittografare/decrittografare le password durante operazioni di autenticazione utente. La classe UserManager definita nel passaggio 3, creare una nuova classe SQLPasswordHasher e copiare il codice riportato di seguito.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Risolvere gli errori di compilazione tramite l'importazione di spazi dei nomi System. Text e System.Security.Cryptography.

    Il metodo EncodePassword crittografa la password in base l'implementazione di crittografia l'appartenenza SQL predefinita. Ciò viene ottenuto dalla dll System. Web. Se l'app precedente utilizzata un'implementazione personalizzata deve riflettersi qui. È necessario definire altri due metodi *HashPassword* e *VerifyHashedPassword* che utilizzano il *EncodePassword* di hash di una password o verificare un testo normale (metodo) password con uno esistente nel database.

    Il sistema di appartenenze SQL utilizzato PasswordHash e PasswordSalt PasswordFormat per l'hash della password immesse dagli utenti quando si registra o cambiare la password. Durante la migrazione i tre campi vengono archiviati nella colonna della tabella AspNetUser separati da PasswordHash di ' |' caratteri. Quando un utente effettua l'accesso e la password è questi campi, utilizziamo di crittografia di appartenenze SQL per verificare la password; in caso contrario, utilizziamo crittografia predefinita del sistema di identità per verificare la password. In questo modo gli utenti precedenti non avranno modificare le relative password dopo l'applicazione viene eseguita la migrazione.
5. Dichiarare il costruttore per la classe UserManager e passare la variabile come il SQLPasswordHasher alla proprietà nel costruttore.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crea nuovo account di pagine di gestione

Il passaggio successivo della migrazione consiste nell'aggiungere pagine di gestione di account che verranno consentono agli utenti di registrare e accedere. Le pagine dell'account precedente dall'appartenenza SQL usano i controlli che non funzionano con il nuovo sistema di identità. Per aggiungere il nuovo utente pagine di gestione di seguono l'esercitazione questo collegamento [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) a partire dal passaggio 'Aggiunta di Web Form per registrare gli utenti all'applicazione' perché già abbiamo creato il progetto e aggiunta NuGet pacchetti.

È necessario apportare alcune modifiche per l'esempio funzionare con il progetto che è disponibile qui.

- Il codice Register.aspx.cs e Login.aspx.cs classi utilizzano il `UserManager` dai pacchetti di identità per creare un utente. Per questo esempio utilizzare la classe UserManager aggiunta nella cartella Models seguendo i passaggi indicati in precedenza.
- Utilizzare la classe utente creata anziché IdentityUser in Register.aspx.cs e Login.aspx.cs code-behind di classi. Questo hook della classe utente personalizzata nel sistema di identità.
- La parte per creare il database può essere ignorata.
- Lo sviluppatore deve impostare l'ID applicazione per il nuovo utente corrispondere l'ID dell'applicazione corrente. Questa operazione può essere eseguita eseguendo una query di ApplicationId per l'applicazione prima che venga creato un oggetto utente nella classe Register.aspx.cs e impostarlo prima di creare l'utente. 

    Esempio:

    Definire un metodo nella pagina Register.aspx.cs per eseguire una query aspnet\_applicazioni tabella e ottenere l'Id applicazione in base al nome di applicazione

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Ora ottenere impostare nell'oggetto utente

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Utilizzare il vecchio nome utente e la password per accedere a un utente esistente. Utilizzare la pagina di registrazione per creare un nuovo utente. Verificare inoltre che gli utenti siano in ruoli, come previsto.

Porting al sistema di identità consente all'utente di aggiungere Open Authentication (OAuth) all'applicazione. Fare riferimento all'esempio [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) con OAuth abilitato.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato illustrato come porta agli utenti di appartenenza SQL di ASP.NET Identity, ma non è la porta dati di profilo. Nella prossima esercitazione verrà esaminato nel porting di dati di profilo dall'appartenenza SQL nel nuovo sistema di identità.

È possibile lasciare commenti e suggerimenti nella parte inferiore di questo articolo.

*Grazie a Tom Dykstra e Rick Anderson per esaminare l'articolo.*
