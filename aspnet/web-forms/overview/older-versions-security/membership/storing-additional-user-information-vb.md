---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: L'archiviazione delle informazioni utente aggiuntive (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali guestbook. In questo modo, si esaminerà le opzioni diverse per modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 33e686cc3b977c6c740dfaf1057e1e399d5a298b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829602"
---
<a name="storing-additional-user-information-vb"></a>L'archiviazione delle informazioni utente aggiuntive (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali guestbook. In questo modo, verranno esaminate diverse opzioni per la modellazione di informazioni utente in un database e verrà quindi illustrato come associare i dati degli account utente creati dal framework di appartenenza.


## <a name="introduction"></a>Introduzione

ASP. Dell'appartenenza a .NET framework offre un'interfaccia flessibile per la gestione degli utenti. L'API di appartenenza include metodi per la convalida delle credenziali, il recupero delle informazioni sull'utente attualmente connesso, la creazione di un nuovo account utente e l'eliminazione di un account utente, tra gli altri. Ogni account utente nel framework di appartenenza contiene solo le proprietà necessarie per la convalida delle credenziali ed eseguire attività relative all'account utente essenziali. Questa situazione è comprovata dai metodi e proprietà del [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), che modella un account utente nel framework di appartenenza. Questa classe dispone di proprietà quali [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), e [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), e i metodi, ad esempio [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Spesso, le applicazioni devono archiviare informazioni utente aggiuntive non inclusi nel framework di appartenenza. Ad esempio, un rivenditore online potrebbe essere necessario consentire a ogni utente archiviare proprio gli indirizzi di spedizione e di fatturazione, le informazioni di pagamento, preferenze di recapito e numero di telefono del contatto. Inoltre, ogni oggetto order nel sistema è associata a un account utente specifico.

Il `MembershipUser` classe non includono proprietà quali `PhoneNumber` oppure `DeliveryPreferences` o `PastOrders`. In che modo è rilevare le informazioni utente necessari per l'applicazione e fare in modo che si integrano con il framework di appartenenza? In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali guestbook. In questo modo, verranno esaminate diverse opzioni per la modellazione di informazioni utente in un database e verrà quindi illustrato come associare i dati degli account utente creati dal framework di appartenenza. Iniziamo!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Passaggio 1: Creazione del modello di dati dell'applicazione Guestbook

Esistono varie tecniche che possono essere adottati per acquisire le informazioni utente in un database e associarlo con gli account utente creati dal framework di appartenenza. Per queste tecniche vengono illustrate, è necessario potenziare l'applicazione web di esercitazioni in modo che acquisisca una sorta di dati relativi all'utente. (Attualmente, il modello di dati dell'applicazione contiene solo le tabelle necessite per l'applicazione dei servizi di `SqlMembershipProvider`.)

È possibile creare un'applicazione guestbook molto semplice in cui un utente autenticato può lasciare un commento. Oltre ad archiviare guestbook commenti, è possibile consentire a ogni utente archiviare sua città Natale, home page e firma. Se fornito, città natale dell'utente, home page e firma apparirà su ogni messaggio che ha lasciato il guestbook.

### <a name="adding-theguestbookcommentstable"></a>Aggiunta di`GuestbookComments`tabella

Per acquisire i commenti guestbook, è necessario creare una tabella di database denominata `GuestbookComments` che contiene colonne, ad esempio `CommentId`, `Subject`, `Body`, e `CommentDate`. È anche necessario che ogni record `GuestbookComments` riferimento l'utente che ha lasciato il commento di tabella.

Per aggiungere questa tabella al database, passare al Database Explorer in Visual Studio e il drill down di `SecurityTutorials` database. Fare clic sulla cartella le tabelle e scegliere Aggiungi nuova tabella. Verrà visualizzata un'interfaccia che consente di definire le colonne per la nuova tabella.


[![Aggiungere una nuova tabella nel database SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Figura 1**: aggiungere una nuova tabella per il `SecurityTutorials` Database ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image3.png))


Successivamente, definire il `GuestbookComments`di colonne. Iniziare aggiungendo una colonna denominata `CommentId` di tipo `uniqueidentifier`. Questa colonna verrà identificare in modo univoco ogni commento nel guestbook, pertanto, non consentire `NULL` s e contrassegnarlo come chiave primaria della tabella. Anziché fornire un valore per il `CommentId` campo in ogni `INSERT`, è possibile indicare che un nuovo `uniqueidentifier` valore deve essere generato automaticamente per questo campo sul `INSERT` impostando il valore predefinito della colonna su `NEWID()`. Dopo aver aggiunto il primo campo, contrassegnandolo come chiave primaria e le impostazioni sul valore predefinito, la schermata dovrebbe essere simile allo screenshot illustrato nella figura 2.


[![Aggiungere una colonna primaria denominata CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Figura 2**: aggiungere una colonna primaria denominata `CommentId` ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image6.png))


Successivamente, aggiungere una colonna denominata `Subject` typu `nvarchar(50)` e una colonna denominata `Body` typu `nvarchar(MAX)`, divieto `NULL` s in entrambe le colonne. In seguito, aggiungere una colonna denominata `CommentDate` di tipo `datetime`. Non consentire `NULL` s e impostare il `CommentDate` il valore predefinito della colonna da `getdate()`.

Tutto questo punto, è sufficiente aggiungere una colonna che associa un account utente a ogni commento guestbook. Sarebbe un'opzione per aggiungere una colonna denominata `UserName` di tipo `nvarchar(256)`. Questa è una scelta appropriata quando si usa diverso da un provider di appartenenze di `SqlMembershipProvider`. Ma quando si usa il `SqlMembershipProvider`, come siamo ancora in questa serie di esercitazioni, il `UserName` colonna il `aspnet_Users` tabella è necessariamente essere univoci. Il `aspnet_Users` chiave primaria della tabella viene `UserId` ed è di tipo `uniqueidentifier`. Di conseguenza, il `GuestbookComments` tabella richiede una colonna denominata `UserId` typu `uniqueidentifier` (disattivazione `NULL` valori). Proseguire e aggiungere la colonna.

> [!NOTE]
> Come accennato nel [ *creazione dello Schema di appartenenza in SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md) dell'esercitazione, il framework di appartenenza è progettato per consentire più applicazioni web con diversi account utente di condividere lo stesso archivio utente. Ciò avviene tramite il partizionamento degli account utente in diverse applicazioni. E anche se ogni nome utente è garantito a essere univoco all'interno di un'applicazione, lo stesso nome utente può essere usato in diverse applicazioni usando lo stesso archivio utente. È un composto `UNIQUE` vincolo nel `aspnet_Users` tabella sul `UserName` e `ApplicationId` campi, ma non quello in solo il `UserName` campo. Di conseguenza, è possibile che aspnet\_tabella di utenti che i record di due (o più) con lo stesso `UserName` valore. C'è, tuttavia, un `UNIQUE` vincolo per il `aspnet_Users` della tabella `UserId` campo (perché è la chiave primaria). Oggetto `UNIQUE` vincolo è importante poiché senza di essa è possibile stabilire un vincolo di chiave esterna tra le `GuestbookComments` e `aspnet_Users` tabelle.


Dopo aver aggiunto il `UserId` colonna, salvare la tabella facendo clic sull'icona Salva nella barra degli strumenti. Denominare la nuova tabella `GuestbookComments`.

Abbiamo un ultimo problema allontanarsi con il `GuestbookComments` tabella: è necessario creare un [vincolo di chiave esterna](https://msdn.microsoft.com/library/ms175464.aspx) tra il `GuestbookComments.UserId` colonna e il `aspnet_Users.UserId` colonna. A tale scopo, fare clic sull'icona relazione nella barra degli strumenti per aprire la finestra di dialogo Relazioni chiavi esterne. (In alternativa, è possibile avviare questa finestra di dialogo selezionando il menu di progettazione tabelle e scegliendo le relazioni.)

Fare clic sul pulsante Aggiungi nell'angolo inferiore sinistro della finestra di dialogo Relazioni chiavi esterne. Verrà aggiunto un nuovo vincolo foreign key, anche se è necessario definire le tabelle che fanno parte della relazione.


[![Usare la finestra di dialogo relazioni di chiave esterna per gestire i vincoli di chiave esterna di una tabella](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Figura 3**: usare la finestra di dialogo di relazioni di chiave esterna per gestire i vincoli di chiave esterna di una tabella ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image9.png))


Successivamente, selezionare l'icona di puntini di sospensione nella riga "Specifiche di tabelle e colonne" a destra. Verrà avviata la finestra di dialogo tabelle e colonne, da cui è possibile specificare la tabella della chiave primaria e di colonna e la colonna chiave esterna dal `GuestbookComments` tabella. In particolare, selezionare `aspnet_Users` e `UserId` come la tabella di chiave primaria e la colonna, e `UserId` dal `GuestbookComments` tabella come colonna chiave esterna (vedere la figura 4). Dopo aver definito le tabelle di chiavi primarie ed esterne e colonne, fare clic su OK per tornare alla finestra di dialogo Relazioni chiavi esterne.


[![Stabilire un Foreign Key vincolo tra il aspnet_Users e GuesbookComments tabelle](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Figura 4**: stabilire un Foreign Key vincolo tra il `aspnet_Users` e `GuesbookComments` tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image12.png))


A questo punto il vincolo di chiave esterna è stato stabilito. La presenza di questo vincolo garantisce [integrità relazionale](http://en.wikipedia.org/wiki/Referential_integrity) tra le due tabelle dai tipi garantendo che non sarà mai una voce del guestbook che fa riferimento a un account utente inesistente. Per impostazione predefinita, un vincolo di chiave esterna sarà un record padre deve essere eliminato se sono presenti da record figlio corrispondenti. Vale a dire, se un utente effettua uno o più commenti guestbook e quindi si prova a eliminare l'account utente, l'eliminazione non riuscirà a meno che non vengono eliminati prima di tutto il suo commenti guestbook.

Vincoli di chiave esterna possono essere configurati per eliminare automaticamente i record figlio associate quando viene eliminato un record padre. In altre parole, è possibile configurare questo vincolo di chiave esterna in modo che le voci del guestbook dell'utente vengono eliminate automaticamente quando viene eliminato l'account utente. A tale scopo, espandere la sezione "INSERT e UPDATE specifica" e impostare la proprietà "Eliminazione della regola" Cascade.


[![Configurare il vincolo di chiave esterna per le eliminazioni a catena](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Figura 5**: configurare il vincolo di chiave esterna per l'eliminazione a catena ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image15.png))


Per salvare il vincolo foreign key, fare clic sul pulsante Chiudi per uscire da relazioni di chiave esterna. Scegliere l'icona Salva nella barra degli strumenti per salvare la tabella e questa relazione.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>L'archiviazione Town Home dell'utente, della home page e firma

Il `GuestbookComments` tabella viene illustrato come archiviare le informazioni che condivide una relazione uno-a-molti con gli account utente. Poiché ogni account utente potrebbe avere un numero arbitrario di commenti associati, questa relazione viene modellata tramite la creazione di una tabella per contenere il set di commenti che include una colonna che consente di tornare ogni commento a un utente specifico. Quando si usa la `SqlMembershipProvider`, questo collegamento viene stabilito meglio mediante la creazione di una colonna denominata `UserId` typu `uniqueidentifier` e un vincolo di chiave esterna tra la colonna e `aspnet_Users.UserId`.

È ora necessario associare tre colonne con ogni account utente per l'archiviazione dell'utente città natale della home page e firma, che verrà visualizzato nel suo commenti guestbook. Sono disponibili numerose modalità diverse per eseguire questa operazione:

- <strong>Aggiungere nuove colonne per la</strong><strong>`aspnet_Users`</strong><strong>oppure</strong><strong>`aspnet_Membership`</strong><strong>tabelle.</strong> È consigliabile non questo approccio perché modifica lo schema utilizzato dal `SqlMembershipProvider`. Questa decisione può tornare a recuperare è lungo la strada. Ad esempio, cosa accade se una versione futura di ASP.NET utilizza una diversa `SqlMembershipProvider` dello schema. Microsoft può includere uno strumento per eseguire la migrazione di ASP.NET.2.0 `SqlMembershipProvider` dei dati per il nuovo schema, ma se è stato modificato in ASP.NET 2.0 `SqlMembershipProvider` dello schema, tale conversione potrebbe non essere possibile.

- **Utilizzare le pagine ASP. Framework di profilo della rete, che definisce una proprietà del profilo per la città natale della home page e firma.** ASP.NET include un framework di profilo che è progettato per archiviare dati aggiuntivi specifici dell'utente. Ad esempio il framework di appartenenza, il framework del profilo viene compilato sopra il modello di provider. .NET Framework viene fornito con un `SqlProfileProvider` che archivi profiling dei dati in un database di SQL Server. In effetti, il database esiste già la tabella utilizzata dal `SqlProfileProvider` (`aspnet_Profile`), come è stato aggiunto al momento dell'aggiunta dei servizi applicativi di nuovo il [ *creazione dello Schema di appartenenza in SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md)tutorial.   
  Il vantaggio principale del framework di profilo è che consente agli sviluppatori di definire le proprietà del profilo in `Web.config` : nessun codice deve essere scritto per serializzare i dati del profilo da e verso l'archivio dati sottostante. In breve, è incredibilmente facile per definire un set di proprietà del profilo e utilizzarle nel codice. Tuttavia, il sistema di profilo lascia molti per essere utile se si desidera il controllo delle versioni, pertanto se si dispone di un'applicazione in cui si prevede di nuove proprietà specifiche dell'utente da aggiungere a un secondo momento o quelle esistenti per rimuovere o modificare, quindi il framework di profilo non può essere la  opzione migliore. Inoltre, il `SqlProfileProvider` archivia le proprietà del profilo in modo fortemente denormalizzato, rendendo possibile eseguire query direttamente sui dati del profilo (ad esempio, quanti utenti hanno una casa città di New York).   
  Per altre informazioni sul framework di profilo, consultare la sezione "Ulteriori letture" alla fine di questa esercitazione.

- <strong>Aggiungere queste tre colonne in una nuova tabella nel database e stabilire una relazione uno a uno tra questa tabella e</strong><strong>`aspnet_Users`</strong><strong>.</strong> Questo approccio comporta un lavoro maggiore rispetto a con il framework di profilo, ma offre la massima flessibilità nel modo in cui vengono modellate le proprietà aggiuntive dell'utente nel database. Si tratta dell'opzione che verrà usato in questa esercitazione.

Si creerà una nuova tabella denominata `UserProfiles` per salvare la città Natale, home page e firma per ogni utente. Pulsante destro del mouse sulla cartella tabelle nella finestra Esplora Database e scegliere di creare una nuova tabella. Assegnare un nome nella prima colonna `UserId` e impostarne il tipo di `uniqueidentifier`. Non consentire `NULL` valori e contrassegnare la colonna come una chiave primaria. Successivamente, aggiungere le colonne denominate: `HomeTown` di tipo `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; e la firma del tipo `nvarchar(500)`. Ognuna di queste tre colonne può accettare un `NULL` valore.


[![Creare la tabella UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Figura 6**: creare le `UserProfiles` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image18.png))


Salvataggio della tabella e denominarla `UserProfiles`. Infine, definire un vincolo di chiave esterna tra le `UserProfiles` della tabella `UserId` campo e il `aspnet_Users.UserId` campo. Come è stato fatto con il vincolo di chiave esterna tra le `GuestbookComments` e `aspnet_Users` tabelle, dispone di questo vincolo eliminazioni a catena. Poiché il `UserId` campo `UserProfiles` è la replica primaria chiave, in questo modo che non saranno presenti più record nel `UserProfiles` tabella per ogni account utente. Questo tipo di relazione viene definito come uno a uno.

Ora che abbiamo creato il modello di dati, siamo pronti a usarlo. Nei passaggi 2 e 3, si esaminerà come l'utente attualmente connesso può visualizzare e modificare le relative informazioni Town (città), home page e firma iniziale. Nel passaggio 4 si creerà l'interfaccia per gli utenti autenticati inviare nuovi commenti per il guestbook e visualizzare quelli esistenti.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Passaggio 2: Visualizzazione Home Town (città) dell'utente, della home page e firma

Esistono diversi modi per consentire all'utente attualmente connesso visualizzare e modificare le sue informazioni Town (città), home page e firma iniziale. È stato possibile creare manualmente l'interfaccia utente con casella di testo e controlli etichetta o è possibile usare uno dei controlli Web, ad esempio il controllo DetailsView i dati. Per eseguire il database `SELECT` e `UPDATE` istruzioni potremmo utilizzare ADO.NET il codice nella classe code-behind della pagina o, in alternativa, usare un approccio dichiarativo con SqlDataSource. Idealmente l'applicazione contiene un'architettura a più livelli, che è stato possibile richiamare a livello di codice da una classe code-behind della pagina o in modo dichiarativo tramite il controllo ObjectDataSource.

Poiché questa serie di esercitazioni è incentrato sull'autenticazione basata su form, autorizzazione, gli account utente e ruoli, non sarà disponibile una discussione dettagliata del motivo per cui un'architettura a più livelli è preferibile rispetto all'esecuzione di istruzioni SQL direttamente o le opzioni di accesso di dati diversi dalla pagina ASP.NET. Verrà illustrato l'uso di un controllo DetailsView e SqlDataSource-l'opzione più semplice e rapido- ma i concetti illustrati possono certamente essere applicati a logica alternativa di accesso ai dati e i controlli di Web. Per altre informazioni sull'utilizzo dei dati in ASP.NET, fare riferimento a mio *[usare i dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni.

Aprire il `AdditionalUserInfo.aspx` nella pagina la `Membership` cartella e aggiungere un controllo DetailsView alla pagina, impostare la relativa proprietà ID `UserProfile` e cancellare relativo `Width` e `Height` proprietà. Espandere DetailsView Smart Tag e scegliere di eseguirne l'associazione a un nuovo controllo origine dati. Verrà avviata la configurazione guidata origine dati (vedere la figura 7). Il primo passaggio viene richiesto di specificare il tipo di origine dati. Poiché si intende connettersi direttamente al `SecurityTutorials` del database, scegliere l'icona di Database, specificando le `ID` come `UserProfileDataSource`.


[![Aggiungere un nuovo controllo SqlDataSource denominato UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Figura 7**: aggiungere un nuovo controllo SqlDataSource denominato `UserProfileDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image21.png))


Nella schermata successiva richiede il database da utilizzare. Abbiamo già definito in una stringa di connessione `Web.config` per il `SecurityTutorials` database. Questo nome di stringa di connessione: `SecurityTutorialsConnectionString` : deve essere incluso nell'elenco di riepilogo a discesa. Selezionare questa opzione e fare clic su Avanti.


[![Scegliere SecurityTutorialsConnectionString dall'elenco a discesa](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Figura 8**: scegliere `SecurityTutorialsConnectionString` dall'elenco a discesa scegliere ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image24.png))


La schermata successiva viene chiesto di specificare per specificare la tabella e le colonne a query. Scegliere il `UserProfiles` tabella nell'elenco a discesa e selezionare tutte le colonne.


[![Portare indietro tutte le colonne dalla tabella UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Figura 9**: portare nuovamente tutte le colonne dai `UserProfiles` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image27.png))


La query corrente nella figura 9 restituisce *tutte* dei record di `UserProfiles`, ma si è interessati solo record dell'utente attualmente connesso. Per aggiungere un `WHERE` clausola, fare clic sui `WHERE` pulsante per visualizzare il componente `WHERE` clausola dialogo (vedere la figura 10). Qui è possibile selezionare la colonna da filtrare, l'operatore e l'origine del parametro filter. Selezionare `UserId` come la colonna e "=" come l'operatore.

Sfortunatamente non è Nessuna origine del parametro predefinito per restituire l'utente attualmente connesso `UserId` valore. È necessario ottenere questo valore a livello di codice. Pertanto, impostare l'elenco di riepilogo a discesa di origine da "None", fare clic su Aggiungi pulsante per aggiungere il parametro e quindi fare clic su OK.


[![Aggiungere un parametro di filtro nella colonna UserId](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Figura 10**: aggiungere un parametro di filtro sul `UserId` colonna ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image30.png))


Fare clic su OK verrà di nuovo la schermata illustrata nella figura 9. Questa volta, tuttavia, la query SQL nella parte inferiore della schermata deve includere un `WHERE` clausola. Fare clic su Avanti per passare alla schermata di "Query di Test". Qui è possibile eseguire la query e visualizzare i risultati. Fare clic su Fine per completare la procedura guidata.

Dopo aver completato la configurazione guidata origine dati, Visual Studio crea il controllo SqlDataSource basato sulle impostazioni specificate nella procedura guidata. Inoltre, manualmente aggiunge BoundField di DetailsView per ogni colonna restituita dal SqlDataSource `SelectCommand`. Non è necessario per mostrare il `UserId` campo nel controllo DetailsView, poiché l'utente non deve necessariamente conoscere questo valore. È possibile rimuovere il campo direttamente dal markup dichiarativo del controllo DetailsView o facendo clic su "Modifica campi" collegare dal suo Smart Tag.

A questo punto markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

È necessario impostare a livello di codice del controllo SqlDataSource `UserId` parametro per l'utente attualmente connesso `UserId` prima che vengano selezionati i dati. Questo può essere eseguito mediante la creazione di un gestore eventi per il SqlDataSource `Selecting` codice presenti eventi e aggiungendo il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Il codice sopra riportato inizia ottenendo un riferimento all'utente attualmente connesso tramite una chiamata di `Membership` della classe `GetUser` (metodo). Restituisce un `MembershipUser` dell'oggetto, la cui proprietà `ProviderUserKey` proprietà contiene il `UserId`. Il `UserId` valore viene quindi assegnato a per SqlDataSource `@UserId` parametro.

> [!NOTE]
> Il `Membership.GetUser()` metodo restituisce informazioni sull'utente attualmente connesso. Se un utente anonimo è sinonimo di visitare la pagina, verrà restituito un valore di `Nothing`. In tal caso, ciò causerà un `NullReferenceException` nella riga successiva del codice durante il tentativo di leggere il `ProviderUserKey` proprietà. Naturalmente, non è necessario preoccuparsi `Membership.GetUser()` nulla nella restituzione di `AdditionalUserInfo.aspx` pagina perché è stato configurato l'autorizzazione URL in un'esercitazione precedente in modo che solo gli utenti autenticati è stato possibile accedere alle risorse ASP.NET in questa cartella. Se si desidera accedere alle informazioni sull'utente attualmente connesso in una pagina in cui è consentito l'accesso anonimo, assicurarsi di verificare che il `MembershipUser` oggetto restituito dal `GetUser()` metodo non esegue alcuna operazione prima di fare riferimento alle relative proprietà.


Se visitano i `AdditionalUserInfo.aspx` pagina tramite un browser si noterà una pagina vuota poiché è stato ancora aggiunto tutte le righe per il `UserProfiles` tabella. Nel passaggio 6 si esaminerà come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga per il `UserProfiles` tabella quando viene creato un nuovo account utente. Per il momento, tuttavia, è necessario creare manualmente un record nella tabella.

Passare al Database Explorer in Visual Studio ed espandere la cartella di tabelle. Fare clic sui `aspnet_Users` tabella e scegliere "Mostra dati tabella" per visualizzare i record nella tabella, eseguire la stessa operazione il `UserProfiles` tabella. Figura 11 Mostra questi risultati quando si Affianca verticalmente. Nel mio database sono presenti attualmente `aspnet_Users` record di Bruce, Fred e Tito, ma nessun record nel `UserProfiles` tabella.


[![Il contenuto della finestra di aspnet_Users e UserProfiles tabelle vengono visualizzate](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Figura 11**: il contenuto del `aspnet_Users` e `UserProfiles` vengono visualizzate tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image33.png))


Aggiungere un nuovo record per il `UserProfiles` manualmente digitando i valori per la tabella di `HomeTown`, `HomepageUrl`, e `Signature` campi. Il modo più semplice per ottenere un valore valido `UserId` valore nel nuovo `UserProfiles` record consiste nel selezionare il `UserId` campo da un account utente specifico nel `aspnet_Users` tabella e copiare e incollare nel `UserId` campo `UserProfiles`. Figura 12 vengono illustrate le `UserProfiles` tabella dopo l'aggiunta di un nuovo record per Bruce.


[![È stato aggiunto un Record a UserProfiles per Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Figura 12**: un Record è stato aggiunto al `UserProfiles` per Bruce ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image36.png))


Tornare al `AdditionalUserInfo.aspx page`, connesso come Bruce. Come illustrato nella figura 13, vengono visualizzate le impostazioni di Bruce.


[![L'utente attualmente visita è illustrato le impostazioni di His](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Figura 13**: attualmente visita utente è illustrato le impostazioni di His ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Andare avanti e manualmente aggiungere record di `UserProfiles` tabella per ogni utente di appartenenza. Nel passaggio 6 si esaminerà come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga per il `UserProfiles` tabella quando viene creato un nuovo account utente.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Passaggio 3: Consentendo all'utente di modificare il suo Home Town (città), home page e firma

A questo punto l'utente attualmente connesso può visualizzare le città Natale, home page e impostazione della firma, ma non ancora modificarle. Aggiorniamo il controllo DetailsView in modo che i dati possono essere modificati.

La prima cosa dobbiamo fare è aggiungere un' `UpdateCommand` per SqlDataSource, specificando il `UPDATE` istruzione da eseguire e i relativi parametri corrispondenti. Selezionare SqlDataSource e, dalla finestra delle proprietà, fare clic sui puntini di sospensione accanto alla proprietà UpdateQuery per visualizzare la finestra di dialogo Editor comandi e parametri. Immettere le informazioni seguenti `UPDATE` istruzione nella casella di testo:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Successivamente, fare clic sul pulsante "Aggiorna parametri", che verrà creato un parametro del controllo SqlDataSource `UpdateParameters` raccolta per ognuno dei parametri in di `UPDATE` istruzione. Lasciare l'origine per tutti i set di parametri su None e fare clic su OK per completare la finestra di dialogo.


[![Specificare il SqlDataSource UpdateCommand e UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Figura 14**: specificare il SqlDataSource `UpdateCommand` e `UpdateParameters` ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image42.png))


A causa delle aggiunte sono apportate al controllo SqlDataSource, DetailsView controllo ora supporta la modifica. Dallo Smart Tag di DetailsView, selezionare la casella di controllo "Abilita modifica". Verrà aggiunto un CommandField al controllo `Fields` raccolta con il relativo `ShowEditButton` proprietà è impostata su True. Si esegue il rendering di un pulsante di modifica quando DetailsView viene visualizzata in modalità di sola lettura e aggiornamento e i pulsanti Annulla quando vengono visualizzate nella modalità di modifica. Invece di richiedere all'utente di fare clic su Modifica, tuttavia, possiamo di rendering DetailsView in uno stato "sempre modificabile" tramite l'impostazione del controllo DetailsView [ `DefaultMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) a `Edit`.

Con queste modifiche, markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Si noti l'aggiunta del CommandField e `DefaultMode` proprietà.

Proseguo e testare questa pagina tramite un browser. Gli utenti in visita a un utente che dispone di un record corrispondente in `UserProfiles`, vengono visualizzate le impostazioni dell'utente in un'interfaccia modificabile.


[![DetailsView esegue il rendering di un'interfaccia modificabile](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Figura 15**: DetailsView esegue il rendering di un'interfaccia modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image45.png))


Provare a modificare i valori e fare clic sul pulsante di aggiornamento. Viene visualizzato come se non accade nulla. È presente un postback e i valori vengono salvati nel database, ma non vi è alcun feedback visivo che si è verificato durante il salvataggio.

Per risolvere questo problema, tornare a Visual Studio e aggiungere un controllo etichetta di sopra di DetailsView. Impostare relativo `ID` per `SettingsUpdatedMessage`, la relativa `Text` proprietà su "sono state aggiornate le impostazioni," e la relativa `Visible` e `EnableViewState` proprietà `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

È necessario visualizzare il `SettingsUpdatedMessage` etichettare ogni volta che viene aggiornato il DetailsView. A tale scopo, creare un gestore eventi per DetailsView `ItemUpdated` eventi e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Tornare al `AdditionalUserInfo.aspx` pagina tramite un browser e aggiornare i dati. Questa volta, viene visualizzato un messaggio di stato utile.


[![Un breve messaggio viene visualizzato quando l'aggiornamento delle impostazioni](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Figura 16**: viene visualizzato un breve messaggio quando vengono aggiornate le impostazioni ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> Il controllo DetailsView di modifica della interfaccia lascia un possono essere notevolmente. Usa dimensioni standard nelle caselle di testo, ma il campo della firma dovrebbe essere una casella di testo su più righe. RegularExpressionValidator deve essere utilizzato per assicurarsi che l'URL della home page, se inserito, inizia con "http://" o "https://". Inoltre, poiché DetailsView dispone di controllo relativi `DefaultMode` impostata su `Edit`, il pulsante Annulla non ha alcun effetto. Devono entrambi essere rimossi o, quando si fa clic, reindirizzare l'utente a un'altra pagina (ad esempio `~/Default.aspx`). Posso lasciare questi miglioramenti come esercizio per il lettore.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Aggiunta di un collegamento per il`AdditionalUserInfo.aspx`pagina nella pagina Master

Attualmente, il sito Web non fornisce i collegamenti al `AdditionalUserInfo.aspx` pagina. L'unico modo per raggiungere lo è per immettere l'URL della pagina direttamente nella barra degli indirizzi del browser. È possibile aggiungere un collegamento a questa pagina per il `Site.master` pagina master.

È importante ricordare che la pagina master contiene un controllo LoginView Web nel relativo `LoginContent` ContentPlaceHolder che consente di visualizzare i tag diversi per i visitatori autenticati e anonimi. Aggiornare il controllo LoginView `LoggedInTemplate` per includere un collegamento per il `AdditionalUserInfo.aspx` pagina. Dopo aver apportato queste modifiche di LoginView markup dichiarativo del controllo dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Si noti l'aggiunta del `lnkUpdateSettings` controllo collegamento ipertestuale per la `LoggedInTemplate`. Con questo collegamento posto, gli utenti autenticati possono passare rapidamente alla pagina per visualizzare e modificare le relative impostazioni Town (città), home page e firma iniziale.

## <a name="step-4-adding-new-guestbook-comments"></a>Passaggio 4: Aggiunta di nuovi commenti Guestbook

Il `Guestbook.aspx` pagina è in cui gli utenti autenticati possono visualizzare il guestbook e lasciare un commento. Iniziamo con la creazione dell'interfaccia per aggiungere nuovi commenti guestbook.

Aprire il `Guestbook.aspx` pagina in Visual Studio e creare un'interfaccia utente costituito da due controlli TextBox, uno per il soggetto del nuovo commento e uno per il relativo corpo. Impostare il primo controllo casella di testo `ID` proprietà `Subject` e il relativo `Columns` proprietà a 40, imposta secondo `ID` a `Body`, la relativa `TextMode` a `MultiLine`e il relativo `Width` e `Rows` la proprietà "95%" e 8, rispettivamente. Per completare l'interfaccia utente, aggiungere un controllo Web pulsante denominato `PostCommentButton` e impostare il `Text` proprietà su "Post Comment Your".

Poiché ogni commento guestbook richiede un oggetto e corpo, aggiungere un RequiredFieldValidator per ognuna delle caselle di testo. Impostare il `ValidationGroup` proprietà di questi controlli per "EnterComment"; in modo analogo, impostare il `PostCommentButton` del controllo `ValidationGroup` proprietà su "EnterComment". Per altre informazioni su ASP. I controlli di convalida della rete, estrazione [convalida dei Form in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e il [esercitazione su controlli di convalida Server](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) sul [W3Schools](http://www.w3schools.com/).

Dopo la creazione dell'interfaccia utente markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Con l'interfaccia utente completa, l'attività successiva consiste nell'inserire un nuovo record nel `GuestbookComments` tabella quando il `PostCommentButton` si fa clic. Ciò può essere eseguita in diversi modi: è possibile scrivere codice ADO.NET nel pulsante `Click` gestore dell'evento; è possibile aggiungere un controllo SqlDataSource per la pagina, configurare relativo `InsertCommand`e quindi chiamare relativo `Insert` metodo dal `Click` evento gestore. o è stato possibile compilare un livello intermedio che è stato responsabile dell'inserimento di nuovi commenti guestbook e richiamare questa funzionalità dal `Click` gestore dell'evento. Poiché è stata esaminata tramite un SqlDataSource nel passaggio 3, utilizziamo qui il codice ADO.NET.

> [!NOTE]
> Le classi ADO.NET usate per accedere a livello di codice ai dati da un database Microsoft SQL Server si trovano nel `System.Data.SqlClient` dello spazio dei nomi. Potrebbe essere necessario importare questo spazio dei nomi in classe code-behind della pagina (ad esempio, `Imports System.Data.SqlClient`).


Creare un gestore eventi per il `PostCommentButton`del `Click` eventi e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

Il `Click` gestore eventi controlla prima di tutto che i dati specificati dall'utente siano validi. In caso contrario, il gestore eventi prima di inserire un record. Supponendo che i dati forniti siano validi, l'utente attualmente connesso `UserId` valore viene recuperato e archiviato nel `currentUserId` variabile locale. Questo valore è necessario perché è necessario specificare una `UserId` valore durante l'inserimento di un record in `GuestbookComments`.

In seguito, la stringa di connessione per il `SecurityTutorials` database viene recuperato dal `Web.config` e il `INSERT` istruzione SQL è specificata. Oggetto `SqlConnection` oggetto viene quindi creato e aperto. Successivamente, una `SqlCommand` costruito e i valori per i parametri utilizzati nel `INSERT` query sono assegnati. Il `INSERT` istruzione viene quindi eseguita e la connessione è chiusa. Alla fine del gestore dell'evento, il `Subject` e `Body` caselle di testo `Text` le proprietà vengono eliminate in modo che i valori dell'utente non sono mantenuti tra il postback.

Proseguo e testare questa pagina in un browser. Poiché in questa pagina la `Membership` cartella non è accessibile ai visitatori anonimi. Di conseguenza, dovrai prima di tutto accedere (se non è ancora disponibile). Immettere un valore nel `Subject` e `Body` caselle di testo e fare clic su di `PostCommentButton` pulsante. Questa operazione comporterà un nuovo record da aggiungere alla `GuestbookComments`. Durante il postback, oggetto e corpo specificato vengono cancellati da caselle di testo.

Dopo aver scelto il `PostCommentButton` pulsante non vi è alcun feedback visivo che è stato aggiunto il commento per il guestbook. È comunque necessario aggiornare questa pagina per visualizzare i commenti guestbook esistenti, che verranno eseguite operazioni nel passaggio 5. Una volta è eseguire questa operazione, il commento just-aggiunto verrà visualizzato nell'elenco dei commenti, che fornisce feedback visivo adeguate. Per ora, confermare che è stato salvato il commento guestbook esaminando il contenuto del `GuestbookComments` tabella.

Figura 17 mostra i contenuti del `GuestbookComments` tabella dopo che sono stati lasciati due commenti.


[![È possibile visualizzare i commenti Guestbook nella tabella GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Figura 17**: È possibile visualizzare i commenti Guestbook il `GuestbookComments` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Se un utente tenta di inserire un commento guestbook contenente potenzialmente pericoloso markup – ad esempio HTML – ASP.NET genererà un `HttpRequestValidationException`. Per altre informazioni su questa eccezione, perché viene generata, e su come consentire agli utenti di inviare valori potenzialmente pericolosi, consultare il [white paper di convalida richiesta](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Passaggio 5: Elenca i commenti Guestbook esistente

Oltre a lasciando commenti, un utente visita il `Guestbook.aspx` pagina deve inoltre essere in grado di visualizzare commenti esistenti del guestbook. A tale scopo, aggiungere un controllo ListView denominato `CommentList` nella parte inferiore della pagina.

> [!NOTE]
> Il controllo ListView è nuovo in ASP.NET versione 3.5. È progettato per visualizzare un elenco di elementi in un layout molto personalizzabile e flessibile, offrendo comunque in incorporati modifica, inserimento, eliminazione, paging e ordinamento funzionalità quali il controllo GridView. Se si usa ASP.NET 2.0, è necessario usare invece il controllo DataList o Repeater. Per altre informazioni sull'uso di ListView, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog [il controllo asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e il mio articolo [visualizzazione dei dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Aprire Smart Tag del ListView e, dall'elenco a discesa elenco scegliere l'origine dati, associare il controllo a una nuova origine dati. Come abbiamo visto nel passaggio 2, verrà avviata la configurazione guidata origine dati. Selezionare l'icona di Database, assegnare un nome risultante SqlDataSource `CommentsDataSource`, fare clic su OK. Selezionare quindi il `SecurityTutorialsConnectionString` connessione stringa nell'elenco a discesa e fare clic su Avanti.

A questo punto nel passaggio 2 è stata specificata i dati per eseguire query per il prelievo di `UserProfiles` tabella nell'elenco a discesa e selezionando le colonne da restituire (vedere la figura 9). Questa volta, tuttavia, si vuole elaborare un'istruzione SQL che esegue il pull nuovamente non solo i record da `GuestbookComments`, ma anche dell'autore principale Town (città), home page, firma e il nome utente. Pertanto, selezionare il pulsante di opzione "Specifica un'istruzione SQL personalizzata o una stored procedure" e fare clic su Avanti.

Verrà visualizzata la schermata "Definiscono le istruzioni o Stored procedure personalizzate". Fare clic sul pulsante Generatore delle Query per creare graficamente la query. Il generatore delle Query che richiede di specificare le tabelle a cui che si vuole eseguire una query viene avviata. Selezionare il `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle e fare clic su OK. Tutte le tre tabelle verrà aggiunto all'area di progettazione. Poiché esistono vincoli di chiave esterna tra le `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle, il generatore delle Query automaticamente `JOIN` s queste tabelle.

Tutto ciò che resta consiste nello specificare le colonne da restituire. Dal `GuestbookComments` tabella selezionare il `Subject`, `Body`, e `CommentDate` colonne; restituiscono il `HomeTown`, `HomepageUrl`, e `Signature` colonne dal `UserProfiles` tabella; e restituire `UserName` da `aspnet_Users`. Inoltre, aggiungere "`ORDER BY CommentDate DESC`" alla fine del `SELECT` eseguire una query in modo che i post più recenti vengono restituiti per primi. Dopo aver apportato queste selezioni, l'interfaccia del generatore delle Query dovrebbe essere simile allo screenshot nella figura 18.


[![La Query Constructed unisce il GuestbookComments UserProfiles e aspnet_Users tabelle](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Figura 18**: la Query costruita `JOIN` s le `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image54.png))


Fare clic su OK per chiudere la finestra di generatore di Query e tornare alla schermata "Definiscono le istruzioni o Stored procedure personalizzate". Fare clic su Avanti per passare alla schermata di "Test Query", in cui è possibile visualizzare i risultati della query facendo clic sul pulsante Query di Test. Quando si è pronti, fare clic su Fine per completare la procedura guidata Configura origine dati.

Quando viene completata la procedura guidata Configura origine dati del passaggio 2, il controllo DetailsView associato `Fields` raccolta è stata aggiornata per includere un BoundField per ogni colonna restituita dalla `SelectCommand`. Tuttavia, il ListView, rimane invariato; è comunque necessario definire il layout. Layout del ListView può essere creata manualmente tramite il relativo markup dichiarativo o dall'opzione "Configura ListView" nel suo Smart Tag. In genere preferisce che definisce il codice manualmente, ma usare qualsiasi metodo è più semplice.

Ho fatto alla fine con i seguenti `LayoutTemplate`, `ItemTemplate`, e `ItemSeparatorTemplate` per il controllo ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

Il `LayoutTemplate` definisce il markup generato dal controllo, mentre il `ItemTemplate` esegue il rendering di ogni elemento restituito da SqlDataSource. Il `ItemTemplate`del markup risultante viene inserito nella `LayoutTemplate`del `itemPlaceholder` controllo. Oltre al `itemPlaceholder`, il `LayoutTemplate` include un controllo DataPager, che limita il ListView per mostrare solo 10 guestbook commenti per ogni pagina (predefinito) ed esegue il rendering di un'interfaccia di paging.

Mia `ItemTemplate` soggetto che ogni guestbook del commento consente di visualizzare un `<h4>` elemento con il corpo situato di sotto l'oggetto. Si noti che la sintassi usata per visualizzare il corpo avrà i dati restituiti dai `Eval("Body")` istruzione di associazione dati, lo converte in una stringa e interruzioni di riga sostituisce con i `<br />` elemento. Questa conversione è necessaria per visualizzare le interruzioni di riga immesse durante l'invio del commento perché lo spazio vuoto viene ignorato dal codice HTML. Firma dell'utente viene visualizzata sotto il corpo in corsivo, seguita da città natale dell'utente, un collegamento suo della home page, la data e ora che il commento è stato eseguito e il nome utente della persona che ha lasciato il commento.

Si consiglia di visualizzare la pagina tramite un browser. È consigliabile vedere i commenti che è stato aggiunto per il guestbook nel passaggio 5 visualizzati qui.


[![Guestbook ora consente di visualizzare i commenti del Guestbook](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Figura 19**: `Guestbook.aspx` ora visualizza i commenti del Guestbook ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image57.png))


Provare ad aggiungere un nuovo commento per il guestbook. Fare clic su di `PostCommentButton` eseguito il postback di pulsante della pagina e il commento viene aggiunto al database, ma il controllo ListView non viene aggiornato per mostrare il nuovo commento. Questo problema può essere risolto in uno dei modi:

- L'aggiornamento di `PostCommentButton` del pulsante `Click` gestore dell'evento in modo che venga richiamato del controllo ListView `DataBind()` metodo dopo aver inserito il nuovo commento nel database, o
- Impostazione del controllo ListView `EnableViewState` proprietà `False`. Questo approccio funziona perché disabilitando lo stato di visualizzazione del controllo, è necessario riassociarlo ai dati sottostanti in ogni postback.

Sito Web di esercitazione possono essere scaricati da questa esercitazione illustra entrambe le tecniche. Del controllo ListView `EnableViewState` proprietà `False` e il codice necessario per riassociare a livello di codice i dati di ListView è presente nel `Click` gestore eventi, ma viene impostata come commento.

> [!NOTE]
> Attualmente il `AdditionalUserInfo.aspx` pagina consente di visualizzare e modificare le relative impostazioni Town (città), home page e firma iniziale. Potrebbe essere opportuno aggiornare `AdditionalUserInfo.aspx` visualizzare usato per l'accesso nei commenti guestbook dell'utente. Vale a dire, oltre a esaminare e modificare le informazioni, un utente può visitare la `AdditionalUserInfo.aspx` pagina per visualizzare quali guestbook commenti che viene eseguita in precedenza. Lasciare come esercizio per il lettore di interesse.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Passaggio 6: Personalizzare il controllo CreateUserWizard per includere un'interfaccia per la home page Town (città), home page e firma

Il `SELECT` query usata per il `Guestbook.aspx` pagina Usa un `INNER JOIN` per combinare i record correlati tra il `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle. Se un utente che non dispone di alcun record in `UserProfiles` esegue una guestbook commento, il commento non verranno visualizzato nella finestra di ListView perché il `INNER JOIN` restituisce solo `GuestbookComments` al momento esistono record corrispondenti in record `UserProfiles` e `aspnet_Users`. E come abbiamo visto nel passaggio 3, se un utente non ha un record `UserProfiles` Anna non è possibile visualizzare o modificare propria le impostazioni nel `AdditionalUserInfo.aspx` pagina.

Inutile a dirsi, a causa di un nostro progettazione decisioni è importante che ogni account utente nel sistema di appartenenze esiste un corrispondente registrano il `UserProfiles` tabella. Ciò che vogliamo è per un record corrispondente da aggiungere alla `UserProfiles` ogni volta che viene creato un nuovo account utente di appartenenza tramite CreateUserWizard.

Come descritto nel [ *creazione degli account utente* ](creating-user-accounts-vb.md) esercitazione dopo che il nuovo account utente di appartenenza viene creato il controllo CreateUserWizard genera relativo [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). È possibile creare un gestore eventi per questo evento, ottenere l'ID utente per l'utente appena creato e quindi inserire un record nel `UserProfiles` tabella con i valori predefiniti per il `HomeTown`, `HomepageUrl`, e `Signature` colonne. Inoltre, è possibile richiedere all'utente per questi valori personalizzando l'interfaccia del controllo CreateUserWizard per includere altre caselle di testo.

Questa sezione descrive come aggiungere una nuova riga per il `UserProfiles` nella tabella di `CreatedUser` gestore dell'evento con i valori predefiniti. In seguito, si vedrà come personalizzare l'interfaccia utente del controllo CreateUserWizard per includere i campi del modulo aggiuntiva per la raccolta di città natale del nuovo utente, della home page e firma.

### <a name="adding-a-default-row-touserprofiles"></a>Aggiunta di una riga predefinita`UserProfiles`

Nel [ *creazione degli account utente* ](creating-user-accounts-vb.md) esercitazione è stato aggiunto un controllo CreateUserWizard per la `CreatingUserAccounts.aspx` nella pagina di `Membership` cartella. Per avere CreateUserWizard controllo del codice aggiungere un record a `UserProfiles` tabella al momento della creazione di account utente, è necessario aggiornare la funzionalità del controllo CreateUserWizard. Invece di effettuare queste modifiche per il `CreatingUserAccounts.aspx` pagina, aggiungiamo invece un nuovo controllo CreateUserWizard per `EnhancedCreateUserWizard.aspx` pagina e apportare le modifiche per questa esercitazione non esiste.

Aprire il `EnhancedCreateUserWizard.aspx` pagina in Visual Studio e trascinare un controllo CreateUserWizard dalla casella degli strumenti nella pagina. Impostare il controllo CreateUserWizard `ID` proprietà `NewUserWizard`. Come accennato nel [ *creazione degli account utente* ](creating-user-accounts-vb.md) dell'esercitazione, l'interfaccia utente predefinita del CreateUserWizard richiede il visitatore per le informazioni necessarie. Dopo che queste informazioni sono state fornite, il controllo crea internamente un nuovo account utente nel framework di appartenenza, tutto senza dover scrivere una singola riga di codice.

Il controllo CreateUserWizard genera un numero di eventi durante il flusso di lavoro. Dopo un visitatore fornisce le informazioni sulla richiesta e invia il form, il controllo CreateUserWizard viene inizialmente attivato relativi [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si è verificato un problema durante il processo di creazione, la [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) è attivato; tuttavia, se l'utente è stato creato, il [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) viene generato. Nel [ *creazione degli account utente* ](creating-user-accounts-vb.md) esercitazione è stato creato un gestore eventi per il `CreatingUser` eventi per garantire che il nome utente fornito non contiene un iniziale o finale spazi e che la nome utente non venivano visualizzati in un punto qualsiasi nella password.

Per aggiungere una riga nel `UserProfiles` tabella per l'utente appena creato, è necessario creare un gestore eventi per il `CreatedUser` evento. Entro l'ora di `CreatedUser` dell'evento, l'account utente è già stato creato nel framework di appartenenza ci permette di recuperare il valore di ID utente dell'account.

Creare un gestore eventi per il `NewUserWizard`del `CreatedUser` eventi e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Esseri di codice precedente recuperando l'ID utente dell'account utente appena aggiunto. Questa operazione viene eseguita tramite il `Membership.GetUser(username)` per restituire informazioni su un utente specifico e viene quindi utilizzato il `ProviderUserKey` proprietà da recuperare le ID utente. Il nome utente immesso dall'utente nel controllo CreateUserWizard è disponibile tramite il [ `UserName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Successivamente, la stringa di connessione viene recuperata dal `Web.config` e il `INSERT` istruzione è specificata. Gli oggetti ADO.NET necessari vengono create istanze e il comando eseguito. Il codice assegna un [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) dell'istanza per il `@HomeTown`, `@HomepageUrl`, e `@Signature` parametri, che ha l'effetto di inserimento del database `NULL` i valori per il `HomeTown`, `HomepageUrl`, e `Signature` campi.

Visita il `EnhancedCreateUserWizard.aspx` pagina tramite un browser e creare un nuovo account utente. Al termine dell'operazione, tornare a Visual Studio ed esaminare il contenuto del `aspnet_Users` e `UserProfiles` tabelle (come abbiamo fatto nella figura 12). Si dovrebbe vedere il nuovo account utente in `aspnet_Users` e un oggetto corrispondente `UserProfiles` riga (con `NULL` i valori per `HomeTown`, `HomepageUrl`, e `Signature`).


[![Sono stati aggiunti un nuovo Account utente e UserProfiles Record](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Figura 20**: un nuovo Account utente e `UserProfiles` Record sono stati aggiunti ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image60.png))


Dopo che il visitatore ha fornito il suo nuove informazioni sull'account e fatto clic sul pulsante "Create User", l'account utente viene creata e aggiunta di una riga per il `UserProfiles` tabella. Visualizza quindi CreateUserWizard relativo `CompleteWizardStep`, che consente di visualizzare un messaggio di conferma e un pulsante Continua. Facendo clic sul pulsante Continua, un postback, ma viene eseguita alcuna azione, lasciando all'utente bloccati sul `EnhancedCreateUserWizard.aspx` pagina.

È possibile specificare un URL per indirizzare l'utente quando si fa clic sul pulsante Continua tramite il controllo CreateUserWizard [ `ContinueDestinationPageUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Impostare il `ContinueDestinationPageUrl` proprietà su "~ / Membership/AdditionalUserInfo.aspx". Questa operazione richiede all'utente di nuova `AdditionalUserInfo.aspx`, in cui possono visualizzare e aggiornare le proprie impostazioni.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizzazione dell'interfaccia del CreateUserWizard al messaggio di richiesta per la home page Town (città), home page e firma il nuovo utente

Interfaccia predefinita del controllo CreateUserWizard è sufficiente per gli scenari di creazione di account semplice in cui devono essere raccolti solo informazioni sull'account utente core, ad esempio nome utente, password e il messaggio di posta elettronica. Ma cosa succede se volessimo richiedere il visitatore per immettere sua città Natale, home page e firma durante la creazione il proprio account? È possibile personalizzare l'interfaccia del controllo CreateUserWizard per raccogliere informazioni aggiuntive al momento dell'iscrizione e queste informazioni possono essere usate nel `CreatedUser` gestore eventi per inserire record aggiuntivi nel database sottostante.

Il controllo CreateUserWizard estende il controllo procedura guidata ASP.NET, che è un controllo che consente a uno sviluppatore di pagina definire una serie di ordinato `WizardSteps`. Controllo della procedura guidata esegue il rendering di passaggio attivo e offre un'interfaccia di navigazione che consente il visitatore per spostarsi tra questi passaggi. Il controllo Wizard è ideale per la suddivisione di un'attività di lunga durata in diversi passaggi brevi. Per altre informazioni sul controllo della procedura guidata, vedere [creazione di un'interfaccia utente dettagliata con il controllo di ASP.NET 2.0 Wizard](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Vengono definiti due tag predefinito del controllo CreateUserWizard `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

Il primo `WizardStep`, `CreateUserWizardStep`, esegue il rendering di interfaccia che richiede il nome utente, password, indirizzo di posta elettronica e così via. Dopo che il visitatore fornisce le informazioni e fa clic su "Create User", Anna è illustrata il `CompleteWizardStep`, che mostra il messaggio di conferma e un pulsante Continua.

Per personalizzare l'interfaccia del controllo CreateUserWizard per includere i campi del modulo aggiuntiva, è possibile:

- <strong>Creare uno o più nuovi</strong><strong>`WizardStep`</strong><strong>s per contenere gli elementi dell'interfaccia utente aggiuntiva</strong>. Per aggiungere una nuova `WizardStep` a CreateUserWizard, fare clic su di "Aggiungi/Rimuovi `WizardStep` s" collegamento dal suo Smart Tag per avviare il `WizardStep` Editor della raccolta. Da qui è possibile aggiungere, rimuovere o riordinare i passaggi della procedura guidata. Questo è l'approccio che verrà usato per questa esercitazione.

- <strong>Convertire le</strong><strong>`CreateUserWizardStep`</strong><strong>in un modificabile</strong><strong>`WizardStep`</strong><strong>.</strong> Questa impostazione sostituisce il `CreateUserWizardStep` con un equivalente `WizardStep` il cui markup definisce un'interfaccia utente che corrisponde alla `CreateUserWizardStep`' s. Convertendo il `CreateUserWizardStep` in un `WizardStep` è possibile riposizionare i controlli oppure aggiungere elementi dell'interfaccia utente aggiuntiva per questo passaggio. Per convertire le `CreateUserWizardStep` oppure `CompleteWizardStep` in un modificabile `WizardStep`, scegliere il "creare un utente Personalizza passaggio" o "Personalizzare completare il passaggio" link dallo Smart Tag del controllo.

- **Usare una combinazione delle due opzioni precedenti.**

Una cosa importante da tenere presente consiste nel fatto che il controllo CreateUserWizard esegue il processo di creazione di account utente quando fa clic sul pulsante "Create User" dall'interno relativo `CreateUserWizardStep`. Non è importante se sono presenti ulteriori `WizardStep` s dopo il `CreateUserWizardStep` o non.

Quando si aggiunge una classe personalizzata `WizardStep` per il controllo CreateUserWizard per raccogliere input utente aggiuntivo, l'oggetto personalizzato `WizardStep` può essere inserito prima o dopo il `CreateUserWizardStep`. Se posta prima la `CreateUserWizardStep` quindi l'input utente aggiuntivo raccolti da personalizzata `WizardStep` è disponibile per il `CreatedUser` gestore dell'evento. Tuttavia, se personalizzata `WizardStep` segue `CreateUserWizardStep` quindi entro l'ora di personalizzata `WizardStep` viene visualizzato il nuovo account utente è già stato creato e il `CreatedUser` evento è stato generato.

Figura 21 vengono illustrati il flusso di lavoro durante l'aggiunta `WizardStep` precede il `CreateUserWizardStep`. Poiché le informazioni utente aggiuntive sono stati raccolti dal momento le `CreatedUser` viene generato l'evento, tutti noi dobbiamo fare è aggiornamento il `CreatedUser` gestore eventi per recuperare i dati di input e usandole per il `INSERT` i valori dei parametri dell'istruzione (anziché `DBNull.Value`).


[![Il flusso di lavoro CreateUserWizard quando un WizardStep aggiuntive precede il CreateUserWizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Figura 21**: The CreateUserWizard del flusso di lavoro quando un aggiuntive `WizardStep` Precedes le `CreateUserWizardStep` ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image63.png))


Se l'oggetto personalizzato `WizardStep` viene inserito *dopo* il `CreateUserWizardStep`, tuttavia, il processo di account utente di creazione si verifica prima che l'utente abbia avuto la possibilità di immettere sua città Natale, home page o firma. In tal caso, queste informazioni aggiuntive devono essere inseriti nel database dopo aver creato l'account utente, come illustrato dalla figura 22.


[![Il flusso di lavoro CreateUserWizard quando viene fornito un WizardStep aggiuntive dopo il CreateUserWizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Figura 22**: The CreateUserWizard del flusso di lavoro quando un aggiuntive `WizardStep` viene fornito dopo le `CreateUserWizardStep` ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image66.png))


Il flusso di lavoro illustrato nella figura 22 rimane in attesa di inserire un record nel `UserProfiles` tabella fino al termine del passaggio 2. Se il visitatore chiude il browser dopo il passaggio 1, tuttavia, abbiamo avremo raggiunto uno stato in cui è stato creato un account utente, ma è stato aggiunto nessun record `UserProfiles`. Una soluzione alternativa consiste nel disporre di un record con `NULL` o inseriti valori predefiniti `UserProfiles` nel `CreatedUser` gestore eventi (che viene attivato dopo il passaggio 1) e quindi aggiornare questo record dopo al completamento del passaggio 2. Ciò garantisce che un `UserProfiles` record verranno aggiunti per l'account utente anche se l'utente chiude la registrazione processo durante l'esecuzione.

Per questa esercitazione verrà creata una nuova `WizardStep` che si verifica dopo il `CreateUserWizardStep` ma prima di `CompleteWizardStep`. È possibile ottenere prima la WizardStep in posizionare e configurata e quindi verrà esaminato il codice.

Dallo Smart Tag del controllo CreateUserWizard, selezionare il "Aggiungi/Rimuovi `WizardStep` s", che consente di visualizzare il `WizardStep` finestra di dialogo Editor della raccolta. Aggiungere un nuovo `WizardStep`, l'impostazione relativa `ID` a `UserSettings`, la relativa `Title` a "Your Settings" e il relativo `StepType` a `Step`. Quindi posizionarlo in modo che lo segue il `CreateUserWizardStep` ("Sign Up for Your New Account") e prima di `CompleteWizardStep` ("completo"), come illustrato nella figura 23.


[![Aggiungere un nuovo WizardStep per il controllo CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Figura 23**: aggiungere un nuovo `WizardStep` per il controllo CreateUserWizard ([fare clic per visualizzare l'immagine con dimensioni normali](storing-additional-user-information-vb/_static/image69.png))


Fare clic su OK per chiudere il `WizardStep` finestra di dialogo Editor della raccolta. Il nuovo `WizardStep` si evince da markup dichiarativo aggiornato del controllo CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Si noti il nuovo `<asp:WizardStep>` elemento. È necessario aggiungere l'interfaccia utente per la raccolta di città natale del nuovo utente, della home page e firma qui. È possibile immettere il contenuto nella sintassi dichiarativa o tramite la finestra di progettazione. Per usare la finestra di progettazione, selezionare il passaggio "Impostazioni" nell'elenco a discesa nello Smart Tag per visualizzare il passaggio nella finestra di progettazione.

> [!NOTE]
> Selezione di un passaggio tramite elenco dello Smart Tag aggiorna il controllo CreateUserWizard [ `ActiveStepIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), che specifica l'indice del passaggio inizio. Pertanto, se si usa questo elenco a discesa per modificare il passaggio "Impostazioni" nella finestra di progettazione, assicurarsi di impostarlo nuovamente su "Sign Up for Your New Account", in modo che questo passaggio viene visualizzato quando gli utenti visitano prima di tutto la `EnhancedCreateUserWizard.aspx` pagina.


Creare un'interfaccia utente all'interno del passaggio "Your Settings" che contiene tre controlli TextBox denominati `HomeTown`, `HomepageUrl`, e `Signature`. Dopo la costruzione di questa interfaccia, markup dichiarativo di CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Proseguire e visitare questa pagina tramite un browser e creare un nuovo account utente, specificando i valori per la città Natale, home page e firma. Dopo aver completato la `CreateUserWizardStep` viene creato l'account utente nel framework di appartenenza e il `CreatedUser` esecuzione del gestore di evento, che aggiunge una nuova riga alla `UserProfiles`, ma con un database `NULL` value per `HomeTown`, `HomepageUrl`, e `Signature`. Mai vengono utilizzati i valori immessi per la città Natale, home page e firma. Il risultato finale è un nuovo account utente con un `UserProfiles` registrare la cui proprietà `HomeTown`, `HomepageUrl`, e `Signature` campi devono ancora essere specificato.

È necessario eseguire il codice dopo il passaggio "Your Settings" che accetta i valori Town (città), honepage e firma iniziale immessi dall'utente e degli aggiornamenti appropriati `UserProfiles` record. Controllare ogni volta che l'utente si sposta tra i passaggi in una procedura guidata, la procedura guidata [ `ActiveStepChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) viene attivato. È possibile creare un gestore eventi per questo evento e l'aggiornamento di `UserProfiles` tabella quando il passaggio "Your Settings" è stata completata.

Aggiungere un gestore eventi per il CreateUserWizard `ActiveStepChanged` eventi e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Il codice sopra riportato viene avviato, determinando se è giunta semplicemente il passaggio "Completa". Poiché il passaggio "Completa" viene generato immediatamente dopo il passaggio "Impostazioni", quindi quando si raggiunge il visitatore "Complete" passaggio che significa che Anna appena terminato il passaggio "Impostazioni".

In tal caso, è necessario fare riferimento a livello di codice i controlli casella di testo all'interno di `UserSettings WizardStep`. Questa operazione viene eseguita usando prima il `FindControl` metodo a livello di codice che fa riferimento il `UserSettings WizardStep`e quindi di nuovo per fare riferimento a caselle di testo all'interno di `WizardStep`. Dopo che le caselle di testo è stato fatto riferimento, siamo pronti per l'esecuzione di `UPDATE` istruzione. Il `UPDATE` istruzione ha lo stesso numero di parametri come la `INSERT` istruzione nel `CreatedUser` gestore eventi, ma qui vengono usati i valori principali relativi a Town (città), home page e firma forniti dall'utente.

A questo gestore eventi posto, visitare il `EnhancedCreateUserWizard.aspx` pagina tramite un browser e creare un nuovo account utente specificando i valori per la città Natale, home page e firma. Dopo aver creato il nuovo account deve essere reindirizzato al `AdditionalUserInfo.aspx` pagina, in cui just-immesso home Town (città), home page e firma vengono visualizzate le informazioni.

> [!NOTE]
> Il sito Web Microsoft attualmente ha due pagine da cui un visitatore può creare un nuovo account: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. Mappa del sito del sito Web e pagina di accesso scegliere la `CreatingUserAccounts.aspx` pagina, ma la `CreatingUserAccounts.aspx` pagina non richiedere all'utente le informazioni di Town (città), home page e firma iniziale e non aggiunge una riga corrispondente a `UserProfiles`. Di conseguenza, aggiornare il `CreatingUserAccounts.aspx` pagina in modo che offre questa funzionalità o aggiornare la pagina della mappa del sito e account di accesso per fare riferimento a `EnhancedCreateUserWizard.aspx` invece di `CreatingUserAccounts.aspx`. Se si sceglie la seconda opzione, assicurarsi di aggiornare il `Membership` ereditabili `Web.config` file in modo da consentire l'accesso a utenti anonimi di `EnhancedCreateUserWizard.aspx` pagina.


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato le tecniche per modellare i dati relativi agli account utente all'interno del framework di appartenenza. In particolare, rimando all'entità che condividono una relazione uno-a-molti con gli account utente, nonché i dati che condivide una relazione uno a uno di modellazione. Inoltre, abbiamo visto come questo correlato informazioni potrebbero essere visualizzate, inserite e aggiornate, con alcuni esempi di utilizzo del controllo SqlDataSource e ad altri utenti usando il codice ADO.NET.

Questa esercitazione viene completata l'esaminare gli account utente. Inizia con la prossima esercitazione verrà convertito l'attenzione sui ruoli. Prossimi diverse esercitazioni si esaminerà il framework di ruoli, vedere come creare nuovi ruoli, come assegnare ruoli agli utenti, come per determinare quali ruoli appartiene a un utente e su come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e l'aggiornamento dei dati in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 Wizard Control](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Creazione di un'interfaccia utente dettagliata con il controllo ASP.NET 2.0 Wizard](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Creazione dei parametri del controllo origine dati personalizzata](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizzare il controllo CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Guide introduttive di controllo DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Visualizzazione dei dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Modifica di inserimento ed eliminazione dei dati](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Convalida dei form in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Raccolta delle informazioni di registrazione utente personalizzato](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profili in ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Il controllo asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Guida introduttiva di profili utente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](user-based-authorization-vb.md)
