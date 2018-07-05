---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Creazione di account utente (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verranno analizzati usando il framework di appartenenza (tramite il provider SqlMembershipProvider) per creare nuovi account utente. Si vedrà come creare nuovi Stati Uniti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 161d7b71f169252699e0c33ad969bef9c2c62d5f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386598"
---
<a name="creating-user-accounts-c"></a>Creazione di account utente (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> In questa esercitazione verranno analizzati usando il framework di appartenenza (tramite il provider SqlMembershipProvider) per creare nuovi account utente. Si vedrà come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato di NET.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [precedente esercitazione](creating-the-membership-schema-in-sql-server-cs.md) è stato installato lo schema di servizi dell'applicazione in un database, aggiunta di tabelle, viste e stored procedure necessite per il `SqlMembershipProvider` e `SqlRoleProvider`. Ciò creata l'infrastruttura che è necessario per il resto delle esercitazioni di questa serie. In questa esercitazione verranno analizzati usando il framework di appartenenza (tramite il `SqlMembershipProvider`) per creare nuovi account utente. Si vedrà come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato di NET.

Oltre a imparare a creare nuovi account utente, è anche necessario aggiorna il sito Web demo abbiamo creati prima di tutto nel *<a id="_msoanchor_2"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-cs.md)* esercitazione e quindi migliorato nel  *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"> </a> configurazione dell'autenticazione form e argomenti avanzati* esercitazione. L'applicazione web demo dispone di una pagina di accesso che convalida le credenziali degli utenti su coppie di nome utente/password hardcoded. Inoltre, `Global.asax` include codice che crea personalizzato `IPrincipal` e `IIdentity` oggetti per gli utenti autenticati. Microsoft potrebbe aggiornare la pagina di accesso per convalidare le credenziali degli utenti con il framework di appartenenza e rimuovere la logica personalizzata di principal e identity.

Iniziamo!

## <a name="the-forms-authentication-and-membership-checklist"></a>L'autenticazione basata su form e l'elenco di controllo per l'appartenenza

Prima di iniziare funzionante con il framework di appartenenza, è opportuno esaminare i passaggi importanti che sono state prese per raggiungere questo punto. Quando si usa il framework di appartenenza con il `SqlMembershipProvider` in uno scenario di autenticazione basata su form, i passaggi seguenti devono essere eseguite prima di implementare funzionalità di appartenenza nell'applicazione web:

1. **Abilitare l'autenticazione basata su form.** Come descritto  *<a id="_msoanchor_4"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-cs.md)*, autenticazione basata su form è abilitata per la modifica `Web.config` e impostando il `<authentication>` dell'elemento `mode` dell'attributo `Forms`. Con autenticazione basata su form abilitato, viene esaminato ogni richiesta in ingresso per un *ticket di autenticazione form*, che, se presente, identifica il richiedente.
2. **Aggiungere lo schema di servizi dell'applicazione al database appropriato.** Quando si usa il `SqlMembershipProvider` è necessario installare lo schema di servizi dell'applicazione a un database. In genere questo schema viene aggiunto allo stesso database che contiene il modello di dati dell'applicazione. Il *<a id="_msoanchor_5"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* esercitazione preso in esame usando le `aspnet_regsql.exe` dello strumento per eseguire questa operazione.
3. **Personalizzare le impostazioni dell'applicazione Web per fare riferimento al database dal passaggio 2.** Il *creazione dello Schema di appartenenza in SQL Server* esercitazione illustra due modi per configurare l'applicazione web in modo che le `SqlMembershipProvider` userà il database selezionato nel passaggio 2: modificando il `LocalSqlServer` nome stringa di connessione; o aggiungendo un nuovo provider registrati all'elenco dei provider di appartenenze framework e personalizzazione tale nuovo provider per usare il database dal passaggio 2.

Quando la compilazione di un'applicazione web che utilizza il `SqlMembershipProvider` e l'autenticazione basata su form, sarà necessario eseguire questi tre passaggi prima di usare il `Membership` classe o i controlli Web di accesso di ASP.NET. Poiché è già stata eseguita questa procedura nelle esercitazioni precedenti, siamo pronti per iniziare a usare il framework di appartenenza.

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i successivi tre è verrà esaminando le funzioni relative alle appartenenze e le funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. È possibile creare le pagine e quindi un file di mappa del sito `(Web.sitemap)`.

Iniziare creando una nuova cartella nel progetto denominato `Membership`. Successivamente, aggiungere cinque nuove pagine ASP.NET per la `Membership` cartella, ogni pagina con il collegamento di `Site.master` pagina master. Denominare le pagine:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Esplora soluzioni del progetto a questo punto dovrebbe essere simile allo screenshot illustrato nella figura 1.


[![Sono stati aggiunti cinque nuove pagine nella cartella di appartenenza](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Figura 1**: cinque nuove pagine sono stati aggiunti per il `Membership` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image3.png))


Ogni pagina, a questo punto, avranno i controlli contenuto di due, uno per ognuno degli elementi ContentPlaceHolder della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Si tenga presente che il `LoginContent` markup predefinite del controllo ContentPlaceHolder viene visualizzato un collegamento per effettuare l'accesso o disconnessione dal sito, a seconda che l'utente è autenticato. La presenza del `Content2` controllo contenuto, tuttavia, esegue l'override di markup predefinito della pagina master. Come descritto *<a id="_msoanchor_6"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-cs.md)* esercitazione, ciò è utile nelle pagine in cui non si desidera visualizzare le opzioni correlate all'accesso nella colonna sinistra.

Per questi cinque pagine, tuttavia, si vuole visualizzare il markup predefinito della pagina master per il `LoginContent` ContentPlaceHolder. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, ognuno dei markup della pagina cinque deve contenere solo un controllo contenuto.

## <a name="step-2-creating-the-site-map"></a>Passaggio 2: Creazione della mappa del sito

Tutti tranne i siti Web più semplice da implementare qualche forma di un'interfaccia utente per la navigazione. L'interfaccia utente di navigazione può essere un semplice elenco di collegamenti alle diverse sezioni del sito. In alternativa, i collegamenti seguenti potrebbero essere disposti in menu o delle visualizzazioni dell'albero. Come gli sviluppatori di pagine, la creazione dell'interfaccia utente per la navigazione è solo la metà della storia. È necessario anche metodi per definire la struttura logica del sito in modo gestibile e aggiornabile. Quando vengono aggiunte nuove pagine o rimuovere pagine esistenti, si desidera essere in grado di aggiornare una singola origine, la mappa del sito: e riflettono tali modifiche nell'interfaccia utente per la navigazione del sito.

Queste due attività, che definisce la mappa del sito e che implementa un'interfaccia utente per la navigazione basata sulla mappa del sito, è facile grazie al framework di mappa del sito e i controlli di spostamento Web aggiunto in ASP.NET versione 2.0. Il framework di mappa del sito consente agli sviluppatori di definire una mappa del sito e quindi accedervi tramite un'API a livello di codice (il [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Predefinito controlli Web di navigazione, includere una [controllo Menu](https://msdn.microsoft.com/library/bz09dy46.aspx), il [controllo TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)e il [controllo SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Ad esempio i framework di appartenenza e ruoli, il framework di mappa del sito è stato creato nella parte superiore di [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Il processo della classe di provider di mappa del sito consiste nel generare la struttura in memoria usata dal `SiteMap` classe da un archivio dati permanente, ad esempio un file XML o una tabella di database. .NET Framework viene fornito con un provider di mappa del sito predefinito che legge i dati della mappa del sito da un file XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), e si tratta del provider verranno usati in questa esercitazione. Per alcune implementazioni del provider della mappa del sito alternativi, vedere la sezione ulteriori letture alla fine di questa esercitazione.

Il provider di mappa del sito predefinito prevede che un file XML formattato correttamente denominato `Web.sitemap` l'esistenza della directory radice. Poiché si sta usando il provider predefinito, è necessario aggiungere tale file e definire la struttura della mappa del sito nel formato XML appropriato. Per aggiungere il file, fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Nella finestra di dialogo, scegliere di aggiungere un file del tipo di mappa del sito denominato `Web.sitemap`.


[![Aggiungere un File denominato Web. sitemap alla Directory radice del progetto](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Figura 2**: aggiungere un File denominato `Web.sitemap` alla Directory radice del progetto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image6.png))


Il file di mappa del sito XML definisce la struttura del sito Web come una gerarchia. Questa relazione gerarchica è modellata nel file XML tramite la cronologia del `<siteMapNode>` elementi. Il `Web.sitemap` deve iniziare con un `<siteMap>` nodo padre che dispone di esattamente un `<siteMapNode>` figlio. Questo livello superiore `<siteMapNode>` elemento rappresenta la radice della gerarchia e potrebbe essere un numero arbitrario di nodi discendenti. Ogni `<siteMapNode>` elemento deve includere una `title` dell'attributo e può includere facoltativamente `url` e `description` gli attributi, tra gli altri; ogni non vuoto `url` attributo deve essere univoco.

Immettere il seguente codice XML nel `Web.sitemap` file:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

Il markup di mappa del sito precedente definisce la gerarchia illustrata nella figura 3.


[![Mappa del sito rappresenta una struttura per la navigazione gerarchica](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Figura 3**: la mappa del sito rappresenta una struttura di spostamento gerarchica ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Passaggio 3: Aggiornamento della pagina Master per includere un'interfaccia utente per la navigazione

ASP.NET include un numero di navigazione di controlli Web per la progettazione di un'interfaccia utente. Sono inclusi i Menu, TreeView e i controlli SiteMapPath. I controlli Menu e TreeView rappresentati rispettivamente, la struttura della mappa del sito in un menu o una struttura ad albero, mentre il controllo SiteMapPath consente di visualizzare una barra di navigazione che viene visualizzato il nodo corrente da visitare, nonché i relativi predecessori. I dati della mappa del sito può essere associato ad altri dati controlli Web, usando il controllo SiteMapDataSource ed è possibile accedervi a livello di programmazione tramite le `SiteMap` classe.

Poiché una discussione approfondita del framework di mappa del sito e i controlli di spostamento è oltre l'ambito di questa serie di esercitazioni, invece di dedicare tempo alla creazione la propria interfaccia utente per la navigazione è possibile invece presi in prestito quella usata nella mio *[ Uso dei dati in ASP.NET 2.0](../../data-access/index.md)* serie di esercitazioni, che usa un controllo Repeater per visualizzare un elenco puntato SVM a due dei collegamenti di navigazione, come illustrato nella figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Aggiunta di un elenco a due livelli di collegamenti nella colonna sinistra

Per creare questa interfaccia, aggiungere il seguente markup dichiarativo per la `Site.master` pagina master della colonna a sinistra in cui il testo "TODO: Menu consentono di accedere..." al momento si trova.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

Il markup riportato sopra si associa un controllo Repeater denominato `menu` a un controllo SiteMapDataSource, che restituisce la gerarchia della mappa sito definita `Web.sitemap`. Poiché il controllo SiteMapDataSource [ `ShowStartingNode` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) è impostata su False inizia a restituire gerarchia della mappa del sito iniziano con i discendenti del nodo "Home". Il Repeater ciascuno di questi nodi (attualmente solo "appartenenza") vengono visualizzati un `<li>` elemento. Un altro, interna Repeater Visualizza quindi gli elementi figlio del nodo corrente in un elenco non ordinato annidato.

Figura 4 Mostra output di rendering del markup riportato sopra con struttura della mappa del sito che è stato creato nel passaggio 2. Il controllo Repeater esegue il rendering di markup di "vanilla" elenco non ordinato. le regole di foglio di stile CSS definita `Styles.css` sono responsabili per il layout esteticamente piacevole. Per una descrizione più dettagliata del funzionamento del markup riportato sopra, vedere la [pagine Master e spostamento nel sito](https://asp.net/learn/data-access/tutorial-03-cs.aspx) esercitazione.


[![L'interfaccia utente per la navigazione è sottoposto a rendering utilizzando annidati non ordinati sono elencati](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Figura 4**: l'interfaccia utente per la navigazione è sottoposto a rendering utilizzando annidati non ordinata elenchi ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Aggiunta dello spostamento Breadcrumb

Oltre all'elenco di collegamenti nella colonna sinistra, è possibile anche avere ogni visualizzazione delle pagine una [breadcrumb](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Un percorso di navigazione è un elemento dell'interfaccia utente per la navigazione che indica rapidamente agli utenti la posizione corrente nella gerarchia del sito. Il controllo SiteMapPath Usa il framework di mappa del sito per determinare il percorso della pagina corrente nella mappa del sito e quindi visualizza una barra di navigazione basato su queste informazioni.

In particolare, aggiungere un `<span>` elemento all'intestazione della pagina master `<div>` elemento e impostare il nuovo `<span>` dell'elemento `class` "breadcrumb" dell'attributo. (Il `Styles.css` classe contiene una regola per una classe "breadcrumb".) Successivamente, aggiungere un SiteMapPath al nuovo `<span>` elemento.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

Figura 5 mostra l'output del controllo SiteMapPath quando si visita `~/Membership/CreatingUserAccounts.aspx`.


[![Il Breadcrumb Visualizza la pagina corrente e i predecessori del sito eseguire il mapping](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Figura 5**: il Breadcrumb Visualizza la pagina corrente e i relativi predecessori della mappa del sito ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Passaggio 4: Rimuovere l'entità personalizzata e logica di identità

Nel *<a id="_msoanchor_7"> </a> [configurazione dell'autenticazione form e argomenti avanzati](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* esercitazione è stato illustrato come associare oggetti principal e identity personalizzati per l'utente autenticato. Tale processo tramite la creazione di un gestore eventi in `Global.asax` per l'applicazione `PostAuthenticateRequest` evento, che viene attivato dopo il `FormsAuthenticationModule` ha autenticato l'utente. In questo gestore dell'evento è stato sostituito il `GenericPrincipal` e `FormsIdentity` oggetti aggiunti dalle `FormsAuthenticationModule` con il `CustomPrincipal` e `CustomIdentity` oggetti è stato creato in tale esercitazione.

Anche se gli oggetti principal e identity personalizzati sono utili in alcuni scenari, nella maggior parte dei casi il `GenericPrincipal` e `FormsIdentity` gli oggetti sono sufficienti. Di conseguenza, penso che sarebbe utile per restituire il comportamento predefinito. Apportare questa modifica rimuovendo o impostare come commento il `PostAuthenticateRequest` gestore dell'evento o eliminando il `Global.asax` file interamente.

## <a name="step-5-programmatically-creating-a-new-user"></a>Passaggio 5: A livello di programmazione creando un nuovo utente

Per creare un nuovo account utente tramite l'uso di framework di appartenenza i `Membership` della classe [ `CreateUser` metodo](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Questo metodo dispone di parametri di input per il nome utente, password e altri campi relativi agli utenti. Quando vengono chiamati, delega la creazione del nuovo account utente al provider di appartenenze configurati e quindi restituisce un [ `MembershipUser` oggetto](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) che rappresenta l'account utente appena creato.

Il `CreateUser` metodo dispone di quattro overload, ciascuna delle quali accetta un numero diverso di parametri di input:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Questi quattro overload si differenziano per la quantità di informazioni raccolte. Il primo overload, ad esempio, richiede solo il nome utente e password per il nuovo account utente, mentre il secondo richiede anche l'indirizzo di posta elettronica dell'utente.

Questi overload esistono perché le informazioni necessarie per creare un nuovo account utente dipendono dalle impostazioni di configurazione del provider di appartenenze. Nel *<a id="_msoanchor_8"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* esercitazione sono stati esaminati specifica impostazioni di configurazione di provider di appartenenza in `Web.config`. Tabella 2 è incluso un elenco completo delle impostazioni di configurazione.

Una tale configurazione del provider appartenenza impostazione che influisce su cosa `CreateUser` overload può essere usato è il `requiresQuestionAndAnswer` impostazione. Se `requiresQuestionAndAnswer` è impostata su `true` (impostazione predefinita), quindi quando si crea un nuovo account utente è necessario specificare una domanda di sicurezza e una risposta. Queste informazioni vengono utilizzate in un secondo momento se l'utente dovrà reimpostare o modificare la password. In particolare, in quel momento vengono visualizzati la domanda di sicurezza e gli utenti devono immettere la risposta corretta per reimpostare o modificare la password. Di conseguenza, se il `requiresQuestionAndAnswer` è impostata su `true` la chiamata a uno dei primi due `CreateUser` overload genera un'eccezione perché la domanda di sicurezza e la risposta sono mancanti. Poiché l'applicazione è attualmente configurata per richiedere una domanda di sicurezza e una risposta, è necessario usare uno dei due overload quest'ultimo durante la creazione dell'utente a livello di codice.

Per illustrare l'utilizzo di `CreateUser` metodo, è possibile creare un'interfaccia utente in cui si richiede all'utente per il proprio nome, password, indirizzo di posta elettronica e una risposta alla domanda di sicurezza predefiniti. Aprire il `CreatingUserAccounts.aspx` nella pagina di `Membership` cartella e aggiungere i seguenti controlli Web per il controllo contenuto:

- Una casella di testo denominato `Username`
- Una casella di testo denominato `Password`, la cui `TextMode` è impostata su `Password`
- Una casella di testo denominato `Email`
- Un'etichetta denominata `SecurityQuestion` con relativo `Text` cancellare la proprietà
- Una casella di testo denominato `SecurityAnswer`
- Un pulsante denominato `CreateAccountButton` proprietà il cui testo è impostata su "Crea l'Account utente"
- Un controllo etichetta denominato `CreateAccountResults` con relativo `Text` cancellare la proprietà

A questo punto la schermata dovrebbe essere simile allo screenshot illustrato nella figura 6.


[![Aggiungere i vari controlli Web alla pagina CreatingUserAccounts.aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Figura 6**: aggiungere i vari controlli Web per il `CreatingUserAccounts.aspx` pagina ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image18.png))


Il `SecurityQuestion` Label e `SecurityAnswer` nella casella di testo sono progettati per visualizzare una domanda di sicurezza predefiniti e di raccogliere la risposta dell'utente. Si noti che la domanda di sicurezza e la risposta vengono archiviati in base a utente, pertanto è possibile consentire a ogni utente definire le proprie domande di sicurezza. Tuttavia, per questo esempio ho deciso di utilizzare una domanda di sicurezza universali, vale a dire: "Qual è il colore preferito?"

Per implementare questa domanda di sicurezza predefinite, aggiungere una costante alla classe code-behind della pagina denominato `passwordQuestion`, assegnandolo alla domanda di sicurezza. Quindi, nella `Page_Load` gestore dell'evento, assegnare questo costante per il `SecurityQuestion` dell'etichetta `Text` proprietà:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

Successivamente, creare un gestore eventi per il `CreateAccountButton`del `Click` eventi e aggiungere il codice seguente:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

Il `Click` gestore dell'evento inizia con la definizione di una variabile denominata `createStatus` typu [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` è un'enumerazione che indica lo stato del `CreateUser` operazione. Ad esempio, se l'account utente viene creata correttamente, l'oggetto risultante `MembershipCreateStatus` istanza verrà impostata sul valore `Success`; sulla base di altra parte, se l'operazione ha esito negativo perché esiste già un utente con lo stesso nome utente, verrà impostato su un valore di `DuplicateUserName`. Nel `CreateUser` overload utilizziamo, dobbiamo passare una `MembershipCreateStatus` istanza al metodo come un `out` parametro. Questo parametro è impostato sul valore appropriato all'interno di `CreateUser` metodo ed è possibile esaminare il relativo valore dopo la chiamata al metodo per determinare se l'account utente è stato creato correttamente.

Dopo avere chiamato `CreateUser`, passando `createStatus`, un `switch` istruzione viene utilizzata per restituire un messaggio appropriato in base al valore assegnato a `createStatus`. Figure 7 mostra l'output quando è stato creato un nuovo utente. Figure 8 e 9 mostra l'output quando non viene creato l'account utente. Nella figura 8, il visitatore immesso una password, cinque lettere che non soddisfa i requisiti di complessità delle password venga specificati nelle impostazioni di configurazione del provider di appartenenze. Nella figura 9, il visitatore sta tentando di creare un account utente con un nome utente esistente (quello creato nella figura 7).


[![Un nuovo Account utente è stato creato](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Figura 7**: un nuovo Account utente è stato creato ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image21.png))


[![L'Account utente non viene creato perché la Password specificata è troppo debole](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Figura 8**: l'Account dell'utente non viene creato perché la Password specificata è troppo debole ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image24.png))


[![L'Account utente non è che creata perché il nome utente è già in uso](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Figura 9**: l'Account dell'utente non è creata perché il nome utente è già in uso ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image27.png))


> [!NOTE]
> Si potrebbe chiedere come determinare l'esito positivo o negativo quando si utilizza uno dei primi due `CreateUser` overload del metodo, nessuno dei quali presenta un parametro di tipo `MembershipCreateStatus`. Questi primi due overload generano un [ `MembershipCreateUserException` eccezioni](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) in caso di errore, che include un [ `StatusCode` proprietà](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.


Dopo aver creato alcuni account utente, verificare che siano stati creati gli account dall'elenco del contenuto dei `aspnet_Users` e `aspnet_Membership` nelle tabelle di `SecurityTutorials.mdf` database. Come illustrato nella figura 10, è stato aggiunto due utenti tramite il `CreatingUserAccounts.aspx` pagina: Tito e Bruce.


[![Sono presenti due utenti nel Store di utente di appartenenza: Tito e Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Figura 10**: sono presenti due utenti nel Store di utente di appartenenza: Tito e Bruce ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image30.png))


Mentre l'archivio utente di appartenenza include ora le informazioni sull'account di Bruce e di Tito, dobbiamo ancora implementare funzionalità che consente di Bruce o Tito accedere al sito. Attualmente `Login.aspx` convalida le credenziali dell'utente rispetto a un set di coppie di nome utente/password – hardcoded avviene *non* convalidare le credenziali specificate con il framework di appartenenza. Per ora visualizzare i nuovi account utente nel `aspnet_Users` e `aspnet_Membership` tabelle dovranno bastare. Nell'esercitazione successiva  *<a id="_msoanchor_9"> </a> [convalida utente credenziali contro l'appartenenza utente Store](validating-user-credentials-against-the-membership-user-store-cs.md)*, verrà aggiornata la pagina di accesso da convalidare a fronte dell'archivio di appartenenza.

> [!NOTE]
> Se non è possibile visualizzare tutti gli utenti nel `SecurityTutorials.mdf` database, è possibile che l'applicazione web Usa il provider di appartenenze predefinito, `AspNetSqlMembershipProvider`, che usa il `ASPNETDB.mdf` database come suo archivio utente. Per determinare se è questo il problema, fare clic sul pulsante Aggiorna in Esplora soluzioni. Se un database denominato `ASPNETDB.mdf` è stato aggiunto il `App_Data` cartella, questo è il problema. Tornare al passaggio 4 della *<a id="_msoanchor_10"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* esercitazione per istruzioni su come configurare correttamente il provider di appartenenze.


Nella maggior parte dei utenti creati scenari di account, il visitatore viene presentato con un tipo di interfaccia di immettere il nome utente, password, indirizzo di posta elettronica e altre informazioni essenziali, a questo punto viene creato un nuovo account. In questo passaggio viene preso in esame creazione manuale di tale interfaccia e quindi illustrato come usare il `Membership.CreateUser` metodo a livello di codice aggiungere il nuovo account utente in base gli input dell'utente. Il codice, tuttavia, appena creato il nuovo account utente. Non ha raggiunto prestazioni conseguenti azioni, ad esempio la registrazione dell'utente al sito con l'account utente appena creato, o l'invio di un messaggio di posta elettronica di conferma all'utente. Questi passaggi aggiuntivi richiede codice aggiuntivo del pulsante `Click` gestore dell'evento.

ASP.NET viene fornito con il controllo CreateUserWizard, che è progettato per gestire il processo di creazione account utente, dal rendering di un'interfaccia utente per la creazione di nuovi account utente, per la creazione dell'account di Framework di appartenenza e l'esecuzione di post-account attività di creazione, ad esempio l'invio di un messaggio di posta elettronica di conferma e l'accesso l'utente appena creato il sito. Usa il controllo CreateUserWizard è semplice come trascinare il controllo CreateUserWizard dalla casella degli strumenti in una pagina e quindi impostando alcune proprietà. Nella maggior parte dei casi, non sarà necessario scrivere una singola riga di codice. Esamineremo questo controllo ingegnosa in dettaglio nel passaggio 6.

Se i nuovi account utente vengono create solo tramite una normale pagina web di creare un Account, è improbabile che sia mai necessario scrivere codice che usa il `CreateUser` metodo, come il controllo CreateUserWizard probabilmente soddisferà le proprie esigenze. Tuttavia, il `CreateUser` metodo è utile negli scenari in cui è necessario un'esperienza utente di creare un Account altamente personalizzata o quando è necessario creare nuovi account utente tramite un'interfaccia alternativa. Ad esempio, potrebbe essere una pagina che consente agli utenti di caricare un file XML contenente le informazioni utente da un'altra applicazione. La pagina potrebbe essere analizza il contenuto del codice XML caricato file e crea un nuovo account per ogni utente rappresentato nel codice XML tramite la chiamata di `CreateUser` (metodo).

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Passaggio 6: Creazione di un nuovo utente con il controllo CreateUserWizard

ASP.NET viene fornito con un numero di controlli Web di accesso. Questi controlli semplificano molti scenari utente comuni relative all'account e account di accesso. Il [controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) una tale controllo progettato per presentare un'interfaccia utente per l'aggiunta di un nuovo account utente per il framework di appartenenza.

Come molti altri controlli correlati all'accesso Web, CreateUserWizard può essere usato senza scrivere una sola riga di codice. In modo intuitivo fornisce un'interfaccia utente basata su provider di appartenenze le impostazioni di configurazione e chiama internamente il `Membership` della classe `CreateUser` metodo dopo che l'utente immette le informazioni necessarie e fa clic sul pulsante "Create User". Il controllo CreateUserWizard è estremamente personalizzabile. Esistono una serie di eventi che attivano durante varie fasi del processo di creazione di account. È possibile creare gestori eventi, in base alle esigenze, per inserire la logica personalizzata nel flusso di lavoro di creazione di account. Inoltre, l'aspetto del CreateUserWizard è molto flessibile. Sono disponibili numerose proprietà che definiscono l'aspetto dell'interfaccia predefinita. Se necessario, il controllo può essere convertito in un modello o la registrazione utente aggiuntive "procedura" può essere aggiunti.

Iniziamo esaminando l'utilizzo del controllo CreateUserWizard interfaccia predefinita e il comportamento. Verrà quindi illustrato come personalizzare l'aspetto tramite proprietà ed eventi del controllo.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Esaminare l'interfaccia predefinita e il comportamento di CreateUserWizard

Tornare al `CreatingUserAccounts.aspx` nella pagina di `Membership` cartella, passare alla modalità di progettazione o Dividi e quindi aggiungere un controllo CreateUserWizard nella parte superiore della pagina. Il controllo CreateUserWizard si trova nella sezione relativa ai controlli di accesso della casella degli strumenti. Dopo aver aggiunto il controllo, impostare relativi `ID` proprietà `RegisterUser`. Come l'immagine riportata nella figura 11 Mostra, CreateUserWizard esegue il rendering di un'interfaccia con caselle di testo per il nuovo utente nome utente, password, indirizzo di posta elettronica e domanda di sicurezza e risposte.


[![I renderer di controllo CreateUserWizard generica crea interfaccia utente](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Figura 11**: il controllo CreateUserWizard esegue il rendering di un'interfaccia utente generica creare ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image33.png))


È opportuno confrontare l'interfaccia utente predefinita generata dal controllo CreateUserWizard con l'interfaccia che è stato creato nel passaggio 5. Prima di tutto, il controllo CreateUserWizard consente il visitatore specificare sia la domanda di sicurezza e la risposta, mentre l'interfaccia creata manualmente utilizzata una domanda di sicurezza predefiniti. Interfaccia del controllo CreateUserWizard include anche i controlli di convalida, mentre avevamo ancora implementare la convalida nei campi modulo nostro dell'interfaccia. E l'interfaccia del controllo CreateUserWizard include una casella di testo "Confermare Password" (insieme a un controllo CompareValidator per garantire che il testo immesso la "Password" e "Password confrontare" nelle caselle di testo sono uguali).

La cosa interessante è che il controllo CreateUserWizard consulta le impostazioni di configurazione del provider di appartenenze per il rendering dell'interfaccia utente. Ad esempio, la sicurezza domande e risposte nelle caselle di testo vengono visualizzati solo se `requiresQuestionAndAnswer` è impostata su True. Analogamente, CreateUserWizard aggiunge automaticamente un controllo RegularExpressionValidator per assicurarsi che siano soddisfatti i requisiti di complessità delle password e imposta relativi `ErrorMessage` e `ValidationExpression` proprietà in base il `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression` le impostazioni di configurazione.

Il controllo CreateUserWizard, come suggerisce il nome, è derivato dal [controllo della procedura guidata](https://msdn.microsoft.com/library/s2etd1ek.aspx). Controlli procedura guidata sono progettati per fornire un'interfaccia per il completamento delle attività più passaggi. Un controllo della procedura guidata potrebbe essere un numero arbitrario di `WizardSteps`, ognuno dei quali è un modello che definisce il codice HTML e controlli per Web, tale passaggio. Controllo della procedura guidata visualizza inizialmente il primo `WizardStep`, insieme ai controlli di navigazione che consentono all'utente per passare da un passaggio alla successiva, o di tornare ai passaggi precedenti.

Come illustrato nella figura 11 markup dichiarativo, interfaccia predefinita del controllo CreateUserWizard include due `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) : esegue il rendering l'interfaccia per raccogliere informazioni per la creazione del nuovo account utente. Questo è il passaggio mostrato nella figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) : esegue il rendering di un messaggio che indica che l'account è stato creato correttamente.

Aspetto e il comportamento di CreateUserWizard può essere modificati mediante la conversione di uno di questi passaggi per i modelli o aggiungendo il proprio `WizardSteps`. Verranno ora esaminati aggiungendo un `WizardStep` all'interfaccia di registrazione nel *archiviare informazioni utente aggiuntive* esercitazione.

Vediamo il controllo CreateUserWizard in azione. Visita il `CreatingUserAccounts.aspx` pagina tramite un browser. Immettere alcuni valori non validi nell'interfaccia del CreateUserWizard prima. Provare a immettere una password che non sono conformi ai requisiti di complessità o lasciando il "nome utente" nella casella di testo vuoto. CreateUserWizard verrà visualizzato un messaggio di errore appropriato. Figura 12 mostra l'output quando si prova a creare un utente con una password non sufficientemente complessa.


[![CreateUserWizard inserisce automaticamente i controlli di convalida](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Figura 12**: The CreateUserWizard automaticamente inserisce i controlli di convalida ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image36.png))


Successivamente, immettere i valori appropriati in CreateUserWizard e fare clic sul pulsante "Create User". Supponendo che i campi obbligatori sono stati immessi e complessità della password è sufficiente, CreateUserWizard crea un nuovo account utente tramite il framework di appartenenza e quindi visualizzano il `CompleteWizardStep`dell'interfaccia (vedere la figura 13). Dietro le quinte, CreateUserWizard chiama il `Membership.CreateUser` metodo, come abbiamo fatto nel passaggio 5.


[![Un nuovo Account utente è stato creato correttamente](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Figura 13**: un nuovo Account utente è stato creato correttamente ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image39.png))


> [!NOTE]
> Come illustrato nella figura 13, il `CompleteWizardStep`dell'interfaccia include un pulsante Continua. Tuttavia, a questo punto selezionandolo appena esegue un postback, lasciando il visitatore nella stessa pagina. Nella sezione "Personalizzazione di aspetto e il comportamento tramite le proprietà di CreateUserWizard" si esaminerà come si può avere questo pulsante di inviare il visitatore per `Default.aspx` (o un'altra pagina).


Dopo aver creato un nuovo account utente, tornare a Visual Studio ed esaminare i `aspnet_Users` e `aspnet_Membership` tabelle come abbiamo fatto nella figura 10 per verificare che l'account è stato creato correttamente.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizzazione di CreateUserWizard comportamento e aspetto tramite le relative proprietà

È possibile personalizzare in svariati modi, tramite le proprietà, CreateUserWizard `WizardSteps`e gestori di eventi. In questa sezione si esaminerà come personalizzare l'aspetto del controllo tramite le relative proprietà; Nella sezione successiva vengono esaminati si estende il comportamento del controllo tramite i gestori eventi.

Praticamente tutto il testo visualizzato nell'interfaccia utente predefinita del controllo CreateUserWizard può essere personalizzato tramite la vasta gamma di proprietà. Ad esempio, le etichette "User Name", "Password", "Conferma Password", "Messaggio di posta elettronica", "Domanda di sicurezza" e "Risposta segreta" visualizzate a sinistra delle caselle di testo possono essere personalizzate per il [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), e [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx)le proprietà, rispettivamente. Analogamente, sono disponibili proprietà per specificare il testo per i pulsanti "Create User" e "Continua" nel `CreateUserWizardStep` e `CompleteWizardStep`, nonché come se questi pulsanti vengono visualizzati come pulsanti, i controlli LinkButton o ImageButtons.

I colori, i bordi, i tipi di carattere e altri elementi visivi possono essere configurate tramite una serie di proprietà di stile. Lo stesso controllo CreateUserWizard presenta le proprietà di stile di controllo Web comuni – `BackColor`, `BorderStyle`, `CssClass`, `Font`e così via, e sono disponibili numerose proprietà di stile per la definizione dell'aspetto per sezioni specifiche del Interfaccia del CreateUserWizard. Il [ `TextBoxStyle` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), ad esempio, definisce gli stili per le caselle di testo nel `CreateUserWizardStep`, mentre il [ `TitleTextStyle` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definisce lo stile per il titolo ("Sign Up for Your nuovo Account").

Oltre alle proprietà correlate all'aspetto, sono disponibili numerose proprietà che influiscono sul comportamento del controllo CreateUserWizard. Il [ `DisplayCancelButton` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se impostato su True, consente di visualizzare un pulsante Annulla accanto al pulsante "Create User" (il valore predefinito è False). Se si visualizza il pulsante Annulla, assicurarsi di impostare anche il [ `CancelDestinationPageUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), che specifica la pagina all'utente venga inviato a dopo facendo clic su Annulla. Come indicato nella sezione precedente, il pulsante Continua nella `CompleteWizardStep`dell'interfaccia causa un postback, ma lascia il visitatore nella stessa pagina. Per inviare il visitatore a un'altra pagina dopo aver fatto clic sul pulsante Continua, è sufficiente specificare l'URL nel [ `ContinueDestinationPageUrl` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Aggiorniamo il `RegisterUser` controllo CreateUserWizard per visualizzare un pulsante Annulla e inviare il visitatore per `Default.aspx` quando vengono selezionati i pulsanti Annulla o continua. A tale scopo, impostare il `DisplayCancelButton` proprietà su True e sia il `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` proprietà "~ / default. aspx". Figura 14 mostra CreateUserWizard aggiornati quando viene visualizzato tramite un browser.


[![Il CreateUserWizardStep include un pulsante Annulla](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Figura 14**: il `CreateUserWizardStep` include un pulsante Annulla ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image42.png))


Quando un visitatore immette un nome utente, password, indirizzo di posta elettronica e domanda di sicurezza e risposte e fa clic su "Create User", viene creato un nuovo account utente e il visitatore è effettuato l'accesso come utente che appena creato. Supponendo che la persona visitando la pagina viene creato un nuovo account per se stessi, il problema è dovuto al comportamento desiderato. Tuttavia, è possibile consentire agli amministratori di aggiungere nuovi account utente. In questo modo, verrebbe creato l'account utente, ma l'amministratore potrebbe rimanere connesso come amministratore (e non come l'account appena creato). Questo comportamento può essere modificato tramite il valore booleano [ `LoginCreatedUser` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Gli account utente nel framework di appartenenza contengono un flag approvati. gli utenti che non sono approvati in grado di accedere al sito. Per impostazione predefinita, un account appena creato viene contrassegnato come approvato, consentendo all'utente di accedere al sito immediatamente. È possibile, tuttavia, affinché i nuovi account utente contrassegnato come non approvato. Si supponga di un amministratore di approvare manualmente i nuovi utenti per poter accedere. o forse si desidera verificare che l'indirizzo di posta elettronica immesso al momento dell'iscrizione sia valido prima di consentire all'utente di eseguire l'accesso. Qualsiasi elemento può essere il caso, è possibile avere account utente appena creato contrassegnato come non approvate tramite l'impostazione del controllo CreateUserWizard [ `DisableCreatedUser` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) su True (impostazione predefinita è False).

Altre proprietà correlate al comportamento di nota includono `AutoGeneratePassword` e `MailDefinition`. Se il [ `AutoGeneratePassword` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) è impostata su True, il `CreateUserWizardStep` non vengono visualizzate le caselle di testo "Confermare Password" e "Password"; al contrario, la password dell'utente appena creato viene generata automaticamente utilizzando il `Membership` della classe [ `GeneratePassword` metodo](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Il `GeneratePassword` metodo costruisce una password di lunghezza specificata e con un numero sufficiente di caratteri non alfanumerici per soddisfare i requisiti di complessità delle password configurata.

Il [ `MailDefinition` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) è utile se si desidera inviare un messaggio di posta elettronica all'indirizzo di posta elettronica specificato durante il processo di creazione di account. Il `MailDefinition` proprietà include una serie di sottoproprietà per la definizione delle informazioni sul messaggio di posta elettronica costruito. Queste sottoproprietà includono opzioni come `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, e `BodyFileName`. Il [ `BodyFileName` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) punta a un file di testo o HTML che contiene il corpo del messaggio di posta elettronica. Il corpo supporta due segnaposto predefiniti: `<%UserName%>` e `<%Password%>`. Questi segnaposto, se presente nel `BodyFileName` file, verrà sostituito con nome e la password dell'utente appena creato.

> [!NOTE]
> Il `CreateUserWizard` del controllo `MailDefinition` proprietà specifica solo i dettagli sul messaggio di posta elettronica inviato quando viene creato un nuovo account. Non include tutti i dettagli sul modo in cui viene effettivamente inviato il messaggio di posta elettronica (vale a dire, se viene utilizzata una directory di server o di destinazione della posta elettronica SMTP, eventuali informazioni di autenticazione e così via). Questi dettagli di basso livello devono essere definiti nel `<system.net>` sezione `Web.config`. Per altre informazioni su queste impostazioni di configurazione e sull'invio di posta elettronica da ASP.NET 2.0 in generale, vedere la [domande frequenti in SystemNetMail.com](http://www.systemnetmail.com/) e il mio articolo [l'invio di posta elettronica in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estensione del comportamento del CreateUserWizard utilizzando i gestori eventi

Il controllo CreateUserWizard genera un numero di eventi durante il flusso di lavoro. Ad esempio, dopo un visitatore immette il nome utente, password e altre informazioni e fa clic sul pulsante "Create User", il controllo CreateUserWizard genera relativi [ `CreatingUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si è verificato un problema durante il processo di creazione, la [ `CreateUserError` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) è attivato; tuttavia, se l'utente è stato creato, il [ `CreatedUser` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) viene generato. Sono presenti altri eventi di controllo CreateUserWizard che venga generati, ma sono quelle più pertinente tre.

In determinati scenari si potrebbe attingere CreateUserWizard flusso di lavoro, possiamo mediante la creazione di un gestore eventi per l'evento appropriato. Per illustrare questo concetto, è possibile migliorare il `RegisterUser` controllo CreateUserWizard per includere una convalida personalizzata del nome utente e password. In particolare, è possibile migliorare nostro CreateUserWizard in modo che i nomi utente non possono contenere spazi iniziali o finali e il nome utente non può trovarsi in un punto qualsiasi nella password. In breve, si vuole impedire a utenti creare un nome utente, ad esempio "Scott" o una combinazione di nome utente/password, ad esempio "Scott" e "Scott.1234".

A tale scopo si creerà un gestore eventi per il `CreatingUser` evento per eseguire i controlli di convalida aggiuntiva. Se i dati forniti non sono validi è necessario annullare il processo di creazione. È anche necessario aggiungere un controllo etichetta Web alla pagina per visualizzare un messaggio che spiega che il nome utente o password non è valida. Iniziare aggiungendo un controllo etichetta sotto il controllo CreateUserWizard, l'impostazione relativa `ID` proprietà `InvalidUserNameOrPasswordMessage` e il relativo `ForeColor` proprietà `Red`. Cancellare il contenuto relativo `Text` proprietà e set relativo `EnableViewState` e `Visible` proprietà su False.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per il controllo CreateUserWizard `CreatingUser` evento. Per creare un gestore eventi, selezionare il controllo nella finestra di progettazione e quindi passare alla finestra Proprietà. Da qui, fare clic sull'icona a forma di fulmine e quindi fare doppio clic sull'evento appropriato per creare il gestore dell'evento.

Aggiungere il codice seguente al gestore eventi `CreatingUser`:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Si noti che sono disponibili tramite il nome utente e la password immessi nel controllo CreateUserWizard relativi [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [ `Password` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), rispettivamente. Utilizziamo queste proprietà nel gestore dell'evento precedente per determinare se il nome utente fornito contiene spazi iniziali o finali e indica se il nome utente è stato trovato all'interno della password. Se una di queste condizioni è soddisfatta, viene visualizzato un messaggio di errore nel `InvalidUserNameOrPasswordMessage` Label e il gestore di eventi `e.Cancel` è impostata su `true`. Se `e.Cancel` è impostata su `true`, CreateUserWizard consente di bloccare il flusso di lavoro, in modo efficace annullare il processo di creazione di account utente.

Figura 15 viene mostrata una schermata di `CreatingUserAccounts.aspx` quando l'utente immette un nome utente con gli spazi iniziali.


[![Non sono consentiti nomi utente con leader o spazi finali](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Figura 15**: non sono consentiti nomi utente con leader o spazi finali ([fare clic per visualizzare l'immagine con dimensioni normali](creating-user-accounts-cs/_static/image45.png))


> [!NOTE]
> Vedremo un esempio dell'uso del controllo CreateUserWizard `CreatedUser` eventi nel *<a id="_msoanchor_11"> </a> [archiviare informazioni utente aggiuntive](storing-additional-user-information-cs.md)* esercitazione.


## <a name="summary"></a>Riepilogo

Il `Membership` della classe `CreateUser` metodo crea un nuovo account utente nel framework di appartenenza. Esegue l'operazione mediante la delega la chiamata al provider di appartenenze configurato. Nel caso del `SqlMembershipProvider`, il `CreateUser` metodo aggiunge un record per il `aspnet_Users` e `aspnet_Membership` tabelle di database.

Sebbene sia possibile creare nuovi account utente a livello di programmazione (come mostrato nel passaggio 5), un approccio più veloce e più semplice consiste nell'utilizzare il controllo CreateUserWizard. Questo controllo esegue il rendering di un'interfaccia utente costituito da più passaggi per la raccolta di informazioni utente e la creazione di un nuovo utente nel framework di appartenenza. Sotto le quinte, il controllo Usa lo stesso `Membership.CreateUser` metodo come esaminato nel passaggio 5, ma il controllo crea l'interfaccia utente, i controlli di convalida e risponde agli errori di creazione di account utente senza dover scrivere codice.

A questo punto è disponibile la funzionalità in grado di creare nuovi account utente. Tuttavia, la pagina di accesso comunque è la convalida rispetto a queste credenziali a livello di codice specificato nella seconda esercitazione. Nel <a id="_msoanchor_12"> </a> [esercitazione successiva](validating-user-credentials-against-the-membership-user-store-cs.md) verrà aggiornato `Login.aspx` per convalidare l'utente ha specificato le credenziali per il framework di appartenenza.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [`CreateUser` Documentazione tecnica](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Cenni preliminari sul controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Creazione di un Provider di mappa di File basate sul sistema del sito](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Creazione di un'interfaccia utente dettagliata con il controllo ASP.NET 2.0 Wizard](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Analisi di ASP.NET 2.0 di esplorazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Pagine master e spostamento nel sito](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Il Provider della mappa del sito SQL che aspettavi](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla.com, ha lavorato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola  *[Sams Teach Yourself ASP.NET 2.0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. È possibile contattarlo Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o il suo blog all'indirizzo [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali...

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](creating-the-membership-schema-in-sql-server-cs.md)
> [Successivo](validating-user-credentials-against-the-membership-user-store-cs.md)
