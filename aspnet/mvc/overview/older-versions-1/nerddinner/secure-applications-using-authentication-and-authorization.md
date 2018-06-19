---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteggere le applicazioni tramite l'autenticazione e autorizzazione | Documenti Microsoft
author: microsoft
description: Passaggio 9 viene illustrato come aggiungere l'autenticazione e autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti devono registrare e accedere al sito da creare...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4a9b1e6d7d453bd8dc5a61b1f1cec4617af7d693
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874382"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteggere le applicazioni tramite l'autenticazione e autorizzazione
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 9 di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 9 viene illustrato come aggiungere l'autenticazione e autorizzazione per proteggere l'applicazione NerdDinner, in modo che gli utenti devono registrare e l'accesso al sito per creare nuovi dinners e solo l'utente che ospita una cena possibile modificarlo in un secondo momento.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner passaggio 9: Autenticazione e autorizzazione

Ora il nostro NerdDinner applicazione concede chiunque visitare il sito la possibilità di creare e modificare i dettagli di qualsiasi dinner. È necessario modificare questo, in modo che gli utenti devono registrare e account di accesso al sito per creare nuovi dinners e aggiungere una restrizione in modo che solo l'utente che ospita una cena possibile modificarlo in un secondo momento.

A tale scopo si userà l'autenticazione e autorizzazione per proteggere l'applicazione.

### <a name="understanding-authentication-and-authorization"></a>Autorizzazione e informazioni sull'autenticazione

*Autenticazione* è il processo di identificazione e la convalida dell'identità di un client che accede a un'applicazione. In termini più semplici, risulta sull'identificazione "che l'utente finale quando visitano un sito Web". ASP.NET supporta più modalità per autenticare gli utenti del browser. Per le applicazioni web di Internet, l'approccio più comune di autenticazione utilizzato è denominato "Autenticazione basata su form". Autenticazione basata su form consente agli sviluppatori di creare un form di accesso HTML all'interno di propria applicazione e quindi convalidare il nome utente/password che un utente finale invia rispetto a un database o un altro archivio delle credenziali di password. Se la combinazione di nome utente/password è corretta, lo sviluppatore può quindi chiedere ASP.NET per rilasciare un cookie HTTP crittografato per identificare l'utente in tutte le richieste future. Ti invieremo un utilizzando l'autenticazione basata su form con l'applicazione NerdDinner.

*Autorizzazione* è il processo volto a determinare se un utente autenticato dispone dell'autorizzazione per accedere a un particolare URL/risorsa o di eseguire un'azione. Ad esempio, all'interno dell'applicazione NerdDinner si desidera autorizzare che possono accedere solo gli utenti che sono registrati il *Dinners/crea* URL e creare di nuovo Dinners. Si consiglia inoltre di aggiungere la logica di autorizzazione in modo che solo l'utente che ospita una cena può modificare, ad esempio e negare l'accesso in modifica a tutti gli altri utenti.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticazione basata su form e il AccountController

Il modello di progetto di Visual Studio predefinito per ASP.NET MVC Abilita autenticazione basata su form automaticamente quando vengono create nuove applicazioni ASP.NET MVC. Aggiunge automaticamente anche un'implementazione di pagina di accesso account preesistente al progetto, che rende molto semplice per l'integrazione di protezione all'interno di un sito.

Pagina master predefinita Site. master consente di visualizzare un collegamento "Accedi" in alto a destra del sito quando l'utente accede a tale colonna non è autenticata:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Facendo clic sul collegamento "Accedi" consente a un utente di */Account/accesso* URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Possono farlo facendo clic sul collegamento "Register" – giungeranno per visitatori che è ancora stato registrato il */Account/Register* URL e consentire loro di immettere i dettagli dell'account:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Fare clic sul pulsante "Register" verrà creare un nuovo utente all'interno del sistema di appartenenze di ASP.NET e autenticare l'utente al sito utilizzando l'autenticazione basata su form.

Quando un utente è connesso, Site cambia alto a destra della pagina per l'output di un "pagina iniziale [username]!" messaggio ed esegue il rendering di un "Disconnetti" collegamento anziché un "Accedi" uno. Facendo clic sul collegamento "Disconnetti" si disconnette l'utente:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funzionalità di accesso, disconnessione e la registrazione precedente viene implementata all'interno della classe AccountController che è stato aggiunto al progetto da Visual Studio, durante la creazione del progetto. L'interfaccia utente per il AccountController viene implementata utilizzando i modelli di visualizzazione all'interno della directory \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController utilizza il sistema di autenticazione basata su form ASP.NET per rilasciare i cookie di autenticazione crittografata e l'API di appartenenza ASP.NET per archiviare e convalidare i nomi utente e password. L'API di appartenenza ASP.NET è estendibile e consente a qualsiasi archivio di credenziali di password da utilizzare. ASP.NET viene fornito con implementazioni del provider di appartenenze predefinito che archiviano nome utente e password all'interno di un database SQL o all'interno di Active Directory.

È possibile configurare il provider di appartenenze deve utilizzare l'applicazione NerdDinner aprendo il file "Web. config" nella directory principale del progetto e cercare il &lt;appartenenza&gt; sezione all'interno di esso. Registra il provider di appartenenze SQL, aggiunti quando è stato creato il progetto Web. config predefinito e lo configura per utilizzare una stringa di connessione denominata "ApplicationServices" per specificare il percorso del database.

La stringa di connessione predefinita "ApplicationServices" (specificato all'interno di &lt;connectionStrings&gt; sezione del file Web. config) è configurato per utilizzare SQL Express. Punta a un database di SQL Express denominato "ASPNETDB. MDF"dell'applicazione" App\_dati "directory. Se questo database non esiste la prima volta che viene utilizzata l'API di appartenenza all'interno dell'applicazione, ASP.NET verrà automaticamente creato il database e il provisioning lo schema del database di appartenenza appropriata all'interno di essa:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Se anziché utilizzare SQL Express Desideravamo per utilizzare un'istanza completa di SQL Server (o per connettersi a un database remoto), tutti sarà necessaria attività da eseguire consiste nell'aggiornare la stringa di connessione "ApplicationServices" all'interno del file Web. config e verificare che lo schema appropriato appartenenza è stato aggiunto al database di che cui punta. È possibile eseguire il "aspnet\_regsql.exe" utilità all'interno della directory \Windows\Microsoft.NET\Framework\v2.0.50727\ per aggiungere lo schema appropriato per l'appartenenza e di altri servizi delle applicazioni ASP.NET a un database.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorizzazione URL Dinners/Create utilizzando il filtro [Authorize]

Non è necessario scrivere codice per abilitare un'implementazione di gestione di account per l'applicazione NerdDinner e l'autenticazione protetta. Gli utenti possono registrare i nuovi account con l'applicazione e account di accesso/disconnessione del sito.

È ora possibile aggiungere logica di autorizzazione all'applicazione e utilizzare lo stato di autenticazione nome utente di visitatori per controllare cosa possono e non all'interno del sito. Si inizierà dall'aggiunta di logica di autorizzazione per i metodi di azione della classe organizzazione DinnersController "Crea". In particolare, si richiederà che gli utenti che accedono il *Dinners/crea* URL deve essere registrato. Se non sono connessi è sarà reindirizza alla pagina di accesso in modo che essi possono Accedi.

Implementazione di questa logica è piuttosto semplice. Ecco cosa da fare è aggiungere un attributo di filtro [Authorize] per i metodi di azione Create in questo modo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC supporta la possibilità di creare "filtri azione" che possono essere utilizzati per implementare la logica riutilizzabile che può essere applicata in modo dichiarativo a metodi di azione. Il filtro [Authorize] è uno dei filtri azione predefiniti forniti da ASP.NET MVC e consente agli sviluppatori di regole di autorizzazione in modo dichiarativo applicate ai metodi di azione e alle classi controller.

Quando applicato senza parametri (come sopra) il filtro [Authorize] impone che l'utente che effettua la richiesta del metodo di azione deve essere effettuato l'accesso, e verrà reindirizzati automaticamente il browser all'URL di accesso se non sono. Quando esegue il reindirizzamento all'URL richiesto originariamente viene passato come argomento di stringa di query (ad esempio: / Account/accesso? ReturnUrl = % 2fDinners % 2fCreate). Il AccountController quindi reindirizzerà l'utente all'URL richiesto originariamente una volta all'accesso.

Il filtro [Authorize] supporta facoltativamente la possibilità di specificare una proprietà "Users" o "Ruoli" che può essere utilizzata per richiedere che l'utente sia nel registro e all'interno di un elenco di utenti consentiti o un membro del ruolo di sicurezza consentiti. Ad esempio, il codice riportato di seguito consente solo due utenti specifici, "scottgu" e "billg" accedere all'URL Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

I nomi utente specifico all'interno di codice di incorporamento tende a essere tuttavia abbastanza non gestibile. Un approccio migliore consiste nel definire "ruoli" di livello superiore che controlla il codice, quindi eseguire il mapping di utenti al ruolo di utilizzo di un database o un sistema di active directory (l'elenco di mapping utente effettivo da archiviare esternamente dal codice di attivazione). ASP.NET include un'API di gestione di ruoli predefiniti, nonché un set predefinito di provider di ruoli (incluse quelle per SQL e Active Directory) che consentono di eseguire il mapping di utenti e ruoli. È quindi possibile aggiornare il codice per consentire agli utenti all'interno di un ruolo specifico "admin" accedere all'URL Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Utilizzando la proprietà User.Identity.Name durante la creazione di Dinners

È possibile recuperare il nome utente dell'utente connesso di una richiesta utilizzando la proprietà User.Identity.Name esposta nella classe di base Controller.

Precedente quando è implementata la versione HTTP-POST, il metodo di creazione del metodo di azione abbiamo hardcoded la proprietà "HostedBy" della cena in una stringa statica. È possibile aggiornare questo codice per utilizzare invece la proprietà User.Identity.Name, nonché aggiungere automaticamente una RSVP per la creazione di Dinner dell'host:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Poiché è stato aggiunto un attributo [Authorize] per il metodo di metodo di creazione, ASP.NET MVC assicura che il metodo di azione viene eseguita solo se è connesso l'utente visita l'URL Dinners/Create nel sito. Di conseguenza, il valore della proprietà User.Identity.Name conterrà sempre un nome utente valido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Utilizzando la proprietà User.Identity.Name durante la modifica di Dinners

Ora aggiungere una logica di autorizzazione che limita gli utenti in modo che possano modificare solo le proprietà di dinners che vengono anch'essi ospitano.

A tale scopo, innanzitutto verrà aggiunto un metodo di supporto "IsHostedBy(username)" all'oggetto Dinner (all'interno della classe parziale di Dinner.cs che è stato creato in precedenza). Questo metodo di supporto restituisce true o false a seconda se un nome utente fornito corrisponde alla proprietà Dinner HostedBy e incapsula la logica necessaria per eseguire un confronto di stringhe tra maiuscole e minuscole di essi:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Si aggiungerà quindi un attributo [Authorize] ai metodi di azione Edit() all'interno della classe DinnersController. Ciò garantisce che gli utenti necessario eseguire l'accesso alla richiesta di un */Dinners/modifica / [id]* URL.

È quindi possibile aggiungere codice per i metodi di modifica che utilizza il metodo di supporto Dinner.IsHostedBy(username) per verificare che l'utente connesso corrisponda all'host Dinner. Se l'utente non è l'host, viene visualizza una vista "InvalidOwner" e terminare la richiesta. Il codice per eseguire questa operazione è simile al seguente:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

È quindi possibile fare doppio clic sulla directory \Views\Dinners e scegliere Aggiungi -&gt;consente di visualizzare il comando di menu per creare una nuova visualizzazione "InvalidOwner". È possibile popolarla con il messaggio di errore seguente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

E, quando un utente tenta di modificare una cena che non possiedono, verrà visualizzato un messaggio di errore:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

È possibile ripetere gli stessi passaggi per i metodi di azione Delete () all'interno di questo controller per bloccare l'autorizzazione per eliminare anche Dinners e assicurarsi che solo l'host di una cena possibile eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Modifica di mostrare o nascondere e collegamenti Elimina

Il metodo di azione di modifica e l'eliminazione della classe organizzazione DinnersController è stiamo collegamento da questo URL dettagli:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Attualmente viene mostrato il collegamenti ad azioni di modifica ed eliminazione indipendentemente dal fatto se il visitatore per l'URL di dettagli è l'host di dinner. È necessario modificare questo in modo che i collegamenti vengono visualizzati solo se l'utente visita è il proprietario della cena.

Il metodo di azione all'interno di questo DinnersController Details() recupera un oggetto Dinner e quindi passa come modello di oggetto per il modello di visualizzazione:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

È possibile aggiornare il modello di visualizzazione per in modo condizionale mostrare/nascondere i collegamenti di modifica ed eliminazione utilizzando la Dinner.IsHostedBy() come il seguente metodo di supporto:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Passaggi successivi

Ora esaminiamo come è possibile abilitare gli utenti autenticati possono RSVP per l'utilizzo di AJAX dinners.

> [!div class="step-by-step"]
> [Precedente](implement-efficient-data-paging.md)
> [Successivo](use-ajax-to-deliver-dynamic-updates.md)
