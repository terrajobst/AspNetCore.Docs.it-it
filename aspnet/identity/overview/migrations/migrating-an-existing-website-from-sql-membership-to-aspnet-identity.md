---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity | Documenti Microsoft
author: Rick-Anderson
description: Questa esercitazione viene illustrata la procedura per eseguire la migrazione di un'applicazione web esistente con utenti e i dati del ruolo creati mediante l'appartenenza di SQL per la nuova identità di ASP.NET s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1766c11dabec3931ec2bfc4ae2e15332427d7855
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314013"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>La migrazione di un sito Web esistente dall'appartenenza SQL per ASP.NET Identity
====================
dal [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Questa esercitazione viene illustrata la procedura per eseguire la migrazione di un'applicazione web esistente con dati a utenti e ruoli creati mediante l'appartenenza di SQL per il nuovo sistema di identità di ASP.NET. Questo approccio comporta la modifica lo schema del database esistente a quella necessaria alle identità di ASP.NET e hook nelle classi vecchia/nuovo ad esso. Dopo che si adotta questo approccio, dopo la migrazione del database, verranno gestiti facilmente gli aggiornamenti futuri all'identità.


Per questa esercitazione, si avrà un modello di applicazione web (Web Form) creati utilizzando Visual Studio 2010 per creare dati utente e il ruolo. Si utilizzerà quindi gli script SQL per eseguire la migrazione del database esistente per le tabelle necessite per il sistema di identità. È quindi, installare i pacchetti NuGet necessari e vengono aggiunte nuove pagine di gestione di account che usano il sistema di identità per la gestione delle appartenenze. Come un test della migrazione, gli utenti creati mediante l'appartenenza SQL devono essere in grado di accedere e devono essere in grado di registrare nuovi utenti. È possibile trovare l'esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Vedere anche [migrazione dall'appartenenza ASP.NET ad ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Per iniziare

### <a name="creating-an-application-with-sql-membership"></a>Creazione di un'applicazione con l'appartenenza di SQL

1. È necessario iniziare con un'applicazione esistente che utilizza l'appartenenza SQL e che contenga dati utente e il ruolo. Allo scopo di questo articolo, creare un'applicazione web in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Utilizzare lo strumento di configurazione di ASP.NET, creare 2 utenti: **oldAdminUser** e **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Creare un ruolo denominato amministratore e aggiungere 'oldAdminUser' come utente nel ruolo in questione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Creare una sezione di amministrazione del sito con un default. aspx. Impostare il tag di autorizzazione nel file Web. config per abilitare l'accesso solo agli utenti nei ruoli di amministratore. Altre informazioni, vedere [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Visualizzare il database in Esplora Server per comprendere le tabelle create dal sistema di appartenenze SQL. I dati dell'account di accesso utente viene archiviati in aspnet\_gli utenti e aspnet\_tabelle delle appartenenze, mentre i dati del ruolo viene archiviati nella aspnet\_tabella ruoli. Informazioni su quali utenti sono diversi ruoli viene archiviato in aspnet\_UsersInRoles tabella. Per la gestione di appartenenza base è sufficiente trasferire le informazioni nelle tabelle precedenti per il sistema di identità di ASP.NET.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>La migrazione a Visual Studio 2013

1. Installare Visual Studio Express 2013 per Web o di Visual Studio 2013, insieme ai [gli aggiornamenti più recenti](https://www.microsoft.com/download/details.aspx?id=44921).
2. Nella versione installata di Visual Studio, aprire il progetto precedente. Se SQL Server Express non è installato nel computer, un messaggio viene visualizzato quando si apre il progetto, poiché viene utilizzata la stringa di connessione SQL Express. È possibile scegliere di installare SQL Express o come aggirare, cambiare la stringa di connessione a LocalDb. Per questo articolo, verranno modificate in LocalDb.
3. Aprire Web. config e modificare la stringa di connessione. SQLExpess a v11.0 (LocalDb). Rimuovere ' User Instance = true' dalla stringa di connessione.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Aprire Esplora Server e verificare che lo schema di tabella e dei dati possono essere analizzati.
5. Il sistema di ASP.NET Identity funziona con la versione 4.5 o versione successiva di framework. Reindirizzare l'applicazione a 4.5 o versione successiva.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compilare il progetto per verificare che non siano presenti errori.

### <a name="installing-the-nuget-packages"></a>Installazione dei pacchetti Nuget

1. In Esplora soluzioni, fare clic sul progetto &gt; **Gestisci pacchetti NuGet**. Nella casella di ricerca, immettere "Asp.net Identity". Selezionare il pacchetto nell'elenco dei risultati e fare clic su Installa. Accettare il contratto di licenza facendo clic sul pulsante "Accetto". Si noti che questo pacchetto installerà i pacchetti di dipendenza: EntityFramework e Microsoft ASP.NET Identity Core. In modo analogo, installare i pacchetti seguenti (se non si desidera abilitare Accedi OAuth, ignorare i pacchetti OWIN ultime 4):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Eseguire la migrazione di database nel nuovo sistema di identità

Il passaggio successivo è per la migrazione del database esistente a uno schema richiesto dal sistema ASP.NET Identity. Per ottenere questo eseguiamo un database SQL include nello script che ha un set di comandi per creare nuove tabelle e migrazione delle informazioni utente esistente alle nuove tabelle. Il file di script è reperibile [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Questo file di script è specifico per questo esempio. Se lo schema per le tabelle creato mediante l'appartenenza SQL personalizzato o modificato la necessità di script per essere modificati di conseguenza.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Come generare lo script SQL per la migrazione dello schema

Per le classi di ASP.NET Identity a funziona automaticamente con i dati degli utenti esistenti, è necessario eseguire la migrazione dello schema del database a quella richiesta da ASP.NET Identity. È possibile farlo mediante l'aggiunta di nuove tabelle e copia le informazioni esistenti a tali tabelle. Per impostazione predefinita ASP.NET Identity EntityFramework viene utilizzato per eseguire il mapping delle classi del modello di identità nel database per archiviare/recuperare le informazioni. Queste classi di modello implementano le interfacce di identità core definizione di utente e gli oggetti role. Le tabelle e le colonne nel database sono basate su queste classi di modello. Le classi modello EntityFramework v2.1.0 identità e le relative proprietà sono definite come segue

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | stringa | Id | RoleId | ProviderKey | Id |
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

È necessario disporre di tabelle per ognuno di questi modelli le cui colonne corrispondono alle proprietà. Il mapping tra le classi e le tabelle viene definito nel `OnModelCreating` metodo il `IdentityDBContext`. Questo è noto come il metodo API fluent della configurazione e altre informazioni sono disponibili [qui](https://msdn.microsoft.com/data/jj591617.aspx). La configurazione per le classi sia come riportato in precedenza

| **Classe** | **Tabella** | **Chiave primaria** | **chiave esterna** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Utente\_Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Utente\_Id -&gt;AspnetUsers |

Con queste informazioni è possibile creare istruzioni SQL per creare nuove tabelle. È possibile scrivere ogni istruzione singolarmente o generare l'intero script utilizzando i comandi di EntityFramework PowerShell che è quindi possibile modificare come richiesto. A tale scopo, in Visual Studio aprire il **Console di gestione pacchetti** dal **vista** o **strumenti** menu

- Comando Run "Enable-Migrations" per abilitare le migrazioni EntityFramework.
- Eseguire il comando "Add-migration iniziale" che consente di creare il codice di configurazione iniziale per creare il database nel linguaggio c# / VB.
- Il passaggio finale consiste nell'eseguire "Update-Database-Script" comando che genera lo script SQL in base alle classi del modello.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Questo script di generazione del database può essere utilizzato come un avvio in cui è necessario apportare ulteriori modifiche per aggiungere nuove colonne e copiare i dati. Il vantaggio consiste nel fatto che si genera il `_MigrationHistory` tabella utilizzato dalla EntityFramework per modificare lo schema del database quando il modello di classi di modifica per le future versioni delle versioni di identità. 

Le informazioni utente di appartenenza SQL conteneva altre proprietà oltre a quelli nella classe di modello di identità utente vale a dire posta elettronica, tentativi di password, data dell'ultimo accesso, data di blocco e così via. Si tratta di informazioni utili e si desidera essere trasferito per il sistema di identità. Questa operazione può essere eseguita tramite l'aggiunta di proprietà aggiuntive al modello di utente e mapping di questi ultimi Torna alle colonne della tabella nel database. È possibile farlo mediante l'aggiunta di una classe che rappresenta una sottoclasse di `IdentityUser` modello. È possibile aggiungere le proprietà per questa classe personalizzata e modificare lo script SQL per aggiungere le colonne corrispondenti quando si crea la tabella. Il codice per questa classe verrà descritto dettagliatamente nell'articolo. Lo script SQL per la creazione di `AspnetUsers` tabella dopo l'aggiunta di nuove proprietà sarebbe

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Successivamente è necessario copiare le informazioni esistenti dal database delle appartenenze SQL alle tabelle appena aggiunte per l'identità. È possibile farlo tramite SQL copiando i dati direttamente da una tabella a un'altra. Per aggiungere dati nelle righe della tabella, viene usata la `INSERT INTO [Table]` costruire. Per copiare da un'altra tabella è possibile usare il `INSERT INTO` istruzione insieme al `SELECT` istruzione. Per ottenere tutte le informazioni utente è necessario eseguire una query il *aspnet\_gli utenti* e *aspnet\_appartenenza* le tabelle e copiare i dati per il *AspNetUsers*tabella. Utilizziamo la `INSERT INTO` e `SELECT` insieme a `JOIN` e `LEFT OUTER JOIN` istruzioni. Per ulteriori informazioni sull'esecuzione di query e la copia dei dati tra tabelle, fare riferimento a [ciò](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) collegamento. Inoltre le tabelle AspnetUserLogins e AspnetUserClaims sono vuote per iniziare poiché non sono disponibili informazioni di appartenenza SQL che esegue il mapping a questo per impostazione predefinita. L'unica informazione copiato è per utenti e ruoli. Per il progetto creato nei passaggi precedenti, la query SQL da copia informazioni nella tabella gli utenti sarebbero

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

Nell'istruzione SQL precedente, le informazioni relative a ciascun utente dal *aspnet\_gli utenti* e *aspnet\_appartenenza* viene copiato nelle colonne delle tabelle di  *AspnetUsers* tabella. La modifica sola in questo caso è quando si copia la password. Poiché l'algoritmo di crittografia per le password nelle appartenenze SQL utilizzato 'PasswordSalt' e 'PasswordFormat', copiamo che troppo e la password con hash in modo che può essere utilizzato per decrittografare la password dall'identità. Queste operazioni sono illustrate ulteriormente nell'articolo quando l'associazione di un hasher password personalizzata. 

Questo file di script è specifico per questo esempio. Per le applicazioni che hanno tabelle aggiuntive, gli sviluppatori possono seguire un approccio simile per aggiungere ulteriori proprietà sulla classe di modello utente ed eseguirne il mapping alle colonne nella tabella AspnetUsers. Per eseguire lo script,

1. Aprire Esplora Server. Espandere la connessione 'ApplicationServices' per visualizzare le tabelle. Fare clic con il pulsante destro sul nodo tabelle e selezionare l'opzione 'Nuova Query'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Nella finestra query, copiare e incollare l'intero script SQL dal file Migrations.sql. Eseguire il file di script facendo clic sul pulsante freccia 'Execute'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aggiornare la finestra di Esplora Server. Cinque nuove tabelle vengono create nel database.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Di seguito è come le informazioni nelle tabelle delle appartenenze SQL viene eseguito il mapping al nuovo sistema di identità.

    ASPNET\_ruoli -&gt; AspNetRoles

    ASP\_netUsers e asp\_netMembership -&gt; AspNetUsers

    ASPNET\_UserInRoles -&gt; AspNetUserRoles

    Come illustrato nella sezione precedente, le tabelle AspNetUserClaims e AspNetUserLogins sono vuote. Il campo 'Discriminatore' nella tabella AspNetUser deve corrispondere al nome di classe modello che viene definito come passaggio successivo. Anche la colonna PasswordHash è nel formato ' password crittografata | valore salt della password | formato della password'. Ciò consente di utilizzare logica speciale sul SQL appartenenza crittografia in modo che è possibile riutilizzare delle vecchie password. Che viene descritto avanti in questo articolo.

### <a name="creating-models-and-membership-pages"></a>Creazione di modelli e pagine di appartenenze

Come accennato in precedenza, la funzionalità di identità Usa Entity Framework per comunicare con il database per l'archiviazione di informazioni sull'account per impostazione predefinita. Per lavorare con i dati esistenti nella tabella, è necessario creare le classi modello che eseguire il mapping a tabelle e associare il sistema di identità. Come parte del contratto di identità, le classi di modello devono implementare le interfacce definite nella dll Identity.Core o estendono l'implementazione esistente di queste interfacce disponibili in EntityFramework.

Nel nostro esempio, le tabelle AspNetRoles, AspNetUserClaims, AspNetLogins e AspNetUserRole presentano le colonne che sono simili all'implementazione esistente del sistema di identità. Di conseguenza è possibile riutilizzare le classi esistenti per eseguire il mapping a queste tabelle. La tabella AspNetUser contiene alcune colonne aggiuntive che vengono usati per archiviare informazioni aggiuntive dalle tabelle di appartenenze SQL. Ciò può essere mappato tramite la creazione di una classe modello che estendono l'implementazione di 'IdentityUser' esistente e aggiungere le proprietà aggiuntive.

1. Crea modelli nella cartella di progetto e aggiungere una classe utente. Il nome della classe deve corrispondere i dati aggiunti nella colonna 'Discriminatore' della tabella 'AspnetUsers'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La classe dell'utente deve estendere la classe IdentityUser trovata nel *EntityFramework* dll. Dichiarare le proprietà nella classe che torna alle colonne AspNetUser. Le proprietà ID, nome utente, PasswordHash e SecurityStamp sono definite nel IdentityUser e pertanto sono stati omessi. Di seguito è il codice per la classe utente che dispone di tutte le proprietà

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Una classe DbContext di Entity Framework è necessario per rendere persistenti i dati nei modelli a tabelle e recuperare dati da tabelle per popolare i modelli. *EntityFramework* dll definisce la classe di IdentityDbContext che interagisce con le tabelle di identità per recuperare e archiviare le informazioni. IdentityDbContext&lt;tuser&gt; accetta una classe 'TUser' che può essere qualsiasi classe che estende la classe IdentityUser.

    Creare una nuova classe ApplicationDBContext che estende IdentityDbContext sotto la cartella 'Modelli', passando la classe 'User' creata nel passaggio 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Gestione degli utenti nel nuovo sistema di identità viene eseguita utilizzando la classe UserManager&lt;tuser&gt; classe definita nel *EntityFramework* dll. È necessario creare una classe personalizzata che estende UserManager, passando la classe 'User' creata nel passaggio 1.

    Nella cartella Models creare una nuova classe UserManager che estende UserManager&lt;utente&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Le password degli utenti dell'applicazione vengono crittografate e archiviate nel database. L'algoritmo di crittografia utilizzato nelle appartenenze SQL è diversa da quella nel nuovo sistema di identità. Per riutilizzare delle vecchie password è necessario decrittografare in modo selettivo le password quando gli utenti precedenti accedere utilizzando l'algoritmo di appartenenze SQL quando si utilizza l'algoritmo di crittografia nell'identità per i nuovi utenti.

    La classe UserManager ha una proprietà 'PasswordHasher', che archivia un'istanza di una classe che implementa l'interfaccia 'IPasswordHasher'. Viene utilizzato per crittografare/decrittografare le password durante le transazioni di autenticazione utente. La classe UserManager definita nel passaggio 3, creare una nuova classe SQLPasswordHasher e copiare il codice riportato di seguito.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Risolvere gli errori di compilazione importando gli spazi dei nomi System. Text e Cryptography.

    Il metodo EncodePassword crittografa la password in base all'implementazione di crittografia l'appartenenza SQL predefinita. Ciò viene ricavato dalla dll System. Web. Se l'app precedente utilizzata un'implementazione personalizzata deve essere riflessa qui. È necessario definire altri due metodi *HashPassword* e *VerifyHashedPassword* che utilizzano il *EncodePassword* metodo per l'hashing di una password o verificare un testo password con uno esistente nel database.

    Il sistema di appartenenze SQL utilizzato PasswordHash, PasswordSalt e PasswordFormat per l'hash della password immesse dagli utenti quando si registra o cambiare la password. Durante la migrazione dei tre campi vengono archiviati nella colonna PasswordHash nella tabella AspNetUser separata da di ' |' caratteri. Quando un utente esegue l'accesso e la password è questi campi, utilizziamo di crittografia di appartenenze SQL per verificare la password; in caso contrario, utilizziamo crittografia predefinita del sistema di identità per verificare la password. In questo modo utenti precedente operazione non deve modificare le relative password dopo che l'app viene eseguita la migrazione.
5. Dichiarare il costruttore per la classe UserManager e passare la variabile come la SQLPasswordHasher alla proprietà nel costruttore.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crea nuovo account di pagine di gestione

Il passaggio successivo della migrazione consiste nell'aggiungere le pagine di gestione di account che verranno consentono agli utenti di registrare e accedere. Le pagine dell'account precedente dall'appartenenza SQL usano i controlli che non funzionano con il nuovo sistema di identità. Per aggiungere il nuovo utente pagine di gestione di seguono l'esercitazione questo collegamento [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) a partire dal passaggio 'Aggiunta di Web Form per registrare gli utenti all'applicazione' perché già abbiamo creato il progetto e aggiunta NuGet pacchetti.

È necessario apportare alcune modifiche per l'esempio funzionare con il progetto che è disponibile qui.

- Register.aspx.cs e Login.aspx.cs code-behind uso di classi di `UserManager` dai pacchetti di identità per creare un utente. In questo esempio utilizzare la classe UserManager aggiunta nella cartella Models seguendo i passaggi indicati in precedenza.
- Utilizzare la classe utente creata anziché IdentityUser in Register.aspx.cs e Login.aspx.cs code-behind di classi. Questo hook della classe utente personalizzata il sistema di identità.
- La parte per creare il database può essere ignorata.
- Lo sviluppatore deve impostare l'ID applicazione per il nuovo utente in modo che corrisponda ID dell'applicazione corrente. Questa operazione può essere eseguita eseguendo una query ApplicationId per questa applicazione prima che venga creato un oggetto utente nella classe Register.aspx.cs e impostazione prima di creare l'utente. 

    Esempio:

    Definire un metodo nella pagina Register.aspx.cs per eseguire una query aspnet\_le applicazioni di tabella e ottenere l'Id applicazione in base al nome dell'applicazione

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Ora ottenere impostare questo valore sull'oggetto utente

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Utilizzare il vecchio nome utente e la password per l'accesso di un utente esistente. Utilizzare la pagina di registrazione per creare un nuovo utente. Verificare inoltre che gli utenti siano in ruoli come previsto.

Porting per il sistema di identità consente all'utente di aggiungere l'autenticazione Openauth (OAuth) all'applicazione. Vedere l'esempio [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) cui con OAuth abilitato.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione abbiamo anche mostrato come eseguire il porting agli utenti di appartenenza SQL di ASP.NET Identity, ma è non porta i dati di profilo. Nella prossima esercitazione verrà esaminato in portabilità dei dati di profilo dall'appartenenza SQL il nuovo sistema di identità.

È possibile lasciare commenti e suggerimenti nella parte inferiore di questo articolo.

*Grazie a Tom Dykstra e Rick Anderson per esaminare l'articolo.*
