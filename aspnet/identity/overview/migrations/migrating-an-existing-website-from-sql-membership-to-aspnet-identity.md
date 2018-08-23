---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: La migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra i passaggi per eseguire la migrazione di un'applicazione web esistente con l'utente e i dati del ruolo creati mediante l'appartenenza di SQL per la nuova identità di ASP.NET s...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 51b97ee413ea0304177d5963b5fd9d7253778d4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835785"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>La migrazione di un sito Web esistente dall'appartenenza SQL ad ASP.NET Identity
====================
dal [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Questa esercitazione illustra i passaggi per eseguire la migrazione di un'applicazione web esistente con l'utente e i dati del ruolo creati mediante l'appartenenza di SQL nel nuovo sistema di identità di ASP.NET. Questo approccio comporta la modifica lo schema del database esistente a quella necessaria per l'identità di ASP.NET e hook nelle classi vecchi/nuovi a esso. Dopo che si adotta questo approccio, dopo la migrazione del database, gli aggiornamenti futuri all'identità verranno gestiti senza fatica.


Per questa esercitazione verrà usato un modello di applicazione web (Web Form) creato utilizzando Visual Studio 2010 per creare dati utente e il ruolo. Si userà quindi gli script SQL per eseguire la migrazione del database esistente per le tabelle necessite per il sistema di identità. Verranno quindi installare i pacchetti NuGet necessari e aggiungere nuove pagine di gestione di account che usano il sistema di identità per gestione delle appartenenze. Come un test della migrazione, gli utenti creati tramite l'appartenenza SQL devono essere in grado di accedere e devono essere in grado di registrare i nuovi utenti. È possibile trovare l'esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Vedere anche [eseguire la migrazione dall'appartenenza ASP.NET ad ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Per iniziare

### <a name="creating-an-application-with-sql-membership"></a>Creazione di un'applicazione con l'appartenenza di SQL

1. È necessario iniziare con un'applicazione esistente che usa l'appartenenza SQL e include dati utente e il ruolo. Ai fini di questo articolo, è possibile creare un'applicazione web in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Utilizzare lo strumento di configurazione di ASP.NET, creare 2 utenti: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Creare un ruolo denominato amministratore e aggiungere 'oldAdminUser' come utente nel ruolo in questione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Creare una sezione di amministrazione del sito con un default. aspx. Impostare il tag di autorizzazione nel file Web. config per abilitare l'accesso solo agli utenti nei ruoli di amministratore. Altre informazioni sono disponibili qui [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Visualizzare il database in Esplora Server per comprendere le tabelle create dal sistema di appartenenze SQL. I dati dell'account di accesso utente viene archiviati in aspnet\_utenti e aspnet\_tabelle delle appartenenze, mentre i dati del ruolo viene archiviati nella aspnet\_tabella ruoli. Informazioni su quali utenti sono in ruoli che sono archiviati in aspnet\_UsersInRoles tabella. Per la gestione di appartenenza base è sufficiente trasferire le informazioni nelle tabelle precedenti per il sistema di identità di ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Eseguire la migrazione a Visual Studio 2013

1. Installare Visual Studio Express 2013 per Web o di Visual Studio 2013 con il [gli aggiornamenti più recenti](https://www.microsoft.com/download/details.aspx?id=44921).
2. Nella versione installata di Visual Studio, aprire il progetto precedente. Se SQL Server Express non è installato nel computer, viene visualizzato un prompt dei comandi quando si apre il progetto, dato che la stringa di connessione Usa SQL Express. È possibile scegliere di installare SQL Express o come soluzione alternativa modifica la stringa di connessione a LocalDb. Per questo articolo, verranno modificate in LocalDb.
3. Aprire Web. config e modificare la stringa di connessione. SQLExpess a v11.0 (LocalDb). Rimuovere ' User Instance = true' dalla stringa di connessione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Aprire Esplora Server e verificare che i dati e lo schema di tabella possono essere osservati.
5. Il sistema di identità ASP.NET funziona con la versione 4.5 o versione successiva del framework. Ridestinare l'applicazione alla versione 4.5 o versione successiva.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compilare il progetto per verificare che non siano presenti errori.

### <a name="installing-the-nuget-packages"></a>Installazione dei pacchetti Nuget

1. In Esplora soluzioni fare clic sul progetto &gt; **Gestisci pacchetti NuGet**. Nella casella di ricerca immettere "Asp.net Identity". Selezionare il pacchetto nell'elenco dei risultati e fare clic su Installa. Accettare il contratto di licenza facendo clic sul pulsante "Accetto". Si noti che questo pacchetto installerà i pacchetti di dipendenza: Entity Framework e Microsoft ASP.NET Identity Core. Analogamente, installare i pacchetti seguenti (se non si vuole abilitare log aggiuntivo OAuth, ignorare le ultime 4 pacchetti OWIN):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Eseguire la migrazione di database nel nuovo sistema di identità

Il passaggio successivo consiste nella migrazione del database esistente a uno schema richiesto dal sistema ASP.NET Identity. Per ottenere questo eseguiamo un database SQL di script che ha un set di comandi per creare nuove tabelle e la migrazione delle informazioni utente esistenti nelle nuove tabelle. Il file di script sono reperibili [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Questo file di script è specifico per questo esempio. Se lo schema per le tabelle create tramite l'appartenenza SQL personalizzato o modificato la necessità di script per essere modificato di conseguenza.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Come generare lo script SQL per la migrazione dello schema

Per le classi di ASP.NET Identity a funziona automaticamente con i dati degli utenti esistenti, è necessario eseguire la migrazione dello schema del database a quello richiesto dall'identità di ASP.NET. È possibile farlo mediante l'aggiunta di nuove tabelle e copia le informazioni esistenti in tali tabelle. Per impostazione predefinita ASP.NET Identity Usa EntityFramework per eseguire il mapping di classi del modello di identità nel database di archiviare/recuperare le informazioni. Queste classi modello implementano le interfacce di identità core la definizione di utente e gli oggetti role. Le tabelle e le colonne nel database si basano su queste classi di modello. Le classi modello EntityFramework 2.1.0 in identità e le relative proprietà sono definite come segue

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | stringa | Id | RoleId | Provider | Id |
| Nome utente | stringa | nome | UserId | UserId | Tipo di attestazione |
| PasswordHash | stringa |  |  | LoginProvider | ClaimValue |
| SecurityStamp | stringa |  |  |  | Utente\_Id |
| Email | stringa |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Numero di telefono | stringa |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

È necessario disporre di tabelle per ognuno di questi modelli le cui colonne corrispondono alle proprietà. Il mapping tra le classi e le tabelle viene definito nel `OnModelCreating` metodo del `IdentityDBContext`. Ciò è noto come il metodo API fluent della configurazione e altre informazioni sono reperibili [qui](https://msdn.microsoft.com/data/jj591617.aspx). La configurazione per le classi è come indicato di seguito

| **Classe** | **Tabella** | **Chiave primaria** | **Chiave esterna** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Utente\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Provider + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Utente\_Id -&gt;AspnetUsers |

Con queste informazioni è possibile creare istruzioni SQL per creare nuove tabelle. È possibile scrivere ogni istruzione individualmente o generare l'intero script tramite EntityFramework PowerShell comandi che è quindi possibile modificare in base alle esigenze. A tale scopo, in Visual Studio aprire il **Console di gestione pacchetti** dal **View** o **strumenti** menu

- Esegui comando "Enable-Migrations" per abilitare le migrazioni di Entity Framework.
- Eseguire il comando "Add-migration initial" che consente di creare il codice di configurazione iniziale per creare il database in c# o VB.
- Il passaggio finale consiste nell'eseguire "Update-Database-Script" comando che genera lo script SQL basati sulle classi del modello.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Questo script di generazione di database può essere utilizzato come punto di partenza in cui è possibile apportare ulteriori modifiche per aggiungere nuove colonne e copiare i dati. Il vantaggio consiste nel fatto che si generino la `_MigrationHistory` tabella cui viene usata da Entity Framework per modificare lo schema del database quando il modello di classi di modifica per le versioni future dei rilasci di identità. 

Le informazioni sull'utente di appartenenza SQL era altri proprietà oltre a quelli nella classe di modello di identità utente particolare messaggio di posta elettronica, tentativi di password, data dell'ultimo accesso, l'ultima data di blocco e così via. Si tratta di informazioni utili e ci piacerebbe che deve essere eseguita il sistema di identità. Questa operazione può essere eseguita aggiungendo le proprietà aggiuntive per il modello di utente e li esegue il mapping a colonne della tabella nel database. È possibile farlo mediante l'aggiunta di una classe che rappresenti le sottoclassi di `IdentityUser` modello. È possibile aggiungere le proprietà per questa classe personalizzata e modificare lo script SQL per aggiungere le colonne corrispondenti durante la creazione della tabella. Il codice per questa classe è descritto più dettagliatamente nell'articolo. Lo script SQL per la creazione di `AspnetUsers` tabella dopo l'aggiunta di nuove proprietà saranno

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Successivamente è necessario copiare le informazioni esistenti dal database di appartenenze SQL alle tabelle appena aggiunte per l'identità. Questa operazione può essere eseguita tramite SQL copiando i dati direttamente da una tabella a altra. Per aggiungere i dati nelle righe della tabella, usiamo il `INSERT INTO [Table]` costruire. Per copiare da un'altra tabella è possibile usare la `INSERT INTO` istruzione con il `SELECT` istruzione. Per ottenere tutte le informazioni utente è necessario eseguire una query il *aspnet\_gli utenti* e *aspnet\_appartenenza* tabelle e copiare i dati di *AspNetUsers*nella tabella. Usiamo il `INSERT INTO` e `SELECT` insieme a `JOIN` e `LEFT OUTER JOIN` istruzioni. Per altre informazioni sull'esecuzione di query e la copia dei dati tra tabelle, fare riferimento a [ciò](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) collegamento. Inoltre le tabelle AspnetUserLogins e AspnetUserClaims sono vuote per iniziare perché non sono disponibili informazioni di appartenenza SQL che esegue il mapping a questo per impostazione predefinita. L'unica informazione copiato è per utenti e ruoli. Per il progetto creato nei passaggi precedenti, sarà la query SQL per copiare le informazioni alla tabella users

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Nell'istruzione SQL precedente, le informazioni relative a ciascun utente dal *aspnet\_gli utenti* e *aspnet\_appartenenza* tabelle viene copiato in colonne di  *AspnetUsers* tabella. L'unica modifica eseguita in questo caso è quando si copia la password. Poiché l'algoritmo di crittografia per le password nell'appartenenza SQL utilizzato 'PasswordSalt' e 'PasswordFormat', copiamo che troppo con password con hash in modo che può essere utilizzato per decrittografare la password dall'identità. Ciò è illustrato più dettagliatamente nell'articolo quando agganciarmi hasher una password personalizzata. 

Questo file di script è specifico per questo esempio. Per le applicazioni che hanno tabelle aggiuntive, gli sviluppatori possono seguire un approccio simile per aggiungere ulteriori proprietà sulla classe di modello utente ed eseguirne il mapping alle colonne nella tabella AspnetUsers. Per eseguire lo script,

1. Aprire Esplora Server. Espandere la connessione 'ApplicationServices' per visualizzare le tabelle. Fare clic con il pulsante destro sul nodo Tables e selezionare l'opzione 'Nuova Query'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Nella finestra di query, copiare e incollare l'intero script SQL dal file Migrations.sql. Eseguire il file di script facendo clic sul pulsante freccia 'Execute'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aggiornare la finestra di Esplora Server. Vengono create cinque nuove tabelle nel database.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Di seguito è la modalità di mapping le informazioni nelle tabelle di appartenenza SQL nel nuovo sistema di identità.

    ASPNET\_ruoli -&gt; AspNetRoles

    ASP\_netUsers e asp\_netMembership -&gt; AspNetUsers

    ASPNET\_UserInRoles -&gt; AspNetUserRoles

    Come spiegato nella sezione precedente, le tabelle AspNetUserClaims e AspNetUserLogins sono vuote. Il campo 'Discriminatore' nella tabella AspNetUser deve corrispondere al nome di classe modello che è definito come un passaggio successivo. Anche la colonna PasswordHash è nel formato ' password crittografata | valore salt della password | il formato della password'. In questo modo è possibile usare per la logica crypto di appartenenze SQL speciale in modo da poterla riutilizzare password precedenti. Che viene spiegato in seguito in questo articolo.

### <a name="creating-models-and-membership-pages"></a>Creazione di modelli e le pagine di appartenenza

Come accennato in precedenza, la funzionalità di identità Usa Entity Framework per comunicare con il database per archiviare le informazioni sull'account per impostazione predefinita. Per lavorare con i dati esistenti nella tabella, è necessario creare le classi modello che eseguire il mapping a tabelle e agganciare il sistema di identità. Come parte del contratto dell'identità, le classi del modello devono implementare le interfacce definite nella dll Identity.Core o estendono l'implementazione esistente di queste interfacce disponibili in EntityFramework.

Nel nostro esempio, le tabelle AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole presentano colonne che sono simili per l'implementazione esistente del sistema di identità. Di conseguenza è possibile riutilizzare le classi esistenti per eseguire il mapping a queste tabelle. La tabella AspNetUser include alcune colonne aggiuntive che vengono usati per archiviare informazioni aggiuntive dalle tabelle delle appartenenze di SQL. Ciò può essere mappato mediante la creazione di una classe di modello che estendono l'implementazione esistente di 'IdentityUser' e aggiungere le proprietà aggiuntive.

1. Cartella modelli crea un progetto e aggiungere una classe utente. Il nome della classe deve corrispondere i dati aggiunti nella colonna 'Discriminatore' della tabella 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe dell'utente deve estendere la classe IdentityUser trovata nel *EntityFramework* dll. Dichiarare le proprietà nella classe che fanno riferimento al AspNetUser colonne. Le proprietà ID, nome utente, PasswordHash e SecurityStamp definite nel IdentityUser e pertanto vengono omessi. Di seguito è riportato il codice per la classe utente che dispone di tutte le proprietà

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Una classe DbContext di Entity Framework è necessaria per rendere persistenti i dati nei modelli a tabelle e recuperare dati dalle tabelle per popolare i modelli. *EntityFramework* dll definisce la classe di IdentityDbContext che interagisce con le tabelle di identità per recuperare e archiviare informazioni. IdentityDbContext&lt;tuser&gt; accetta una classe 'TUser' che può essere qualsiasi classe che estende la classe IdentityUser.

    Creare una nuova classe ApplicationDBContext che estende IdentityDbContext sotto la cartella 'Modelli', passando la classe 'User' creata nel passaggio 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gestione degli utenti nel nuovo sistema di identità viene eseguita usando la classe UserManager&lt;tuser&gt; definito nella classe la *EntityFramework* dll. È necessario creare una classe personalizzata che estende UserManager, passando la classe 'User' creata nel passaggio 1.

    Nella cartella Models creare una nuova classe UserManager che estende UserManager&lt;utente&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Le password degli utenti dell'applicazione vengono crittografate e archiviate nel database. L'algoritmo di crittografia usato nell'appartenenza SQL è diversa da quella nel nuovo sistema di identità. Per riutilizzare le password precedenti è necessario decrittografare in modo selettivo le password quando dei vecchi utenti accedono usando l'algoritmo di appartenenze SQL quando si usa l'algoritmo di crittografia nell'identità per i nuovi utenti.

    La classe UserManager ha una proprietà 'PasswordHasher' che contiene un'istanza di una classe che implementa l'interfaccia 'IPasswordHasher'. Viene utilizzato per crittografare/decrittografare le password durante operazioni di autenticazione utente. La classe UserManager definita nel passaggio 3, creare una nuova classe SQLPasswordHasher e copiare il codice riportato di seguito.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Per risolvere gli errori di compilazione, l'importazione di spazi dei nomi System. Text e Cryptography.

    Il metodo EncodePassword crittografa la password in base all'implementazione di operazioni di crittografia appartenenze SQL predefinita. Questi dati vengono estratti dalla dll System. Web. Se l'app precedente usato un'implementazione personalizzata deve riflettersi qui. È necessario definire due altri metodi *HashPassword* e *VerifyHashedPassword* che utilizzano il *EncodePassword* metodo per l'hashing di una password o verificare un testo normale password con uno esistente nel database.

    Il sistema di appartenenze SQL utilizzato PasswordHash, PasswordSalt e PasswordFormat per l'hash della password immesse dagli utenti quando si registra o si modificano la propria password. Durante la migrazione dei tre campi vengono archiviati nella colonna PasswordHash nella tabella AspNetUser separata con il ' |' caratteri. Quando un utente esegue l'accesso e la password contiene questi campi, di crittografia di appartenenze SQL viene usato per verificare la password; in caso contrario è usare la crittografia predefinita del sistema di identità per verificare la password. In questo modo gli utenti precedenti non dovrà modificare la password una volta che viene eseguita la migrazione dell'app.
5. Dichiarare il costruttore per la classe UserManager e passarlo come il SQLPasswordHasher alla proprietà nel costruttore.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crea nuovo account di pagine di gestione

Il passaggio successivo nel processo di migrazione consiste nell'aggiungere le pagine di gestione di account che verranno consentono agli utenti di registrare ed eseguire l'accesso. Le pagine di account precedente dall'appartenenza SQL usano i controlli che non funzionano con il nuovo sistema di identità. Per aggiungere il nuovo utente pagine di gestione di seguono l'esercitazione in questo collegamento [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) partendo dal passaggio 'Aggiunta di Web Form per la registrazione degli utenti all'applicazione' poiché abbiamo già creato il progetto e aggiunta di NuGet pacchetti.

È necessario apportare alcune modifiche per l'esempio di funzioni con il progetto che è disponibile qui.

- Register.aspx.cs e Login.aspx.cs code-behind Usa le classi di `UserManager` dai pacchetti di identità per creare un utente. Per questo esempio Usa la classe UserManager aggiunta nella cartella Models da seguendo la procedura descritta in precedenza.
- Usare la classe utente creata anziché IdentityUser in Register.aspx.cs e Login.aspx.cs code-behind di classi. Questo hook nella nostra classe utente personalizzata nel sistema di identità.
- La parte per creare il database può essere ignorata.
- Lo sviluppatore deve impostare l'ID applicazione per il nuovo utente in base l'ID dell'applicazione corrente. Questa operazione può essere eseguita l'esecuzione di query l'ID applicazione per l'applicazione prima della creazione di un oggetto utente nella classe Register.aspx.cs e impostandolo prima della creazione utente. 

    Esempio:

    Un metodo definito nella pagina Register.aspx.cs per eseguire una query aspnet\_applicazioni di tabella e ottenere l'Id applicazione in base al nome dell'applicazione

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    A questo punto recupero / impostazione questo nell'oggetto utente

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Usare il nome utente precedente e la password per accedere un utente esistente. Utilizzare la pagina di registrazione per creare un nuovo utente. Verificare inoltre che gli utenti siano in ruoli come previsto.

Porting per il sistema di identità consente all'utente di aggiungere l'autenticazione aperta (OAuth) all'applicazione. Vedere l'esempio [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) con OAuth abilitato.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato illustrato come convertire gli utenti dall'appartenenza SQL ad ASP.NET Identity, ma è non trasferire i dati del profilo. Nella prossima esercitazione verrà illustrato nel trasferimento dei dati di profilo dall'appartenenza SQL nel nuovo sistema di identità.

È possibile lasciare commenti e suggerimenti nella parte inferiore di questo articolo.

*Tom Dykstra e Rick Anderson grazie per aver letto l'articolo.*
