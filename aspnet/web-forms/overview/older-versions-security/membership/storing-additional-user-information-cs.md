---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: L'archiviazione delle informazioni utente aggiuntive (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali visitatori. In questo modo, verranno esaminati opzioni diverse per modeli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 63aa9dce45ce7ac3e33f542f549b2ec620191d50
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="storing-additional-user-information-c"></a>L'archiviazione delle informazioni utente aggiuntive (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali visitatori. In questo modo, si esaminerà diverse opzioni per la modellazione delle informazioni utente in un database e vedere quindi come associare i dati degli account utente creati dal framework di appartenenza.


## <a name="introduction"></a>Introduzione

ASP. Dell'appartenenza a .NET framework offre un'interfaccia flessibile per la gestione degli utenti. L'API di appartenenza include metodi per la convalida delle credenziali, il recupero delle informazioni relative all'utente attualmente connesso, la creazione di un nuovo account utente e l'eliminazione di un account utente, tra gli altri. Ogni account utente in framework appartenenza contiene solo le proprietà necessarie per la convalida delle credenziali e l'esecuzione di attività relative all'account utente essenziali. Questo è evidenziato dai metodi e proprietà del [ `MembershipUser` classe](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), che modella un account utente nel framework di appartenenza. Questa classe dispone di proprietà quali [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), e [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), e i metodi come [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Spesso, le applicazioni debbano archiviare informazioni utente aggiuntive non inclusi nel framework di appartenenza. Ad esempio, un punto vendita online potrebbe essere necessario consentire a ogni utente archiviare proprio gli indirizzi di spedizione e di fatturazione, informazioni di pagamento, le preferenze di recapito e numero di telefono. Inoltre, ogni ordine nel sistema è associata a un account utente specifico.

Il `MembershipUser` non include le proprietà come `PhoneNumber` o `DeliveryPreferences` o `PastOrders`. In che modo è tenere traccia delle informazioni utente necessarie per l'applicazione e fare in modo che si integrano con il framework di appartenenza In questa esercitazione verrà rispondere a questa domanda creando un'applicazione molto rudimentali visitatori. In questo modo, si esaminerà diverse opzioni per la modellazione delle informazioni utente in un database e vedere quindi come associare i dati degli account utente creati dal framework di appartenenza. Iniziamo!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Passaggio 1: Creazione del modello di dati dell'applicazione visitatori

Esistono diverse tecniche che può essere utilizzata per acquisire informazioni utente in un database e associarlo all'account utente creati dal framework di appartenenza. Per illustrare queste tecniche, sarà necessario potenziare l'applicazione web di esercitazione in modo che acquisisce una qualche forma di dati relativi agli utenti. (Attualmente, il modello di dati dell'applicazione contiene solo le tabelle necessarie per l'applicazione dei servizi di `SqlMembershipProvider`.)

Creare un'applicazione molto semplice visitatori in cui un utente autenticato può lasciare un commento. Oltre ad archiviare commenti visitatori, consentire consente a ogni utente archiviare la città Natale, home page e firma. Se fornito, città natale dell'utente, verranno visualizzati su ogni messaggio che ha lasciato il visitatori home page e firma.

### <a name="adding-theguestbookcommentstable"></a>Aggiunta di`GuestbookComments`tabella

Per acquisire i commenti visitatori, è necessario creare una tabella di database denominata `GuestbookComments` che contiene colonne come `CommentId`, `Subject`, `Body`, e `CommentDate`. È anche necessario che ogni record di `GuestbookComments` l'utente che ha lasciato il commento di riferimento a tabella.

Per aggiungere questa tabella per il database, accedere a Esplora Database in Visual Studio e drill-down di `SecurityTutorials` database. Pulsante destro del mouse sulla cartella tabelle e scegliere Aggiungi nuova tabella. Verrà visualizzata un'interfaccia che consente di definire le colonne per la nuova tabella.


[![Aggiungere una nuova tabella nel database SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figura 1**: aggiungere una nuova tabella per la `SecurityTutorials` Database ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image3.png))


Successivamente, definire il `GuestbookComments`di colonne. Per iniziare, aggiungere una colonna denominata `CommentId` di tipo `uniqueidentifier`. Questa colonna verrà identificare in modo univoco ogni commento nei visitatori, pertanto, non consentire `NULL` s e contrassegnarlo come chiave primaria della tabella. Anziché fornire un valore per il `CommentId` ogni campo `INSERT`, per indicare che un nuovo `uniqueidentifier` valore deve essere generato automaticamente per questo campo su `INSERT` impostando il valore predefinito della colonna su `NEWID()`. Dopo aver aggiunto il primo campo, contrassegnarlo come le impostazioni e chiavi primarie, il valore predefinito, la schermata dovrebbe essere simile alla schermata illustrata nella figura 2.


[![Aggiungere una colonna primaria denominata CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figura 2**: aggiungere una colonna primaria denominata `CommentId` ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image6.png))


Successivamente, aggiungere una colonna denominata `Subject` di tipo `nvarchar(50)` e una colonna denominata `Body` di tipo `nvarchar(MAX)`, impedendo `NULL` s in entrambe le colonne. Successivamente, aggiungere una colonna denominata `CommentDate` di tipo `datetime`. Non consentire `NULL` s e impostare il `CommentDate` il valore predefinito della colonna da `getdate()`.

È comunque per aggiungere una colonna che associa un account utente a ogni commento visitatori. Un'opzione, è possibile aggiungere una colonna denominata `UserName` di tipo `nvarchar(256)`. Si tratta di una scelta appropriata quando si utilizza un provider di appartenenze diverso di `SqlMembershipProvider`. Ma quando si utilizza il `SqlMembershipProvider`, poiché in questa serie di esercitazioni, la `UserName` colonna il `aspnet_Users` tabella non deve necessariamente essere univoco. Il `aspnet_Users` chiave primaria della tabella è `UserId` ed è di tipo `uniqueidentifier`. Pertanto, il `GuestbookComments` tabella richiede una colonna denominata `UserId` di tipo `uniqueidentifier` (disattivazione `NULL` valori). Proseguo e aggiungere questa colonna.

> [!NOTE]
> Come accennato nel [ *creazione dello Schema di appartenenza in SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) dell'esercitazione, il framework di appartenenza è progettato per consentire più applicazioni web con diversi account utente di condividere lo stesso archivio utente. Ciò avviene tramite il partizionamento degli account utente in diverse applicazioni. E, mentre ogni nome utente è sicuramente univoco all'interno di un'applicazione, lo stesso nome utente può essere utilizzato in applicazioni diverse utilizzando lo stesso archivio utente. Vi è un raggruppamento `UNIQUE` vincolo nel `aspnet_Users` tabella il `UserName` e `ApplicationId` campi, ma non una in solo il `UserName` campo. Di conseguenza, è possibile che aspnet\_tabella gli utenti di avere i record di due (o più) con lo stesso `UserName` valore. Tuttavia, un `UNIQUE` vincolo il `aspnet_Users` della tabella `UserId` campo (perché è la chiave primaria). Oggetto `UNIQUE` vincolo è importante perché senza di esso è possibile definire un vincolo di chiave esterna tra le `GuestbookComments` e `aspnet_Users` tabelle.


Dopo aver aggiunto il `UserId` colonna, salvare la tabella facendo clic sull'icona Salva nella barra degli strumenti. Denominare la nuova tabella `GuestbookComments`.

È necessario allontanarsi con uno dei problemi ultimo il `GuestbookComments` tabella: è necessario creare un [vincolo di chiave esterna](https://msdn.microsoft.com/library/ms175464.aspx) tra il `GuestbookComments.UserId` colonna e `aspnet_Users.UserId` colonna. A tale scopo, fare clic sull'icona relazione nella barra degli strumenti per avviare la finestra di dialogo Relazioni chiavi esterne. (In alternativa, è possibile avviare questa finestra di dialogo selezionando il menu Progettazione tabelle e scegliendo le relazioni.)

Fare clic sul pulsante Aggiungi nell'angolo inferiore sinistro della finestra di dialogo Relazioni chiavi esterne. Verrà aggiunto un nuovo vincolo foreign key, anche se è necessario definire le tabelle che fanno parte della relazione.


[![Utilizzare la finestra di dialogo relazioni di chiave esterna per gestire i vincoli di chiave esterna di una tabella](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figura 3**: utilizzare la finestra di dialogo di relazioni chiave esterna per gestire i vincoli di chiave esterna di una tabella ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image9.png))


Successivamente, fare clic sull'icona puntini di sospensione nella riga "Tabella e le colonne specifiche" a destra. Verrà avviata la finestra di dialogo tabelle e colonne da cui è possibile specificare la tabella di chiave primaria e di colonna e la colonna chiave esterna dal `GuestbookComments` tabella. In particolare, selezionare `aspnet_Users` e `UserId` come la tabella di chiave primaria e la colonna, e `UserId` dal `GuestbookComments` tabella come colonna chiave esterna (vedere la figura 4). Dopo aver definito le tabelle di chiavi primarie ed esterne e le colonne, fare clic su OK per tornare alla finestra di dialogo Relazioni chiavi esterne.


[![Stabilire un'esterna chiave vincolo tra il aspnet_Users e tabelle GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figura 4**: stabilire un'esterna chiave vincolo tra il `aspnet_Users` e `GuesbookComments` tabelle ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image12.png))


A questo punto il vincolo di chiave esterno è stato stabilito. La presenza di questo vincolo garantisce [integrità relazionale](http://en.wikipedia.org/wiki/Referential_integrity) tra le due tabelle, assicurando che non sarà mai una voce di registro dei visitatori che fa riferimento a un account utente inesistente. Per impostazione predefinita, un vincolo di chiave esterna non sarà un record padre deve essere eliminato se sono disponibili corrispondenti record figlio. Ovvero, se un utente esegue uno o più commenti visitatori e quindi si tenta di eliminare l'account utente, l'eliminazione non riuscirà a meno che i propri commenti visitatori vengono eliminati per primi.

Vincoli di chiave esterna possono essere configurati per eliminare automaticamente i record figlio associato quando viene eliminato un record padre. In altre parole, è possibile configurare questo vincolo di chiave esterna in modo che le voci di registro dei visitatori di un utente vengono eliminate automaticamente quando il proprio account utente viene eliminato. A tale scopo, espandere la sezione "Inserimento e aggiornamento specifica" e impostare la proprietà "Eliminazione della regola" Cascade.


[![Configurare il vincolo di chiave esterno per le eliminazioni a catena](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figura 5**: configurare il vincolo di chiave esterna di eliminazione a catena ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image15.png))


Per salvare il vincolo di chiave esterno, fare clic sul pulsante chiude per uscire dalle relazioni di chiave esterna. Quindi fare clic sull'icona Salva nella barra degli strumenti per salvare la tabella e questa relazione.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>L'archiviazione Home città dell'utente, home page e firma

Il `GuestbookComments` tabella di seguito viene illustrato come archiviare le informazioni che condivide una relazione uno-a-molti con gli account utente. Poiché ogni account utente potrebbe avere un numero arbitrario di commenti associati, questa relazione viene modellata tramite la creazione di una tabella per contenere il set di commenti che include una colonna che consente di tornare ogni commento a un utente specifico. Quando si utilizza il `SqlMembershipProvider`, questo collegamento viene stabilito migliori creando una colonna denominata `UserId` di tipo `uniqueidentifier` e un vincolo di chiave esterna tra la colonna e `aspnet_Users.UserId`.

È ora necessario associare tre colonne a ogni account utente per archiviare città Natale, home page e firma, verrà visualizzato nei suoi commenti visitatori dell'utente. Sono presenti numeri diversi modi per eseguire questa operazione:

- **Aggiungere nuove colonne il * * *`aspnet_Users`* * * o * * *`aspnet_Membership`* * * le tabelle.** È consigliabile non questo approccio poiché consente di modificare lo schema utilizzato per il `SqlMembershipProvider`. Questa decisione può essere utile consultare per recuperare il percorso. Ad esempio, se una versione futura di ASP.NET utilizza un altro `SqlMembershipProvider` dello schema. Microsoft può includere uno strumento per eseguire la migrazione di ASP.NET 2.0 `SqlMembershipProvider` dati al nuovo schema, ma se sono state modificate ASP.NET 2.0 `SqlMembershipProvider` dello schema, questo tipo di conversione potrebbe non essere possibile.

- **Utilizzare le pagine ASP. Framework di profilo della rete, la definizione di una proprietà di profilo per la città Natale, home page e firma.** ASP.NET include un framework di profilo che è progettato per archiviare dati specifici dell'utente aggiuntivi. Ad esempio il framework di appartenenza, il framework di profilo viene compilato nella parte superiore del modello di provider. .NET Framework viene fornito con un `SqlProfileProvider` gradi archivia i dati di profilo in un database di SQL Server. In realtà, il database esiste già la tabella utilizzata dal `SqlProfileProvider` (`aspnet_Profile`), come è stato aggiunto al momento dell'aggiunta nuovamente i servizi delle applicazioni di <a id="_msoanchor_2"> </a> [ *creazione dello Schema di appartenenza in SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) esercitazione.   
 Il vantaggio principale di framework profilo è che consente agli sviluppatori di definire le proprietà di profilo in `Web.config` : nessun codice deve essere scritto per serializzare i dati di profilo da e verso l'archivio dati sottostante. In breve, è molto semplice per definire un set di proprietà di profilo e di utilizzarle nel codice. Tuttavia, il sistema profilo migliorate per essere desiderato per quanto riguarda il controllo delle versioni, pertanto se si dispone di un'applicazione in cui si prevede di nuove proprietà specifiche dell'utente da aggiungere a un secondo momento o quelle esistenti da rimuovere o modificare, quindi il framework di profilo non può essere il  opzione migliore. Inoltre, il `SqlProfileProvider` archivia le proprietà del profilo in modo denormalizzato elevata, rendendo praticamente impossibile eseguire query direttamente sui dati del profilo (ad esempio, quanti utenti dispongono di una home città di New York).   
 Per ulteriori informazioni su framework profilo, consultare la sezione "Ulteriori letture" alla fine di questa esercitazione.

- **Aggiungere queste tre colonne in una nuova tabella nel database e stabilire una relazione uno a uno tra questa tabella e * * *`aspnet_Users`* * *.** Questo approccio è un po' più semplice che con il framework di profilo, ma offre la massima flessibilità nella modalità le proprietà aggiuntive dell'utente vengono create nel database. Questa è l'opzione che verrà utilizzato in questa esercitazione.

Si creerà una nuova tabella denominata `UserProfiles` per salvare la città Natale, home page e firma per ogni utente. Pulsante destro del mouse sulla cartella tabelle nella finestra Esplora Database e scegliere di creare una nuova tabella. Nome della prima colonna `UserId` e impostarne il tipo di `uniqueidentifier`. Non consentire `NULL` valori e contrassegnare la colonna come chiave primaria. Successivamente, aggiungere le colonne denominate: `HomeTown` di tipo `nvarchar(50)`; `HomepageUrl` di tipo `nvarchar(100)`; e la firma del tipo `nvarchar(500)`. Ognuna di queste tre colonne può accettare un `NULL` valore.


[![Creare la tabella UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figura 6**: creare il `UserProfiles` tabella ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image18.png))


Salvataggio della tabella e denominarla `UserProfiles`. Infine, stabilire un vincolo di chiave esterna tra le `UserProfiles` della tabella `UserId` campo e `aspnet_Users.UserId` campo. Come è stato fatto con il vincolo di chiave esterno tra le `GuestbookComments` e `aspnet_Users` tabelle disporre questo vincolo eliminazioni a catena. Poiché il `UserId` campo `UserProfiles` è quello primario chiave, in questo modo che non vi saranno presenti più record nel `UserProfiles` tabella per ogni account utente. Questo tipo di relazione viene definito come uno a uno.

Ora che abbiamo creato il modello di dati, ci sono pronti per utilizzarla. Nei passaggi 2 e 3 verranno esaminati come l'utente attualmente connesso è possibile visualizzare e modificare le informazioni di città, home page e firma home. In passaggio 4 verrà creata l'interfaccia per gli utenti autenticati possono inviare nuovi commenti per i visitatori e visualizzare quelli esistenti.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Passaggio 2: Visualizzare Home città dell'utente, home page e firma

Esistono diversi modi per consentire all'utente attualmente connesso visualizzare e modificare le informazioni di città, home page e firma home. È stato possibile creare manualmente l'interfaccia utente con una casella di testo e controlli etichetta oppure è possibile utilizzare uno dei dati di controlli Web, ad esempio il controllo DetailsView. Per eseguire il database `SELECT` e `UPDATE` istruzioni potremmo ADO.NET il codice nella classe code-behind della pagina o in alternativa, utilizzare un approccio dichiarativo con SqlDataSource. Idealmente, l'applicazione deve contenere un'architettura a più livelli, che è stato possibile richiamare a livello di codice dalla classe code-behind della pagina o in modo dichiarativo tramite il controllo ObjectDataSource.

Poiché questa serie di esercitazioni si concentra sull'autenticazione basata su form, autorizzazione, gli account utente e ruoli, non sarà disponibile una descrizione dettagliata di queste opzioni di accesso ai dati diversi o perché un'architettura a più livelli è preferibile eseguire direttamente le istruzioni SQL dalla pagina ASP.NET. Verrà illustrata utilizzando un controllo DetailsView e SqlDataSource: l'opzione più semplice e veloce: ma i concetti illustrati certamente possono essere applicati a logica alternativa di accesso ai dati e i controlli di Web. Per ulteriori informazioni sull'utilizzo dei dati in ASP.NET, vedere il  *[utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)*  serie di esercitazioni.

Aprire il `AdditionalUserInfo.aspx` nella pagina di `Membership` cartella e aggiungere un controllo DetailsView alla pagina, l'impostazione relativa `ID` proprietà `UserProfile` e cancellare relativo `Width` e `Height` proprietà. Espandere di DetailsView Smart Tag e scegliere di associarlo a un nuovo controllo origine dati. Verrà avviata la configurazione guidata origine dati (vedere la figura 7). Il primo passaggio viene chiesto di specificare il tipo di origine dati. Poiché si stabilirà connettersi direttamente al `SecurityTutorials` del database, scegliere l'icona di Database, specificando il `ID` come `UserProfileDataSource`.


[![Aggiungere un nuovo controllo SqlDataSource denominato UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figura 7**: aggiungere un nuovo controllo SqlDataSource denominato `UserProfileDataSource` ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image21.png))


Nella schermata successiva è richiesto per il database da utilizzare. Abbiamo già definito in una stringa di connessione `Web.config` per il `SecurityTutorials` database. Il nome di stringa di connessione: `SecurityTutorialsConnectionString` : deve essere nell'elenco a discesa. Selezionare questa opzione e fare clic su Avanti.


[![Scegliere SecurityTutorialsConnectionString dall'elenco a discesa](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figura 8**: scegliere `SecurityTutorialsConnectionString` dall'elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image24.png))


La schermata successiva viene chiesto di specificare la tabella e le colonne a query. Scegliere il `UserProfiles` tabella dall'elenco a discesa e selezionare tutte le colonne.


[![Portare indietro di tutte le colonne dalla tabella UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Figura 9**: portare nuovamente tutte le colonne di `UserProfiles` tabella ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image27.png))


La query corrente nella figura 9 restituisce *tutti* dei record di `UserProfiles`, ma si desidera utilizzare solo i record dell'utente attualmente connesso. Per aggiungere un `WHERE` clausola, fare clic su di `WHERE` pulsante per visualizzare il componente `WHERE` clausola dialogo (figura 10). Qui è possibile selezionare l'origine del parametro di filtro, l'operatore e la colonna da filtrare. Selezionare `UserId` come colonna e "=" come l'operatore.

Non è Sfortunatamente disponibile alcuna origine parametro predefinito per restituire l'utente attualmente connesso `UserId` valore. È necessario ottenere questo valore a livello di codice. Pertanto, impostare l'elenco di riepilogo a discesa di origine per "None", fare clic su Aggiungi pulsante per aggiungere il parametro e quindi fare clic su OK.


[![Aggiungere un parametro di filtro nella colonna di ID utente](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figura 10**: aggiungere un parametro di filtro sul `UserId` colonna ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image30.png))


Dopo aver scelto OK verrà di nuovo la schermata illustrata nella figura 9. Questa volta, tuttavia, la query SQL nella parte inferiore della schermata deve includere un `WHERE` clausola. Fare clic su Avanti per passare alla schermata "Test Query". Qui è possibile eseguire la query e visualizzare i risultati. Fare clic su Fine per completare la procedura guidata.

Dopo avere completato la configurazione guidata origine dati, Visual Studio crea il controllo SqlDataSource in base alle impostazioni specificate nella procedura guidata. Inoltre, viene aggiunto manualmente BoundField al controllo DetailsView per ogni colonna restituita per SqlDataSource `SelectCommand`. Non è necessario per visualizzare il `UserId` campo nel controllo DetailsView, poiché non è necessario che l'utente conosce questo valore. È possibile rimuovere il campo direttamente dal codice dichiarativo del controllo DetailsView o facendo clic su "Modifica campi" collegare dal suo Smart Tag.

Markup dichiarativo della pagina a questo punto dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

È necessario impostare a livello di programmazione del controllo SqlDataSource `UserId` parametro per l'utente attualmente connesso `UserId` prima che venga selezionato il tipo di dati. Questo può essere eseguito mediante la creazione di un gestore eventi per il SqlDataSource `Selecting` evento e aggiungere il codice seguente di codice sono:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Il codice sopra riportato viene avviato da ottenere un riferimento all'utente attualmente connesso tramite una chiamata di `Membership` della classe `GetUser` metodo. Restituisce un `MembershipUser` oggetto la cui proprietà `ProviderUserKey` proprietà contiene il `UserId`. Il `UserId` valore viene quindi assegnato a per SqlDataSource `@UserId` parametro.

> [!NOTE]
> Il `Membership.GetUser()` restituisce informazioni sull'utente attualmente connesso. Se un utente anonimo è visitando la pagina, verrà restituito un valore di `null`. In tal caso, questo può provocare un `NullReferenceException` nella riga successiva del codice durante il tentativo di leggere il `ProviderUserKey` proprietà. Ovviamente, non è necessario preoccuparsi `Membership.GetUser()` restituendo un `null` valore il `AdditionalUserInfo.aspx` pagina perché è stato configurato l'autorizzazione dell'URL in un'esercitazione precedente in modo che solo gli utenti autenticati Impossibile accedere alle risorse ASP.NET in questa cartella. Se si desidera accedere alle informazioni sull'utente attualmente connesso in una pagina in cui è consentito l'accesso anonimo, assicurarsi di verificare che non`null MembershipUser` oggetto viene restituito dal `GetUser()` metodo prima di fare riferimento le relative proprietà.


Se si visita il `AdditionalUserInfo.aspx` pagina tramite un browser si noterà una pagina vuota perché è stato ancora aggiunto tutte le righe per il `UserProfiles` tabella. Nel passaggio 6 verranno esaminati come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga per il `UserProfiles` tabella quando viene creato un nuovo account utente. Per il momento, tuttavia, si sarà necessario creare manualmente un record nella tabella.

Passare a Esplora Database in Visual Studio ed espandere la cartella di tabelle. Fare clic su di `aspnet_Users` tabella e scegliere "Mostra i dati di tabella" per visualizzare i record nella tabella, eseguire la stessa operazione il `UserProfiles` tabella. Figura 11 Mostra i risultati quando si Affianca verticalmente. Nel database sono attualmente `aspnet_Users` record per Tito Bruce e Fred, ma nessun record di `UserProfiles` tabella.


[![Vengono visualizzate il contenuto di aspnet_Users e tabelle UserProfiles](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figura 11**: il contenuto del `aspnet_Users` e `UserProfiles` vengono visualizzate le tabelle ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image33.png))


Aggiungere un nuovo record per il `UserProfiles` tabella manualmente digitando i valori per il `HomeTown`, `HomepageUrl`, e `Signature` campi. Il modo più semplice per ottenere un oggetto valido `UserId` valore nel nuovo `UserProfiles` record consiste nel selezionare il `UserId` campo da un account utente specifico nel `aspnet_Users` tabella e copiare e incollare nel `UserId` campo `UserProfiles`. Figura 12 illustra il `UserProfiles` tabella dopo l'aggiunta di un nuovo record per Bruce.


[![Un Record è stato aggiunto all'UserProfiles per Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figura 12**: un Record è stato aggiunto al `UserProfiles` per Bruce ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image36.png))


Restituito per il `AdditionalUserInfo.aspx` pagina, l'accesso come Bruce. Come illustrato nella figura 13, vengono visualizzate le impostazioni di Bruce.


[![L'utente attualmente visita è illustrato His impostazioni](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figura 13**: attualmente visita utente è illustrato impostazioni His ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Andare avanti e manualmente aggiungere record di `UserProfiles` tabella per ogni utente di appartenenza. Nel passaggio 6 verranno esaminati come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga per il `UserProfiles` tabella quando viene creato un nuovo account utente.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Passaggio 3: Consentire all'utente di modificare la città Home, home page e firma

A questo punto è possibile visualizzare l'utente attualmente connesso le città Natale, home page e impostazione della firma, ma non ancora modificarle. Aggiornare consente il controllo DetailsView in modo che i dati possono essere modificati.

La prima cosa da fare è aggiungere un `UpdateCommand` per SqlDataSource, specificando il `UPDATE` istruzione da eseguire e i relativi parametri corrispondenti. Selezionare SqlDataSource e, dalla finestra delle proprietà, fare clic sui puntini di sospensione accanto alla proprietà UpdateQuery per visualizzare la finestra di dialogo Editor comandi e parametri. Immettere le informazioni seguenti `UPDATE` istruzione nella casella di testo:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Successivamente, fare clic sul pulsante "Aggiorna parametri", verrà creato un parametro del controllo SqlDataSource `UpdateParameters` raccolta per ognuno dei parametri di `UPDATE` istruzione. Lasciare l'origine per tutti i set di parametri su None e fare clic sul pulsante OK per completare la finestra di dialogo.


[![Specificare il SqlDataSource UpdateCommand e UpdateParameters](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Nella figura 14**: specificare il SqlDataSource `UpdateCommand` e `UpdateParameters` ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image42.png))


A causa di Virtual Machine additions è effettuato il controllo SqlDataSource, il controllo può ora supportare la modifica di DetailsView. Dalla Smart Tag del controllo DetailsView., selezionare la casella di controllo "Abilita modifica". Aggiunge un CommandField al controllo `Fields` insieme con il relativo `ShowEditButton` proprietà è impostata su True. Si esegue il rendering di un pulsante Modifica quando DetailsView viene visualizzato nella modalità di sola lettura e aggiornamento e i pulsanti Annulla quando visualizzata modalità di modifica. Anziché richiedere all'utente di fare clic su Modifica, tuttavia, possibilità di installare il rendering di DetailsView in uno stato "sempre modificabile" tramite l'impostazione del controllo DetailsView [ `DefaultMode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) a `Edit`.

Con queste modifiche, markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Si noti l'aggiunta del CommandField e `DefaultMode` proprietà.

Vado avanti e testare questa pagina tramite un browser. Durante la visita a un utente che dispone di un record corrispondente in `UserProfiles`, le impostazioni dell'utente vengono visualizzate in un'interfaccia modificabile.


[![Controllo DetailsView esegue il rendering di un'interfaccia modificabile](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figura 15**: DetailsView esegue il rendering di un'interfaccia modificabile ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image45.png))


Provare a cambiare i valori e fare clic sul pulsante di aggiornamento. Viene visualizzato come se non accade nulla. È un postback e i valori vengono salvati nel database, ma vi è alcun feedback visivo che si è verificato durante il salvataggio.

Per risolvere questo problema, tornare a Visual Studio e aggiungere un controllo etichetta di sopra di DetailsView. Impostare il relativo `ID` per `SettingsUpdatedMessage`, le `Text` proprietà su "sono state aggiornate le impostazioni," e il relativo `Visible` e `EnableViewState` proprietà `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

È necessario visualizzare il `SettingsUpdatedMessage` etichettare ogni volta che viene aggiornato il controllo DetailsView. A tale scopo, creare un gestore eventi in DetailsView `ItemUpdated` eventi e aggiungere il codice seguente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Restituito per il `AdditionalUserInfo.aspx` pagina tramite un browser e aggiornare i dati. Questa volta, viene visualizzato un messaggio di stato utile.


[![Un breve messaggio viene visualizzato quando l'aggiornamento delle impostazioni](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figura 16**: un breve messaggio viene visualizzato quando vengono aggiornate le impostazioni ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> Il controllo DetailsView di modifica della interfaccia lascia numerosi per essere desiderato. Usa dimensioni standard nelle caselle di testo, ma il campo della firma probabilmente deve essere una casella di testo su più righe. RegularExpressionValidator deve essere utilizzato per verificare che l'URL della home page, se specificato, inizia con "http://" o "https://". Inoltre, poiché controllo DetailsView relativo `DefaultMode` proprietà impostata su `Edit`, il pulsante Annulla non ha alcun effetto. Deve di essere rimosso o, quando si fa clic, reindirizzerà l'utente a un'altra pagina (ad esempio `~/Default.aspx`). Un esercizio lasciare questi miglioramenti per il lettore.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Aggiunta di un collegamento per il`AdditionalUserInfo.aspx`pagina nella pagina Master

Attualmente, il sito Web non fornisce i collegamenti per il `AdditionalUserInfo.aspx` pagina. L'unico modo per raggiungere è immettere l'URL della pagina direttamente nella barra degli indirizzi del browser. Aggiungere un collegamento a questa pagina per il `Site.master` pagina master.

Tenere presente che la pagina master contiene un controllo LoginView Web nel relativo `LoginContent` ContentPlaceHolder che consente di visualizzare i tag diverso per i visitatori anonimi e autenticati. Aggiornamento del controllo LoginView `LoggedInTemplate` per includere un collegamento per il `AdditionalUserInfo.aspx` pagina. Dopo aver apportato queste modifiche di LoginView markup dichiarativo del controllo dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Si noti l'aggiunta del `lnkUpdateSettings` controllo collegamento ipertestuale per la `LoggedInTemplate`. Con questo collegamento sul posto, gli utenti autenticati possono passare rapidamente alla pagina per visualizzare e modificare le impostazioni di città, home page e firma home.

## <a name="step-4-adding-new-guestbook-comments"></a>Passaggio 4: Aggiunta di nuovi commenti visitatori

Il `Guestbook.aspx` pagina è in cui gli utenti autenticati possono visualizzare il registro dei visitatori e lascia un commento. Per iniziare la creazione dell'interfaccia per aggiungere nuovi commenti visitatori.

Aprire il `Guestbook.aspx` pagina in Visual Studio e creare un'interfaccia utente costituita da due controlli casella di testo, uno per il soggetto del nuovo commento e uno per il relativo corpo. Impostare il primo controllo TextBox `ID` proprietà `Subject` e il relativo `Columns` proprietà a 40, imposta secondo `ID` per `Body`, l'oggetto `TextMode` per `MultiLine`e il relativo `Width` e `Rows` la proprietà "95%" e 8, rispettivamente. Per completare l'interfaccia utente, aggiungere un controllo pulsante Web denominato `PostCommentButton` e impostare il relativo `Text` proprietà su "Post il commento".

Poiché ogni commento visitatori richiede un oggetto e il corpo, aggiungere un controllo RequiredFieldValidator per ognuna delle caselle di testo. Impostare il `ValidationGroup` proprietà di questi controlli per "EnterComment"; allo stesso modo, impostare il `PostCommentButton` del controllo `ValidationGroup` proprietà su "EnterComment". Per ulteriori informazioni su ASP. I controlli di convalida della rete, estrazione [della convalida del Form in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e [esercitazione controlli di convalida Server](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) in [W3Schools](http://www.w3schools.com/).

Dopo la creazione dell'interfaccia utente markup dichiarativo della pagina dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Con l'interfaccia utente completa, l'attività successiva consiste nell'inserire un nuovo record nel `GuestbookComments` tabella quando la `PostCommentButton` si fa clic. Può essere eseguita in diversi modi: è possibile scrivere codice ADO.NET nel pulsante `Click` gestore dell'evento, è possibile aggiungere un controllo SqlDataSource per la pagina, configurare relativo `InsertCommand`e quindi chiamare relativo `Insert` metodo il `Click` evento gestore. o è stato possibile creare un livello intermedio è responsabile per l'inserimento di nuovi commenti visitatori e richiamare questa funzionalità dal `Click` gestore dell'evento. Poiché è stato esaminato utilizzando un SqlDataSource nel passaggio 3, utilizziamo qui il codice ADO.NET.

> [!NOTE]
> Le classi ADO.NET usate per accedere a livello di codice ai dati da un database di Microsoft SQL Server si trovano nel `System.Data.SqlClient` dello spazio dei nomi. Potrebbe essere necessario importare questo spazio dei nomi di classe code-behind della pagina (ad esempio, `using System.Data.SqlClient;`).


Creare un gestore eventi per il `PostCommentButton`del `Click` eventi e aggiungere il codice seguente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Il `Click` gestore dell'evento inizia verificando che i dati forniti dall'utente siano validi. In caso contrario, il gestore eventi prima di inserire un record. Supponendo che i dati forniti siano validi, l'utente attualmente connesso `UserId` valore viene recuperato e archiviato nel `currentUserId` variabile locale. Questo valore è necessaria in quanto è necessario fornire un `UserId` valore quando si inserisce un record in `GuestbookComments`.

Successivamente, la stringa di connessione per il `SecurityTutorials` database viene recuperato dalla `Web.config` e `INSERT` è specificata l'istruzione SQL. Oggetto `SqlConnection` oggetto viene quindi creato e aperto. Successivamente, un `SqlCommand` oggetto viene creato e i valori per i parametri utilizzati nel `INSERT` della query sono assegnati. Il `INSERT` istruzione viene quindi eseguita e la connessione è chiusa. Alla fine del gestore dell'evento, il `Subject` e `Body` nelle caselle di testo `Text` le proprietà vengono eliminate in modo che i valori dell'utente non sono persistenti tra il postback.

Proseguo e testare questa pagina in un browser. Poiché in questa pagina di `Membership` cartella non è accessibile a utenti anonimi. Pertanto, è necessario innanzitutto effettuare l'accesso (se non è già). Immettere un valore nel `Subject` e `Body` nelle caselle di testo e fare clic sul `PostCommentButton` pulsante. In questo modo, un nuovo record da aggiungere alla `GuestbookComments`. Durante il postback, l'oggetto e il corpo specificato vengono cancellati da caselle di testo.

Dopo aver selezionato il `PostCommentButton` pulsante vi è alcun feedback visivo che è stato aggiunto il commento per i visitatori. È comunque necessario aggiornare questa pagina per visualizzare i commenti di visitatori esistenti, verranno eseguite nel passaggio 5. Una volta è eseguire questa operazione, il commento appena aggiunto verrà visualizzato nell'elenco dei commenti, fornire feedback visivo adeguate. Per il momento, confermare che è stato salvato il commento visitatori esaminando il contenuto del `GuestbookComments` tabella.

Figura 17 mostra i contenuti del `GuestbookComments` dopo che sono stati lasciati due commenti.


[![È possibile visualizzare i commenti visitatori nella tabella GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figura 17**: È possibile visualizzare i commenti visitatori il `GuestbookComments` tabella ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Se un utente tenta di inserire un commento visitatori contenente potenzialmente pericoloso markup, ad esempio HTML – ASP.NET genererà un `HttpRequestValidationException`. Per ulteriori informazioni su questa eccezione, perché viene generata, e su come consentire agli utenti di inviare valori potenzialmente pericolosi, consultare il [white paper convalida richiesta](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Passaggio 5: Elenca i commenti visitatori esistenti

Oltre a lasciare commenti, un utente visita il `Guestbook.aspx` pagina deve inoltre essere in grado di visualizzare commenti esistenti del visitatori. A tale scopo, aggiungere un controllo ListView denominato `CommentList` nella parte inferiore della pagina.

> [!NOTE]
> Il controllo ListView è nuovo in ASP.NET versione 3.5. È progettato per visualizzare un elenco di elementi in un layout molto personalizzabile e flessibile, ma offrono incorporato modifica, inserimento, eliminazione, spostamento e l'ordinamento funzionalità quali il controllo GridView. Se si utilizza ASP.NET 2.0, è necessario utilizzare il controllo DataList o Repeater. Per ulteriori informazioni sull'utilizzo del controllo ListView, vedere [Scott Guthrie](https://weblogs.asp.net/scottgu/)del post di blog, [asp: ListView controllo](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e l'articolo, [visualizzazione di dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Aprire Smart Tag del controllo ListView e, dall'elenco a discesa Scegli origine dati, è necessario associare il controllo a una nuova origine dati. Come illustrato nel passaggio 2, verrà avviata la configurazione guidata origine dati. Selezionare l'icona del Database, assegnare il nome risultante SqlDataSource `CommentsDataSource`, fare clic su OK. Selezionare quindi il `SecurityTutorialsConnectionString` dall'elenco a discesa di stringa di connessione e fare clic su Avanti.

A questo punto nel passaggio 2 specificato i dati per eseguire query per il prelievo di `UserProfiles` tabella dall'elenco a discesa e selezionando le colonne da restituire (vedere la figura 9). Questa volta, tuttavia, si desidera creare un'istruzione SQL che inserisce nuovamente non solo i record da `GuestbookComments`, ma anche una città natale dell'autore, home page, la firma e nome utente. Pertanto, selezionare il pulsante di opzione "Specifica un'istruzione SQL personalizzata o una stored procedure" e fare clic su Avanti.

Verrà visualizzata la schermata "Definire istruzioni o Stored procedure personalizzate". Fare clic sul pulsante Generatore di Query per compilare graficamente la query. Avvia il generatore di Query che richiede di specificare le tabelle a cui da che si desidera eseguire una query. Selezionare il `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle e fare clic su OK. Le tre tabelle verrà aggiunta all'area di progettazione. Poiché sono presenti vincoli di chiave esterna tra le `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle, il generatore di Query automaticamente `JOIN` s queste tabelle.

Tutto ciò che è comunque necessario specificare le colonne da restituire. Dal `GuestbookComments` tabella selezionare la `Subject`, `Body`, e `CommentDate` colonne; restituito il `HomeTown`, `HomepageUrl`, e `Signature` le colonne di `UserProfiles` tabella; e restituire `UserName` da `aspnet_Users`. Inoltre, aggiungere "`ORDER BY CommentDate DESC`" alla fine del `SELECT` query in modo che post più recenti vengono restituite per prime. Dopo aver effettuato le selezioni, l'interfaccia del generatore di Query dovrebbe essere simile alla schermata nella figura 18.


[![La Query di costruito unisce il GuestbookComments UserProfiles e aspnet_Users tabelle](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figura 18**: Query costruita `JOIN` s il `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image54.png))


Fare clic su OK per chiudere la finestra Generatore di Query e tornare alla schermata "Definire istruzioni o Stored procedure personalizzate". Fare clic su Avanti per passare alla schermata "Test Query", in cui è possibile visualizzare i risultati della query facendo clic sul pulsante Test Query. Quando si è pronti, fare clic su Fine per completare la configurazione guidata origine dati.

Quando viene completata la configurazione guidata origine dati del passaggio 2, il controllo DetailsView associato `Fields` raccolta è stata aggiornata per includere un BoundField per ogni colonna restituita dal `SelectCommand`. ListView, tuttavia, rimane invariato; è comunque necessario definire il layout. È possibile costruire il layout di ListView manualmente tramite il markup dichiarativo o utilizzando il comando "Configura ListView" Smart Tag. In genere si preferisce definire manualmente il markup, ma utilizzare il metodo è più naturale.

Finisce con i seguenti `LayoutTemplate`, `ItemTemplate`, e `ItemSeparatorTemplate` per il controllo ListView:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

Il `LayoutTemplate` definisce il markup generato dal controllo, mentre il `ItemTemplate` esegue il rendering di ogni elemento restituito da SqlDataSource. Il `ItemTemplate`del markup risultante viene inserito nella `LayoutTemplate`del `itemPlaceholder` controllo. Oltre al `itemPlaceholder`, `LayoutTemplate` include un controllo DataPager, che limita il controllo ListView per mostrare solo 10 visitatori commenti per pagina (impostazione predefinita) ed esegue il rendering di un'interfaccia di paging.

My `ItemTemplate` consente di visualizzare il soggetto del commento ogni visitatori in un `<h4>` elemento con il corpo situato di sotto dell'oggetto. Si noti che la sintassi utilizzata per visualizzare il corpo accetta i dati restituiti dal `Eval("Body")` istruzione l'associazione dati, viene convertito in una stringa e le interruzioni di riga sostituisce con il `<br />` elemento. Questa conversione è necessaria per visualizzare le interruzioni di riga immesse durante l'invio del commento, poiché gli spazi vuoti viene ignorato da HTML. Firma dell'utente viene visualizzato sotto il corpo in corsivo, seguito da città natale dell'utente, un collegamento per la home page, la data e ora che è stato eseguito il commento e il nome utente della persona che ha lasciato il commento.

Richiedere qualche istante per visualizzare la pagina tramite un browser. Si dovrebbero vedere i commenti aggiunti alla visitatori nel passaggio 5 visualizzati qui.


[![Guestbook. aspx ora consente di visualizzare i commenti del visitatori](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figura 19**: `Guestbook.aspx` ora vengono visualizzati i commenti del visitatori ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image57.png))


Provare ad aggiungere un nuovo commento per i visitatori. Fare clic sul `PostCommentButton` pulsante pagina esegue il postback e il commento verrà aggiunto al database, ma il controllo ListView non viene aggiornato per mostrare il nuovo commento. Per risolvere questo problema tramite:

- L'aggiornamento di `PostCommentButton` del pulsante `Click` gestore dell'evento in modo che richiama il controllo ListView `DataBind()` metodo dopo aver inserito il nuovo commento nel database, o
- Impostazione del controllo ListView `EnableViewState` proprietà `false`. Questo approccio funziona perché disabilitando lo stato di visualizzazione del controllo, è necessario riassociare ai dati sottostanti a ogni postback.

Il sito Web dell'esercitazione scaricabile da questa esercitazione vengono illustrate entrambe le tecniche. Il controllo ListView `EnableViewState` proprietà `false` e il codice necessario per riassociare a livello di programmazione i dati al controllo ListView è presente nel `Click` gestore eventi, ma è impostata come commento.

> [!NOTE]
> Attualmente il `AdditionalUserInfo.aspx` pagina consente di visualizzare e modificare le impostazioni di città, home page e firma home. Potrebbe essere utile aggiornare `AdditionalUserInfo.aspx` per visualizzare la registrazione nei commenti visitatori dell'utente. Vale a dire, oltre a esaminare e modificare le informazioni, un utente può esaminare il `AdditionalUserInfo.aspx` per vedere quali visitatori commenti che viene eseguita in precedenza. Lasciare questo campo come esercizio per il lettore di interesse.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Passaggio 6: Personalizzazione del controllo CreateUserWizard per includere un'interfaccia per la città Home, home page e firma

Il `SELECT` utilizzato da query di `Guestbook.aspx` pagina utilizza un `INNER JOIN` per combinare i record correlati tra il `GuestbookComments`, `UserProfiles`, e `aspnet_Users` tabelle. Se un utente che non dispone di alcun record in `UserProfiles` commento effettua una visitatori, il commento non saranno visualizzato nel controllo ListView perché il `INNER JOIN` restituisce solo `GuestbookComments` record quando sono presenti record corrispondenti nel `UserProfiles` e `aspnet_Users`. E come illustrato nel passaggio 3, se un utente non dispone di un record in `UserProfiles` ha non è possibile visualizzare o modificare proprio impostazioni nel `AdditionalUserInfo.aspx` pagina.

Ovviamente, la progettazione a causa di decisioni è importante che ogni account utente nel sistema di appartenenze esiste un corrispondente record nel `UserProfiles` tabella. Cosa è per un record corrispondente da aggiungere alla `UserProfiles` ogni volta che un nuovo account utente di appartenenza viene creato tramite CreateUserWizard.

Come descritto nel [ *creazione degli account utente* ](creating-user-accounts-cs.md) esercitazione, dopo il nuovo account utente di appartenenza viene creato il controllo CreateUserWizard genera relativo [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). È possibile creare un gestore eventi per questo evento, ottenere l'ID utente per l'utente appena creato e quindi inserire un record nel `UserProfiles` tabella con i valori predefiniti per il `HomeTown`, `HomepageUrl`, e `Signature` colonne. Inoltre, è possibile richiedere all'utente per questi valori personalizzando l'interfaccia del controllo CreateUserWizard per includere altre caselle di testo.

Questa sezione descrive come aggiungere una nuova riga per il `UserProfiles` tabella il `CreatedUser` gestore eventi con i valori predefiniti. Successivamente, verrà spiegato come personalizzare l'interfaccia utente del controllo CreateUserWizard per includere i campi modulo aggiuntivi per raccogliere città Natale, home page e firma il nuovo utente.

### <a name="adding-a-default-row-touserprofiles"></a>Aggiunta di una riga predefinita`UserProfiles`

Nel [ *creazione degli account utente* ](creating-user-accounts-cs.md) esercitazione è stato aggiunto un controllo CreateUserWizard per il `CreatingUserAccounts.aspx` nella pagina di `Membership` cartella. Per avere CreateUserWizard controllo aggiungere un record a `UserProfiles` tabella al momento della creazione di account utente, è necessario aggiornare la funzionalità del controllo CreateUserWizard. Anziché apportare queste modifiche per il `CreatingUserAccounts.aspx` pagina, aggiungere invece un nuovo controllo CreateUserWizard `EnhancedCreateUserWizard.aspx` pagina e apportare le modifiche per questa esercitazione non esiste.

Aprire il `EnhancedCreateUserWizard.aspx` pagina in Visual Studio e trascinare un controllo CreateUserWizard dalla casella degli strumenti nella pagina. Impostare il controllo CreateUserWizard `ID` proprietà `NewUserWizard`. Come accennato nel <a id="_msoanchor_5"> </a> [ *creazione degli account utente* ](creating-user-accounts-cs.md) dell'esercitazione, l'interfaccia utente predefinita del CreateUserWizard richiede il visitatore per le informazioni necessarie. Quando queste informazioni sono state fornite, il controllo crea internamente un nuovo account utente nel framework di appartenenza, senza la necessità di scrivere una singola riga di codice.

Il controllo CreateUserWizard genera un numero di eventi durante il flusso di lavoro. Dopo un visitatore fornisce le informazioni sulla richiesta e invia il form, il controllo CreateUserWizard generato inizialmente relativo [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si verifica un problema durante il processo di creazione, la [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) è attivato; tuttavia, se l'utente è stato creato, quindi il [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) viene generato. Nel <a id="_msoanchor_6"> </a> [ *creazione degli account utente* ](creating-user-accounts-cs.md) esercitazione è stato creato un gestore eventi per il `CreatingUser` eventi per garantire che il nome utente fornito non contiene un iniziale o spazi finali, e che il nome utente non include un punto qualsiasi nella password.

Per aggiungere una riga di `UserProfiles` tabella per l'utente appena creato, è necessario creare un gestore eventi per il `CreatedUser` evento. Una volta il `CreatedUser` ha generato l'evento, l'account utente è già stato creato in framework appartenenza, consentono di recuperare il valore di ID utente dell'account.

Creare un gestore eventi per il `NewUserWizard`del `CreatedUser` eventi e aggiungere il codice seguente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Uomo di codice precedente recuperando l'ID utente dell'account utente appena aggiunto. Questa operazione viene eseguita tramite il `Membership.GetUser(username)` per restituire informazioni su un utente specifico e viene quindi utilizzato il `ProviderUserKey` proprietà per recuperare i relativi ID utente. Il nome utente immesso dall'utente nel controllo CreateUserWizard è disponibile tramite il relativo [ `UserName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Successivamente, la stringa di connessione viene recuperata da `Web.config` e `INSERT` istruzione è specificata. Vengono creata un'istanza di ADO.NET gli oggetti necessari e il comando eseguito. Nel codice viene assegnato un [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) istanza per il `@HomeTown`, `@HomepageUrl`, e `@Signature` parametri, che produce l'effetto di inserimento dei database `NULL` valori per il `HomeTown`, `HomepageUrl`, e `Signature` campi.

Visitare il `EnhancedCreateUserWizard.aspx` pagina tramite un browser e creare un nuovo account utente. Al termine dell'operazione, tornare a Visual Studio ed esaminarne il contenuto del `aspnet_Users` e `UserProfiles` tabelle (ad esempio, abbiamo fatto tornare nella figura 12). Verrà visualizzato il nuovo account utente in `aspnet_Users` e un oggetto corrispondente `UserProfiles` riga (con `NULL` i valori del parametro `HomeTown`, `HomepageUrl`, e `Signature`).


[![Sono stati aggiunti un nuovo Account utente e il Record UserProfiles](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figura 20**: un nuovo Account utente e `UserProfiles` Record sono stati aggiunti ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image60.png))


Dopo il visitatore ha fornito il suo nuove informazioni sull'account e fa clic sul pulsante "Crea utente", viene creato l'account utente e aggiunta di una riga per il `UserProfiles` tabella. Visualizza quindi CreateUserWizard relativo `CompleteWizardStep`, che visualizza un messaggio di conferma e un pulsante Continua. Fare clic sul pulsante Continua provoca un postback, ma viene eseguita alcuna azione, lasciando l'utente bloccati nel `EnhancedCreateUserWizard.aspx` pagina.

È possibile specificare un URL per inviare all'utente a cui si è fatto clic sul pulsante Continua tramite il controllo CreateUserWizard [ `ContinueDestinationPageUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Impostare il `ContinueDestinationPageUrl` proprietà su "~ / Membership/AdditionalUserInfo.aspx". Questa operazione richiede all'utente di nuovo `AdditionalUserInfo.aspx`, in cui possano visualizzare e aggiornare le relative impostazioni.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizzazione dell'interfaccia del CreateUserWizard al prompt dei comandi per città Home, home page e firma il nuovo utente

Interfaccia predefinita del controllo CreateUserWizard è sufficiente per gli scenari di creazione di account semplice in cui è necessario raccolti solo informazioni sull'account utente principale come nome utente, password e posta elettronica. Ma cosa accade se si desidera richiedere il visitatore per immettere la città Natale, home page e firma durante la creazione del proprio account? È possibile personalizzare l'interfaccia del controllo CreateUserWizard per raccogliere ulteriori informazioni al momento dell'iscrizione, e queste informazioni possono essere utilizzate nel `CreatedUser` gestore eventi per inserire nuovi record nel database sottostante.

Il controllo CreateUserWizard estende il controllo procedura guidata ASP.NET, che è un controllo che consente a uno sviluppatore di pagina definire una serie di ordinata `WizardSteps`. Controllo della procedura guidata esegue il rendering il passaggio attivo e offre un'interfaccia di navigazione che consente il visitatore per spostarsi tra questi passaggi. Controllo della procedura guidata è ideale per la suddivisione di un'attività di lunga durata in diversi passaggi brevi. Per ulteriori informazioni sul controllo della procedura guidata, vedere [la creazione di un'interfaccia utente dettagliata con il controllo di ASP.NET 2.0 guidata](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Markup predefinito del controllo CreateUserWizard definisce due `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

Il primo `WizardStep`, `CreateUserWizardStep`, esegue il rendering di interfaccia che richiede il nome utente, password, messaggio di posta elettronica e così via. Dopo il visitatore fornisce le informazioni e fa clic su "Create User", viene visualizzata il `CompleteWizardStep`, che mostra il messaggio di conferma e un pulsante Continua.

Per personalizzare l'interfaccia del controllo CreateUserWizard per includere i campi modulo aggiuntivi, è possibile:

- **Creare uno o più nuovi * * *`WizardStep`* * * s per contenere gli elementi dell'interfaccia utente aggiuntivi**. Per aggiungere un nuovo `WizardStep` per CreateUserWizard, fare clic su di "Aggiungi/Rimuovi `WizardSteps`" collegamento dal suo Smart Tag per avviare il `WizardStep` Editor della raccolta. Da qui è possibile aggiungere, rimuovere o riordinare i passaggi della procedura guidata. Si tratta dell'approccio che per questa esercitazione verrà utilizzato.

- **Convertire il * * *`CreateUserWizardStep`* * * in un modificabile * * *`WizardStep`* * *.** Questa impostazione sostituisce il `CreateUserWizardStep` con un equivalente `WizardStep` cui markup definisce un'interfaccia utente che corrisponda il `CreateUserWizardStep`' s. Convertendo il `CreateUserWizardStep` in un `WizardStep` è possibile riposizionare i controlli o aggiungere elementi dell'interfaccia utente aggiuntivi per questo passaggio. Per convertire il `CreateUserWizardStep` o `CompleteWizardStep` in un modificabile `WizardStep`, fare clic sul "Personalizza Create User passaggio" o "Personalizza passaggio completamento" collegamento da Smart Tag del controllo.

- **Utilizzare una combinazione delle due opzioni precedenti.**

Una cosa importante da ricordare è che il controllo CreateUserWizard esegue il processo di creazione di account utente, quando si fa clic sul pulsante "Crea utente" dall'interno relativo `CreateUserWizardStep`. Non è rilevante se esistono altre `WizardStep` s dopo la `CreateUserWizardStep` o non.

Quando si aggiunge un oggetto personalizzato `WizardStep` al controllo CreateUserWizard per raccogliere input utente aggiuntivo, personalizzata `WizardStep` può essere inserita prima o dopo il `CreateUserWizardStep`. Se compare prima di `CreateUserWizardStep` quindi l'input dell'utente aggiuntivi raccolti da personalizzata `WizardStep` è disponibile per il `CreatedUser` gestore dell'evento. Tuttavia, se personalizzata `WizardStep` segue `CreateUserWizardStep` quindi entro l'ora di personalizzata `WizardStep` viene visualizzato il nuovo account utente è già stato creato e `CreatedUser` evento è stato generato.

Figura 21 viene illustrato il flusso di lavoro quando aggiunto `WizardStep` precede il `CreateUserWizardStep`. Poiché le informazioni utente aggiuntive sono stati raccolti dal momento la `CreatedUser` viene generato l'evento, è sufficiente eseguire è aggiornare il `CreatedUser` il gestore eventi per recuperare i dati di input e utilizzarli per il `INSERT` i valori dei parametri dell'istruzione (anziché `DBNull.Value`).


[![Flusso di lavoro CreateUserWizard quando un WizardStep aggiuntive precede il CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figura 21**: il CreateUserWizard del flusso di lavoro quando un aggiuntive `WizardStep` Precedes il `CreateUserWizardStep` ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image63.png))


Se l'oggetto personalizzato `WizardStep` viene inserito *dopo* il `CreateUserWizardStep`, tuttavia, il processo di account utente di creazione si verifica prima che l'utente abbia la possibilità di immettere la città Natale, home page o firma. In tal caso, devono essere inserito nel database dopo aver creato l'account utente, come illustrato nella figura 22 queste informazioni aggiuntive.


[![Flusso di lavoro CreateUserWizard quando viene fornito un WizardStep aggiuntive dopo il CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figura 22**: il CreateUserWizard del flusso di lavoro quando un aggiuntive `WizardStep` fornito dopo il `CreateUserWizardStep` ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image66.png))


Il flusso di lavoro illustrato nella figura 22 attende prima di inserire un record nel `UserProfiles` tabella fino al termine del passaggio 2. Se il visitatore chiude il browser dopo il passaggio 1, tuttavia, si raggiunge uno stato in cui è stato creato un account utente, ma è stato aggiunto nessun record `UserProfiles`. Una soluzione consiste nel disporre di un record con `NULL` o inseriti valori predefiniti `UserProfiles` nel `CreatedUser` gestore di eventi (che viene generato dopo il passaggio 1) e quindi aggiornare questo record di termine del passaggio 2. Ciò garantisce che un `UserProfiles` record verranno aggiunti per l'account utente anche se l'utente viene chiusa a metà il processo di registrazione tramite.

Per questa esercitazione verrà creata una nuova `WizardStep` che si verifica dopo il `CreateUserWizardStep` ma prima che il `CompleteWizardStep`. Ottenere innanzitutto il WizardStep nell'inserire e configurato e quindi verrà esaminiamo il codice.

Smart Tag del controllo CreateUserWizard, selezionare il "Aggiungi/Rimuovi `WizardStep` s", che visualizza il `WizardStep` finestra di dialogo Editor della raccolta. Aggiungere un nuovo `WizardStep`, impostando il relativo `ID` per `UserSettings`, le `Title` per "Impostazioni" e il relativo `StepType` per `Step`. Posizionarlo in modo da posizionarla dopo il `CreateUserWizardStep` ("effettuare l'iscrizione per il nuovo Account") e prima di `CompleteWizardStep` ("completa"), come illustrato nella figura 23.


[![Aggiungere un nuovo WizardStep al controllo CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Nella figura 23**: aggiungere un nuovo `WizardStep` al controllo CreateUserWizard ([fare clic per visualizzare l'immagine ingrandita](storing-additional-user-information-cs/_static/image69.png))


Fare clic su OK per chiudere la `WizardStep` finestra di dialogo Editor della raccolta. Il nuovo `WizardStep` è evidenziato da dichiarativo del controllo CreateUserWizard aggiornato:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Si noti il nuovo `<asp:WizardStep>` elemento. È necessario aggiungere l'interfaccia utente per raccogliere città Natale, home page e firma qui il nuovo utente. Nella sintassi dichiarativa o tramite la finestra di progettazione, è possibile immettere questo contenuto. Per utilizzare la finestra di progettazione, selezionare il passaggio "Impostazioni" nell'elenco di riepilogo a discesa nello Smart Tag per vedere il passaggio nella finestra di progettazione.

> [!NOTE]
> Selezione di un passaggio tramite elenco a discesa dello Smart Tag aggiorna il controllo CreateUserWizard [ `ActiveStepIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), che specifica l'indice del passaggio inizio. Pertanto, se si utilizza questo elenco a discesa per modificare il passaggio "Impostazioni" nella finestra di progettazione, assicurarsi di impostare nuovamente "Sign Up for Your New Account" in modo che questo passaggio viene visualizzato quando gli utenti visitano innanzitutto il `EnhancedCreateUserWizard.aspx` pagina.


Creare un'interfaccia utente all'interno del passo "Impostazioni" che contiene tre controlli casella di testo denominati `HomeTown`, `HomepageUrl`, e `Signature`. Al termine della creazione di questa interfaccia, markup dichiarativo del CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Vado avanti e visitare questa pagina tramite un browser e creare un nuovo account utente, specificando i valori per la città Natale, home page e firma. Dopo aver completato il `CreateUserWizardStep` viene creato l'account utente nel framework di appartenenza e `CreatedUser` eseguito gestore eventi, che aggiunge una nuova riga a `UserProfiles`, ma con un database `NULL` valore per `HomeTown`, `HomepageUrl`, e `Signature`. I valori immessi per la città Natale, home page e firma non vengono mai utilizzati. Il risultato è un nuovo account utente con un `UserProfiles` record il cui `HomeTown`, `HomepageUrl`, e `Signature` campi devono ancora essere specificato.

È necessario eseguire codice dopo il passaggio "Impostazioni" che accetta l'home città, honepage e firma i valori immessi dall'utente e gli aggiornamenti appropriati `UserProfiles` record. Controllare ogni volta che l'utente si sposta tra i passaggi di una procedura guidata, la procedura guidata [ `ActiveStepChanged` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) generato. È possibile creare un gestore eventi per questo evento e l'aggiornamento di `UserProfiles` tabella quando è stato completato il passaggio "Impostazioni".

Aggiungere un gestore eventi per il CreateUserWizard `ActiveStepChanged` eventi e aggiungere il codice seguente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Il codice sopra riportato avvia determinando se è stato raggiunto solo il passaggio "Completa". Poiché il passaggio "Completa" si verifica immediatamente dopo il passaggio "Impostazioni", quindi quando raggiunge il visitatore "Completa" passaggio che indica che l'utente appena terminato il passaggio "Impostazioni".

In tal caso, è necessario a livello di codice fa riferimento ai controlli casella di testo all'interno di `UserSettings WizardStep`. Questa operazione viene eseguita utilizzando innanzitutto il `FindControl` metodo a livello di codice che fa riferimento il `UserSettings WizardStep`e quindi nuovamente per fare riferimento a caselle di testo all'interno di `WizardStep`. Una volta caselle di testo è stato fatto riferimento, si è pronti per eseguire il `UPDATE` istruzione. Il `UPDATE` istruzione ha lo stesso numero di parametri come la `INSERT` istruzione il `CreatedUser` gestore eventi, ma qui è utilizzare la home città, home page e firma i valori forniti dall'utente.

A questo gestore eventi sul posto, visitare il `EnhancedCreateUserWizard.aspx` pagina tramite un browser e creare un nuovo account utente specificando i valori per la città Natale, home page e firma. Dopo aver creato il nuovo account devono essere reindirizzate al `AdditionalUserInfo.aspx` pagina, in cui appena immesso home città, home page e firma vengono visualizzate le informazioni.

> [!NOTE]
> Il sito Web dispone di due pagine da cui un visitatore può creare un nuovo account: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. Mappa del sito del sito Web e pagina di accesso scegliere il `CreatingUserAccounts.aspx` pagina, ma la `CreatingUserAccounts.aspx` pagina non richiedere all'utente le informazioni di città, home page e firma principale e non aggiunge una riga corrispondente a `UserProfiles`. Di conseguenza, aggiornare il `CreatingUserAccounts.aspx` pagina in modo che offre questa funzionalità o aggiornare la pagina della mappa del sito e l'account di accesso per fare riferimento a `EnhancedCreateUserWizard.aspx` anziché `CreatingUserAccounts.aspx`. Se si sceglie la seconda opzione, assicurarsi di aggiornare il `Membership` della cartella `Web.config` file in modo da consentire l'accesso a utenti anonimi di `EnhancedCreateUserWizard.aspx` pagina.


## <a name="summary"></a>Riepilogo

In questa esercitazione è stato esaminato le tecniche per la modellazione di dati relativi agli account utente nell'ambito di appartenenza. In particolare, è stato esaminato modellazione di entità che condividono una relazione uno-a-molti con l'account utente, nonché i dati che condivide una relazione uno a uno. Inoltre, è stato illustrato come questo correlato potrebbero visualizzate, inserite e aggiornate, con alcuni esempi di utilizzo del controllo SqlDataSource e altre informazioni utilizzando il codice ADO.NET.

Questa esercitazione consente di completare il nostro esaminare gli account utente. A partire dalla prossima esercitazione verrà convertito l'attenzione ai ruoli. Su quella successiva diverse esercitazioni verranno esaminati il framework di ruoli, vedere come creare nuovi ruoli, come assegnare ruoli agli utenti, come per determinare quali ruoli appartiene a un utente e come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [L'accesso e l'aggiornamento dei dati in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Controllo ASP.NET 2.0 guidata](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Creazione di un'interfaccia utente dettagliate con il controllo ASP.NET 2.0 guidata](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Creazione di parametri di controllo origine dati personalizzata](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizzazione del controllo CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Guida introduttiva di controllo DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Visualizzazione dei dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [La modifica di inserimento ed eliminazione di dati](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Convalida del form in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [La raccolta di informazioni di registrazione utente personalizzata](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profili in ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Il controllo asp: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Guida introduttiva di profili utente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Precedente](user-based-authorization-cs.md)
[Successivo](creating-the-membership-schema-in-sql-server-vb.md)
