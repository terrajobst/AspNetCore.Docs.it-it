---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Creazione di account utente (VB) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione verrà illustrato utilizzando il framework di appartenenza (tramite SqlMembershipProvider) per creare nuovi account utente. Verrà spiegato come creare nuovi noi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 43e8423cd69a9cc345f2d8ef7d0a022252235462
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-user-accounts-vb"></a>Creazione di account utente (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) o [Scarica il PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> In questa esercitazione verrà illustrato utilizzando il framework di appartenenza (tramite SqlMembershipProvider) per creare nuovi account utente. Verrà spiegato come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato della rete.


## <a name="introduction"></a>Introduzione

Nel <a id="_msoanchor_1"> </a> [esercitazione precedente](creating-the-membership-schema-in-sql-server-vb.md) lo schema di servizi applicazione sono stati installati in un database, aggiunta di tabelle, viste e stored procedure necessite per il `SqlMembershipProvider` e `SqlRoleProvider`. Tale creata l'infrastruttura che è necessario per il resto delle esercitazioni di questa serie. In questa esercitazione verrà illustrato utilizzando il framework di appartenenza (tramite il `SqlMembershipProvider`) per creare nuovi account utente. Verrà spiegato come creare nuovi utenti a livello di codice e tramite ASP. Controllo CreateUserWizard incorporato della rete.

Oltre a informazioni su come creare nuovi account utente, è necessario anche aggiornare il sito Web demo creato nella prima il  *<a id="_msoanchor_2"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)*  esercitazione e quindi migliorato il  *<a id="_msoanchor_3"> </a> [configurazione dell'autenticazione form e argomenti avanzati](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  esercitazione. L'applicazione web demo dispone di una pagina di accesso che convalida le credenziali degli utenti rispetto coppie di nome utente/password hardcoded. Inoltre, `Global.asax` include codice che crea personalizzato `IPrincipal` e `IIdentity` gli oggetti per gli utenti autenticati. Microsoft aggiornerà la pagina di accesso per convalidare le credenziali degli utenti il framework di appartenenza e rimuovere la logica personalizzata principal e identity.

Iniziamo!

## <a name="the-forms-authentication-and-membership-checklist"></a>L'autenticazione basata su form e un elenco di controllo di appartenenza

Prima di iniziare a utilizzare il framework di appartenenza, è opportuno esaminare i passaggi importanti che abbiamo adottato per raggiungere questo punto. Quando si utilizza il framework di appartenenza con il `SqlMembershipProvider` in uno scenario di autenticazione basata su form, devono essere eseguite prima dell'implementazione delle funzionalità di appartenenza nell'applicazione web i passaggi seguenti:

1. **Abilitare l'autenticazione basata su form.** Come accennato  *<a id="_msoanchor_4"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)*, autenticazione basata su form è abilitato per la modifica `Web.config` e impostando il `<authentication>` elemento `mode` attributo `Forms`. Con autenticazione basata su form abilitato, viene esaminato ogni richiesta in ingresso per un *form il ticket di autenticazione*, che, se presente, identifica il richiedente.
2. **Aggiungere lo schema di servizi di applicazione al database appropriato.** Quando si utilizza il `SqlMembershipProvider` è necessario installare lo schema di servizi di applicazione a un database. In genere questo schema viene aggiunto allo stesso database che contiene il modello di dati dell'applicazione. Il  *<a id="_msoanchor_5"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  esercitazione descritto l'utilizzo di `aspnet_regsql.exe` strumento per eseguire questa operazione.
3. **Personalizzare le impostazioni dell'applicazione Web per fare riferimento al database dal passaggio 2.** Il *creazione dello Schema di appartenenza in SQL Server* esercitazione mostrano due modi per configurare l'applicazione web in modo che il `SqlMembershipProvider` utilizzerà il database selezionato nel passaggio 2: modificando il `LocalSqlServer` nome di stringa di connessione. o aggiungendo un nuovo provider registrati all'elenco dei provider di appartenenze framework e personalizzazione tale nuovo provider per utilizzare il database dal passaggio 2.

Quando la compilazione di un'applicazione web che utilizza il `SqlMembershipProvider` e l'autenticazione basata su form, sarà necessario eseguire tre passaggi seguenti prima di utilizzare la `Membership` classe o i controlli Web di accesso di ASP.NET. Poiché questi passaggi è già stata eseguita nelle esercitazioni precedenti, si è pronti per iniziare a usare il framework di appartenenza!

## <a name="step-1-adding-new-aspnet-pages"></a>Passaggio 1: Aggiunta di nuove pagine ASP.NET

In questa esercitazione e i successivi tre è verrà esaminando le funzioni di appartenenza e le funzionalità. È necessario una serie di pagine ASP.NET per implementare gli argomenti esaminati in queste esercitazioni. Creare le pagine e quindi un file di mappa del sito `(Web.sitemap)`.

Iniziare creando una nuova cartella nel progetto denominato `Membership`. Successivamente, aggiungere cinque nuove pagine ASP.NET per il `Membership` cartella, il collegamento di ogni pagina con il `Site.master` pagina master. Le pagine dei nomi:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Esplora soluzioni del progetto a questo punto dovrebbe essere simile alla schermata illustrata nella figura 1.


[![Sono stati aggiunti cinque nuove pagine nella cartella di appartenenza](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Figura 1**: cinque nuove pagine sono stati aggiunti i `Membership` cartella ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image3.png))


Ogni pagina, dovrebbe rispondere a questo punto, includere i due controlli contenuti, uno per ognuno dei ContentPlaceHolder della pagina master: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Tenere presente che il `LoginContent` markup predefinito del ContentPlaceHolder viene visualizzato un collegamento per connettersi o disconnettersi del sito, a seconda che l'utente è autenticato. La presenza del `Content2` controllo contenuto, tuttavia, esegue l'override di markup predefinito della pagina master. Come accennato  *<a id="_msoanchor_6"> </a> [una panoramica dell'autenticazione basata su form](../introduction/an-overview-of-forms-authentication-vb.md)*  esercitazione, ciò è utile nelle pagine in cui non si desidera visualizzare le opzioni relative all'accesso nella colonna sinistra.

Per questi cinque pagine, tuttavia, si desidera mostrare il markup della pagina master predefinita per il `LoginContent` ContentPlaceHolder. Pertanto, rimuovere il markup dichiarativo per la `Content2` controllo contenuto. Al termine dell'operazione, ognuno dei markup della pagina cinque deve contenere solo un controllo contenuto.

## <a name="step-2-creating-the-site-map"></a>Passaggio 2: Creazione della mappa del sito

Tutti i siti Web di più semplice necessario implementare una qualche forma di un'interfaccia utente per la navigazione. L'interfaccia utente di spostamento può essere un semplice elenco di collegamenti alle diverse sezioni del sito. In alternativa, questi collegamenti possono essere disposte in menu o visualizzazioni dell'albero. Gli sviluppatori di pagina, la creazione dell'interfaccia utente per la navigazione è solo la metà della storia. È necessario anche metodi per definire la struttura logica del sito in modo gestibile e aggiornabile. Che vengono aggiunte nuove pagine o si rimuovono le pagine esistenti, si vuole essere in grado di aggiornare una singola origine - mappa del sito - e riflettono le modifiche apportate nell'interfaccia utente per la navigazione del sito.

Queste due attività - definire la mappa del sito e implementazione di un'interfaccia utente per la navigazione in base alla mappa del sito - sono molto semplici grazie ai framework mappa del sito e controlli di navigazione Web aggiunto in ASP.NET versione 2.0. Il framework di mappa del sito consente allo sviluppatore di definire una mappa del sito e quindi per accedervi tramite un'API a livello di codice (il [ `SiteMap` classe](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)). Predefinito controlli Web di navigazione, includere un [controllo Menu](https://msdn.microsoft.com/en-us/library/bz09dy46.aspx), [controllo TreeView](https://msdn.microsoft.com/en-us/library/3eafky27.aspx)e [controllo SiteMapPath](https://msdn.microsoft.com/en-us/library/3eafky27.aspx).

Come i framework di appartenenza e ruoli, il framework di mappa del sito è incorporato nella parte superiore di [modello provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Il processo della classe del provider di mappa del sito consiste nel generare la struttura in memoria utilizzata dalla `SiteMap` classe da un archivio dati persistenti, ad esempio un file XML o una tabella di database. .NET Framework viene fornito con un provider di mappa del sito predefinito che legge i dati della mappa del sito da un file XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx)), e si tratta del provider che verrà usato in questa esercitazione. Per alcune implementazioni del provider della mappa del sito alternativi, vedere la sezione ulteriori letture alla fine di questa esercitazione.

Il provider di mappa del sito predefinito prevede che un file XML formattato correttamente denominato `Web.sitemap` esista la directory radice. Poiché si sta usando il provider predefinito, è necessario aggiungere tale file e definire la struttura della mappa del sito nel formato XML appropriato. Per aggiungere il file, fare clic sul nome del progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento. Nella finestra di dialogo, scegliere di aggiungere un file di tipo mappa del sito denominato `Web.sitemap`.


[![Aggiungere un File sitemap directory radice del progetto](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Figura 2**: aggiungere un File denominato `Web.sitemap` alla Directory radice del progetto ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image6.png))


Il file di mappa del sito XML definisce la struttura del sito Web come una gerarchia. Questa relazione gerarchica viene modellata nel file XML tramite la cronologia del `<siteMapNode>` elementi. Il `Web.sitemap` deve iniziare con un `<siteMap>` nodo padre che dispone di esattamente un `<siteMapNode>` figlio. Questo livello superiore `<siteMapNode>` elemento rappresenta la radice della gerarchia e potrebbe essere un numero arbitrario di nodi discendenti. Ogni `<siteMapNode>` elemento deve includere un `title` degli attributi e può includere facoltativamente `url` e `description` gli attributi, tra gli altri; ogni non vuoto `url` attributo deve essere univoco.

Immettere il seguente codice XML nel `Web.sitemap` file:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Il markup di mappa del sito precedente definisce la gerarchia illustrata nella figura 3.


[![Mappa del sito rappresenta una struttura gerarchica](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Figura 3**: mappa del sito rappresenta una struttura gerarchica per la navigazione ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Passaggio 3: Aggiornare la pagina Master per includere un'interfaccia utente per la navigazione

ASP.NET include un numero di navigazione di controlli Web per la progettazione di un'interfaccia utente. Tra i Menu, TreeView e i controlli SiteMapPath. I controlli Menu e TreeView eseguire il rendering della struttura della mappa del sito in un menu o una struttura ad albero, rispettivamente, mentre il controllo SiteMapPath Visualizza una barra di navigazione che viene visualizzato il nodo corrente viene visitato nonché i relativi predecessori. I dati della mappa del sito può essere associato ad altri dati di controlli Web utilizzando SiteMapDataSource ed è possibile accedervi a livello di codice tramite la `SiteMap` classe.

Poiché una trattazione completa di framework mappa del sito e i controlli di spostamento non rientra nell'ambito di questa serie di esercitazioni, invece di dedicare tempo crea la propria interfaccia utente per la navigazione consente invece prestito quella usata nel mio  *[ Utilizzo dei dati in ASP.NET 2.0](../../data-access/index.md)*  serie di esercitazioni, che utilizza un controllo Repeater per visualizzare un elenco completo di due puntato di collegamenti di navigazione, come illustrato nella figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Aggiunta di un elenco di due livelli di collegamenti nella colonna sinistra

Per creare questa interfaccia, aggiungere il seguente markup dichiarativo per la `Site.master` colonna sinistro della pagina master in cui il testo TODO: Menu verrà fare clic qui... attualmente si trova.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Il markup precedente associa un controllo Repeater denominato `menu` a un controllo SiteMapDataSource, che restituisce la gerarchia della mappa del sito definita in `Web.sitemap`. Poiché il controllo SiteMapDataSource [ `ShowStartingNode` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) è impostata su False, all'avvio la restituzione di gerarchia della mappa del sito a partire dai discendenti del nodo principale. Ripetitore ognuno di questi nodi (attualmente solo Membership) vengono visualizzati un `<li>` elemento. Ripetitore interna, un altro Visualizza gli elementi figlio del nodo corrente in un elenco non ordinato annidato.

La figura 4 Mostra output di rendering del markup precedente con la struttura della mappa del sito che è stati creati nel passaggio 2. Ripetitore esegue il rendering di markup di vaniglia elenco non ordinato. le regole di foglio di stile CSS definita `Styles.css` sono responsabili per il layout del piacevole. Per una descrizione più dettagliata del funzionamento di codice precedente, vedere il [pagine Master e esplorazione del sito](https://asp.net/learn/data-access/tutorial-03-vb.aspx) esercitazione.


[![L'interfaccia utente per la navigazione è sottoposto a rendering utilizzo annidato non ordinata di elenchi](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Figura 4**: l'interfaccia utente per la navigazione è sottoposto a rendering utilizzo annidato non ordinata di elenchi ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Aggiunta di struttura di spostamento

Oltre all'elenco di collegamenti nella colonna sinistra, verrà inoltre disporre ogni visualizzazione pagina un [navigazione](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Una barra di navigazione è un elemento dell'interfaccia utente per la navigazione che è possibile visualizzare rapidamente gli utenti della posizione corrente all'interno della gerarchia del sito. Il controllo SiteMapPath utilizza il framework di mappa del sito per determinare il percorso della pagina corrente nella mappa del sito e quindi visualizza una barra di navigazione in base a queste informazioni.

In particolare, aggiungere un `<span>` elemento all'intestazione della pagina master `<div>` elemento e impostare il nuovo `<span>` dell'elemento `class` attributo barra di navigazione. (La `Styles.css` classe contiene una regola per una classe di navigazione.) Successivamente, aggiungere un SiteMapPath a questo nuovo `<span>` elemento.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Figura 5 mostra l'output del controllo SiteMapPath durante la visita `~/Membership/CreatingUserAccounts.aspx`.


[![La barra di navigazione viene visualizzata la pagina corrente e mappare i predecessori del sito](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Figura 5**: della barra di navigazione consente di visualizzare la pagina corrente e i relativi predecessori nella mappa del sito ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Passaggio 4: Rimuovere l'entità personalizzata e la logica di identità

Nel  *<a id="_msoanchor_7"> </a> [configurazione dell'autenticazione form e argomenti avanzati](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  esercitazione è stato illustrato come associare oggetti principal e identity personalizzati per l'utente autenticato. Questa operazione è eseguita mediante la creazione di un gestore eventi in `Global.asax` per l'applicazione `PostAuthenticateRequest` evento, che viene generato dopo il `FormsAuthenticationModule` ha autenticato l'utente. In questo gestore eventi, pertanto abbiamo sostituito il `GenericPrincipal` e `FormsIdentity` oggetti aggiunti dal `FormsAuthenticationModule` con il `CustomPrincipal` e `CustomIdentity` gli oggetti creati in tale esercitazione.

Mentre gli oggetti principal e identity personalizzati sono utili in alcuni scenari, nella maggior parte dei casi il `GenericPrincipal` e `FormsIdentity` gli oggetti sono sufficienti. Di conseguenza, ritengo che sarebbe utile per ripristinare il comportamento predefinito. Apportare questa modifica rimuovendo o impostare come commento il `PostAuthenticateRequest` gestore dell'evento o eliminando il `Global.asax` file completamente.

> [!NOTE]
> Dopo che è stata impostata come commento o rimosso il codice in `Global.asax`, sarà necessario impostare come commento il codice in `Default.aspx's` classe code-behind che esegue il cast di `User.Identity` proprietà per un `CustomIdentity` istanza.


## <a name="step-5-programmatically-creating-a-new-user"></a>Passaggio 5: A livello di programmazione creando un nuovo utente

Per creare un nuovo account utente tramite l'utilizzo di framework di appartenenza di `Membership` della classe [ `CreateUser` metodo](https://msdn.microsoft.com/En-US/library/system.web.security.membership.createuser.aspx). Questo metodo dispone di parametri di input per il nome utente, password e altri campi correlati all'utente. Nella chiamata, delega la creazione del nuovo account utente al provider di appartenenze configurati e quindi restituisce un [ `MembershipUser` oggetto](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx) che rappresenta l'account utente appena creato.

Il `CreateUser` metodo dispone di quattro overload, ciascuna delle quali accetta un numero diverso di parametri di input:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/En-US/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/En-US/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/En-US/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/En-US/library/ms152012.aspx)

Questi quattro overload diversi per la quantità di informazioni raccolte. Il primo overload, ad esempio, richiede solo il nome utente e password per il nuovo account utente, mentre la seconda richiede anche l'indirizzo di posta elettronica dell'utente.

Questi overload esistono perché le informazioni necessarie per creare un nuovo account utente dipendono dalle impostazioni di configurazione del provider di appartenenze. Nel  *<a id="_msoanchor_8"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  esercitazione sono state esaminate specifica impostazioni di configurazione di provider di appartenenze in `Web.config`. Tabella 2 è incluso un elenco completo delle impostazioni di configurazione.

Una tale appartenenza provider impostazione di configurazione che influisce sulle quali `CreateUser` overload possono essere utilizzati è il `requiresQuestionAndAnswer` impostazione. Se `requiresQuestionAndAnswer` è impostato su `true` (impostazione predefinita), quindi quando si crea un nuovo account utente è necessario specificare una domanda di sicurezza e una risposta. Queste informazioni vengono utilizzate in un secondo momento se l'utente deve reimpostare o modificare la password. In particolare, in quel momento vengono visualizzati alla domanda di sicurezza e utenti devono immettere la risposta corretta per reimpostare o modificare la password. Di conseguenza, se il `requiresQuestionAndAnswer` è impostato su `true` quindi chiamando uno dei primi due `CreateUser` overload generano un'eccezione perché mancante la domanda di sicurezza e la risposta. Poiché l'applicazione è attualmente configurato per richiedere una domanda di sicurezza e una risposta, è necessario utilizzare uno degli overload di due quest'ultimo durante la creazione dell'utente a livello di codice.

Per illustrare l'utilizzo di `CreateUser` (metodo), creare un'interfaccia utente in cui è richiesta per il proprio nome, la password, messaggio di posta elettronica e una risposta alla domanda di sicurezza predefiniti. Aprire il `CreatingUserAccounts.aspx` nella pagina di `Membership` cartella e aggiungere i seguenti controlli Web per il controllo contenuto:

- Una casella di testo denominato`Username`
- Una casella di testo denominato `Password`, il cui `TextMode` è impostata su`Password`
- Una casella di testo denominato`Email`
- Un'etichetta denominata `SecurityQuestion` con il relativo `Text` cancellare la proprietà
- Una casella di testo denominato`SecurityAnswer`
- Un pulsante denominato `CreateAccountButton` cui `Text` proprietà è impostata per creare l'Account utente
- Un controllo etichetta denominato `CreateAccountResults` con il relativo `Text` cancellare la proprietà

A questo punto la schermata dovrebbe essere simile alla schermata illustrata nella figura 6.


[![Aggiungere i controlli Web vari CreatingUserAccounts.aspx](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Figura 6**: aggiungere i vari controlli Web per il `CreatingUserAccounts.aspx Page` ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image18.png))


Il `SecurityQuestion` etichetta e `SecurityAnswer` casella di testo cui si intende visualizzare una domanda di sicurezza predefiniti e raccogliere la risposta dell'utente. Si noti che la domanda di sicurezza e la risposta sono archiviati su base utente dall'utente, pertanto è possibile consentire a ogni utente definire i propri domanda di sicurezza. Tuttavia, per questo esempio si è deciso di utilizzare una domanda di sicurezza universale, vale a dire: che cos'è il colore preferito?

Per implementare questa domanda di sicurezza predefiniti, aggiungere una costante alla classe code-behind della pagina denominata `passwordQuestion`, assegnarlo alla domanda di sicurezza. Quindi, nel `Page_Load` gestore dell'evento, assegnare questo costante per il `SecurityQuestion` dell'etichetta `Text` proprietà:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Successivamente, creare un gestore eventi per il `CreateAccountButton'` s `Click` eventi e aggiungere il codice seguente:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Il `Click` gestore dell'evento inizia con la definizione di una variabile denominata `createStatus` di tipo [ `MembershipCreateStatus` ](https://msdn.microsoft.com/En-US/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus`è un'enumerazione che indica lo stato di `CreateUser` operazione. Ad esempio, se l'account utente viene creato correttamente, il valore risultante `MembershipCreateStatus` istanza verrà impostata su un valore di `Success;` l'operazione non riesce perché esiste già un utente con lo stesso nome utente, d'altra parte, verrà impostato su un valore di `DuplicateUserName`. Nel `CreateUser` overload viene usato, è necessario passare un `MembershipCreateStatus` istanza al metodo. Questo parametro è impostato sul valore appropriato all'interno di `CreateUser` metodo ed è possibile esaminare il relativo valore dopo la chiamata al metodo per determinare se l'account utente è stato creato correttamente.

Dopo la chiamata `CreateUser`, passando `createStatus`, `Select Case` istruzione viene utilizzata per restituire un messaggio appropriato a seconda del valore assegnato a `createStatus`. Le figure 7 mostra l'output quando un nuovo utente è stato creato correttamente. Cifre 8 e 9 mostra l'output quando non viene creato l'account utente. Nella figura 8, il visitatore immesso una password di cinque lettere, che non soddisfa i requisiti di complessità delle password pronunciati nelle impostazioni di configurazione del provider di appartenenze. Nella figura 9, il visitatore sta tentando di creare un account utente con un nome utente esistente (quello creato nella figura 7).


[![Un nuovo Account utente è stato creato](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Figura 7**: un nuovo Account utente è stata creata ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image21.png))


[![L'Account utente non viene creato perché la Password specificata è troppo vulnerabili](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Figura 8**: l'Account dell'utente non viene creato perché la Password specificata è troppo vulnerabili ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image24.png))


[![L'Account utente non è che creato perché il nome utente è già in uso](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Figura 9**: l'Account dell'utente non è creato perché il nome utente è già in uso ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> Per comprendere come determinare l'esito positivo o negativo quando si utilizza uno dei primi due `CreateUser` overload del metodo, nessuno dei quali presenta un parametro di tipo `MembershipCreateStatus`. Questi primi due overload generano un [ `MembershipCreateUserException` eccezione](https://msdn.microsoft.com/en-us/library/system.web.security.membershipcreateuserexception.aspx) in caso di errore, che include un [ `StatusCode` proprietà](https://msdn.microsoft.com/en-us/library/system.web.security.membershipcreateuserexception.statuscode.aspx) di tipo `MembershipCreateStatus`.


Dopo avere creato alcuni account utente, verificare che siano stati creati gli account dall'elenco del contenuto del `aspnet_Users` e `aspnet_Membership` nelle tabelle di `SecurityTutorials.mdf` database. Come mostrato nella figura 10, ho aggiunto due utenti tramite il `CreatingUserAccounts.aspx` pagina: Tito e Bruce.


[![Esistono due utenti nell'archivio dell'utente di appartenenza: Tito e Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Figura 10**: ci sono due utenti nell'archivio dell'utente di appartenenza: Tito e Bruce ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image30.png))


Mentre l'archivio utente l'appartenenza include ora Bruce e di Tito informazioni sull'account, abbiamo ancora implementare funzionalità che consente il Bruce o Tito accedere al sito. Attualmente, `Login.aspx` convalida le credenziali dell'utente rispetto a un set a livello di codice di coppie di nome utente/password - caso *non* convalidare le credenziali fornite per il framework di appartenenza. Per ora visualizzare i nuovi account utente nel `aspnet_Users` e `aspnet_Membership` tabelle deve essere sufficiente. Nella prossima esercitazione,  *<a id="_msoanchor_9"> </a> [convalida utente credenziali rispetto dell'appartenenza utente archivio](validating-user-credentials-against-the-membership-user-store-vb.md)*, Microsoft aggiornerà la pagina di accesso per la convalida di archivio di appartenenza.

> [!NOTE]
> Se non viene visualizzato agli utenti il `SecurityTutorials.mdf` database, è possibile che l'applicazione web utilizza il provider di appartenenze predefinito, `AspNetSqlMembershipProvider`, che utilizza il `ASPNETDB.mdf` database come archivio utente. Per determinare se questo è il problema, fare clic sul pulsante Aggiorna in Esplora soluzioni. Se un database denominato `ASPNETDB.mdf` è stato aggiunto il `App_Data` cartella, questo è il problema. Tornare al passaggio 4 del  *<a id="_msoanchor_10"> </a> [creazione dello Schema di appartenenza in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  esercitazione per istruzioni su come configurare correttamente il provider di appartenenze.


Nella maggior parte dei scenari utente account, il visitatore viene presentato con un tipo di interfaccia di immettere il proprio nome utente, password, messaggio di posta elettronica e altre informazioni essenziali, a quel punto viene creato un nuovo account. In questo passaggio esaminato creazione manuale di tale interfaccia e quindi illustrato come utilizzare il `Membership.CreateUser` metodo a livello di codice aggiungere il nuovo account utente basato sugli input dell'utente. Il codice, tuttavia, appena creato il nuovo account utente. Non eseguito le azioni, ad esempio accesso l'utente al sito con l'account utente appena creato, o l'invio di un messaggio di posta elettronica di conferma all'utente di completamento. Questi passaggi aggiuntivi richiede codice aggiuntivo del pulsante `Click` gestore dell'evento.

ASP.NET viene fornito con il controllo CreateUserWizard, che consente di gestire il processo di creazione account utente, da eseguire il rendering di un'interfaccia utente per la creazione di nuovi account utente, per la creazione dell'account in framework appartenenza e l'esecuzione di post-account attività di creazione, ad esempio l'invio di un messaggio di posta elettronica di conferma e l'accesso l'utente appena creato il sito. Usa il controllo CreateUserWizard è semplice come trascinare il controllo CreateUserWizard dalla casella degli strumenti in una pagina e quindi impostando alcune proprietà. Nella maggior parte dei casi, non sarà necessario scrivere una singola riga di codice. Si esaminerà questo controllo elegante in dettaglio nel passaggio 6.

Se i nuovi account utente vengono create solo tramite una tipica pagina web di creare un Account, è improbabile che sarà mai necessario scrivere codice che usa il `CreateUser` (metodo), come il controllo CreateUserWizard verrà probabilmente alle proprie esigenze. Tuttavia, il `CreateUser` metodo è utile negli scenari in cui è necessario un'esperienza utente di creare un Account altamente personalizzata o quando è necessario creare nuovi account utente tramite un'interfaccia alternativo. Ad esempio, potrebbe essere una pagina che consente all'utente di caricare un file XML che contiene le informazioni utente da un'altra applicazione. La pagina potrebbe essere analizza il contenuto del XML caricati file e crea un nuovo account per ogni utente rappresentato nel codice XML tramite la chiamata di `CreateUser` metodo.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Passaggio 6: Creazione di un nuovo utente con il controllo CreateUserWizard

ASP.NET viene fornito con un numero di controlli Web di accesso. Questi controlli aiutano a molti scenari utente comuni correlate all'accesso e account. Il [controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) è un controllo di questo tipo che è progettato per presentare un'interfaccia utente per l'aggiunta di un nuovo account utente per il framework di appartenenza.

Come molti altri controlli Web relative all'accesso, è possibile utilizzare CreateUserWizard senza scrivere una singola riga di codice. In modo intuitivo fornisce un'interfaccia utente basata su provider di appartenenze le impostazioni di configurazione e internamente chiama il `Membership` della classe `CreateUser` dopo che l'utente immette le informazioni necessarie e fa clic sul pulsante Crea utente. Il controllo CreateUserWizard è estremamente personalizzabile. Esistono una serie di eventi che attivano durante le varie fasi del processo di creazione di account. È possibile creare i gestori eventi, se necessario, inserire la logica personalizzata nel flusso di lavoro di creazione di account. Inoltre, l'aspetto del CreateUserWizard è molto flessibile. Sono disponibili numerose proprietà che definiscono l'aspetto dell'interfaccia predefinita. Se necessario, il controllo può essere convertito in un modello o operazioni di registrazione utente aggiuntivi possono essere aggiunti.

Per iniziare è illustrato l'utilizzo di interfaccia predefinito e il comportamento del controllo CreateUserWizard. Verrà quindi illustrato come personalizzare l'aspetto tramite proprietà ed eventi del controllo.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Esaminare l'interfaccia predefinita e il comportamento di CreateUserWizard

Restituito per il `CreatingUserAccounts.aspx` nella pagina di `Membership` , passare alla modalità progettazione o di divisione e quindi aggiungere un controllo CreateUserWizard nella parte superiore della pagina. Il controllo CreateUserWizard viene archiviato in una sezione di controlli di accesso della casella degli strumenti. Dopo aver aggiunto il controllo, impostare il relativo `ID` proprietà `RegisterUser`. Come nell'immagine riportata nella figura 11 Mostra, CreateUserWizard esegue il rendering di un'interfaccia con caselle di testo per il nuovo utente nome utente, password, indirizzo di posta elettronica e domanda di sicurezza e risposte.


[![Interfaccia utente di creare l'esegue il rendering del controllo CreateUserWizard generico](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Figura 11**: il controllo CreateUserWizard esegue il rendering di un'interfaccia utente generica creare ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image33.png))


È opportuno per confrontare l'interfaccia utente predefinito generato dal controllo CreateUserWizard con l'interfaccia creata nel passaggio 5. Per iniziare, il controllo CreateUserWizard consente il visitatore specificare la domanda di sicurezza e di risposta, mentre l'interfaccia creati manualmente utilizzata una domanda di sicurezza predefiniti. Interfaccia del controllo CreateUserWizard include anche i controlli di convalida, mentre è stato ancora necessario implementare la convalida nei campi modulo nostri dell'interfaccia. E l'interfaccia del controllo CreateUserWizard include una casella di testo Conferma Password (insieme CompareValidator per assicurarsi che il testo immesso la Password e confrontare Password nelle caselle di testo sono uguali).

L'aspetto interessante è che il controllo CreateUserWizard consulta le impostazioni di configurazione del provider di appartenenze per il rendering dell'interfaccia utente. Ad esempio, la sicurezza domanda e risposta nelle caselle di testo vengono visualizzati solo se `requiresQuestionAndAnswer` è impostata su True. Allo stesso modo, CreateUserWizard aggiunge automaticamente un controllo RegularExpressionValidator per assicurarsi che siano soddisfatti i requisiti di complessità delle password e imposta il relativo `ErrorMessage` e `ValidationExpression` proprietà in base il `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression` le impostazioni di configurazione.

Il controllo CreateUserWizard, come suggerisce il nome, è derivato dal [controllo Wizard](https://msdn.microsoft.com/en-us/library/s2etd1ek.aspx). Procedura guidata controlli sono progettati per fornire un'interfaccia per il completamento delle attività più passaggi. Un controllo procedura guidata potrebbe essere un numero arbitrario di `WizardSteps`, ognuno dei quali è un modello che definisce il codice HTML e controlli Web per tale passaggio. Controllo della procedura guidata vengono inizialmente visualizzate sia la prima `WizardStep`, insieme ai controlli di navigazione che consentono all'utente per passare da un passaggio alla successiva o per tornare alla procedura precedente.

Come mostrato nella figura 11 dichiarativo, interfaccia predefinita del controllo CreateUserWizard include due `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? esegue il rendering l'interfaccia per raccogliere informazioni per la creazione del nuovo account utente. Questo è il passaggio mostrato nella figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.completewizardstep.aspx) ? esegue il rendering di un messaggio che indica che l'account è stato creato correttamente.

Aspetto e il comportamento di CreateUserWizard possono essere modificati mediante la conversione di uno di questi passaggi per modelli, o aggiungendo la propria `WizardStep` s. Verranno esaminati aggiungendo un `WizardStep` all'interfaccia di registrazione nel *l'archiviazione di informazioni utente aggiuntive* esercitazione.

Esaminare il funzionamento del controllo CreateUserWizard. Visitare il `CreatingUserAccounts.aspx` pagina tramite un browser. Avviare immettendo alcuni valori non validi nell'interfaccia del CreateUserWizard. Provare a immettere una password che non sono conformi ai requisiti del livello di attendibilità password o lasciare vuota la casella di testo Nome utente. CreateUserWizard verrà visualizzato un messaggio di errore appropriato. Figura 12 illustra l'output durante il tentativo di creare un utente con una password sufficientemente complessa.


[![CreateUserWizard inserisce automaticamente i controlli di convalida](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Figura 12**: il CreateUserWizard automaticamente inserisce i controlli di convalida ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image36.png))


Successivamente, immettere i valori appropriati in CreateUserWizard e fare clic sul pulsante Crea utente. Supponendo che i campi obbligatori sono stati immessi e complessità della password è sufficiente, CreateUserWizard crea un nuovo account utente tramite il framework di appartenenza e quindi visualizzare il `CompleteWizardStep`dell'interfaccia (vedere Figura 13). Dietro le quinte, CreateUserWizard chiama il `Membership.CreateUser` (metodo), proprio come abbiamo fatto nel passaggio 5.


[![Un nuovo Account utente è stato creato correttamente](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Figura 13**: un nuovo Account utente è stata creata ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Come illustrato nella figura 13, il `CompleteWizardStep`dell'interfaccia include un pulsante Continua. Tuttavia, a questo punto verrà semplicemente esegue un postback, lasciando il visitatore nella stessa pagina. Nella personalizzazione di CreateUserWizard aspetto e comportamento tramite la proprietà sezione si esaminerà modo è possibile avere questo pulsante di invio visitatore `Default.aspx` (o un'altra pagina).


Dopo aver creato un nuovo account utente, tornare a Visual Studio ed esaminare il `aspnet_Users` e `aspnet_Membership` tabelle come abbiamo visto nella figura 10 per verificare che l'account è stato creato.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizzazione di CreateUserWizard comportamento e l'aspetto tramite le relative proprietà

In diversi modi, tramite le proprietà, è possibile personalizzare CreateUserWizard `WizardStep` s e gestori di eventi. In questa sezione verranno esaminati come personalizzare l'aspetto del controllo tramite le relative proprietà; la sezione successiva vengono esaminati si estende il comportamento del controllo tramite i gestori eventi.

Quasi tutto il testo visualizzato nell'interfaccia utente del controllo CreateUserWizard predefinito può essere personalizzato tramite la proprietà di moltissime. Ad esempio, è possono personalizzare le etichette del nome utente, Password, confermare la Password, posta elettronica, domanda di sicurezza e la risposta segreta visualizzate a sinistra delle caselle di testo con il [ `UserNameLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), e [ `AnswerLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) proprietà, rispettivamente. Analogamente, sono disponibili proprietà per specificare il testo dei pulsanti Create User e continua nel `CreateUserWizardStep` e `CompleteWizardStep`, nonché come se questi pulsanti vengono visualizzati come pulsanti, LinkButton o ImageButtons.

I colori, bordi, tipi di carattere e altri elementi visivi sono configurabili tramite un host di proprietà di stile. Il controllo CreateUserWizard presenta le proprietà di stile di controllo Web comuni - `BackColor`, `BorderStyle`, `CssClass`, `Font`e così via, e non vi sono un numero di proprietà di stile per definire l'aspetto per particolari sezioni del Interfaccia del CreateUserWizard. Il [ `TextBoxStyle` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), ad esempio, definisce gli stili per caselle di testo nel `CreateUserWizardStep`, mentre il [ `TitleTextStyle` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definisce lo stile per il titolo (effettuare l'iscrizione per il nuovo Account).

Oltre alle proprietà correlate all'aspetto, esistono diverse proprietà che influiscono sul comportamento del controllo CreateUserWizard. Il [ `DisplayCancelButton` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se impostato su True, consente di visualizzare un pulsante Annulla accanto al pulsante Crea utente (il valore predefinito è False). Se si visualizza il pulsante Annulla, assicurarsi di impostare anche la [ `CancelDestinationPageUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), che consente di specificare la pagina in cui l'utente venga inviato a dopo facendo clic su Annulla. Come indicato nella sezione precedente, il pulsante Continua il `CompleteWizardStep`dell'interfaccia provoca un postback, ma lascia il visitatore nella stessa pagina. Per inviare il visitatore a un'altra pagina dopo aver fatto clic sul pulsante Continua, è sufficiente specificare l'URL di [ `ContinueDestinationPageUrl` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Consente di aggiornare il `RegisterUser` controllo CreateUserWizard per mostrare un pulsante di annullamento e di inviare il visitatore per `Default.aspx` quando fa clic su pulsante Annulla o continua. A tale scopo, impostare il `DisplayCancelButton` proprietà su True, entrambe le `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` proprietà ~ / Default.aspx. Nella figura 14 viene CreateUserWizard aggiornata quando viene visualizzato tramite un browser.


[![Il CreateUserWizardStep include un pulsante Annulla](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Nella figura 14**: il `CreateUserWizardStep` include un pulsante Annulla ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image42.png))


Quando un visitatore immette un nome utente, password, indirizzo di posta elettronica e domanda di sicurezza e una risposta e si fa clic su Crea utente, viene creato un nuovo account utente e il visitatore è effettuato l'accesso come utente appena creato. Supponendo che l'utente visita la pagina è la creazione di un nuovo account per se stessi, il problema è dovuto al comportamento desiderato. Tuttavia, è consigliabile consentire agli amministratori di aggiungere nuovi account utente. In questo modo, verrebbe creato l'account utente, ma l'amministratore potrebbe rimanere connesso come amministratore (e non come account appena creato). Questo comportamento può essere modificato tramite il valore booleano [ `LoginCreatedUser` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Gli account utente in framework appartenenza contengono un flag approvati. gli utenti che non sono stati approvati sono in grado di accedere al sito. Per impostazione predefinita, un account appena creato viene contrassegnato come approvata, consentendo all'utente di accedere al sito immediatamente. È possibile, tuttavia, disporre di nuovi account utente contrassegnato come non approvati. Si supponga di un amministratore di approvare manualmente i nuovi utenti per poter accedere. o forse si desidera verificare che l'indirizzo di posta elettronica immesso al momento dell'iscrizione sia valido prima di consentire all'utente di accedere. L'elemento può essere il caso, si può avere l'account utente appena creato contrassegnato come non approvati impostando il controllo CreateUserWizard [ `DisableCreatedUser` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) su True (il valore predefinito è False).

Includere altre proprietà relative al comportamento di nota `AutoGeneratePassword` e `MailDefinition`. Se il [ `AutoGeneratePassword` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) è impostata su True, il `CreateUserWizardStep` non vengono visualizzate caselle di testo Password e Conferma Password; al contrario, la password dell'utente appena creato viene generata automaticamente utilizzando il `Membership` della classe [ `GeneratePassword` metodo](https://msdn.microsoft.com/en-us/library/system.web.security.membership.generatepassword.aspx). Il `GeneratePassword` metodo costruisce una password di lunghezza specificata e con un numero sufficiente di caratteri non alfanumerici per soddisfare i requisiti di complessità delle password configurata.

Il [ `MailDefinition` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) è utile se si desidera inviare un messaggio di posta elettronica all'indirizzo di posta elettronica specificato durante il processo di creazione di account. Il `MailDefinition` proprietà include una serie di sottoproprietà per la definizione di informazioni sul messaggio di posta elettronica costruito. Queste sottoproprietà includono opzioni come `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, e `BodyFileName`. Il [ `BodyFileName` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) punta a un file di testo o HTML che contiene il corpo del messaggio di posta elettronica. Il corpo supporta due segnaposto predefiniti: `<%UserName%>` e `<%Password%>`. Questi segnaposto, se presente nel `BodyFileName` file, verrà sostituita con nome e la password dell'utente appena creato.

> [!NOTE]
> Il `CreateUserWizard` del controllo `MailDefinition` proprietà specifica solo i dettagli relativi al messaggio di posta elettronica inviati quando viene creato un nuovo account. Non include i dettagli su come messaggio di posta elettronica viene effettivamente inviato (vale a dire, se viene utilizzata una directory di server o di ricezione posta SMTP, le informazioni di autenticazione e così via). Questi dettagli di basso livello devono essere definiti nel `<system.net>` sezione `Web.config`. Per ulteriori informazioni su queste impostazioni di configurazione e sull'invio di posta elettronica da ASP.NET 2.0 in generale, vedere il [domande frequenti in SystemNetMail.com](http://www.systemnetmail.com/) e l'articolo, [l'invio di posta elettronica in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estendere il comportamento del CreateUserWizard utilizzo dei gestori eventi

Il controllo CreateUserWizard genera un numero di eventi durante il flusso di lavoro. Ad esempio, dopo un visitatore immette il proprio nome utente, password e altre informazioni utili e fa clic sul pulsante Crea utente, il controllo CreateUserWizard genera relativo [ `CreatingUser` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si verifica un problema durante il processo di creazione, la [ `CreateUserError` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) è attivato; tuttavia, se l'utente è stato creato, quindi il [ `CreatedUser` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) viene generato. Sono presenti altri eventi di controllo CreateUserWizard che viene generati, ma sono quelle più pertinenti tre.

In alcuni scenari potrebbe voler toccare nel flusso di lavoro CreateUserWizard che possiamo mediante la creazione di un gestore eventi per l'evento appropriato. Per illustrare questo concetto, è possibile migliorare la `RegisterUser` controllo CreateUserWizard per includere una convalida personalizzata del nome utente e password. In particolare, si migliorano il nostro CreateUserWizard in modo che i nomi utente non possono contenere spazi iniziali o finali e il nome utente non può trovarsi in qualsiasi punto della password. In breve, si vuole impedire che altri utenti di creare un nome utente come "Scott", o una combinazione di nome utente/password come Scott e Scott.1234.

A tale scopo si creerà un gestore eventi per il `CreatingUser` evento per eseguire i controlli di convalida aggiuntivi. Se i dati forniti non sono validi, è necessario annullare il processo di creazione. È inoltre necessario aggiungere un controllo etichetta Web alla pagina per visualizzare un messaggio che indica che il nome utente o password non è valida. Per iniziare, aggiungere un controllo etichetta sotto il controllo CreateUserWizard, l'impostazione relativa `ID` proprietà `InvalidUserNameOrPasswordMessage` e il relativo `ForeColor` proprietà `Red`. Cancellare il contenuto relativo `Text` proprietà e impostare il relativo `EnableViewState` e `Visible` proprietà su False.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Successivamente, creare un gestore eventi per il controllo CreateUserWizard `CreatingUser` evento. Per creare un gestore eventi, selezionare il controllo nella finestra di progettazione e quindi passare alla finestra Proprietà. Da qui, fare clic sull'icona a forma di fulmine e quindi fare doppio clic sull'evento appropriato per creare il gestore dell'evento.

Aggiungere il codice seguente al gestore eventi `CreatingUser`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Si noti che il nome utente e la password immessi nel controllo CreateUserWizard sono disponibili tramite il relativo [ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [ `Password` proprietà](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.password.aspx), rispettivamente. Utilizziamo queste proprietà nel gestore dell'evento precedente per determinare se il nome utente fornito contiene spazi iniziali o finali e indica se il nome utente viene trovato all'interno della password. Se una di queste condizioni è soddisfatta, viene visualizzato un messaggio di errore nel `InvalidUserNameOrPasswordMessage` etichetta e il gestore di evento `e.Cancel` è impostata su `True`. Se `e.Cancel` è impostato su `True`, CreateUserWizard provoca un corto circuito relativo flusso di lavoro, in modo efficace se si annulla il processo di creazione di account utente.

Figura 15 mostra una schermata di `CreatingUserAccounts.aspx` quando l'utente immette un nome utente con gli spazi iniziali.


[![I nomi utente con iniziali o finali spazi non sono consentiti.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Figura 15**: non sono consentiti nomi utente con iniziali o finali spazi ([fare clic per visualizzare l'immagine ingrandita](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> Vedremo un esempio di utilizzo del controllo CreateUserWizard `CreatedUser` evento nel  *<a id="_msoanchor_11"> </a> [l'archiviazione di informazioni utente aggiuntive](storing-additional-user-information-vb.md)*  esercitazione.


## <a name="summary"></a>Riepilogo

Il `Membership` della classe `CreateUser` metodo crea un nuovo account utente nel framework di appartenenza. Esegue l'operazione mediante la delega la chiamata al provider di appartenenze configurato. In caso del `SqlMembershipProvider`, `CreateUser` metodo aggiunge un record per il `aspnet_Users` e `aspnet_Membership` tabelle di database.

Sebbene sia possibile creare nuovi account utente a livello di programmazione (come illustrato nel passaggio 5), un approccio più veloce e semplice consiste nell'utilizzare il controllo CreateUserWizard. Questo controllo viene eseguito il rendering di un'interfaccia utente costituito da più passaggi per la raccolta di informazioni utente e la creazione di un nuovo utente nel framework di appartenenza. Sotto le quinte, questo controllo viene utilizzata la stessa `Membership.CreateUser` metodo come esaminato nel passaggio 5, ma il controllo crea l'interfaccia utente, i controlli di convalida e risponde agli errori di creazione di account utente senza dover scrivere codice.

A questo punto è disponibile la funzionalità in grado di creare nuovi account utente. Tuttavia, la pagina di accesso ancora è convalida in base a tali credenziali a livello di codice specificato nuovamente nell'esercitazione di secondo. Nel <a id="_msoanchor_12"> </a> [esercitazione successiva](validating-user-credentials-against-the-membership-user-store-vb.md) verrà aggiornato `Login.aspx` per convalidare l'utente del fornito le credenziali per il framework di appartenenza.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [`CreateUser`Documentazione tecnica](https://msdn.microsoft.com/En-US/library/system.web.security.membershipprovider.createuser.aspx)
- [Cenni preliminari sul controllo CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Creazione di un Provider di mappe basate sul sistema del sito di File](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Creazione di un'interfaccia utente dettagliate con il controllo ASP.NET 2.0 guidata](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Analisi di ASP.NET 2.0 dell'esplorazione del sito](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Pagine master e esplorazione del sito](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Il Provider della mappa del sito SQL che attendere](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri e fondatore di 4GuysFromRolla, ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è  *[SAM insegna manualmente ASP.NET 2.0 nelle 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott può essere raggiunto al [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog all'indirizzo [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Precedente](creating-the-membership-schema-in-sql-server-vb.md)
[Successivo](validating-user-credentials-against-the-membership-user-store-vb.md)
